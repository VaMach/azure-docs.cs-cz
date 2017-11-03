---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (.NET/.NET) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. Použití zařízení Azure IoT sady SDK pro .NET k implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, která upraví konfiguraci zařízení dvojče zařízení pomocí služby Azure IoT."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 0bb18f1b289dd01866842a5193437dabdb5fd20b
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Použijte požadované vlastnosti pro konfiguraci zařízení
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci tohoto kurzu budete mít dvě aplikace konzoly .NET:

* **SimulateDeviceConfiguration**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery**, back-end aplikace, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.

> [!NOTE]
> Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.
> 
> 

K dokončení tohoto kurzu budete potřebovat následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**. V takovém případě můžete přeskočit na [vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci .NET, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.

1. V sadě Visual Studio vytvořte nový projekt Visual C# Windows klasický desktopový pomocí **konzolové aplikace** šablona projektu. Název projektu **SimulateDeviceConfiguration**.
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]

1. V Průzkumníku řešení klikněte pravým tlačítkem myši **SimulateDeviceConfiguration** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**. Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití. Tento postup stáhne, nainstaluje a přidá odkaz na [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem zařízení, kterou jste si poznamenali v předchozím oddílu.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Přidejte následující metodu do třídy **Program**:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    **Klienta** objekt poskytuje všechny metody vyžadovat interakci s dvojčata zařízení ze zařízení. Inicializuje výše uvedeném kódu **klienta** objektu a potom načte dvojče zařízení pro **myDeviceId**.

1. Přidejte následující metodu do **Program** třídy. Tato metoda nastaví počáteční hodnoty telemetrie na místním zařízení a pak aktualizuje dvojče zařízení.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Přidejte následující metodu do **Program** třídy. Toto je zpětného volání, která zjistí změnu *potřeby vlastnosti* v dvojče zařízení.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Tato metoda aktualizuje hlášené vlastnosti v objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení ve službě. Po úspěšné aktualizaci dvojče zařízení, dokončení změny konfigurace pomocí volání metody `CompleteConfigChange` popsané v další bod.

1. Přidejte následující metodu do **Program** třídy. Tato metoda simuluje resetování zařízení a potom aktualizace místní hlášené vlastnosti nastavení stavu na **úspěch** a odebere **pendingConfig** element. Pak aktualizuje dvojče zařízení ve službě. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key to exit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Nakonec přidejte následující řádky, které se **hlavní** metoda:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, některé může mít do fronty je a některé může odmítnout s chybový stav. Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.
   > 
   > 
1. Sestavte řešení a pak spusťte aplikaci zařízení ze sady Visual Studio kliknutím **F5**. V konzole pro výstup měli byste vidět zprávy indikující, že simulovaného zařízení načítá dvojče zařízení, nastavení telemetrie a čekání na změnu požadované vlastnosti. Nechte aplikaci spuštěnou.

## <a name="create-the-service-app"></a>Vytvořit aplikaci aplikační služby
V této části vytvoříte konzolovou aplikaci .NET, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie. Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Název projektu **SetDesiredConfigurationAndQuery**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. V Průzkumníku řešení klikněte pravým tlačítkem myši **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Přidejte následující metodu do třídy **Program**:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    **Registru** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby. Tento kód inicializuje **registru** objektu, načte dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.
    Poté se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub každých 10 sekund a vytiskne konfigurace požadované a oznámená telemetrie. Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.
   
   > [!IMPORTANT]
   > Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund. Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny. Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].
   > 
   > 
1. Nakonec do metody **Main** přidejte následující řádky:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**. Sestavte řešení.
1. S **SimulateDeviceConfiguration** zařízení aplikace spuštěná, spusťte aplikaci service pomocí sady Visual Studio **F5**. Měli byste vidět hlášené změnu z konfigurace **čekající** k **úspěch** s nové aktivní odeslat frekvenci pět minut místo 24 hodin.

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu. To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah. Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **registru** třídy.
   > 
   > 

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení back-end a napsali aplikace na zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav prostřednictvím hlášení Vlastnosti.

Použijte v následujících zdrojích informací další postup:

* odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
