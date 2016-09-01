<properties
    pageTitle="Začínáme se službou Azure IoT Hub pro jazyk C# | Microsoft Azure"
    description="Úvodní kurz pro službu Azure IoT Hub pro jazyk C#. Implementace internetu věcí pomocí služby Azure IoT Hub a jazyka C# spolu se sadami SDK služby Microsoft Azure IoT."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/16/2016"
     ms.author="dobett"/>

# Začínáme se službou Azure IoT Hub pro rozhraní .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace systému Windows:

* **CreateDeviceIdentity** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulovaného zařízení.
* **ReadDeviceToCloudMessages** zobrazuje telemetrické zprávy odesílané simulovaným zařízením.
* **SimulatedDevice** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu AMQPS.

> [AZURE.NOTE] Informace o různých sadách SDK, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení, najdete v tématu [Sady SDK služby IoT Hub][lnk-hub-sdks ].

K absolvování tohoto kurzu potřebujete:

+ Microsoft Visual Studio 2015.

+ Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial]).

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili službu IoT Hub a máte název hostitele a připojovací řetězec, které potřebujete k dokončení kurzu.

## Vytvoření identity zařízení

V této části vytvoříte konzolovou aplikaci systému Windows, která v registru identit ve službě IoT Hub vytvoří novou identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit zařízení. Další informace najdete v části „Registr identit zařízení“ tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

1. V sadě Visual Studio přidejte k aktuálnímu řešení nový klasický desktopový projekt jazyka Visual C# pro systém Windows pomocí šablony projektu **konzolové aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt pojmenujte **CreateDeviceIdentity**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]

2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **CreateDeviceIdentity** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.

3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tím se stáhne a nainstaluje balíček NuGet [sady SDK služby Microsoft Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.

    ![Okno Správce balíčků NuGet][11]

4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, který jste vytvořili v předchozí části.

        static RegistryManager registryManager;
        static string connectionString = "{iothub connection string}";

6. Do třídy **Program** přidejte následující metodu.

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

    Tato metoda vytvoří novou identitu zařízení s ID **myFirstDevice**. (Pokud toto ID zařízení již v registru existuje, kód jednoduše načte informace o stávajícím zařízení.) Aplikace pak zobrazí primární klíč pro danou identitu. Tento klíč v simulovaném zařízení slouží k připojení do služby IoT Hub.

7. Nakonec do metody **Main** přidejte následující řádky:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Spusťte aplikaci a poznamenejte si klíč zařízení.

    ![Klíč zařízení generovaný aplikací][12]

> [AZURE.NOTE] V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu do služby. Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].

## Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci systému Windows, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Azure Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Další informace o zpracování zpráv ze služby Event Hubs naleznete v kurzu [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial]. (Tento kurz se vztahuje na koncové body kompatibilní se službou IoT Hub Event Hubs.)

> [AZURE.NOTE] Kompatibilní koncový bod služby Event Hubs pro čtení zpráv typu zařízení-cloud vždy používá protokol AMQPS.

1. V sadě Visual Studio přidejte k aktuálnímu řešení nový klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **konzolové aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt nazvěte **ReadDeviceToCloudMessages**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]

2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **ReadDeviceToCloudMessages** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.

3. V okně **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte možnost **Instalovat** a přijměte podmínky používání. Tím se stáhne a nainstaluje služba [Azure Service Bus][lnk-servicebus-nuget] a všechny její závislosti a přidá se na ní odkaz. Tento balíček umožní aplikaci připojení ke koncovému bodu kompatibilnímu se službou Event Hubs ve službě IoT Hub.

4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, které jste vytvořili v části „Vytvoření služby IoT Hub“.

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

    Tato metoda používá instanci **EventHubReceiver** k příjmu zpráv ze všech oddílů pro příjem zpráv typu zařízení-cloud ve službě IoT Hub: Všimněte si, jak při vytváření objektu **EventHubReceiver** předáte parametr `DateTime.Now`, aby objekt přijímal pouze zprávy odeslané po spuštění. To je užitečné v testovacím prostředí, protože uvidíte aktuální sadu zpráv, ale v produkčním prostředí by měl kód zpracovávat všechny zprávy. Další informace naleznete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial]..

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

## Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci systému Windows, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. V sadě Visual Studio přidejte k aktuálnímu řešení nový klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **konzolové aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Projekt pojmenujte **SimulatedDevice**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]

2. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **SimulatedDevice** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.

3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices.Client** a přijměte podmínky používání. Tím se stáhne a nainstaluje [balíček NuGet sady SDK pro zařízení Azure IoT][lnk-device-nuget] a jeho závislosti a přidá se na něj odkaz.

4. Do horní části souboru **Program.cs** přidejte následující příkaz `using`:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Do třídy **Program** přidejte následující pole. Hodnoty zástupných symbolů nahraďte názvem hostitele služby IoT Hub, který jste získali v části „Vytvoření služby IoT Hub“, a klíč k zařízený získaný v částí „Vytvoření identity zařízení“.

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

  Ve výchozím nastavení metoda **Create** vytvoří instanci **DeviceClient**, která se službou IoT Hub komunikuje pomocí protokolu AMQP. Pokud chcete používat protokol HTTPS, použijte přepis metody **Create**, který umožňuje určit protokol. Pokud používáte protokol HTTPS, měli byste také přidat balíček NuGet **Microsoft.AspNet.WebApi.Client**, aby projekt zahrnoval obor názvů **System.Net.Http.Formatting**.

Tento kurz vás provede postupem vytvoření klienta zařízení IoT Hub. K přidání nezbytného kódu do klientské aplikace zařízení můžete také použít rozšíření [Připojená služba pro službu Azure IoT Hub][lnk-connected-service] aplikace Visual Studio.


> [AZURE.NOTE] Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku  [Řešení přechodných chyb][lnk-transient-faults] programu MSDN.

## Spuštění aplikací

Nyní můžete spustit aplikace.

1.  V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**. Vyberte možnost **Více projektů po spuštění** a poté příkaz **Spustit** jako akci pro oba projekty **ReadDeviceToCloudMessages** a **SimulatedDevice**.

    ![Vlastnosti projektu po spuštění][41]

2.  Stisknutím klávesy **F5** spusťte obě aplikace. Výstup konzoly z aplikace **SimulatedDevice** zobrazuje zprávy, které simulované zařízení odesílá do služby IoT Hub. Výstup konzoly z aplikace **ReadDeviceToCloudMessages** zobrazuje zprávy, které služba IoT Hub přijímá.

    ![Výstup konzoly z aplikací][42]

3. Na dlaždici **Využití** v [portálu Azure][lnk-portal] se zobrazuje počet zpráv odeslaných do služby:

    ![Dlaždice Využití v portálu Azure][43]


## Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT Hub v portálu a poté jste vytvořili identitu zařízení v registru identit ve službě. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou. 

Pokud chcete pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tyto články:

- [Připojení zařízení][lnk-connect-device]
- [Začínáme se správou zařízení][lnk-device-management]
- [Začínáme se sadou Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit váš internet věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks ]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/


<!--HONumber=Aug16_HO4-->


