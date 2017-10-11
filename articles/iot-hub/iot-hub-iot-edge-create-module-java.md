---
title: "Vytvořit modul Azure IoT Edge s Javou | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější balíčky Azure IoT Edge Maven."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: 0c430272225d79737baec2be15ed7c93991cdeac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Vytvořit modul Azure IoT Edge s Javou

V tomto kurzu hodnotí, jak jeden může vytvořit modul pro Azure IoT hrany v jazyce Java.

V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí nejnovější balíčky Azure IoT Edge Maven.

## <a name="prerequisites"></a>Požadavky

V této části bude nastavení prostředí pro vývoj modulu IoT okraj. Platí pro obě *64bitová verze Windows* a *64-bit Linux (8 Ubuntu/Debian)* operační systémy.

Je vyžadován následující software:

* [Git klienta](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Otevřete okno příkazového řádku terminálu a klonovat úložiště v následující:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Přehled architektury

Platformy Azure IoT Edge výraznou přijme [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Které znamená, že se celý Azure IoT Edge architektura systému, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup. V tomto kurzu jsme vás seznámí následující dva moduly:

1. Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.
2. Modul, který vytiskne přijatého [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.

Následující obrázek zobrazuje typické toku začátku do konce dat pro tento projekt.

![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")

## <a name="understanding-the-code"></a>Pochopení kódu

### <a name="maven-project-structure"></a>Struktura projekt maven

Vzhledem k tomu, že Azure IoT Edge balíčky jsou založená na Maven, je potřeba vytvořit typické struktura projekt Maven, který obsahuje `pom.xml` souboru.

POM dědí z `com.microsoft.azure.gateway.gateway-module-base` balíček, který deklaruje všechny závislé součásti, které modul projekt, která zahrnuje binární soubory modulu runtime, cesta k souboru konfigurace brány a chování při spuštění. To nám uloží velké množství času a eliminovat potřebu zápisu a opakovaně přepisování stovky řádků kódu.

Je potřeba aktualizovat soubor pom.xml deklarace požadované závislosti nebo moduly plug-in a název konfiguračního souboru pro použití naše modulem, jak je znázorněno v následující fragment kódu.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set the filename of the Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Základní znalosti o modul Azure IoT Edge

Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.

Vstup může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).

Výstup bude vypadat na vstup, se může spustit chování hardware (např. blikající LED), zprávu, která z ostatních modulů, nebo cokoliv jiného (např. tisk do konzoly).

Moduly navzájem komunikují pomocí `com.microsoft.azure.gateway.messaging.Message` třídy. **Obsahu** z `Message` je bajtové pole, která umožňuje představující jakýkoli druh dat se vám líbí. **Vlastnosti** jsou také k dispozici v `Message` a jsou jednoduše mapování řetězec řetězec. Může si můžete představit **vlastnosti** jako hlavičky v požadavku HTTP nebo metadata souboru.

Chcete-li vytvořit modul Azure IoT Edge v jazyce Java, je potřeba vytvořit novou třídu modul, který dědí z `com.microsoft.azure.gateway.core.GatewayModule` a implementovat požadované abstraktní metody `receive()` a `destroy()`. V tomto okamžiku můžete také implementovat volitelné `start()` nebo `create()` také metody. Následující fragment kódu ukazuje, jak začít pracovat, vytváření modul Azure IoT okraj.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Převaděč modulu

| Vstup                    | Procesor                              | Výstup                 | Zdrojový soubor            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Teplotní datových zpráv | Analýza a vytvořit novou zprávu JSON | Struktura JSON zpráv | `ConverterModule.java` |

Tento modul je typické modul Azure IoT okraj. Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje teploty zprávu ve zprávě strukturovaných JSON (včetně připojování ID zprávy, nastavení vlastnosti jestli musíme teploty výstrahu aktivovat a tak dále).

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Modul tiskárny

| Vstup                          | Procesor | Výstup                     | Zdrojový soubor          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Jakékoli zprávy z ostatních modulů | Není k dispozici       | Přihlaste se do konzoly zprávy | `PrinterModule.java` |

Toto je jednoduchá, není potřeba vysvětlovat, modul, který výstupy přijatých zpráv do okna terminálu.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Konfigurace Azure IoT Edge

V posledním kroku před spuštěním moduly je ke konfiguraci Azure IoT okraj a k navázání připojení mezi moduly.

Nejprve je potřeba deklarovat naše zavaděč Java (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech i později.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Jakmile jsme prohlásí, že je naše zavaděče, bude také musíme deklarovat také naše moduly. Podobně jako deklarace zavaděče, že lze také odkazovat podle jejich `name` atribut. Když modul deklarace, musíme určit zavaděč, měla by používat (který by měl být jeden jsme definovali před) a vstupní bod (musí být normalizovaný název třídy Naše modulu) pro každý modul. `simulated_device` Modul je nativní modul, který je zahrnutý v balíčku Azure IoT Edge core runtime. Vždy by měla obsahovat vaše `args` v kódu JSON souboru i v případě, že je `null`.

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
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
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
    "sink": "print"
  }
]
```

## <a name="running-the-modules"></a>Spuštění moduly

Použití `mvn package` k sestavení všechno, co do `target/` složky. `mvn clean package`je také vhodné pro čistá sestavení.

Použití `mvn exec:exec` ke spuštění na Azure IoT okraj a jste měli zjistit že se ke konzole s pevnou sazbou vytištěn teploty data a všechny vlastnosti.

Pokud chcete aplikaci ukončit, stiskněte klávesu `<Enter>` klíč.

> [!IMPORTANT]
> Není doporučeno používat kombinace kláves Ctrl + C k ukončení aplikace IoT hraniční brány. Jak to může způsobit neobvyklým způsobem ukončení procesu.

