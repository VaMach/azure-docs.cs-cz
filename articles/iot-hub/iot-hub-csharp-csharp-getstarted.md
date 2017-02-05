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
ms.date: 09/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: 128e3607829d3838cdbb285fa95a1f7cd2112229


---
# <a name="get-started-with-azure-iot-hub-net"></a>Začínáme se službou Azure IoT Hub (.NET)
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace .NET:

* **CreateDeviceIdentity** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení aplikace simulovaného zařízení.
* **ReadDeviceToCloudMessages** zobrazuje telemetrické údaje odesílané aplikací simulovaného zařízení.
* **SimulatedDevice** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu AMQP.

> [!NOTE]
> Informace o sadách SDK služby Azure IoT Hub, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení, najdete v tématu [Sady SDK služby IoT Hub][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Microsoft Visual Studio 2015.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili službu IoT Hub a máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci .NET, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části Registr identit v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt pojmenujte **CreateDeviceIdentity**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **CreateDeviceIdentity** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.
   
    ![Okno Správce balíčků NuGet][11]
4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu do třídy **Program**:
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    Tato metoda vytvoří identitu zařízení s ID **myFirstDevice**. (Pokud toto ID zařízení již v registru identit existuje, kód jednoduše načte informace o stávajícím zařízení.) Aplikace pak zobrazí primární klíč pro danou identitu. Tento klíč v aplikaci simulovaného zařízení slouží k připojení ke službě IoT Hub.
7. Nakonec do metody **Main** přidejte následující řádky:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Spusťte aplikaci a poznamenejte si klíč zařízení.
   
    ![Klíč zařízení generovaný aplikací][12]

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci .NET, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Azure Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Další informace o zpracování zpráv ze služby Event Hubs najdete v kurzu [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial]. (Tento kurz se vztahuje na koncové body kompatibilní se službou IoT Hub Event Hub.)

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.
> 
> 

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt nazvěte **ReadDeviceToCloudMessages**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **ReadDeviceToCloudMessages** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte možnost **Instalovat** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje služba [Azure Service Bus][lnk-servicebus-nuget] a všechny její závislosti a přidá se na ni odkaz. Tento balíček umožní aplikaci připojení ke koncovému bodu kompatibilnímu se službou Event Hubs ve službě IoT Hub.
4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.ServiceBus.Messaging;
        using System.Threading;
5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v části Vytvoření služby IoT Hub.
   
        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;
6. Přidejte následující metodu do třídy **Program**:
   
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
   
    Tato metoda používá instanci **EventHubReceiver** k příjmu zpráv ze všech oddílů pro příjem zpráv typu zařízení-cloud ve službě IoT Hub: Všimněte si, jak při vytváření objektu **EventHubReceiver** předáte parametr `DateTime.Now`, aby objekt přijímal pouze zprávy odeslané po spuštění. Tento filtr je užitečný v testovacím prostředí, protože uvidíte aktuální sadu zpráv. V produkčním prostředí byste se měli ujistit, že váš kód zpracovává všechny zprávy. Další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].
7. Nakonec do metody **Main** přidejte následující řádky:
   
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

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci .NET, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt pojmenujte **SimulatedDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **SimulatedDevice** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices.Client** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro zařízení Azure IoT][lnk-device-nuget] a jeho závislosti a přidá se na něj odkaz.
4. Do horní části souboru **Program.cs** přidejte následující příkaz `using`:
   
        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;
5. Do třídy **Program** přidejte následující pole. Hodnoty zástupných symbolů nahraďte názvem hostitele služby IoT Hub, který jste získali v části Vytvoření služby IoT Hub, a klíčem zařízení získaným v částí Vytvoření identity zařízení.
   
        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";
6. Přidejte následující metodu do třídy **Program**:
   
        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();
   
            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;
   
                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));
   
                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);
   
                Task.Delay(1000).Wait();
            }
        }
   
    Tato metoda odesílá novou zprávu typu zařízení-cloud každou sekundu. Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovaným číslem, který simuluje snímač rychlosti větru.
7. Nakonec do metody **Main** přidejte následující řádky:
   
        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));
   
        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();
   
   Ve výchozím nastavení metoda **Create** vytvoří instanci **DeviceClient**, která se službou IoT Hub komunikuje pomocí protokolu AMQP. Pokud chcete používat protokol HTTP, použijte přepis metody **Create**, který umožňuje určit protokol. Pokud používáte protokol HTTP, měli byste do svého projektu přidat také balíček NuGet **Microsoft.AspNet.WebApi.Client**, aby projekt zahrnoval obor názvů **System.Net.Http.Formatting**.

Tento kurz vás provede postupem vytvoření aplikace simulovaného zařízení služby IoT Hub. K přidání nezbytného kódu do aplikace zařízení můžete také použít rozšíření [Připojená služba pro službu Azure IoT Hub][lnk-connected-service] sady Visual Studio.

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.
> 
> 

## <a name="run-the-apps"></a>Spouštění aplikací
Nyní jste připraveni aplikaci spustit.

1. V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**. Vyberte možnost **Více projektů po spuštění** a poté příkaz **Spustit** jako akci pro oba projekty **ReadDeviceToCloudMessages** a **SimulatedDevice**.
   
    ![Vlastnosti projektu po spuštění][41]
2. Stisknutím klávesy **F5** spusťte obě aplikace. Výstup konzoly z aplikace **SimulatedDevice** zobrazuje zprávy, které aplikace simulovaného zařízení odesílá do služby IoT Hub. Výstup konzoly z aplikace **ReadDeviceToCloudMessages** zobrazuje zprávy, které služba IoT Hub přijímá.
   
    ![Výstup konzoly z aplikací][42]
3. Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:
   
    ![Dlaždice Využití v portálu Azure][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste na webu Azure Portal nakonfigurovali službu IoT Hub a pak jste v registru identit této služby vytvořili identitu zařízení. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se sadou IoT Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Dec16_HO1-->


