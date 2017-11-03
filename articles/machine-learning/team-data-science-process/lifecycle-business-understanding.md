---
title: "Obchodní principy fáze životního cyklu procesu Team datové vědy - Azure | Microsoft Docs"
description: "Cíle, úlohy a výsledek pro fázi Principy obchodní projekty vědecké účely data."
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
ms.openlocfilehash: 2adce61f8185bd86a6a870bb5752fe936701b0af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="business-understanding"></a>Principy podniku

Toto téma popisuje cíle, úlohy, a výsledek přidružený **obchodní principy fáze** procesu Team dat vědecké účely. Tento proces zajišťuje doporučené životního cyklu, který můžete použít pro struktury projekty vědecké účely data. Životní cyklus popisuje hlavní fází, které projekty obvykle provést, často interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cíle
* **Klíče proměnné** jsou zadány, která bude sloužit jako, které jsou **modelu cíle** a jehož související metriky používají zjistili úspěšnost pro projekt.
* Odpovídajícího **zdroje dat** se identifikovanou že firmy má přístup k nebo je potřeba získat.

## <a name="how-to-do-it"></a>Jak to udělat
Existují dvě hlavní úlohy v této fázi řešit: 

* **Definování cílů**: práce s vašich zákazníků a dalších zúčastněných osob, pochopit a identifikovat obchodních problémů. Formulovali otázky, které definují obchodních cílů a který může cílit na datové vědě techniky.
* **Identifikaci zdroje dat**: najít relevantní data, která vám pomůže odpovědět na otázky, které definují cíle projektu.

### <a name="11-define-objectives"></a>1.1 definování cílů

1. Ústředním cílem tohoto kroku je identifikace klíč **obchodní proměnné** vyžadující analýza předpovědět. Tyto proměnné jsou označovány jako **modelu cíle** a metriky s nimi spojených se používají k určení úspěšnosti projektu. Dva příklady takových cíle jsou prognózy prodeje nebo pravděpodobnost pořadí se podvodné.

2. Definování **projektu cíle** žádostí a upřesnění "ostré" otázky, které jsou relevantní a konkrétní a jednoznačné. Vědecké zpracování dat je proces, pomocí názvů a čísel k odpovědi na tyto otázky. Další informace o klást otázky sharp najdete v tématu [postup vědecké zpracování dat](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Vědecké zpracování dat / machine learning se obvykle používá k odpovědi na pět typů otázek:
 
   * Kolik nebo kolik? (regrese)
   * Které kategorie? (klasifikace)
   * Které skupiny? (clustery)
   * Je to divné? (detekce anomálií)
   * Možnosti, kterou by měla být provedena? (doporučení)

    Určete, který tyto dotazy jsou požádat a jak volaného dosahuje svých cílů firmy.

3. Definování **projektu team** zadáním rolích a zodpovědnostech svých členů. Vytvořte plán vysoké úrovně milník, který iterovat jako je zjistit další informace.  

4. **Definování metriky úspěchu**. Příklad: zákazníka změn předpovědi přesnost X % dosáhnout konce tohoto projektu 3 měsíce, aby nabízíme povýšení ke snížení změn. Musí být metriky **INTELIGENTNÍ**: 
   * **S**určitá 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**vázané na editoru ime 

### <a name="12-identify-data-sources"></a>1.2 identifikaci zdroje dat
Identifikujte zdroje dat, které obsahují známé příklady odpovědi na otázky sharp. Vyhledejte následující data:

* Data, která je **Relevant** na otázku. Máme míry cíl a funkce, které se vztahují k cíli?
* Data, která je **přesně** naše cílových modelu a funkce, které vás zajímají.

Například není neobvyklé, zjistíte, je potřeba stávajících systémů shromažďovat a protokolovat další typy dat potíže vyřešit a dosáhnout cíle projektu. V takovém případě můžete hledat externích zdrojů dat. nebo aktualizovat vaše systémy shromažďovat nová data.

## <a name="artifacts"></a>Artefakty
Zde jsou dodávky v této fázi:

* [**Charterovou dokumentu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standardní šablona je součástí definice struktury TDSP projektu. Toto je životností dokumentu, který se aktualizuje v celém projektu jako nové zjišťování jsou vytvářeny a jako obchodní požadavky se změní. Klíč je k iteraci na tento dokument, přidání další podrobnosti, jako průběh procesu zjišťování. Ponechte zákazníka a ostatní účastníci součástí provádění požadovaných změn a zřetelněji sdělují důvody pro změny k nim.  
* [**Zdroje dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Toto je **nezpracovaná zdroje dat** části **definice dat** sestavu, která se nachází v projektu TDSP **sestavu dat** složky. Určuje původní a cílové umístění nezpracovaná data. Ve fázích novější je zadat další podrobnosti, třeba skripty pro přesun dat do prostředí analýzy.  
* [**Slovník dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Tento dokument obsahuje popis dat, která je zadaných klientem. Tyto popisy obsahují informace o schématu (datové typy, informace o ověřovacích pravidel, pokud existuje) a entity-relation diagramy, pokud je k dispozici.

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na každý krok v průběhu životního cyklu procesu Team dat vědecké účely:

* [1. Pochopení obchodních](lifecycle-business-understanding.md)
* [2. Získávání dat a principy](lifecycle-data.md)
* [3. Modelování](lifecycle-modeling.md)
* [4. Nasazení](lifecycle-deployment.md)
* [5. Přijetí zákazníka](lifecycle-acceptance.md)

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) tématu. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 

Příklady provedením kroků v procesu Team dat. vědecké účely, které používají Azure Machine Learning Studio najdete v tématu [s Azure ML](http://aka.ms/datascienceprocess) kurzů.