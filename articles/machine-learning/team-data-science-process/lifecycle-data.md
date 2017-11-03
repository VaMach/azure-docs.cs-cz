---
title: "Získávání dat a principy fáze životního cyklu procesu Team datové vědy - Azure | Microsoft Docs"
description: "Cíle, úlohy a výsledek pro získávání dat a principy fáze projekty vědecké účely data."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: eea3c357ebf6ad920c0ddebdb979aa07aece4794
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="data-acquisition-and-understanding"></a>Získávání a pochopení dat

Toto téma popisuje cíle, úlohy, a výsledek přidružený **získávání dat a principy fáze** procesu Team dat vědecké účely. Tento proces zajišťuje doporučené životního cyklu, který můžete použít pro struktury projekty vědecké účely data. Životní cyklus popisuje hlavní fází, které projekty obvykle provést, často interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cíle
* Čistá, vysoce kvalitní sady dat se rozumí jehož vztahy proměnným cíl se nacházejí v prostředí příslušné analýzy připraven k modelu.
* Byl vyvinut architektury řešení datového kanálu aktualizujte a pravidelně stanovení skóre data.

## <a name="how-to-do-it"></a>Jak to udělat
Existují tři hlavní úlohy v této fázi řešit:

* **Zpracování příjmu dat** do cílové analytické prostředí.
* **Data prozkoumat** k určení, pokud kvalitu dat je dostačující odpověď na otázku. 
* **Nastavit datovém kanálu** skóre pro nová nebo pravidelně aktualizovat data.

### <a name="21-ingest-the-data"></a>2.1 zpracování příjmu dat
Nastavte proces přesouvat data ze zdrojového umístění do cílového umístění, kde operací analýz jako školení a předpovědi se mají provést. Technické podrobnosti a možnosti o tom, jak to provést pomocí různých služeb Azure dat najdete v tématu [načtení dat do úložiště prostředí pro analýzu](ingest-data.md). 

### <a name="22-explore-the-data"></a>2.2 data prozkoumat
Předtím, než je cvičení modely, budete muset vyvíjet zvukové povědomí o data. Datové sady reálného jsou často aktivní nebo chybí hodnoty nebo hostitel jiných nesrovnalostí. Shrnutí dat a vizualizaci slouží k auditu kvality vaše data a poskytovat informace potřebné ke zpracování dat, než je připraven pro modelování. Tento proces je často iterativní.

TDSP poskytuje automatizované nástroj [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) k vizualizaci dat a připravit data souhrnné sestavy. Doporučujeme začít s IDEAR nejprve k prozkoumání dat k vývoji počáteční data porozumění interaktivně s žádné kódování a pak psát vlastní kód pro zkoumání dat a vizualizaci. Pokyny k čištění data, najdete v části [úkoly přípravy dat pro vylepšený strojového učení](prepare-data.md).  

Jakmile budete spokojeni s kvality vyčištěny data, dalším krokem je lépe pochopit vzorů, které jsou obsaženy v datech, které vám pomůžou vybrat a vytvořte odpovídající prediktivního modelu k cílovému. Podívejte se pro důkaz pro jak dobře připojené dat je k cíli, a zda je dostatek dat přejdete na následující dalších kroků modelování. Tento proces je znovu, často iterativní. Budete muset najít nové zdroje dat se přesnější nebo více odpovídajících dat k posílení původně stanovených ve fázi předchozí datovou sadu.  

### <a name="23-set-up-a-data-pipeline"></a>2.3 nastavit datovém kanálu
Kromě počáteční přijímání a čištění dat obvykle musíte nastavit proces skóre pro nová data nebo pravidelně aktualizuje data v rámci procesu probíhající učení. Tento krok můžete provést nastavením na datovém kanálu nebo pracovního postupu. Tady je [příklad](move-sql-azure-adf.md) o tom, jak nastavit kanál pomocí [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Architektura řešení datového kanálu je vyvinuta v této fázi. Kanál také vyvinutý paralelně s těchto fází projektu vědecké účely data. Kanál může být na základě batch nebo streamování nebo real-bránu nebo hybridní v závislosti na obchodních potřeb a omezení do stávajících systémů, do které je integrované řešení. 

## <a name="artifacts"></a>Artefakty
Níže jsou dodávky v této fázi.

* [**Sestava kvality dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Tato sestava obsahuje souhrnné informace o data, vztahy mezi každý atribut a cíl, proměnné hodnocení atd. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) nástroj, který poskytuje jako součást TDSP můžete rychle vygenerovat tuto sestavu na žádné tabulkové datové sady jako soubor CSV nebo relační tabulku. 
* **Architektura řešení**: to může být diagram nebo popis kanál dat použitý ke spuštění vyhodnocování nebo předpovědi na nová data, po který jste vytvořili model. Obsahuje taky kanálu, který má být přeučování modelu na základě na nová data. Dokument je uložen v [projektu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory při použití šablony TDSP directory struktura.
* **Kontrolní bod rozhodnutí**: než začnete úplné funkce inženýrství a vytváření modelů, prezentovaný projekt, který má-li určit, zda je očekávána hodnota dostatečná pokračujte usilovat o normalizaci ho. Může být například připraveni pokračovat, třeba shromažďovat další data, nebo zrušte projektu jako data neexistuje odpověď na otázku.

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na každý krok v průběhu životního cyklu procesu Team dat vědecké účely:

* [1. Pochopení obchodních](lifecycle-business-understanding.md)
* [2. Získávání dat a principy](lifecycle-data.md)
* [3. Modelování](lifecycle-modeling.md)
* [4. Nasazení](lifecycle-deployment.md)
* [5. Přijetí zákazníka](lifecycle-acceptance.md)

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) tématu. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace.  

Příklady provedením kroků v procesu Team dat. vědecké účely, které používají Azure Machine Learning Studio najdete v tématu [s Azure ML](http://aka.ms/datascienceprocess) kurzů.