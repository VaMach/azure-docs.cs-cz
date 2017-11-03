---
title: "Modelování fáze životního cyklu procesu Team datové vědy - Azure | Microsoft Docs"
description: "Cíle, úlohy a výsledek pro modelování fázi projekty vědecké účely data."
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
ms.openlocfilehash: 0c855faa96d7feb9f762ecaed7654669a5a8a5b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="modeling"></a>Modelování

Toto téma popisuje cíle, úlohy, a výsledek přidružený **modelování fáze** procesu Team dat vědecké účely. Tento proces zajišťuje doporučené životního cyklu, který můžete použít pro struktury projekty vědecké účely data. Životní cyklus popisuje hlavní fází, které projekty obvykle provést, často interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cíle
* Funkce optimální dat pro model machine learning.
* Informativní modelu ML, který nejpřesněji předpovídá cíl.
* Modelu ML, který je vhodný pro produkční prostředí.

## <a name="how-to-do-it"></a>Jak to udělat
Existují tři hlavní úlohy v této fázi řešit:

* **Konstruování**: vytvoření funkce data z původní data, která usnadňují modelu školení.
* **Model školení**: najít model, který odpovídá na otázku nejvíce přesně tak, že porovnáte jejich metriky úspěchu.
* Určí, zda váš model **vhodná pro produkční**.

### <a name="31-feature-engineering"></a>3.1 funkce inženýrství
Funkce inženýrství zahrnuje zahrnutí, agregace a transformaci nezpracovaná proměnných pro vytvoření funkce použité v analýzy. Pokud chcete, aby aspekty faktory určující model, musíte pochopit, jak funkce jsou vzájemně souvisí a jak jsou algoritmy strojového učení k používání těchto funkcí. Tento krok vyžaduje tvůrčí kombinaci domény znalosti a statistiky získat z kroku zkoumání dat. Toto je vyrovnávání operace hledání a včetně informativní proměnné a snižuje příliš mnoho nesouvisejícími proměnné. Informativní proměnné zlepšení našeho výsledek; nesouvisejícími proměnné zavést jako zbytečný rušivý element do modelu. Také budete muset vygenerovat tyto funkce pro žádná nová data získané při vyhodnocování. Proto generování tyto funkce lze pouze závisí na data, která je k dispozici v době vyhodnocování. Technické pokyny v inženýrství funkce pro používání různých technologií dat Azure najdete v tématu [konstruování v procesu vědecké účely dat](create-features.md). 

### <a name="32-model-training"></a>3.2 modelu školení
V závislosti na typu otázku, kterou zkoušíte odpovědí existuje mnoho modelování algoritmy. Pokyny k výběru algoritmy, najdete v části [jak zvolit algoritmy pro Microsoft Azure Machine Learning](../studio/algorithm-choice.md). I když v tomto článku je napsán pro Azure Machine Learning, je užitečné pro všechny strojového učení projekty pokyny, které poskytuje. 

Proces pro trénování modelu zahrnuje následující kroky: 

* **Rozdělení vstupních dat** náhodně pro modelování do trénovací datové sady a testovací datové sady.
* **Vytvářet modely** pomocí trénovací datové sady.
* **Vyhodnocení** optimalizace parametrů (označované jako parametr oblouku), které se mají za cíl zajistit odpovědi na otázky týkající se data aktuálního přidružené řady konkurenční algoritmy strojového učení spolu různé (učení a testovací datové sady).
* **Určit "doporučené" řešení** odpověď na otázku tak, že porovnáte metriky úspěchu mezi alternativní metody.

> [!NOTE]
> **Zabránilo úniku**: úniku dat může být způsobeno zahrnutí dat z mimo datovou sadu školení, která umožňuje, aby modelu nebo algoritmu strojového učení aby unrealistically dobrý předpovědi. Únikům je běžným důvodem, proč data, která vědců získat nervové, když získají prediktivní výsledky, které se zdají příliš výhodné mít hodnotu true. Tyto závislosti může být obtížné zjistit. Aby se zabránilo úniku často vyžaduje iterace mezi vytváření sadu analýzy dat, vytvoření modelu a vyhodnocení přesnost. 
> 
> 

Poskytujeme [automatizované modelování a vytváření sestav nástroje](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) s TDSP, který se bude moct spustit prostřednictvím více algoritmů a parametr změny k vytvoření základního modelu. Vytváří také směrný plán modelování sestavy shrnutí výkonu kombinace každý model a parametr včetně proměnné význam. Tento proces je iterativní také jako ho můžete drive inženýrství další funkce. 

## <a name="artifacts"></a>Artefakty
Artefakty vytvořeného v této fázi patří:

* [**Funkce sady**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): funkce vytvořených pro modelování jsou popsány v **sady funkcí** části **definice dat** sestavy. Obsahuje odkazy na kód pro generování funkce a popis na tom, jak byl vygenerován funkci.
* [**Model sestavy**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): pro každý model, který se pokus o, standard, se vytváří na základě šablon sestav, která poskytuje podrobnosti o každém pokusu.
* **Kontrolní bod rozhodnutí**: vyhodnocení, zda ji nasadit do produkčního systému provádí dostatečně dobře modelu. Jsou některé klíčové otázky:
  * Model odpovědět na otázku s dostatečnou důvěru zadané testovací data? 
  * Vyzkoušejte všechny přístupy: shromažďovat další data, proveďte další funkce inženýrství nebo experimentovat s jiné algoritmy?

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na každý krok v průběhu životního cyklu procesu Team dat vědecké účely:

* [1. Pochopení obchodních](lifecycle-business-understanding.md)
* [2. Získávání dat a principy](lifecycle-data.md)
* [3. Modelování](lifecycle-modeling.md)
* [4. Nasazení](lifecycle-deployment.md)
* [5. Přijetí zákazníka](lifecycle-acceptance.md)

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) tématu. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 

Příklady provedením kroků v procesu Team dat. vědecké účely, které používají Azure Machine Learning Studio najdete v tématu [s Azure ML](http://aka.ms/datascienceprocess) kurzů. 