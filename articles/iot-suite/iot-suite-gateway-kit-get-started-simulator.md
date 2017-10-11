---
title: "Připojení brány Azure IoT Suite pomocí Intel NUC | Microsoft Docs"
description: "Použijte Kit komerční brány Microsoft IoT a předkonfigurované řešení vzdáleného monitorování. Použijte k připojení k řešení vzdáleného monitorování, odeslání simulovanou telemetrii do cloudu a reagovat na metody vyvolané z řídicího panelu řešení Azure IoT hraniční bránu."
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
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 9ed57d3c23e2adbd42c054f33c8ed46e3d6c9792
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Připojte bránu Azure IoT Edge pro předkonfigurované řešení vzdáleného monitorování a odeslat simulovanou telemetrii

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

V tomto kurzu se dozvíte, jak používat Azure IoT Edge k simulaci teploty a vlhkosti data k odeslání do předkonfigurovaného řešení vzdáleného monitorování. Tento kurz používá:

- Azure IoT okraj implementovat ukázka brány.
- Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu je provést následující kroky:

- Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte vaše zařízení brány Intel NUC ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.
- Nakonfigurujte IoT hraniční brána k odesílání simulovanou telemetrii, která můžete zobrazit na řídicí panel řešení.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure. Nasazení odráží architektura skutečné enterprise. Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit. Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Opakujte předchozí kroky pro přidání druhé zařízení pomocí ID zařízení, jako třeba **device02**. Ukázka odešle data ze dvou simulované zařízení v bráně řešení vzdáleného monitorování.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Vytvořit vlastní modul IoT Edge

Nyní můžete vytvořit vlastní modul IoT okraj, který umožňuje brána k odesílání zpráv do řešení vzdáleného monitorování. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

Zdrojový kód pro vlastní moduly IoT Edge stáhněte z Githubu, použijte následující příkazy:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Vytvořit vlastní modul IoT Edge, pomocí následujících příkazů:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Skript sestavení umístí vlastní modul IoT Edge libsimulator.so ve složce sestavení.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Nakonfigurujte a spusťte IoT hraniční brána

Teď můžete konfigurovat IoT hraniční brána k odesílání simulovanou telemetrii do řídicího panelu vzdáleného monitorování. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

> [!TIP]
> V tomto kurzu použijete standardní `vi` textového editoru na Intel NUC. Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - vi Editor kurzu] [ lnk-vi-tutorial] Seznamte se s tohoto editoru. Alternativně můžete nainstalovat více uživatelsky přívětivý [nano](https://www.nano-editor.org/) editor pomocí příkazu `smart install nano -y`.

Otevřete konfigurační soubor ukázka v **vi** editor pomocí následujícího příkazu:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Nahraďte **deviceID** a **deviceKey** zástupné symboly pomocí ID a klíče pro dvě zařízení, které jste vytvořili dříve v řešení vzdáleného monitorování.

Uložte provedené změny.

Teď můžete spustit IoT hraniční bránu, použijte následující příkazy:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

Brána se spouští v Intel NUC a odešle simulovanou telemetrii do řešení vzdáleného monitorování:

![IoT hraniční brána generuje simulovanou telemetrii][img-simulated telemetry]

Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.

## <a name="view-the-telemetry"></a>Zobrazení telemetrie

IoT hraniční brána teď odesílá simulovanou telemetrii do řešení vzdáleného monitorování. Můžete zobrazit telemetrii na řídicí panel řešení.

- Přejděte na řídicí panel řešení.
- Vyberte jednu ze dvou zařízení, které jste nakonfigurovali v bráně v **zařízení do zobrazení** rozevíracího seznamu.
- Telemetrická data ze zařízení brány se zobrazí na řídicím panelu.

![Zobrazení telemetrie z brány simulované zařízení.][img-telemetry-display]

> [!WARNING]
> Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění. Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.

## <a name="next-steps"></a>Další kroky

Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started