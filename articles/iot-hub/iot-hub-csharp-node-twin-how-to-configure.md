---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. Použití zařízení Azure IoT SDK pro Node.js pro implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, která upraví konfiguraci zařízení dvojče zařízení pomocí služby Azure IoT."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 8f9626fc47e7d32cb104b960bea9b7de9efa6f3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Použijte požadované vlastnosti pro konfiguraci zařízení
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci tohoto kurzu budete mít dvě aplikace konzoly:

* **SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery**, back-end aplikace .NET, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.

> [!NOTE]
> Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.
> 
> 

K dokončení tohoto kurzu budete potřebovat následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 4.0.x nebo novější.
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**. V takovém případě můžete přeskočit na [vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.

1. Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**. V **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty.
   
    ```
    npm init
    ```
1. Na příkazovém řádku v **simulatedeviceconfiguration** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** v soubor **simulatedeviceconfiguration** složky.
1. Přidejte následující kód, který **SimulateDeviceConfiguration.js** souboru a nahraďte **{zařízení připojovací řetězec}** zástupného symbolu připojovacím řetězcem zařízení jste zkopírovali, když jste vytvořili **myDeviceId** identitu zařízení:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    **Klienta** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze zařízení. Tento kód inicializuje **klienta** objektu, načte dvojče zařízení pro **myDeviceId**a potom připojí obslužnou rutinu pro aktualizaci na *potřeby vlastnosti*. Obslužná rutina ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte configIds, pak vyvolá metodu, která spustí tuto změnu konfigurace.
   
    Všimněte si, že z důvodu zjednodušení tento kód používá výchozí pevně pro počáteční konfiguraci. Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.
   
   > [!IMPORTANT]
   > Události změny požadované vlastnosti jsou vždy vygenerované jednou na připojení zařízení. Ujistěte se, že zkontrolujte, zda je skutečný změnit ve vlastnostech požadované před provedením jakékoli akce.
   > 
   > 
1. Přidejte následující metody před `client.open()` volání:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    **InitConfigChange** metoda aktualizuje hlášené vlastnosti v objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení na Služba. Po úspěšné aktualizaci dvojče zařízení, simuluje dlouhotrvající proces, který ukončí za běhu **completeConfigChange**. Tato metoda aktualizace místní hlášené vlastnosti nastavení stavu na **úspěch** a odebírání **pendingConfig** objektu. Pak aktualizuje dvojče zařízení ve službě.
   
    Hlášen uložit šířky pásma, která jsou aktualizovány vlastnosti tak, že zadáte pouze vlastnosti upravovat (s názvem **oprava** ve výše uvedeném kódu), místo nahrazení celý dokument.
   
   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, některé může mít do fronty je a některé může odmítnout s chybový stav. Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.
   > 
   > 
1. Spusťte aplikaci zařízení:
   
        node SimulateDeviceConfiguration.js
   
    Měli byste vidět zprávu `retrieved device twin`. Nechte aplikaci spuštěnou.

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
   
            try
            {
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
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
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
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**. Sestavte řešení.
1. S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci .NET pomocí sady Visual Studio **F5** a měli byste vidět změnu z hlášené konfigurace **úspěch**k **čekající** k **úspěch** znova s novou aktivní poslat frekvenci pět minut místo 24 hodin.

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
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
