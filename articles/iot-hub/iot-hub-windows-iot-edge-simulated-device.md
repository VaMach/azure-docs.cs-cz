---
title: "Simulovat zařízení s Azure IoT hranou (Windows) | Microsoft Docs"
description: "Jak používat Azure IoT Edge v systému Windows k vytvoření simulovaného zařízení, které odesílá telemetrická data prostřednictvím Azure IoT vstupní brána do služby IoT hub."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.openlocfilehash: 0aa1836ee1445894022b95fefc2338ef53698240
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Azure IoT Edge používat k odesílání zpráv typu zařízení cloud s simulované zařízení (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Spustit ukázku

**Build.cmd** skript generuje jeho výstup v **sestavení** složky ve vaší místní kopii **iot hranou** úložiště. Tento výstup zahrnuje čtyři IoT Edge moduly používané v této ukázce.

Sestavení skript vytvoří následující soubory:

* **Logger.dll** v **sestavení\\moduly\\protokolovač\\ladění** složky.
* **iothub.dll** v **sestavení\\moduly\\iothub\\ladění** složky.
* **identity\_map.dll** v **sestavení\\moduly\\identitymap\\ladění** složky.
* **Simulované\_device.dll** v **sestavení\\moduly\\simulované\_zařízení\\ladění** složky.

Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno simulovaném\_zařízení\_cloudu\_nahrát\_win nastavení souboru JSON.

Simulovaném\_zařízení\_cloudu\_proces odesílání vzorku má cestu k konfigurační soubor JSON jako argument příkazového řádku. Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\ Simulované\_zařízení\_cloudu\_nahrát\_win.json**. Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Jsou modulu cesty relativní k adresáři kde simulované\_zařízení\_cloudu\_nahrát\_sample.exe nachází. Vzorový konfigurační soubor JSON výchozí zápis do 'deviceCloudUploadGatewaylog.log' v aktuální pracovní adresář.

V textovém editoru otevřete soubor **ukázky\\simulované\_zařízení\_cloudu\_nahrát\\src\\simulované\_zařízení\_cloudu\_nahrát\_win.json** ve vaší místní kopii **iot hranou** úložiště. Tento soubor konfiguruje moduly IoT hraniční brány ukázka:

* **IoTHub** modulu připojuje ke službě IoT hub. Je třeba nakonfigurovat odesílat data do služby IoT hub. Konkrétně nastavit **IoTHubName** hodnoty na název své služby IoT hub a nastavte **IoTHubSuffix** hodnotu **azure devices.net**. Nastavte **přenosu** hodnotu pro jeden z: **HTTP**, **AMQP**, nebo **MQTT**. V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení. Pokud nastavíte hodnotu na **AMQP**, nebo **MQTT**, bránu udržuje samostatného připojení TCP ke službě IoT Hub pro každé zařízení.
* **Mapování** modulu mapuje adresy MAC Simulovaná zařízení na vaše ID zařízení IoT Hub. Nastavte **deviceId** hodnoty ID dvě zařízení, které jste přidali do služby IoT hub. Nastavte **deviceKey** hodnoty pro klíče ze dvou zařízení.
* **BLE1** a **BLE2** moduly jsou Simulovaná zařízení. Všimněte si, jak adresy MAC modulu shodovat s adresami v **mapování** modulu.
* **Protokolovač** modulu ukládá informace o vaší brány činnosti do souboru.
* **Modulu cesta** hodnoty zobrazené v následujícím příkladu jsou relativní vzhledem k adresáři kde simulované\_zařízení\_cloudu\_nahrát\_sample.exe nachází.
* **Odkazy** pole v dolní části souboru JSON připojí **BLE1** a **BLE2** moduly, které **mapování** modul a **mapování** modulu **IoTHub** modulu. Také zajistí, že všechny zprávy v protokolu **Protokolovač** modulu.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Uložte změny, které jste do konfiguračního souboru.

Spustit ukázku:

1. Na příkazovém řádku přejděte do **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.
2. Spusťte následující příkaz:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje ke sledování zprávy, které Centrum IoT získává z brány. Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Další kroky

Získání rozsáhlejšími znalostmi IoT okraj a Experimentujte s příklady kódu, najdete následujících kurzech developer a prostředky:

* [Odesílání zpráv typu zařízení cloud z fyzického zařízení s hranou IoT][lnk-physical-device]
* [Azure IoT Edge][lnk-iot-edge]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Zabezpečení řešení IoT od základů nahoru][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
