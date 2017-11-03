---
title: "Použití fyzického zařízení s Azure IoT hranou | Microsoft Docs"
description: "Jak používat k odesílání dat do služby IoT hub prostřednictvím brány IoT Edge spuštěné na zařízení malin pí 3 zařízením Texas Instruments SensorTag. Brána je sestaven pomocí Azure IoT okraj."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: andbuc
ms.openlocfilehash: b24828ee1a09ba8e5f657954e11936f124270173
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="forward-device-to-cloud-messages-to-iot-hub-using-azure-iot-edge-on-a-raspberry-pi"></a>Předávání zpráv typu zařízení cloud do služby IoT Hub pomocí Azure IoT Edge na malin platformy

Tento návod [Bluetooth nízkou energie ukázka] [ lnk-ble-samplecode] ukazuje, jak používat [Azure IoT Edge] [ lnk-sdk] na:

* Předat dál telemetrie zařízení cloud do služby IoT Hub z fyzického zařízení.
* Směrování příkazů ze služby IoT Hub fyzického zařízení.

Tento návod ilustruje:

* **Architektura**: důležité architektury informace o ukázkové nízkou energie Bluetooth.
* **Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.

## <a name="architecture"></a>Architektura

Průvodce ukazuje, jak sestavit a spustit bránu IoT Edge na malin pí 3, který spouští Raspbian Linux. Brána je sestaven pomocí IoT okraj. Příklad používá ke shromažďování dat teploty zařízením Texas Instruments SensorTag Bluetooth nízká energie (Povolit).

Při spuštění IoT hraniční brána je:

* Připojí se k zařízení SensorTag pomocí protokolu Bluetooth nízká energie (Povolit).
* Připojí se ke službě IoT Hub pomocí protokolu HTTPS.
* Předává telemetrie ze zařízení SensorTag pro IoT Hub.
* Směrování příkazů ze služby IoT Hub zařízení SensorTag.

Brána obsahuje následující moduly hraniční IoT:

* A *zakázat modulu* , rozhraní se zakázat zařízení přijímat teploty data ze zařízení a odesílání příkazů do zařízení.
* A *zakázat cloudu do zařízení modulu* který překládá JSON zprávy odeslané ze služby IoT Hub do zakázat pokyny *zakázat modulu*.
* A *protokoly modulu* který zaznamenává všechny brány zprávy do místního souboru.
* *Modulem mapování identit* který překládá mezi adresy MAC povolit zařízení a identit zařízení Azure IoT Hub.
* *IoT Hub modulu* které odesílá telemetrická data do služby IoT hub a přijímá příkazy zařízení ze služby IoT hub.
* A *zakázat tiskárny modulu* , interpretuje telemetrická data ze zařízení zakázat a výtisků formátovaných dat do konzoly nástroje umožňující řešení potíží a ladění.

### <a name="how-data-flows-through-the-gateway"></a>Tok dat prostřednictvím brány

Následující blok diagram znázorňuje kanálu toku dat odesílání telemetrie:

![Kanál brány odesílání telemetrie](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

Kroky, které položky telemetrie trvá cestě z zakázat zařízení do služby IoT Hub jsou:

1. Zařízení zakázat generuje ukázku teploty a odesílá přes Bluetooth zakázat modulu v bráně.
1. Zakázat modul přijímá vzorku a publikuje do zprostředkovatele spolu s adresa MAC zařízení.
1. Modul mapování identit převezme tuto zprávu a používá interní tabulku přeložit adresa MAC zařízení do služby IoT Hub identity zařízení. Identita zařízení IoT Hub se skládá z ID zařízení a klíč zařízení.
1. Modul mapování identit publikuje novou zprávu, která obsahuje následující informace:
   - Teplotní ukázková data
   - Adresa MAC zařízení
   - ID zařízení
   - Klíč zařízení  
1. Modul služby IoT Hub přijímá tuto novou zprávu (generované modulem mapování identity) a publikuje do služby IoT Hub.
1. Modul protokolovacího nástroje zaznamená všechny zprávy z zprostředkovatele do místního souboru.

Následující blok diagram znázorňuje kanálu toku dat příkaz zařízení:

![Zařízení příkaz brány kanálu](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. Modul služby IoT Hub pravidelně se dotazuje službu IoT hub pro nové zprávy příkaz.
1. Když v modulu služby IoT Hub přijme zprávu o nový příkaz, vydává je pro zprostředkovatele.
1. Modul mapování identit převezme zprávou příkazu a používá interní tabulku přeložit ID zařízení IoT Hub, který má adresa MAC zařízení. Pak publikuje novou zprávu, která obsahuje adresu MAC cílového zařízení v mapě vlastnosti zprávy.
1. Modul zakázat Cloud-zařízení převezme tuto zprávu a překládá do správné zakázat instrukce pro modul zakázat. Pak publikuje novou zprávu.
1. Modul zakázat převezme tuto zprávu a provede instrukci vstupně-výstupních operací podle komunikaci se zařízením zakázat.
1. Modul protokolovacího nástroje zaznamená všechny zprávy z zprostředkovatele do souboru na disku.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.

- Windows nezahrnuje klientem SSH. Doporučujeme používat [PuTTY](http://www.putty.org/).
- Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH. Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Připravte svůj hardware

Tento kurz předpokládá, že používáte [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) zařízení připojené k systémem Raspbian malin pí 3.

### <a name="install-raspbian"></a>Nainstalujte Raspbian

K instalaci Raspbian na vašem zařízení malin pí 3 můžete použít některý z následujících možností.

* Chcete-li nainstalovat nejnovější verzi Raspbian, použijte [NOOBS] [ lnk-noobs] grafické uživatelské rozhraní.
* Ručně [Stáhnout] [ lnk-raspbian] a zapsat nejnovější bitovou kopii operačního systému Raspbian do SD karty.

### <a name="sign-in-and-access-the-terminal"></a>Přihlaste se a přístup k terminálu

Máte dvě možnosti pro přístup k Terminálové prostředí na vaše malin platformy:

* Pokud máte klávesnici a monitorování, které jsou připojené k vaší malin platformy, můžete použít Raspbian grafického uživatelského rozhraní pro přístup k okno terminálu.

* Přístup na příkazovém řádku vaší malin pí pomocí protokolu SSH ze stolního počítače.

#### <a name="use-a-terminal-window-in-the-gui"></a>Použijte okno terminálu v grafickém uživatelském rozhraní

Výchozí pověření pro Raspbian jsou uživatelské jméno **pí** a heslo **malin**. Na hlavním panelu v grafickém uživatelském rozhraní, můžete spustit **Terminálové** nástroj pomocí ikonu, která vypadá jako monitorování.

#### <a name="sign-in-with-ssh"></a>Přihlaste se pomocí protokolu SSH

SSH můžete použít pro příkazového řádku přístup k vaší malin pí. Článek [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje postup konfigurace SSH na vaše malin platformy a jak se připojit z [Windows] [ lnk-ssh-windows] nebo [ Linux & Mac OS][lnk-ssh-linux].

Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.

### <a name="install-bluez-537"></a>Nainstalujte BlueZ 5.37

Obraťte se na hardwaru Bluetooth prostřednictvím zásobníku BlueZ moduly zakázat. Je nutné 5.37 verzi BlueZ pro moduly fungovala správně. Tyto pokyny Ujistěte se, že je nainstalována správná verze BlueZ.

1. Zastavte aktuální démon bluetooth:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Nainstalujte BlueZ závislosti:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf libtool glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Zdrojový kód BlueZ stáhněte z bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Rozbalte zdrojový kód:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Změňte adresáře na nově vytvořená složka:

    ```sh
    cd bluez-5.37
    ```

1. Konfigurovat kód BlueZ má být sestaven:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Sestavení BlueZ:

    ```sh
    make
    ```

1. Až se to dokončí instalaci BlueZ stavbě:

    ```sh
    sudo make install
    ```

1. Změna konfigurace služby systemd pro bluetooth, odkazuje na novou démon bluetooth v souboru `/lib/systemd/system/bluetooth.service`. 'ExecStart' řádku nahraďte následujícím textem:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Povolit připojení k zařízení SensorTag ze zařízení malin pí 3

Před spuštěním ukázky, je třeba ověřit, zda vaše malin pí 3 může připojit k zařízení SensorTag.

1. Ujistěte se, `rfkill` je nainstalován nástroj:

    ```sh
    sudo apt-get install rfkill
    ```

1. Odblokování bluetooth na malin pí 3 a zkontrolujte, jestli číslo verze **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. Pokud chcete zadat prostředí interaktivní bluetooth, spusťte službu bluetooth a provést **bluetoothctl** příkaz:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Zadejte příkaz **zapnutí** mocninu až řadičem bluetooth. Příkaz vrátí výstup podobně jako v následujícím příkladu:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. V prostředí interaktivní bluetooth, zadejte příkaz **kontrolu** kontrolovala zařízeními bluetooth. Příkaz vrátí výstup podobně jako v následujícím příkladu:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Zařízení SensorTag zjistitelnost stisknutím tlačítka malé (zelený DIODU by měl flash). Pí 3 malin by měl zjistit zařízení SensorTag:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    V tomto příkladu vidíte, že je adresa MAC zařízení SensorTag **A0:E6:F8:B5:F6:00**.

1. Vypnout kontrolu tak, že zadáte **kontrolovat vypnout** příkaz:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Připojení k zařízení SensorTag pomocí adresu MAC zadáním **připojit \<adresa MAC\>**. Následující ukázkový výstup je zkratka pro přehlednost:

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Můžete vytvořit seznam vlastností GATT zařízení znovu s použitím **atributy seznamu** příkaz.

1. Teď můžete odpojit ze zařízení pomocí **odpojit** příkaz a poté ukončete pomocí prostředí bluetooth **ukončení** příkaz:

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nyní jste připraveni lit IoT Edge ukázku spustit na vaše malin pí 3.

## <a name="run-the-iot-edge-ble-sample"></a>Zakázat Edge IoT ukázku spustit

Pokud chcete zakázat Edge IoT ukázku spustit, je třeba provést tři úlohy:

* Nakonfigurujte dva ukázkové zařízení ve službě IoT Hub.
* Sestavení IoT Edge ve vašem zařízení malin pí 3.
* Nakonfigurujte a spusťte ukázkové lit na vašem zařízení malin pí 3.

V době psaní podporuje IoT Edge moduly zakázat pouze v brány se systémem Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurovat dva ukázkové zařízení ve službě IoT Hub

* [Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, je třeba název centra pro dokončení tohoto návodu. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* Přidat jednu zařízení s názvem **SensorTag_01** do služby IoT hub a zaznamenání jeho Id a zařízení klíče. Můžete použít [Průzkumníka zařízení nebo iothub-explorer] [ lnk-explorer-tools] nástroje pro přidání tohoto zařízení ke službě IoT hub, který jste vytvořili v předchozím kroku a k načítání svůj klíč. Toto zařízení do zařízení SensorTag mapy, při konfiguraci brány.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Vytvoření Azure IoT Edge ve vaší Malinová pí 3

Nainstalujte závislosti pro hraniční Azure IoT:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev libtool
```

Použijte následující příkazy ke klonování IoT okraj a všechny jeho submodules k domovskému adresáři:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Až budete mít úplnou kopii IoT Edge úložiště na vaše malin pí 3, můžete vytvořit pomocí následujícího příkazu ze složky, která obsahuje sadu SDK:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Nakonfigurujte a spusťte ukázkové lit na vaše malin pí 3

Chcete-li bootstrap a spuštění vzorového, konfigurace modulu každý IoT okraj, který je použit v bráně. Tato konfigurace je součástí souboru JSON a je nutné nakonfigurovat pět zúčastněných moduly IoT okraj. Není k dispozici ukázkový soubor JSON v úložišti názvem **brány\_sample.json** , můžete použít jako výchozí bod pro vytváření konfiguračního souboru. Tento soubor je v **ukázky/ble_gateway/src** složky v místní kopii úložiště IoT okraj.

Následující části popisují, jak upravit tento konfigurační soubor pro ukázku zakázat. Předpokládáme, že úložiště IoT okraj je v **/home/pi/iot-edge /** složky na vaše malin pí 3. Pokud úložiště je jinde, odpovídajícím způsobem upravte cesty.

#### <a name="logger-configuration"></a>Konfigurace protokolovacího nástroje

Za předpokladu, že úložiště brány se nachází v **/home/pi/iot-edge /** složku, nakonfigurujte modul protokolovacího nástroje takto:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>Konfigurace modulu zakázat

Ukázka konfigurace zařízení zakázat předpokládá zařízením Texas Instruments SensorTag. Jakékoli standardní zakázat zařízení, které může fungovat jako GATT periferní by měla fungovat, ale pravděpodobně vyžadovat aktualizaci GATT charakteristik ID a data. Přidáte adresu MAC svého zařízení SensorTag:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Pokud nepoužíváte zařízení SensorTag, přečtěte si dokumentaci k zařízení zakázat k určení, jestli je potřeba aktualizovat vlastnosti GATT ID a hodnot dat.

#### <a name="iot-hub-module"></a>Modul služby IoT Hub

Přidejte název služby IoT Hub. Hodnota přípony je obvykle **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfigurace modulu mapování identit

Přidat adresa MAC zařízení SensorTag a ID zařízení a klíč **SensorTag_01** zařízení, které jste přidali do služby IoT Hub:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfigurace modulu zakázat tiskárny

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>Konfigurace modulu BLEC2D

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Konfigurace směrování

Následující konfigurace zajišťuje následující směrování mezi moduly hraniční IoT:

* **Protokolovač** modul přijímá a zaznamenává všechny zprávy.
* **SensorTag** modul odesílá zprávy i **mapování** a **zakázat tiskárny** moduly.
* **Mapování** modul odesílá zprávy a pokuste se **IoTHub** modulu k odeslání služby IoT Hub.
* **IoTHub** modul odesílá zprávy zpět do **mapování** modulu.
* **Mapování** modul odesílá zprávy a pokuste se **BLEC2D** modulu.
* **BLEC2D** modul odesílá zprávy zpět do **Sensortag** modulu.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Ke spuštění ukázky, předat konfigurační soubor JSON cestu jako parametr, který se **lit\_brány** binární. Tento příkaz předpokládá, že používáte **gateway_sample.json** konfigurační soubor. Provedení tohoto příkazu z **iot hranou** složky na malin platformy:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Potřebujete stiskněte tlačítko malé na zařízení SensorTag zjistitelnost před spuštěním ukázky.

Když spustíte ukázku, můžete použít [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroje ke sledování IoT hraniční brána předává ze zařízení SensorTag zprávy. Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Odesílání zpráv z cloudu do zařízení

Zakázat modul rovněž podporuje odesílání příkazů ze služby IoT Hub zařízení. Můžete použít [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj k odeslání zprávy JSON, které modul zakázat brány předá k zařízení zakázat.

Pokud používáte zařízení Texas Instruments SensorTag, můžete zapnout červené DIODU, zelená DIODU nebo bzučák odesláním příkazů ze služby IoT Hub. Před odesláním příkazy ze služby IoT Hub, nejprve odeslat následující dvě zprávy JSON v pořadí. Potom můžete odeslat žádné příkazy zapnutí indikátory nebo bzučák.

1. Resetovat všechny LED a a odpověď (jejich vypnutím):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. Nakonfigurujte vstupně-výstupních operací jako 'vzdálené':

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Nyní můžete odeslat žádný z následujících příkazů pro zapnutí indikátory nebo bzučák na zařízení SensorTag:

* Zapněte red DIODU:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Zapněte zelená DIODU:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Zapněte a odpověď:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Další kroky

Pokud chcete získat rozsáhlejšími znalostmi IoT okraj a Experimentujte s příklady kódu, přejděte na následující kurzy developer a prostředky:

* [Azure IoT Edge][lnk-sdk]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
