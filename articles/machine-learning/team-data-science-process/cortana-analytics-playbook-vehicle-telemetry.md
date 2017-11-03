---
title: "Předpověď vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: af8b3d5bf891c93c30a05c5f02d86639a466dde5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Scénář řešení analýzy telemetrie vozidla
To **nabídky** odkazy na kapitoly v této scénářem. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Přehled
Super počítače byl přesunut mimo testovací prostředí a jsou nyní parkujících v našem Garážový! Tyto nejmodernější automobilů obsahovat velkého počtu senzorů, bude mít možnost ke sledování a monitorování miliony událostí za sekundu. Očekáváme, že do roku 2020, většinu těchto aut bude mít byl připojený k Internetu. Představte si klepnutím na do této zadávané data pro zajištění větší zabezpečení, spolehlivost a lepší řízení prostředí! Společnost Microsoft vyvinula to sní realitou s Cortana Intelligence.

Cortana Intelligence společnosti Microsoft je plně spravovaná velkých objemů dat a suite pokročilou analýzu, který umožňuje transformovat data do inteligentního akce. Chceme, které vám představí šablona Cortana Intelligence Vehicle Telemetrie Analytics řešení. Toto řešení ukazuje, jak dealerům car, automobilů výrobců a pojištění společností můžete použít možnosti Cortana Intelligence k získání v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí. 

Řešení je implementovaný jako [lambda architektura vzor](https://en.wikipedia.org/wiki/Lambda_architecture) zobrazující potenciální Cortana Intelligence platformy pro celý v reálném čase a dávkové zpracování. Řešení: 

* poskytuje Vehicle telematika simulátoru
* využívá pro příjem miliony událostí simulované vehicle telemetrická data do Azure Event Hubs 
* Stream Analytics používá k získání přehledy v reálném čase na vehicle stavu
* trvá data do dlouhodobého úložiště pro širší batch analýzu. 
* využívá Machine Learning pro zjišťování anomálií v reálném čase a zpracování a získáte přehled o prediktivní služby batch.
* využívá HDInsight k transformaci dat ve velkém měřítku a Data Factory pro zpracování orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu 
* Toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy pomocí Power BI

## <a name="architecture"></a>Architektura
![Diagram architektury řešení](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*obrázek 1 – Architektura řešení Analytics Vehicle Telemetrie*

Toto řešení zahrnuje následující **komponenty Cortana Intelligence** a umožňující prezentovat jejich koncové integrace:

* **Služba Event Hubs** pro příjem miliony událostí vehicle telemetrická data do Azure.
* **Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.
* **Strojového učení** pro zjišťování anomálií v reálném čase a dávkové zpracování a získáte přehled o prediktivní.
* **HDInsight** je využít k transformaci dat ve velkém měřítku
* **Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu.
* **Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.

Toto řešení používá dva různé **zdroje dat**: 

* **Simulated vehicle signály a Diagnostika**: simulátoru telematika vehicle vysílá diagnostické informace a signály, které odpovídají stavu nástroj a podporovat jeho vzor k danému bodu v čase. 
* **Vehicle katalogu**: referenční datová sada obsahující VIN pro mapování modelu.

