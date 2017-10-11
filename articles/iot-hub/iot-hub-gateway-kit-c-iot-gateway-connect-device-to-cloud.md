---
title: "Použít bránu IoT pro připojení zařízení k Azure IoT Hub | Microsoft Docs"
description: "Další informace o použití Intel NUC jako IoT bránu pro připojení TI SensorTag a odesílat data snímačů do služby Azure IoT Hub v cloudu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Brána IOT připojit zařízení do cloudu"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a>Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub

> [!NOTE]
> Než začnete tento kurz, ujistěte se, když jste dokončili [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). V [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), můžete nastavit zařízení Intel NUC jako bránu IoT.

## <a name="what-you-will-learn"></a>Co se dozvíte

Zjistíte, jak používat bránu IoT pro připojení ke službě Azure IoT Hub Texas Instruments SensorTag (CC2650STK). Bránu IoT odešle teploty a vlhkosti data shromážděná z SensorTag do služby Azure IoT Hub.

## <a name="what-you-will-do"></a>Co provedete

- Vytvoření služby IoT hub.
- Registrovat zařízení ve službě IoT hub pro SensorTag.
- Povolte připojení mezi bránou rozhraní IoT a SensorTag.
- Spuštění ukázkové aplikace zakázat k odesílání dat SensorTag do služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) dokončit, ve kterém můžete nastavit Intel NUC jako bránu IoT.
- * Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
- Klient SSH, který běží v hostitelském počítači. V systému Windows se doporučuje puTTY. Linux a systému macOS již obsahují klientem SSH.
- IP adresa a uživatelské jméno a heslo pro přístup k bráně z klienta SSH.
- Připojení k Internetu.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Zde je zaregistrovat toto nové zařízení pro vaše SensorTag

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a>Povolit připojení mezi bránou rozhraní IoT a SensorTag

V této části můžete provádět následující úlohy:

- Získáte adresu MAC SensorTag pro připojení Bluetooth.
- Navázání připojení Bluetooth IoT brány SensorTag.

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a>Získat adresu MAC SensorTag pro připojení Bluetooth

1. Na hostitelském počítači spusťte použije klient SSH a připojení k bráně IoT.
1. Odblokování Bluetooth spuštěním následujícího příkazu:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Spusťte službu Bluetooth na bráně IoT a zadejte prostředí Bluetooth nakonfigurovat Bluetooth spuštěním následujících příkazů:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Zapnutí řadičem Bluetooth spuštěním následujícího příkazu v prostředí Bluetooth:

   ```bash
   power on
   ```

   ![Zapnutí řadičem Bluetooth na bráně IoT s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Spustí vyhledávání blízkými zařízeními Bluetooth spuštěním následujícího příkazu:

   ```bash
   scan on
   ```

   ![Kontrola nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Stisknutím tlačítka párování na SensorTag. Zeleným VEDLA na blikání SensorTag.
1. V prostředí Bluetooth měli byste vidět, že se nachází SensorTag. Poznamenejte si adresu MAC SensorTag. V tomto příkladu je adresu MAC SensorTag `24:71:89:C0:7F:82`.
1. Vypněte vyhledávání tak, že spustíte následující příkaz:

   ```bash
   scan off
   ```

   ![Ukončí skenování nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a>Navázání připojení Bluetooth IoT brány SensorTag

1. Připojte k SensorTag spuštěním následujícího příkazu:

   ```bash
   connect <MAC address>
   ```

   ![Připojení k SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Odpojte od SensorTag a opusťte prostředí Bluetooth spuštěním následujících příkazů:

   ```bash
   disconnect
   exit
   ```

   ![Odpojení od SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Úspěšně jste povolili připojení mezi SensorTag a bránu IoT.

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a>Spuštění ukázkové aplikace zakázat k odesílání dat SensorTag do služby IoT hub

Ukázkovou aplikaci Bluetooth nízká energie (Povolit) zajišťuje Azure IoT okraj. Ukázkovou aplikaci shromažďuje data z zakázat připojení a posílat data do IoT hub. Spuštění ukázkové aplikace, budete muset:

1. Nakonfigurujte ukázkovou aplikaci.
1. Spouštět ukázkovou aplikaci na bráně IoT.

### <a name="configure-the-sample-application"></a>Nakonfigurujte ukázkovou aplikaci

1. Přejděte do složky ukázkové aplikace tak, že spustíte následující příkaz:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Otevřete konfigurační soubor tak, že spustíte následující příkaz:

   ```bash
   vi ble_gateway.json
   ```

1. V konfiguračním souboru zadejte následující hodnoty:

   **IoTHubName**: název služby IoT hub.

   **IoTHubSuffix**: získání IoTHubSuffix z primární klíč zařízení připojovací řetězec, který jste si poznamenali dolů. Ujistěte se, že dostanete primární klíč zařízení připojovací řetězec, není primární klíč připojovací řetězec centra IoT. Primární klíč zařízení připojovací řetězec je ve formátu `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Přenos**: výchozí hodnota je `amqp`. Tato hodnota se uvádí protokol během transpotation. Může to být `http`, `amqp`, nebo `mqtt`.

   **macAddress**: adresa MAC SensorTag, kterou jste si poznamenali dolů.

   **deviceID**: ID zařízení, které jste vytvořili ve službě IoT hub.

   **deviceKey**: primární klíč zařízení připojovací řetězec.

   ![Dokončení konfiguračním souboru zakázat ukázkové aplikace](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Stiskněte klávesu `ESC` a typ `:wq` k uložení souboru.

### <a name="run-the-sample-application"></a>Spuštění ukázkové aplikace

1. Ujistěte se, že je zapnutý SensorTag.
1. Spusťte následující příkaz:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Další kroky

[Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
