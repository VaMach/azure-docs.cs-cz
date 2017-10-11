---
title: "Použití dynamické telemetrie | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat dynamické telemetrie s Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 0114f27f9b8ae76e1170d04ddf66e2c4bf20686a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Použití dynamické telemetrie s předkonfigurovaného řešení vzdáleného monitorování

Dynamické telemetrie umožňuje vizualizovat všechny telemetrická data odesílaná do předkonfigurovaného řešení vzdáleného monitorování. Simulovaná zařízení, které nasadit s předkonfigurovaným řešením odesílat telemetrii teploty a vlhkosti, který můžete vizualizovat na řídicím panelu. Je-li přizpůsobit existující Simulovaná zařízení, vytvořte nové simulované zařízení nebo připojení fyzického zařízení s předkonfigurovaným řešením můžete odeslat další hodnoty telemetrie například externí teplotu, ot. / min nebo větru. Potom můžete vizualizovat tuto další telemetrii na řídicím panelu.

Tento kurz používá jednoduchý Node.js simulovaného zařízení, která můžete snadno upravit a experimentovat s dynamické telemetrie.

K dokončení tohoto kurzu, budete potřebovat:

* Aktivní předplatné Azure. Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
* [Node.js] [ lnk-node] verze 0.12.x nebo novější.

Tento kurz v jakémkoliv operačním systému, jako je například systému Windows nebo Linux, kde můžete nainstalovat Node.js.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Přidání typu telemetrie

Dalším krokem je nahradit telemetrii vygenerovanou simulované zařízení Node.js s novou sadu hodnot:

1. Zastavit simulované zařízení Node.js zadáním **Ctrl + C** v příkazovém řádku nebo prostředí.
2. V souboru remote_monitoring.js uvidíte základní datových hodnot pro existující teploty, vlhkosti a externí teplotní telemetrie. Přidejte hodnotu základní data pro **rpm** následujícím způsobem:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js simulované zařízení používá **generateRandomIncrement** funkce v souboru remote_monitoring.js přidejte náhodný přírůstek hodnoty základní data. Náhodně přeskupit **rpm** hodnota přidáním řádek kódu po existující randomizations následujícím způsobem:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Přidejte novou hodnotu rpm do datové části JSON, které zařízení odesílá do služby IoT Hub:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Spusťte simulované zařízení Node.js pomocí následujícího příkazu:

    `node remote_monitoring.js`

6. Sledujte nový typ telemetrie ot. / min, který se zobrazí v grafu v řídicím panelu:

![Přidat RPM na řídicí panel][image3]

> [!NOTE]
> Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.

## <a name="customize-the-dashboard-display"></a>Přizpůsobení zobrazení řídicího panelu

**Informace o zařízení** zprávy může obsahovat metadata o telemetrie zařízení může odesílat do služby IoT Hub. Tato metadata můžete určit typy telemetrických dat, které zařízení odesílá. Změnit **deviceMetaData** hodnota v souboru remote_monitoring.js **Telemetrie** následující definice **příkazy** definice. Následující fragment kódu ukazuje kód **příkazy** definice (Nezapomeňte přidat `,` po **příkazy** definice):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> Pro porovnání definici metadat s daty v datovém proudu telemetrických dat používá řešení vzdáleného monitorování porovnávání.


Přidání **Telemetrie** definice, jak je uvedeno v předchozím fragmentu kódu nezmění chování řídicího panelu. Však může také obsahovat metadata **DisplayName** atribut pro přizpůsobení zobrazení v řídicím panelu. Aktualizace **Telemetrie** definice metadat, jak je znázorněno v následujícím fragmentu kódu:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Následující snímek obrazovky ukazuje, jak tuto změnu upraví legendu grafu na řídicím panelu:

![Přizpůsobení legendu grafu][image4]

> [!NOTE]
> Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.

## <a name="filter-the-telemetry-types"></a>Filtroval typy telemetrie

Ve výchozím nastavení graf na řídicí panel zobrazuje každé datové řady v datový proud telemetrie. Můžete použít **informace o zařízení** metadata potlačit zobrazení telemetrie konkrétní typy v grafu. 

Chcete-li zobrazit pouze teploty a vlhkosti telemetrie grafu, vynechejte **ExternalTemperature** z **informace o zařízení** **Telemetrie** metadata následujícím způsobem:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Venku teploty** už nebude zobrazovat na graf:

![Filtrovat telemetrii na řídicím panelu][image5]

Tato změna ovlivňuje pouze zobrazení grafu. **ExternalTemperature** hodnoty data jsou stále uloženy a k dispozici pro zpracování všech back-end.

> [!NOTE]
> Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.

## <a name="handle-errors"></a>Zpracování chyb

Pro datový proud pro zobrazení v grafu jeho **typ** v **informace o zařízení** metadata musí odpovídat datovému typu hodnot telemetrie. Například, pokud metadata Určuje, že **typ** vlhkosti dat je **int** a **dvojité** nenajde v datovém proudu telemetrických dat, pak telemetrie vlhkosti nezobrazí. v grafu. Ale **vlhkosti** hodnoty jsou stále uloženy a k dispozici pro zpracování všech back-end.

## <a name="next-steps"></a>Další kroky

Teď, když už víte, jak používat dynamické telemetrie, další informace o tom, jak předkonfigurovaná řešení využívají informace o zařízení: [informace metadat zařízení ve vzdálené monitorování předkonfigurované řešení] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
