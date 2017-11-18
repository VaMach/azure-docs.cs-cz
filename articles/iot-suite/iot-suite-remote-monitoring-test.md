---
title: "Simulace zařízení v řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat v simulátoru zařízení s předkonfigurovaného řešení vzdáleného monitorování."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 8b84b90e72f8cac1fc1f8a90391b7a5a4f6be1f4
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="test-your-solution-with-simulated-devices"></a>Testování řešení pomocí simulovaných zařízení

V tomto kurzu se dozvíte, jak používat přizpůsobit mikroslužbu simulátoru zařízení s předkonfigurovaného řešení vzdáleného monitorování. Chcete-li zobrazit možnosti v simulátoru zařízení, tento kurz používá dva scénáře v aplikaci Contoso IoT.

V prvního scénáře Contoso chce otestovat nového inteligentní žárovek zařízení. Pokud chcete provést testy, vytvořte nového simulovaného zařízení s následujícími charakteristikami:

*Vlastnosti*

| Name (Název)                     | Hodnoty                      |
| ------------------------ | --------------------------- |
| Barva                    | Prázdné, červená, modrá            |
| Také průraznost               | 0 až 100.                    |
| Odhadovaný zbývající dobu životnosti | Odpočítávání 10 000 hodiny |

*Telemetrie*

Následující tabulka obsahuje data že ligthbulb sestav v cloudu jako datový proud:

| Name (Název)   | Hodnoty      |
| ------ | ----------- |
| Status | "na" "off" |
| online | Hodnota TRUE, false |

> [!NOTE]
> **Online** hodnota telemetrie je povinná pro všechny simulované typy.

*Metody*

V následující tabulce jsou uvedeny akce, které podporuje nové zařízení:

| Name (Název)        |
| ----------- |
| Přepnout   |
| Vypnout  |

*Počáteční stav*

Následující tabulka uvádí počáteční stav zařízení:

| Name (Název)                     | Hodnoty |
| ------------------------ | -------|
| Počáteční barvu            | Bílá  |
| Počáteční také průraznost       | 75     |
| Počáteční zbývající dobu životnosti   | 10 000 |
| Stav počáteční telemetrie | "na"   |

Druhý scénář, přidáte nový typ telemetrie a Contoso existující **chladič** zařízení.

V tomto kurzu se dozvíte, jak používat v simulátoru zařízení s předkonfigurovaného řešení vzdáleného monitorování:

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Vytvořte nový typ zařízení
> * Simulovat chování vlastní zařízení
> * Přidejte nový typ zařízení na řídicí panel
> * Odeslat vlastní telemetrii z existující typ zařízení

## <a name="prerequisites"></a>Požadavky

Chcete-li v tomto kurzu, je třeba nasazené instanci řešení vzdáleného monitorování ve vašem předplatném Azure.

Pokud jste nenasadili řešení vzdáleného monitorování ještě by se měla Dokončit [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md) kurzu.

<!-- Dominic please this use as your reference https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models -->

## <a name="the-device-simulation-service"></a>Službu simulace zařízení

Služba simulace zařízení v předkonfigurovaném řešení umožňuje změnit typy předdefinované simulované zařízení a vytvářet nové typy simulované zařízení. Typy vlastní zařízení můžete použít k testování chování řešení vzdáleného monitorování, než připojíte fyzických zařízení do řešení.

## <a name="create-a-simulated-device-type"></a>Vytvoření simulovaného zařízení typu

Nejjednodušší způsob, jak vytvořit nový typ zařízení v simulaci mikroslužbu je zkopírovat a upravit existující typ. Následující kroky vám ukážou, jak zkopírovat integrované **chladič** zařízení pro vytvoření nového **žárovek** zařízení:

1. Použijte následující příkaz klonovat **zařízení simulace** úložiště GitHub do místního počítače:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Každý typ zařízení má soubor modelu JSON a související skripty v `Services/data/devicemodels` složky. Kopírování **chladič** soubory a vytvořte **žárovek** souborů, jak je znázorněno v následující tabulce:

    | Zdroj                      | Cíl                   |
    | --------------------------- | ----------------------------- |
    | chladič 01.json             | 01.json žárovek             |
    | skripty nebo chladič-01-state.js | skripty nebo žárovek-01-state.js |
    | skripty nebo restartování method.js    | skripty nebo SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Definovat vlastnosti nového typu zařízení

`lightbulb-01.json` Souboru definuje vlastnosti typu, jako je například telemetrii generuje a metody podporuje. Následující postup aktualizace `lightbulb-01.json` souboru definujte **žárovek** zařízení:

1. V `lightbulb-01.json` souboru, jak je znázorněno v následujícím fragmentu kódu k aktualizaci metadat zařízení:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. V `lightbulb-01.json` souboru, aktualizujte definici simulaci, jak je znázorněno v následujícím fragmentu kódu:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. V `lightbulb-01.json` souboru, aktualizovat vlastnosti typu zařízení, jak je znázorněno v následujícím fragmentu kódu:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. V `lightbulb-01.json` souboru, aktualizovat definice telemetrie typ zařízení, jak je znázorněno v následujícím fragmentu kódu:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "status": "text"
          }
        }
      }
    ],
    ```

1. V `lightbulb-01.json` souboru, jak je znázorněno v následujícím fragmentu kódu k aktualizaci metody typu zařízení:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      },
    }
    ```

1. Uložit `lightbulb-01.json` souboru.

### <a name="simulate-custom-device-behavior"></a>Simulovat chování vlastní zařízení

`scripts/lightbulb-01-state.js` Soubor definuje chování simulace **žárovek** typu. Následující postup aktualizace `scripts/lightbulb-01-state.js` souboru k definování chování **žárovek** zařízení:

1. Upravit definici stavu v `scripts/lightbulb-01-state.js` souboru, jak je znázorněno v následujícím fragmentu kódu:

    ```js
    // Default state
    var state = {
      online: true,
      status: "on"
    };
    ```

1. Nahraďte **lišit** funkce s následující **překlopit** funkce:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Upravit **hlavní** funkce pro implementaci chování, jak je znázorněno v následujícím fragmentu kódu:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Uložit `scripts/lightbulb-01-state.js` souboru.

`scripts/SwitchOn-method.js` Souboru implementuje **přepínač na** metoda v **žárovek** zařízení. Následující postup aktualizace `scripts/SwitchOn-method.js` souboru:

1. Upravit definici stavu v `scripts/SwitchOn-method.js` souboru, jak je znázorněno v následujícím fragmentu kódu:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Přepnout žárovek, upravit **hlavní** funkce následujícím způsobem:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Uložit `scripts/SwitchOn-method.js` souboru.

1. Zkopírujte `scripts/SwitchOn-method.js` souboru s názvem `scripts/SwitchOff-method.js`.

1. Chcete-li vypnout skutečnost, upravte **hlavní** fungovat v `scripts/SwitchOff-method.js` následujícím způsobem:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Uložit `scripts/SwitchOff-method.js` souboru.

### <a name="test-the-lightbulb-device-type"></a>Test typu zařízení žárovek

K testování **žárovek** typ zařízení, můžete nejdřív otestovat typu vašeho zařízení chová podle očekávání spuštěním místní kopii **zařízení simulace** služby. Když máte testovat a ladit místně nového typu zařízení, můžete znovu vytvořit kontejner a znovu nasaďte **zařízení simulace** službu Azure.

K testování a ladění změny místně, najdete v části [zařízení simulace přehled](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md).

Konfigurace projektu zkopírovat nové **žárovek** souborů zařízení do výstupního adresáře:

* Pokud používáte Visual Studio, ujistěte se, přidejte tři nové soubory žárovek jste vytvořili v předchozím oddílu, který má **služby** projekt v řešení. Potom pomocí **Průzkumníku řešení** označit je zkopírovány do výstupního adresáře.

* Pokud používáte Visual Studio Code, otevřete **Services.csproj** souboru a přidejte tři nové soubory žárovek jste vytvořili v předchozí části. Zobrazit existující položky souboru modelu zařízení v **Services.csproj** soubor jako příklady.

K testování nového zařízení v nasazené řešení, najdete v jednom z:

* [Nasazení kontejnerů z účtu vlastní docker-hub](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Aktualizace kontejner nasazené prostřednictvím ruční kopírování](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Na **zařízení** stránky, můžete zřídit instance nového typu:

![Zobrazit seznam dostupných simulace](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Můžete zobrazit telemetrii ze simulovaného zařízení:

![Zobrazení telemetrie žárovek](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Můžete volat **SwitchOn** a **SwitchOff** metody na vašem zařízení:

![Volání metody žárovek](media/iot-suite-remote-monitoring-test/devicesmethods.png)

K vytvoření bitové kopie Docker s novým typem zařízení pro nasazení do Azure, najdete v části [vytváření bitové kopie Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="add-a-new-telemetry-type"></a>Přidejte nový typ telemetrie

Tato část popisuje postup úpravy existujícího typu simulované zařízení pro podporu nového typu telemetrie.

### <a name="locate-the-chiller-device-type-files"></a>Vyhledejte chladič soubory typ zařízení

Následující kroky vám ukážou, jak najít soubory, které definují integrované **chladič** zařízení:

1. Pokud jste tak již neučinili, použijte následující příkaz klonovat **zařízení simulace** úložiště GitHub do místního počítače:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Každý typ zařízení má soubor modelu JSON a související skripty v `data/devicemodels` složky. Soubory, které definují simulovaném **chladič** typ zařízení:
    * `data/devicemodels/chiller-01.json`
    * `data/devicemodels/scripts/chiller-01-state.js`

### <a name="specify-the-new-telemetry-type"></a>Zadejte nový typ telemetrie

Následující kroky vám ukážou, jak přidat nové **interní teploty** typ, který má **chladič** typ zařízení:

1. Otevřete soubor `chiller-01.json`.

1. Aktualizace **SchemaVersion** hodnotu následujícím způsobem:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. V **InitialState** přidejte toto dvě definice:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. V **Telemetrie** pole, přidejte následující definice:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Uložit `chiller-01.json` souboru.

1. Otevřete soubor `scripts/chiller-01-state.js`.

1. Přidejte následující pole do **stavu** proměnné:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Přidejte následující řádek na **hlavní** funkce:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Uložit `scripts/chiller-01-state.js` souboru.

### <a name="test-the-chiller-device-type"></a>Typ zařízení chladič testu

K testování aktualizovaný **chladič** typ zařízení, můžete nejdřív otestovat typu vašeho zařízení chová podle očekávání spuštěním místní kopii **zařízení simulace** služby. Když máte testovat a ladit typu vašeho zařízení aktualizované místně, můžete znovu vytvořit kontejner a znovu nasaďte **zařízení simulace** službu Azure.

Při spuštění **zařízení simulace** služby místně, odešle telemetrie do vašeho řešení vzdáleného monitorování. Na **zařízení** stránky, můžete zřídit instance vaše aktualizované typu.

K testování a ladění změny místně, najdete v části [službou pomocí sady Visual Studio](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#running-the-service-with-visual-studio) nebo [sestavit a spustit z příkazového řádku](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#build-and-run-from-the-command-line).

K testování nového zařízení v nasazené řešení, najdete v jednom z:

* [Nasazení kontejnerů z účtu vlastní docker-hub](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Aktualizace kontejner nasazené prostřednictvím ruční kopírování](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Na **zařízení** stránky, můžete zřídit instance vaše aktualizované typu:

![Přidejte aktualizovanou chladič](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Můžete zobrazit nové **interní teploty** telemetrie ze simulovaného zařízení.

K vytvoření bitové kopie Docker s novým typem zařízení pro nasazení do Azure, najdete v části [vytváření bitové kopie Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="next-steps"></a>Další kroky

V tomto kurzu ukázal, jak na:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Vytvořte nový typ zařízení
> * Simulovat chování vlastní zařízení
> * Přidejte nový typ zařízení na řídicí panel
> * Odeslat vlastní telemetrii z existující typ zařízení

Nyní jste se naučili jak používat službu zařízení simulace, navrhované dalším krokem je další postup [připojení fyzického zařízení k řešení vzdáleného monitorování](iot-suite-connecting-devices-node.md).

Další informace pro vývojáře o řešení vzdáleného monitorování najdete v části:

* [Referenční příručka pro vývojáře](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Průvodce řešením potíží pro vývojáře](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->