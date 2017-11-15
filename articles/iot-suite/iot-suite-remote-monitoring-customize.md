---
title: "Přizpůsobení řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o přístupu zdrojový kód pro předkonfigurované řešení vzdáleného monitorování."
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
ms.openlocfilehash: 886a4412ac348869563a03d697f4363cb3dea8f9
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="customize-the-remote-monitoring-preconfigured-solution"></a>Přizpůsobení předkonfigurovaného řešení vzdáleného monitorování

Tento článek poskytuje informace o tom, jak můžete přístup ke zdrojovému kódu a přizpůsobit vzdálené monitorování předkonfigurované řešení. Článek popisuje:

* Úložiště Githubu, které obsahují zdrojového kódu a prostředky pro mikroslužeb, který vytváří předkonfigurovaného řešení.
* Běžné scénáře přizpůsobení, jako je například přidávání nového typu zařízení.

## <a name="project-overview"></a>Přehled projektu

### <a name="implementations"></a>Implementace

Řešení vzdáleného monitorování má implementace rozhraní .NET a Java. Obě implementace poskytuje podobné funkce a spoléhá na stejnou základní služby Azure. Najdete zde nejvyšší úrovně úložiště GitHub:

* [Řešení pro rozhraní .NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Řešení Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

### <a name="microservices"></a>Mikroslužby

Pokud vás zajímají konkrétní funkci řešení, dostanete úložišť GitHub pro každé jednotlivé mikroslužby. Každý mikroslužbu implementuje na jinou část funkci řešení. Další informace o přehled architektury, najdete v části [vzdálené monitorování architektura předkonfigurovaného řešení](iot-suite-remote-monitoring-sample-walkthrough.md).

Tato tabulka shrnuje aktuální dostupnost jednotlivých mikroslužbu pro jednotlivé jazyky:

<!-- please add links for each of the repos in the table, you can find them here https://github.com/Azure/azure-iot-pcs-team/wiki/Repositories-->

| Mikroslužbu      | Popis | Java | .NET |
| ----------------- | ----------- | ---- | ---- |
| Webové uživatelské rozhraní            | Webovou aplikaci pro řešení vzdáleného monitorování. Implementuje uživatelského rozhraní pomocí React.js framework. | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) |
| Správce rozbočovače IoT   | Zpracovává komunikace se službou IoT Hub.        | [K dispozici](https://github.com/Azure/iothub-manager-java) | [K dispozici](https://github.com/Azure/iothub-manager-dotnet)   |
| Authentication    |  Spravuje integraci služby Azure Active Directory.  | Ještě není k dispozici | [K dispozici](https://github.com/Azure/pcs-auth-dotnet)   |
| Simulace zařízení | Spravuje fond Simulovaná zařízení. | Ještě není k dispozici | [K dispozici](https://github.com/Azure/device-simulation-dotnet)   |
| Telemetrická data         | Poskytuje přístup k zařízení telemetrie uživatelského rozhraní. | [K dispozici](https://github.com/Azure/device-telemetry-java) | [K dispozici](https://github.com/Azure/device-telemetry-dotnet)   |
| Agent telemetrie   | Analyzuje datový proud telemetrie, ukládá zprávy ze služby Azure IoT Hub a generuje výstrahy podle definovaná pravidla.  | [K dispozici](https://github.com/Azure/telemetry-agent-java) | [K dispozici](https://github.com/Azure/telemetry-agent-dotnet)   |
| Konfigurace uživatelského rozhraní         | Spravuje konfigurační data z uživatelského rozhraní. | [K dispozici](https://github.com/azure/pcs-ui-config-java) | [K dispozici](https://github.com/azure/pcs-ui-config-dotnet)   |
| Adaptér úložiště   |  Spravuje interakce s služby úložiště.   | [K dispozici](https://github.com/azure/pcs-storage-adapter-java) | [K dispozici](https://github.com/azure/pcs-storage-adapter-dotnet)   |
| Reverzní proxy server     | Zpřístupní privátní prostředky způsobem spravované přes koncový bod jedinečný. | Ještě není k dispozici | [K dispozici](https://github.com/Azure/reverse-proxy-dotnet)   |

Řešení Java aktuálně používá ověřování .NET, simulace a mikroslužeb reverzní proxy server. Tyto mikroslužeb budou nahrazeny verzi Javy, jakmile budou k dispozici.

## <a name="presentation-and-visualization"></a>Prezentační a vizualizaci

Následující části popisují možnosti přizpůsobení prezentace a vizualizací vrstvy v řešení vzdáleného monitorování:

### <a name="change-the-logo-in-the-ui"></a>Změnit logo v uživatelském rozhraní

Výchozí nasazení používá název společnosti Contoso a logo v uživatelském rozhraní. Chcete-li změnit tyto prvky uživatelského rozhraní, chcete-li zobrazit název společnosti a loga:

1. Klonování úložiště webového uživatelského rozhraní použijte následující příkaz:

    ```cmd/sh
    git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
    ```

1. Chcete-li změnit název společnosti, otevřete `src/common/lang.js` soubor v textovém editoru.

1. V souboru vyhledejte následující řádek:

    ```js
    CONTOSO: 'Contoso',
    ```

1. Nahraďte `Contoso` s názvem vaší společnosti. Například:

    ```js
    CONTOSO: 'YourCo',
    ```

1. Uložte soubor.

1. Pokud chcete aktualizovat logo, přidejte nový soubor SVG s informacemi Potřebnými k `assets/icons` složky. Existující logo je `assets/icons/Contoso.svg` souboru.

1. Otevřete `src/components/layout/leftNav/leftNav.js` soubor v textovém editoru.

1. V souboru vyhledejte následující řádek:

    ```js
    import ContosoIcon from '../../../assets/icons/Contoso.svg';
    ```

1. Nahraďte `Contoso.svg` s názvem souboru loga. Například:

    ```js
    import ContosoIcon from '../../../assets/icons/YourCo.svg';
    ```

1. V souboru vyhledejte následující řádek:

    ```js
    alt="ContosoIcon"
    ```

1. Nahraďte `ContosoIcon` s vaší `alt` text. Například:

    ```js
    alt="YourCoIcon"
    ```

1. Uložte soubor.

1. Chcete-li otestovat změny, můžete spustit aktualizaci `webui` na místním počítači. Další informace o sestavení a spuštění `webui` řešení místně, najdete v části [sestavení, spuštění a testování místně](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/README.md#build-run-and-test-locally) v `webui` soubor readme úložiště GitHub.

1. Nasazení změn najdete v tématu [referenční příručka vývojáře](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide).

<!--

### Add a new KPI to the Dashboard page

The following steps describe how to add a new KPI to display on the **Dashboard** page. The new KPI shows information about the number of alarms with specific status values as a pie chart:

1. Step 1

1. Step 2
-->

### <a name="customize-the-map"></a>Přizpůsobení mapy

Najdete v článku [přizpůsobit mapy](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) stránky na webu GitHub podrobnosti mapy součásti v řešení.

<!--
### Customize the telemetry chart

See the [Customize telemetry chart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of the telemetry chart components in the solution.

### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

### Duplicate an existing control

To duplicate an existing UI element such as a chart or alert, see the [Duplicate a control](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub.

-->

### <a name="other-customization-options"></a>Další možnosti přizpůsobení

Další změnit prezentace a vizualizací vrstvu v řešení vzdáleného monitorování, můžete upravit kód. Příslušné úložiště Githubu jsou:

* [UIConfig (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [UIConfig (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [WebUI vzdálené monitorování počítačů s Azure](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="device-connectivity-and-streaming"></a>Připojení zařízení a vysílání datového proudu

Následující části popisují možnosti přizpůsobení připojení zařízení a streamování vrstvy v řešení vzdáleného monitorování. [Modely zařízení](https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models) popisují typy zařízení a telemetrie v řešení. Modely zařízení se používá pro fyzické i Simulovaná zařízení.

Příklad implementace fyzické zařízení, naleznete v části [připojení zařízení k předkonfigurovaného řešení vzdáleného monitorování](iot-suite-connecting-devices-node.md).

Pokud používáte _fyzického zařízení_, je nutné zadat klientská aplikace s modelem zařízení, která obsahuje specifikace zařízení metadata a telemetrie.

Následující části popisují modely zařízení pomocí Simulovaná zařízení:

### <a name="add-a-telemetry-type"></a>Přidání typu telemetrie

Typy zařízení v řešení ukázkové společnosti Contoso zadejte telemetrii, kterou odešle každý typ zařízení. Určit další telemetrické typy, zařízení můžete odeslat definice telemetrie jako metadata k řešení. Pokud použijete tento formát, řídicí panel využívá zařízení telemetrie a dostupné metody dynamicky a nemusíte upravit uživatelské rozhraní. Alternativně můžete upravit definici typu zařízení v řešení.

Se dozvíte, jak přidat vlastní telemetrii v _simulátor zařízení_ mikroslužbu, najdete v části [testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).

### <a name="add-a-device-type"></a>Přidejte typ zařízení

Ukázkové řešení Contoso definuje některé typy zařízení ukázka. Toto řešení umožňuje definovat typy vlastní zařízení, aby splňovaly vaše požadavky na konkrétní aplikaci. Vaše společnost může například použít průmyslových brány jako primární zařízení připojené k řešení.

Chcete-li vytvořit přesný reprezentace zařízení, budete muset upravit aplikaci, která běží na zařízení tak, aby odpovídala požadavky na zařízení.

Další informace o přidání nového typu zařízení v _simulátor zařízení_ mikroslužbu, najdete v části [testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).

### <a name="define-custom-methods-for-simulated-devices"></a>Definování vlastních metod pro simulované zařízení

Další postupy k definování vlastních metod pro simulované zařízení v řešení vzdáleného monitorování najdete v tématu [modely zařízení](https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Device-Models) v úložišti GitHub.

<!--
#### Using the simulator service

TODO: add steps for the simulator microservice here
-->

#### <a name="using-a-physical-device"></a>Použití fyzického zařízení

Implementace metod a úlohy na fyzických zařízení, najdete v následujících článcích IoT Hub:

* [Rady pro pochopení a volat přímé metody ze služby IoT Hub](../iot-hub/iot-hub-devguide-direct-methods.md).
* [Plánování úloh na několika zařízeních](../iot-hub/iot-hub-devguide-jobs.md).

### <a name="other-customization-options"></a>Další možnosti přizpůsobení

Pokud chcete upravit další připojení zařízení a streamování vrstvy v řešení vzdáleného monitorování, můžete upravit kód. Příslušné úložiště Githubu jsou:

* [Zařízení Telemetrie (.NET)](https://github.com/Azure/device-telemetry-dotnet)
* [Zařízení Telemetrie (Java)](https://github.com/Azure/device-telemetry-java)
* [Agent telemetrie (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Agent telemetrie (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="data-processing-and-analytics"></a>Zpracování a analýza dat

<!--
The following sections describe options to customize the data processing and analytics layer in the remote monitoring solution:

### Rules and actions

See the [Customize rules and actions](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to customize the rules and actions in solution.


### Other customization options
-->

Pokud chcete upravit zpracování dat a vrstvy analýzy v řešení vzdáleného monitorování, můžete upravit kód. Příslušné úložiště Githubu jsou:

* [Agent telemetrie (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Agent telemetrie (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="infrastructure"></a>Infrastruktura

<!--
The following sections describe options for customizing the infrastructure services in the remote monitoring solution:

### Change storage

The default storage service for the remote monitoring solution is Cosmos DB. See the [Customize storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses.

### Change log storage

The default storage service for logs is Cosmos DB. See the [Customize log storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses for logging.

### Other customization options
-->

Pokud chcete upravit infrastruktury v řešení vzdáleného monitorování, můžete upravit kód. Příslušné úložiště Githubu jsou:

* [Správce IoTHub (.NET)](https://github.com/Azure/iothub-manager-dotnet)
* [Správce IoTHub (Java)](https://github.com/Azure/iothub-manager-java)
* [Adaptér úložiště (.NET)](https://github.com/Azure/pcs-storage-adapter-dotnet)
* [Adaptér úložiště (Java)](https://github.com/Azure/pcs-storage-adapter-java)

## <a name="next-steps"></a>Další kroky

V tomto článku jste se dozvěděli o zdrojích, které vám umožní přizpůsobit předkonfigurovaného řešení.

Další koncepční informace o předkonfigurovaného řešení vzdáleného monitorování najdete v tématu [pro vzdálené monitorování architektura](iot-suite-remote-monitoring-sample-walkthrough.md)

Další informace o přizpůsobení řešení vzdáleného monitorování najdete v tématu:

* [Referenční příručka pro vývojáře](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Průvodce řešením potíží pro vývojáře](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->