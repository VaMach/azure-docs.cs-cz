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
ms.openlocfilehash: bec5066a2e1ba0e4e5e81c4e1be28ed8eb93ceed
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vehicle řešení analýzy Telemetrie playbook
Tato nabídka odkazy na kapitoly v této playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Přehled
Superpočítačů byl přesunut mimo testovací prostředí a jsou nyní parkujících v garáží. Tyto nejmodernější automobilů obsahovat řadu senzorů, které jim poskytnout možnost sledování a monitorování miliony událostí za sekundu. Do roku 2020 většinu těchto vozidel se připojí k Internetu. Klepnutím na do této zadávané dat nabízejí větší zabezpečení, spolehlivost a lepší řízení prostředí. Společnost Microsoft neposkytuje to sní realitou s Cortana Intelligence.

Cortana Intelligence je plně spravovaná velkých objemů dat a suite pokročilou analýzu, který můžete použít pro transformaci dat do inteligentního akce. Šablona Cortana Intelligence Vehicle Telemetrie Analytics řešení ukazuje, jak dealerům car, automobilů výrobců a pojištění společností můžete získat v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí. 

Řešení je implementovaný jako [lambda architektura vzor](https://en.wikipedia.org/wiki/Lambda_architecture), který zobrazuje kompletní potenciální Cortana Intelligence platformy pro v reálném čase a dávkové zpracování.

## <a name="architecture"></a>Architektura
Architektura řešení analýzy Telemetrie Vehicle je zobrazená v tomto diagramu:

![Diagram architektury řešení](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Toto řešení zahrnuje následující součásti Cortana Intelligence a umožňující prezentovat jejich integrace začátku do konce:

* **Azure Event Hubs** ingestuje miliony událostí vehicle telemetrická data do Azure.
* **Azure Stream Analytics** poskytuje přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.
* **Azure Machine Learning** zjistí anomálie v reálném čase a k poskytování prediktivní insights používá dávkové zpracování.
* **Azure HDInsight** transformuje data ve velkém měřítku.
* **Azure Data Factory** zpracovává orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu.
* **Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.

Toto řešení používá dvou různých zdrojů dat.: 

* **Simulated vehicle signály a Diagnostika**: simulátoru telematika vehicle vysílá diagnostické informace a signály, které odpovídají stavu nástroj a podporovat jeho vzor k danému bodu v čase. 
* **Vehicle katalogu**: Tato referenční datová sada mapuje VINs modelů.

