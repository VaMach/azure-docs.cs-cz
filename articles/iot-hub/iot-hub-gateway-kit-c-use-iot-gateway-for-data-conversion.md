---
title: "Převod dat na IoT brány s hranou Azure IoT | Microsoft Docs"
description: "Pomocí brány IoT převést formát data snímačů pomocí vlastní modul od Azure IoT okraje."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Převod dat IOT brány, transformaci dat brány iot"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT

> [!NOTE]
> Než začnete tento kurz, ujistěte se, že jste dokončili následující lekce v pořadí:
> * [Nastavení Intel NUC jako brány IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Jediný účel bránu Iot se má zpracovat shromážděná data před odesláním do cloudu. Azure IoT Edge zavádí moduly, které lze vytvořit a sestaví formuláři zpracování dat pracovního postupu. Modul přijme nějakou zprávu, provádí některé akce a potom posuňte pro jiné modulů pro zpracování.

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak vytvořit modul převést zprávy z SensorTag do jiného formátu.

## <a name="what-you-do"></a>Co dělat

* Modul pro převod do formátu .json přijatou zprávu vytvořte.
* Zkompilujte modul.
* Přidání modulu k ukázkové aplikaci zakázat od Azure IoT okraje.
* Spuštění ukázkové aplikace.

## <a name="what-you-need"></a>Co potřebujete

* Následující kurzy dokončit v pořadí:
  * [Nastavení Intel NUC jako brány IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Klient SSH, který běží v hostitelském počítači. V systému Windows se doporučuje puTTY. Linux a systému macOS již obsahují klientem SSH.
* IP adresa a uživatelské jméno a heslo pro přístup k bráně z klienta SSH.
* Připojení k Internetu.

## <a name="create-a-module"></a>Vytvoření modulu

1. Na hostitelském počítači spusťte použije klient SSH a připojení k bráně IoT.
1. Klonování zdrojové soubory modulu převod z webu GitHub na domovský adresář brány IoT tak, že spustíte následující příkazy:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Toto je nativní modul Azure Edge napsané v C programovací jazyk. Modul převede formát přijaté zprávy tu následující:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>Kompilace modulu

Kompilace modul, spusťte následující příkazy:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

Můžete získat `libmy_module.so` souboru po dokončení kompilaci. Poznamenejte si tento soubor absolutní cesta.

## <a name="add-the-module-to-the-ble-sample-application"></a>Přidání modulu k ukázkové aplikaci zakázat

1. Přejděte do složky Ukázky spuštěním následujícího příkazu:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Otevřete konfigurační soubor tak, že spustíte následující příkaz:

   ```bash
   vi ble_gateway.json
   ```

1. Přidat modul vložením následující kód, který `modules` části.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Nahraďte `[Your libmy_module.so path]` v kódu s absolutní cesta libmy_module.so' souboru.
1. Nahraďte kód v `links` oddíl s následujícím kódem:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Stiskněte klávesu `ESC`a pak zadejte `:wq` k uložení souboru.

## <a name="run-the-sample-application"></a>Spuštění ukázkové aplikace

1. Zapnutí SensorTag.
1. Nastavte proměnnou prostředí SSL_CERT_FILE spuštěním následujícího příkazu:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Spusťte ukázkové aplikace s přidaným modulem spuštěním následujícího příkazu:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Další kroky

Bránu IoT jste úspěšně použijte k převedení zprávy z SensorTag do formátu .json.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
