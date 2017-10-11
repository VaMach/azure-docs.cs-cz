---
title: "Vytvořte modul Azure IoT Edge s Node.js | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější Azure IoT Edge NPM balíčky a Yeoman generátor."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Vytvořte modul Azure IoT Edge s Node.js

V tomto kurzu umožňující prezentovat vytvoření modulu pro Azure IoT hrany v JS.

V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí nejnovější balíčky Azure IoT Edge NPM.

## <a name="prerequisites"></a>Požadavky

V této části nastavíte prostředí pro vývoj modulu IoT okraj. Platí pro obě *64bitová verze Windows* a *Linux 64-bit (Ubuntu 14 +)* operační systémy.

Je vyžadován následující software:
* [Git klienta](https://git-scm.com/downloads).
* [Uzel LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architektura

Platformy Azure IoT Edge výraznou přijme [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Což znamená, že celou architekturu Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup. V tomto kurzu zavedeme následující dva moduly:

1. Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.
2. Modul, který vytiskne přijatého [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.

Následující obrázek zobrazuje typické toku koncová dat pro tento projekt.

![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")

## <a name="set-up-the-environment"></a>Nastavení prostředí
Níže budeme ukazují, jak rychle nastavit prostředí pro začít psát první modul zakázat převaděč JS.

### <a name="create-module-project"></a>Vytvoření projektu modulu
1. Otevřete okno příkazového řádku, spusťte `yo az-iot-gw-module`.
2. Podle pokynů na obrazovce na dokončení inicializace modulu projektu.

### <a name="project-structure"></a>Struktura projektu
Modul projektu JS se skládá z následujících součástí:

`modules`-Přizpůsobené JS modulu zdrojové soubory. Nahraďte výchozí `sensor.js` a `printer.js` s vlastní soubory modulu.

`app.js`-Soubor položku Spustit instance okraj.

`gw.config.json`Chcete-li přizpůsobit moduly, které mají být načteny Edge – konfigurační soubor.

`package.json`-Informace metadat pro modul projekt.

`README.md`-Základní dokumentace pro modul projekt.


### <a name="package-file"></a>Soubor balíčku

To `package.json` deklaruje všechny informace potřebné pro modul projekt, který obsahuje název, verzi, položky, skripty, modulu runtime a vývoj závislosti metadat.

Následující fragment kódu ukazuje, jak nakonfigurovat pro zakázat převaděč ukázkový projekt.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Vstupní soubor
`app.js` Definuje způsob k inicializaci instance okraj. Zde jsme není nutné provést změnu.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Rozhraní modulu
Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.

Vstup může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).

Výstup bude vypadat na vstup, se může spustit chování hardware (např. blikající LED), zprávu, která z ostatních modulů, nebo cokoliv jiného (např. tisk do konzoly).

Moduly navzájem komunikují pomocí `message` objektu. **Obsah** z `message` je bajtové pole, které umožňuje reprezentovat jakýkoli druh dat se vám líbí. **Vlastnosti** jsou také k dispozici v `message` a jsou jednoduše mapování řetězec řetězec. Může si můžete představit **vlastnosti** jako hlavičky v požadavku HTTP nebo metadata souboru.

Chcete-li vytvořit modul Azure IoT Edge v JS, budete muset vytvořit nový objekt modul, který implementuje požadované metody `receive()`. V tomto okamžiku můžete také implementovat volitelné `create()` nebo `start()`, nebo `destroy()` také metody. Následující fragment kódu ukazuje generování uživatelského rozhraní modulu objektu JS.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Převaděč modulu
| Vstup                    | Procesor                              | Výstup                 | Zdrojový soubor            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Teplotní datových zpráv | Analýza a vytvořit novou zprávu JSON | Struktura JSON zpráv | `converter.js` |

Tento modul je typické modul Azure IoT okraj. Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje teploty zprávu ve zprávě strukturovaných JSON (včetně připojování ID zprávy, nastavení vlastnosti jestli musíme teploty výstrahu aktivovat a tak dále).

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Modul tiskárny
| Vstup                          | Procesor | Výstup                     | Zdrojový soubor          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Jakékoli zprávy z ostatních modulů | Není k dispozici       | Přihlaste se do konzoly zprávy | `printer.js` |

Tento modul je jednoduché, není potřeba vysvětlovat, který výstupy přijatých zpráv (vlastnost, obsah) do okna terminálu.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfigurace
V posledním kroku před spuštěním moduly je ke konfiguraci Azure IoT okraj a k navázání připojení mezi moduly.

Nejprve je potřeba deklarovat naše `node` zavaděče (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech i později.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Jakmile jsme prohlásí, že je naše zavaděče, potřebujeme také deklarovat také naše moduly. Podobně jako deklarace zavaděče, že lze také odkazovat podle jejich `name` atribut. Když modul deklarace, musíme určit zavaděč, měla by používat (který by měl být jeden jsme definovali před) a vstupní bod (musí být normalizovaný název třídy Naše modulu) pro každý modul. `simulated_device` Modulu je nativní modul, který je součástí balíčku Azure IoT Edge core runtime. Zahrnout `args` v kódu JSON souboru i v případě, že je `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Na konci konfigurace naváže připojení. Je každé připojení vyjádřená `source` a `sink`. Jejich by měl obě odkazovat modul předem definovaná. Výstup zprávu `source` modulu se předají na vstup `sink` modulu.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a>Spuštění moduly
1. `npm install`
2. `npm start`

Pokud chcete aplikaci ukončit, stiskněte klávesu `<Enter>` klíč.

> [!IMPORTANT]
> Není doporučeno používat kombinace kláves Ctrl + C k ukončení aplikace IoT okraj. Jako tímto způsobem může způsobit neobvyklým způsobem ukončení procesu.
