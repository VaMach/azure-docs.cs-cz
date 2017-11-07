---
title: "Připojení brány Azure IoT Suite pomocí Intel NUC | Microsoft Docs"
description: "Použijte Kit komerční brány Microsoft IoT a předkonfigurované řešení vzdáleného monitorování. Použití brány Azure IoT Edge tak, aby zařízení SensorTag pro připojení k řešení vzdáleného monitorování, odeslání telemetrie do cloudu a reagovat na metody vyvolané z řídicího panelu řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 85b9ed0000c8c5ff2c7d6cc8fa4a051e0b27d6c2
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Připojte bránu Azure IoT Edge pro předkonfigurované řešení vzdáleného monitorování a odesílat telemetrická data z SensorTag

[!INCLUDE [iot-suite-v1-gateway-kit-selector](../../includes/iot-suite-v1-gateway-kit-selector.md)]

V tomto kurzu se dozvíte, jak používat Azure IoT Edge k odesílání teploty a vlhkosti data ze zařízení SensorTag pro předkonfigurované řešení vzdáleného monitorování. SensorTag připojí k bráně Intel NUC pomocí Bluetooth. Tento kurz používá:

- Azure IoT okraj implementovat ukázka brány.
- Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu je provést následující kroky:

- Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte vaše zařízení brány Intel NUC ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.
- Nastaví bránu Intel NUC přijímat telemetrická data ze zařízení SensorTag a odesílat je na řídicím panelu vzdáleného monitorování.

[!INCLUDE [iot-suite-v1-gateway-kit-prerequisites](../../includes/iot-suite-v1-gateway-kit-prerequisites.md)]

[Texas Instruments SensorTag lit][lnk-sensortag]. V tomto kurzu načte ze zařízení SensorTag telemetrická data přes Bluetooth.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure. Nasazení odráží architektura skutečné enterprise. Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit. Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-v1-gateway-kit-view-solution](../../includes/iot-suite-v1-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Konfigurace připojení k Bluetooth

Nakonfigurujte Bluetooth na NUC Intel tak, aby zařízení SensorTag pro připojení a odesílat telemetrická data.

### <a name="find-the-mac-address-of-the-sensortag"></a>Najít adresu MAC SensorTag

1. V prostředí na Intel NUC spusťte následující příkaz, který odblokovat službu Bluetooth:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Spusťte následující příkazy a spusťte službu Bluetooth na Intel NUC a zadejte prostředí Bluetooth:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Spusťte následující příkaz na mocninu na řadiči Bluetooth:

    ```bash
    power on
    ```

    Pokud je na řadič, zobrazí se zpráva **změna power v úspěšné**.

1. Spusťte následující příkaz pro hledání blízkými zařízeními Bluetooth:

    ```bash
    scan on
    ```

1. Stisknutím tlačítka napájení na SensorTag zjistitelnost. Zelená DIODU bliká.

1. Když se zobrazí zpráva v prostředí, že je řadič zjistí SensorTag, poznamenejte adresa MAC zařízení. Adresa MAC vypadá jako **A0:E6:F8:B5:F6:00**. Adresa MAC později v tomto kurzu musíte při konfiguraci brány.

1. Spusťte následující příkaz k vypnutí možnosti prohledávání Bluetooth:

    ```bash
    scan off
    ```

1. Spusťte následující příkaz pro ověření, zda se můžete připojit k zařízení SensorTag:

    ```bash
    connect <SensorTag MAC address>
    ```

    Pokud se úspěšně připojit, prostředí zobrazuje zpráva **úspěšné připojení** a zobrazí informace o zařízení SensorTag. Pokud se nemůžete připojit, zkontrolujte, že že sensortag je pořád zapnutý.

1. Teď můžete odpojit od SensorTag a opusťte prostředí Bluetooth spuštěním následujících příkazů:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-software](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Vytvořit vlastní modul IoT Edge

Nyní můžete vytvořit vlastní modul IoT okraj, který umožňuje brána k odesílání zpráv do řešení vzdáleného monitorování. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

Zdrojový kód pro vlastní moduly IoT Edge stáhněte z Githubu, použijte následující příkazy:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Vytvořit vlastní modul IoT Edge, pomocí následujících příkazů:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Skript sestavení umístí vlastní modul IoT Edge libsensor2remotemonitoring.so ve složce sestavení.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Nakonfigurujte a spusťte IoT hraniční brána

Teď můžete konfigurovat IoT hraniční brána k odesílání telemetrie ze zařízení SensorTag na řídicím panelu vzdáleného monitorování. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

> [!TIP]
> V tomto kurzu použijete standardní `vi` textového editoru na Intel NUC. Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - vi Editor kurzu] [ lnk-vi-tutorial] Seznamte se s tohoto editoru. Alternativně můžete nainstalovat více uživatelsky přívětivý [nano](https://www.nano-editor.org/) editor pomocí příkazu `smart install nano -y`.

Otevřete konfigurační soubor ukázka v **vi** editor pomocí následujícího příkazu:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

V konfiguraci modulu IoTHub vyhledejte následující řádky:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Nahraďte zástupný symbol hodnoty s informacemi IoT Hub vytvořili a uložili na začátku tohoto kurzu. Hodnota IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota IoTSuffix je obvykle **azure devices.net**.

V konfiguraci pro mapování modulu vyhledejte následující řádky:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Nahraďte **macAddress** zástupný symbol adresu MAC vaší SensorTag jste si poznamenali dříve. Nahraďte **deviceID** a **deviceKey** zástupné symboly pomocí ID a klíče pro dvě zařízení, které jste vytvořili dříve v řešení vzdáleného monitorování.

V konfiguraci modulu SensorTag vyhledejte následující řádky:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Nahraďte **zařízení\_mac\_adresu** zástupný symbol adresu MAC vaší SensorTag jste si poznamenali dříve.

Uložte provedené změny.

Teď můžete spustit bránu pomocí následujících příkazů:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

IoT hraniční brána se spouští v Intel NUC a odesílá telemetrická data z SensorTag do řešení vzdáleného monitorování:

![IoT hraniční brána odesílá telemetrii z SensorTag][img-telemetry]

Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.

## <a name="view-the-telemetry"></a>Zobrazení telemetrie

Brána teď odesílá telemetrická data ze zařízení SensorTag do řešení vzdáleného monitorování. Můžete zobrazit telemetrii na řídicí panel řešení. Můžete také odesílat příkazy na vaše zařízení SensorTag prostřednictvím brány na řídicím panelu řešení.

- Přejděte na řídicí panel řešení.
- Vyberte zařízení, které jste nakonfigurovali v brány, kterou představuje SensorTag v **zařízení do zobrazení** rozevíracího seznamu.
- Telemetrická data ze zařízení SensorTag zobrazí na řídicím panelu.

![Zobrazení telemetrie z zařízení SensorTag][img-telemetry-display]

> [!WARNING]
> Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění. Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.


## <a name="next-steps"></a>Další kroky

Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-telemetry]: ./media/iot-suite-v1-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-v1-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started