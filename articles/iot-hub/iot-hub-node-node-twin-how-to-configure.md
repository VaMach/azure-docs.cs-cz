---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (uzel) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. K implementaci aplikace simulovaného zařízení a aplikace služby, která upraví konfiguraci zařízení pomocí dvojče zařízení používáte SDK služby Azure IoT pro Node.js."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: elioda
ms.openlocfilehash: 6ff6f1c331d5a77e7ac0a47af6806f5d90fb0fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a>Použijte požadované vlastnosti pro konfiguraci zařízení (uzel)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci tohoto kurzu budete mít dvě aplikace konzoly Node.js:

* **SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery.js**, aplikace back-end Node.js, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.

> [!NOTE]
> Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.
> 
> 

K dokončení tohoto kurzu budete potřebovat následující:

* Node.js verze 4.0.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**; a můžete přejít k [Vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.

1. Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**. V **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v **simulatedeviceconfiguration** složky, spusťte následující příkaz k instalaci **azure-iot-device**, a **azure-iot zařízení mqtt** balíček:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** v soubor **simulatedeviceconfiguration** složky.
4. Přidejte následující kód, který **SimulateDeviceConfiguration.js** souboru a nahraďte **{zařízení připojovací řetězec}** zástupného symbolu připojovacím řetězcem zařízení jste zkopírovali, když jste vytvořili **myDeviceId** identitu zařízení:
   
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
   
    **Klienta** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze zařízení. Předchozí kód, jakmile ji inicializuje **klienta** objektu, načte dvojče zařízení pro **myDeviceId**a připojí obslužnou rutinu pro aktualizaci na požadované vlastnosti. Obslužná rutina ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte configIds, pak vyvolá metodu, která spustí tuto změnu konfigurace.
   
    Všimněte si, že z důvodu zjednodušení předchozí kód používá výchozí pevně pro počáteční konfiguraci. Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.
   
   > [!IMPORTANT]
   > Události změny požadované vlastnosti jsou vždy jednou vygenerované při připojení zařízení, nezapomeňte si zkontrolujte, zda je skutečný změnit ve vlastnostech požadované před provedením jakékoli akce.
   > 
   > 
5. Přidejte následující metody před `client.open()` volání:
   
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
   
    **InitConfigChange** metoda aktualizuje hlášené vlastnosti objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení na Služba. Po úspěšné aktualizaci dvojče zařízení, simuluje dlouhotrvající proces, který ukončí za běhu **completeConfigChange**. Tato metoda aktualizace dvojče místní zařízení hlášené vlastnosti nastavení stavu na **úspěch** a odebírání **pendingConfig** objektu. Pak aktualizuje dvojče zařízení ve službě.
   
    Hlášen uložit šířky pásma, která jsou aktualizovány vlastnosti tak, že zadáte pouze vlastnosti upravovat (s názvem **oprava** ve výše uvedeném kódu), místo nahrazení celý dokument.
   
   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, ostatní může mít do fronty je a ostatní může odmítnout s chybový stav. Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.
   > 
   > 
6. Spusťte aplikaci zařízení:
   
        node SimulateDeviceConfiguration.js
   
    Měli byste vidět zprávu `retrieved device twin`. Nechte aplikaci spuštěnou.

## <a name="create-the-service-app"></a>Vytvořit aplikaci aplikační služby
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie. Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.

1. Vytvořit novou prázdnou složku s názvem **setdesiredandqueryapp**. V **setdesiredandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v **setdesiredandqueryapp** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Pomocí textového editoru, vytvořte novou **SetDesiredAndQuery.js** v soubor **setdesiredandqueryapp** složky.
4. Přidejte následující kód, který **SetDesiredAndQuery.js** souboru a nahraďte **{iot hub, připojovací řetězec}** zástupný symbol jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec:
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    **Registru** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby. Předchozí kód, jakmile ji inicializuje **registru** objektu, načte dvojče zařízení pro **myDeviceId**a aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie. Poté zavolá **queryTwins** funkce událostí 10 sekund.

    > [!IMPORTANT]
    > Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund. Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny. Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].
    > 
    >.

1. Přidejte následující kód těsně před `registry.getDeviceTwin()` volání implementovat **queryTwins** funkce:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    Předchozí kód dotazy dvojčata zařízení uložené ve službě IoT hub a vytiskne konfigurace požadované a oznámená telemetrie. Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.
2. S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci klávesou:
   
        node SetDesiredAndQuery.js 5m
   
    Byste měli vidět změnu z hlášené konfigurace **úspěch** k **čekající** k **úspěch** znova s novou aktivní poslat frekvenci pět minut místo 24 hodin.
   
   > [!IMPORTANT]
   > Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu. To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah. Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **registru** třídy.
   > 
   > 

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z back-end aplikace a napsali aplikace simulovaného zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav jako  *hlášené vlastnosti* k dvojče zařízení.

Použijte v následujících zdrojích informací další postup:

* odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

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
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
