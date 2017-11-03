---
title: "Zařízení SensorTag & brány Azure IoT - lekci 1: nastavení Intel NUC | Microsoft Docs"
description: "Nastavte Intel NUC fungovat jako bránu IoT mezi senzor a Azure IoT Hub a shromažďovat informace o senzor odeslat do služby IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT brány, intel nuc nuc počítače, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Nastavit Intel NUC jako bránu IoT
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Co provedete

- Nastavte Intel NUC jako bránu IoT.
- Nainstalujte balíček Azure IoT Edge na Intel NUC.
- Spuštění ukázkové aplikace na "hello_world" NUC Intel ověření funkci brány.

  > Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Postup připojení Intel NUC s periferních zařízení.
- Postup instalace a aktualizace požadované balíčky na Intel NUC pomocí inteligentní Správce balíčků.
- Postup spuštění ukázkové aplikace "hello_world" ověření funkci brány.

## <a name="what-you-need"></a>Co potřebujete

- Intel NUC Kit DE3815TYKE s sadě Intel IoT brány (Linux větru oblasti * 7.0.0.13) předinstalována. [Kliknutím sem zakoupit Groove IoT komerční brány Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Kabel Ethernet.
- Klávesnice.
- HDMI nebo VGA kabel.
- Monitorování s k portu HDMI nebo VGA.
- Volitelné: [Texas Instruments Sensortag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Připojit Intel NUC s periferních zařízení

Následující obrázek je příkladem NUC Intel, který je připojen k různým periferních zařízení:

1. Připojení k klávesnici.
2. Připojení k monitorování s kabel VGA nebo HDMI kabel.
3. Připojení k drátové síti pomocí kabelu Ethernet.
4. Připojení k napájení s napájecí kabel.

![Intel NUC připojené k periferních zařízení](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Připojení k systému Intel NUC od hostitelského počítače pomocí protokolu Secure Shell (SSH)

Budete potřebovat klávesnici a monitorování, které získat IP adresu vašeho zařízení Intel NUC. Pokud už znáte IP adresu, můžete přeskočit ke kroku 3 v této části.

1. Zapněte Intel NUC stisknutím tlačítka napájení a znovu přihlásili.

   Výchozí uživatelské jméno a heslo jsou obě `root`.

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Získat IP adresu Intel NUC spuštěním `ifconfig` příkazu na zařízení Intel NUC.

   Tady je příklad výstupu příkazu.

   ![výstup ifconfig zobrazující Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   V tomto příkladu hodnota, která odpovídá `inet addr:` je IP adresa, které potřebujete, až se připojí k Intel NUC z hostitelského počítače.

3. Použijte jednu z následujících klientů SSH z hostitelského počítače pro připojení k Intel NUC.

    - [PuTTY](http://www.putty.org/) pro systém Windows.
    - Integrovaného klienta SSH na Ubuntu nebo systému macOS.

   Je efektivnější a produktivitu pracovat Intel NUC z hostitelského počítače. Budete potřebovat Intel NUC IP adresu, uživatelské jméno a heslo pro připojení k němu prostřednictvím klientem SSH. Tady je příklad, který používá klienta SSH v systému macOS.
   ![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Nainstalovat balíček Azure IoT Edge

Balíček Azure IoT Edge obsahuje předem kompilovaném binární soubory IoT okraj a jeho závislosti. Tyto binární soubory jsou Azure IoT Edge, sady SDK Azure IoT a odpovídající nástroje. Balíček obsahuje také "hello_world" ukázkovou aplikaci slouží k ověření funkci brány. IoT okraj je základní součástí brány. 

Postupujte podle těchto kroků k instalaci balíčku.

1. Spuštěním následujících příkazů v okně terminálu přidejte IoT cloudové úložiště:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Zadejte "y", pokud budete vyzváni k "Zahrnovat tento kanál?"
   
   Pokud se zobrazí `import read failed(-1)` chyby, použijte následující příkazy k vyřešení problému:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   `rpm` Příkaz importuje klíč ot. / min. `smart channel` Příkaz přidá rpm kanál pro inteligentní Správce balíčků. Před spuštěním `smart update` příkaz, zobrazí se výstup jako níže.

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Spusťte příkaz inteligentní aktualizace:

   ```bash
   smart update
   ```

3. Instalovat balíček brány Azure IoT tak, že spustíte následující příkaz:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`je název balíčku. `smart install` Příkaz slouží k instalaci balíčku.

    > Spusťte následující příkaz, pokud se zobrazí tato chyba: veřejný klíč není k dispozici

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Restartujte Intel NUC, pokud se zobrazí tato chyba: 'žádný balíček poskytuje util. linux vývojářů.

   Po instalaci balíčku je Intel NUC fungovat jako brány.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Spuštění ukázkové aplikace Azure IoT Edge "hello_world"

Následující ukázkové aplikace vytvoří brány z `hello_world.json` soubor a používá základní součásti architektury Azure IoT okraj do protokolu zprávu hello world soubor (log.txt) každých 5 sekund.

Hello World vzorek můžete spustit spuštěním následujících příkazů:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Umožní aplikaci Hello, World spustit několik minut a poté stiskněte klávesu Enter zastavte ji.
![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Můžete ignorovat všechny 'handle(NULL) neplatný argument' chyby, které se zobrazí po kliknutí zadejte.

Můžete ověřit, že brána proběhla úspěšně otevřením log.txt souboru, který je teď ve složce hello_world ![log.txt zobrazení adresáře](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Otevřete log.txt pomocí následujícího příkazu:

```bash
vim log.txt
```

Zobrazí se obsah log.txt, který bude výstup naformátovaný JSON protokolování zpráv, které byly napsané každých 5 sekund modulem brány Hello, World.
![zobrazení adresáře log.txt](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Dokončili jste nastavení Intel NUC jako brána. Teď jste připravení přejít Další lekce k nastavení hostitelského počítače, vytvořte Azure IoT Hub a zaregistrovat logické zařízení Azure IoT Hub.

## <a name="next-steps"></a>Další kroky
[Použít bránu IoT pro připojení zařízení k Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

