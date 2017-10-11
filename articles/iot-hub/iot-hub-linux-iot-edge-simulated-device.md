---
title: "Simulovat zařízení s Azure IoT hranou (Linux) | Microsoft Docs"
description: "Jak používat Azure IoT Edge v systému Linux k vytvoření simulovaného zařízení, které odesílá telemetrická data prostřednictvím IoT vstupní brána do služby IoT hub."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a>Azure IoT Edge používat k odesílání zpráv typu zařízení cloud s simulované zařízení (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Spuštění ukázky

Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **iot-edge**. Tento výstup zahrnuje čtyři IoT Edge moduly používané v této ukázce.

Sestavení skriptu místa:

* **liblogger.so** v **sestavení nebo moduly nebo protokolovacího nástroje** složky.
* **libiothub.so** v **sestavení nebo moduly nebo iothub** složky.
* **lib\_identity\_map.so** v **sestavení nebo moduly nebo identitymap** složky.
* **libsimulated\_device.so** v **sestavení, moduly nebo simulated\_zařízení** složky.

Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím nastavení souboru JSON:

Simulovaném\_zařízení\_cloudu\_nahrát\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku. Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\ Simulované\_zařízení\_cloudu\_nahrát\_ukázka\_lin.json**. Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Modul cesty jsou relativní vzhledem k adresáři z kterém spouštíte simulovaném\_zařízení\_cloudu\_nahrát\_ukázka spustitelný soubor, ne adresář, kde se nachází spustitelný soubor. Vzorový konfigurační soubor JSON výchozí zápis do 'deviceCloudUploadGatewaylog.log' v aktuální pracovní adresář.

V textovém editoru otevřete soubor **ukázky/simulated\_zařízení\_cloudu\_nahrát\_ukázkové, src/simulated\_zařízení\_cloudu\_nahrát\_lin.json** ve vaší místní kopii **iot hranou** úložiště. Tento soubor konfiguruje moduly IoT hraniční brány ukázka:

* **IoTHub** modulu připojuje ke službě IoT hub. Je třeba nakonfigurovat odesílat data do služby IoT hub. Konkrétně nastavit **IoTHubName** hodnoty na název své služby IoT hub a nastavte **IoTHubSuffix** hodnotu **azure devices.net**. Nastavte **přenosu** hodnotu pro jeden z: **HTTP**, **AMQP**, nebo **MQTT**. V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení. Pokud nastavíte hodnotu na **AMQP**, nebo **MQTT**, bránu udržuje samostatného připojení TCP ke službě IoT Hub pro každé zařízení.
* **Mapování** modulu mapuje adresy MAC Simulovaná zařízení na vaše ID zařízení IoT Hub. Ujistěte se, že **deviceId** hodnoty odpovídají identifikátory ID dvě zařízení, které jste přidali do služby IoT hub a že **deviceKey** hodnoty obsahovat klíče ze dvou zařízení.
* **BLE1** a **BLE2** moduly jsou Simulovaná zařízení. Všimněte si, jak jejich adresy MAC shodovat s adresami v **mapování** modulu.
* **Protokolovač** modulu ukládá informace o vaší brány činnosti do souboru.
* **Modulu cesta** hodnoty ukazuje příklad předpokládá, že spuštěním ukázky z **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.
* **Odkazy** pole v dolní části souboru JSON připojí **BLE1** a **BLE2** moduly, které **mapování** modul a **mapování** modulu **IoTHub** modulu. Také zajistí, že všechny zprávy v protokolu **Protokolovač** modulu.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "01:01:01:01:01:01"
            }
          },
        {
            "name": "BLE2",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "02:02:02:02:02:02"
            }
          },
        {
            "name": "Logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Uložte změny, které jste do konfiguračního souboru.

Spustit ukázku:

1. Ve vašem prostředí, přejděte na **iot-edge nebo sestavení** složky.
2. Spusťte následující příkaz:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje ke sledování zprávy, které Centrum IoT získává z brány. Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Další kroky

Získání rozsáhlejšími znalostmi Azure IoT okraj a Experimentujte s příklady kódu, najdete následujících kurzech developer a prostředky:

* [Odesílání zpráv typu zařízení cloud z fyzického zařízení s Azure IoT Edge][lnk-physical-device]
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
