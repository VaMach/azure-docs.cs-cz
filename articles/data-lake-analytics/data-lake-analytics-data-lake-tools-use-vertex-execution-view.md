---
title: "Pomocí zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Další informace o použití zobrazení provádění vrcholů na zkoušku úloh Data Lake Analytics."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Použití zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio
Další informace o použití zobrazení provádění vrcholů na zkoušku úloh Data Lake Analytics.

## <a name="prerequisites"></a>Požadavky

Potřebujete základní znalosti o vyvíjet skript U-SQL pomocí nástrojů Data Lake pro Visual Studio.  V tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Otevřete zobrazení provádění vrcholů
Otevřete úlohy U-SQL v nástrojů Data Lake pro Visual Studio. Klikněte na tlačítko **nebo zobrazení provádění vrcholů** v levém dolním rohu. Můžete být vyzváni, abyste nejdřív načíst profily a může trvat nějakou dobu v závislosti na připojení k síti.

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Pochopení nebo zobrazení provádění vrcholů
Zobrazení provádění vrcholů má tři části:

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

**Vrchol selektor** na levém umožňuje vybrat vrcholy funkce (například top 10 dat pro čtení, nebo zvolte fázi). Jednou z nejčastěji používaných filtry je zobrazíte **vrcholy kritické cesty**. **Kritické cesty** je nejdelší řetězu vrcholy úlohy U-SQL. Principy kritické cesty je užitečné pro své úlohy optimalizace kontrolou, které vrchol trvá nejdelší dobu.
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Zobrazuje podokno nahoře uprostřed podívejte **systémem stav všechny vrcholy**.
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

V dolním podokně center zobrazí informace o jednotlivých vrchol:
* Název procesu: Název instance vrchol. Se skládá z různých částí v StageName | VertexName | VertexRunInstance. Například vrchol [62] .v1 SV7_Split znamená druhý běžící instance (.v1, index od 0) čísla vrchol 62 ve fázi SV7_Split.
* Celkový počet dat pro čtení nebo Written: Data byla číst nebo zapisovat pomocí této vrchol.
* Stav stavu nebo ukončení: Konečný stav po ukončení vrchol.
* Ukončovací kód nebo selhání typ: Chyba při vrchol se nezdařilo.
* Vytvoření důvod: Proč vrchol byl vytvořen.
* Latence fronty latence/PN prostředků latenci nebo proces: čas potřebný pro vrchol čekání na prostředky, ke zpracování dat a zůstane ve frontě.
* Identifikátor GUID procesů nebo autora: Identifikátor GUID pro aktuální spuštěné vrchol nebo jeho tvůrce.
* Verze: N-tý instance spuštěné vrchol (systém může nové instance třídy vrchol naplánovat, pro mnoho důvodů, například převzetí služeb při selhání, výpočetní redundance atd.)
* Verze vytvořena, když.
* Zpracování vytvořit počáteční čas nebo proces zařazeno ve frontě čas nebo proces počáteční čas nebo proces dokončete čas: při zahájení procesu vrchol vytvoření; Při spuštění procesu vrchol do fronty; Při spuštění procesu určité vrchol; Po dokončení určité vrchol.

## <a name="next-steps"></a>Další kroky
* Pokud chcete protokolovat diagnostické informace, přečtěte si téma [Zobrazení protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Chcete-li zobrazit podrobnosti o úlohách, najdete v části [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)
