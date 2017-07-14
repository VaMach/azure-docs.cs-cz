---
title: "Začínáme se službou Azure IoT Hub (.NET) | Dokumentace Microsoftu"
description: "Postup odesílání zpráv typu zařízení-cloud ze zařízení do služby Azure IoT Hub pomocí sad SDK Azure IoT pro .NET. Vytvoříte aplikaci simulovaného zařízení pro odesílání zpráv, aplikaci služby pro registraci zařízení v registru identit a aplikaci služby pro čtení zpráv typu zařízení-cloud ze služby IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 477f618c09c8cf572a16d142f63c9b3553050b20
ms.contentlocale: cs-cz
ms.lasthandoff: 06/23/2017


---
# Připojení simulovaného zařízení k IoT Hubu pomocí .NET
<a id="connect-your-simulated-device-to-your-iot-hub-using-net" class="xliff"></a>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace .NET:

* **CreateDeviceIdentity** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení aplikace simulovaného zařízení.
* **ReadDeviceToCloudMessages** zobrazuje telemetrické údaje odesílané aplikací simulovaného zařízení.
* **SimulatedDevice** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.

Řešení Visual Studio, které obsahuje tyto tři aplikace z Githubu, si můžete stáhnout nebo naklonovat.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Informace o sadách SDK služby Azure IoT Hub, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení, najdete v tématu [Sady SDK služby IoT Hub][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili službu IoT Hub a máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení kurzu.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## Příjem zpráv typu zařízení-cloud
<a id="receive-device-to-cloud-messages" class="xliff"></a>
V této části vytvoříte konzolovou aplikaci .NET, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Azure Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Další informace o zpracování zpráv ze služby Event Hubs najdete v kurzu [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial]. (Tento kurz se vztahuje na koncové body kompatibilní se službou IoT Hub Event Hub.)

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.
> 
> 

1. V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt nazvěte **ReadDeviceToCloudMessages**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10a]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **ReadDeviceToCloudMessages** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte možnost **Instalovat** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje služba [Azure Service Bus][lnk-servicebus-nuget] a všechny její závislosti a přidá se na ni odkaz. Tento balíček umožní aplikaci připojení ke koncovému bodu kompatibilnímu se službou Event Hubs ve službě IoT Hub.
4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
   ```csharp
   using Microsoft.ServiceBus.Messaging;
   using System.Threading;
   ```
5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v části Vytvoření služby IoT Hub.
   
   ```csharp
   static string connectionString = "{iothub connection string}";
   static string iotHubD2cEndpoint = "messages/events";
   static EventHubClient eventHubClient;
   ```
6. Přidejte následující metodu do třídy **Program**:
   
   ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
   ```
   
    Tato metoda používá instanci **EventHubReceiver** k příjmu zpráv ze všech oddílů pro příjem zpráv typu zařízení-cloud ve službě IoT Hub: Všimněte si, jak při vytváření objektu **EventHubReceiver** předáte parametr `DateTime.Now`, aby objekt přijímal pouze zprávy odeslané po spuštění. Tento filtr je užitečný v testovacím prostředí, protože uvidíte aktuální sadu zpráv. V produkčním prostředí byste se měli ujistit, že váš kód zpracovává všechny zprávy. Další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].
7. Nakonec do metody **Main** přidejte následující řádky:
   
   ```csharp
    Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
   ```

## Vytvoření aplikace simulovaného zařízení
<a id="create-a-simulated-device-app" class="xliff"></a>
V této části vytvoříte konzolovou aplikaci .NET, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt pojmenujte **SimulatedDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10b]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **SimulatedDevice** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices.Client** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro zařízení Azure IoT][lnk-device-nuget] a jeho závislosti a přidá se na něj odkaz.
4. Do horní části souboru **Program.cs** přidejte následující příkaz `using`:
   
   ```csharp
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   ```
5. Do třídy **Program** přidejte následující pole. Hodnoty zástupných symbolů nahraďte názvem hostitele služby IoT Hub, který jste získali v části Vytvoření služby IoT Hub, a klíčem zařízení získaným v částí Vytvoření identity zařízení.
   
   ```csharp
   static DeviceClient deviceClient;
   static string iotHubUri = "{iot hub hostname}";
   static string deviceKey = "{device key}";
   ```

6. Přidejte následující metodu do třídy **Program**:
   
   ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
   ```
    Tato metoda odesílá novou zprávu typu zařízení-cloud každou sekundu. Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovanými čísly, který simuluje snímač teploty a snímač vlhkosti.
7. Nakonec do metody **Main** přidejte následující řádky:
   
   ```csharp
   Console.WriteLine("Simulated device\n");
   deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey), TransportType.Mqtt);

   SendDeviceToCloudMessagesAsync();
   Console.ReadLine();
   ```
   
   Ve výchozím nastavení metoda **Create** v aplikaci .NET Framework vytvoří instanci **DeviceClient**, která se službou IoT Hub komunikuje pomocí protokolu AMQP (klienti UPW a PCL standardně používají protokol HTTP). Pokud chcete používat protokol MQTT nebo HTTP, použijte přepis metody **Create**, který umožňuje určit protokol. Pokud používáte protokol HTTP, měli byste do svého projektu přidat také balíček NuGet **Microsoft.AspNet.WebApi.Client**, aby projekt zahrnoval obor názvů **System.Net.Http.Formatting**.

Tento kurz vás provede postupem vytvoření aplikace simulovaného zařízení služby IoT Hub. K přidání nezbytného kódu do aplikace zařízení můžete také použít rozšíření [Připojená služba pro službu Azure IoT Hub][lnk-connected-service] sady Visual Studio.

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.
> 
> 

## Spouštění aplikací
<a id="run-the-apps" class="xliff"></a>
Nyní jste připraveni aplikaci spustit.

1. V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**. Vyberte možnost **Více projektů po spuštění** a poté příkaz **Spustit** jako akci pro oba projekty **ReadDeviceToCloudMessages** a **SimulatedDevice**.
   
    ![Vlastnosti projektu po spuštění][41]
2. Stisknutím klávesy **F5** spusťte obě aplikace. Výstup konzoly z aplikace **SimulatedDevice** zobrazuje zprávy, které aplikace simulovaného zařízení odesílá do služby IoT Hub. Výstup konzoly z aplikace **ReadDeviceToCloudMessages** zobrazuje zprávy, které služba IoT Hub přijímá.
   
    ![Výstup konzoly z aplikací][42]
3. Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:
   
    ![Dlaždice Využití na portálu Azure Portal][43]

## Další kroky
<a id="next-steps" class="xliff"></a>
V tomto kurzu jste na webu Azure Portal nakonfigurovali službu IoT Hub a pak jste v registru identit této služby vytvořili identitu zařízení. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou IoT Edge][lnk-iot-edge]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

