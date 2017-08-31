---
title: "Kurz služby Azure Analysis Services – Lekce 3: Označení jako tabulky kalendářních dat | Dokumentace Microsoftu"
description: "Popisuje, jak označit tabulku kalendářních dat v projektu Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: c62f2726fef5219155a08b70c61162c914600d1d
ms.contentlocale: cs-cz
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-3-mark-as-date-table"></a>Lekce 3: Označení jako tabulky kalendářních dat

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V Lekci 2: Získání dat jste naimportovali tabulku dimenzí s názvem DimDate. Přestože ve vašem modelu je tato tabulka pojmenovaná DimDate, je možné ji také označit jako *tabulku kalendářních dat*, protože obsahuje údaje o datu a čase.  
  
Při každém použití funkcí časového měřítka DAX, třeba při pozdějším vytvoření měření, musíte zadat vlastnosti, které zahrnují *tabulku kalendářních dat* a jedinečný identifikátor – *sloupec Date* v této tabulce.
  
V této lekci označíte tabulku DimDate jako *tabulku kalendářních dat* a sloupec Date (v tabulce kalendářních dat) jako *sloupec Date* (jedinečný identifikátor).  

Než označíte tabulku a sloupec kalendářních dat, je vhodná doba provést drobnou údržbu, která zajistí lepší srozumitelnost vašeho modelu. Všimněte si, že tabulka DimDate obsahuje sloupec s názvem **FullDateAlternateKey**. Tento sloupec obsahuje jeden řádek pro každý den každého kalendářního roce zahrnutého v tabulce. Tento sloupec používáte v řadě vzorců měření a v sestavách. Ale FullDateAlternateKey skutečně není dobrý identifikátor pro tento sloupec. Přejmenujete ho na **Date** a usnadníte tak jeho identifikaci a použití ve vzorcích. Je vhodné přejmenovat objekty, jako jsou tabulky a sloupce, aby se usnadnila jejich identifikace v SSDT a klientských aplikacích pro vytváření sestav, jako je Power BI a Excel. 
  
Odhadovaný čas dokončení této lekce: **3 minuty**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 2: Získání dat](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Přejmenování sloupce FullDateAlternateKey

1.  V návrháři modelů klikněte na tabulku **DimDate**.

2.  Dvakrát klikněte na hlavičku sloupce **FullDateAlternateKey** a přejmenujte ho na **Date**.

  
### <a name="to-set-mark-as-date-table"></a>Nastavení Označit jako tabulku kalendářních dat  
  
1.  Vyberte sloupec **Date** a potom v okně **Vlastnosti** v části **Typ dat** ověřte, že je vybraná možnost **Datum**.  
  
2.  Klikněte do nabídky **Tabulka**, potom klikněte na **Datum** a nakonec klikněte na **Označit jako tabulku kalendářních dat**.  
  
3.  V dialogovém okně **Označit jako tabulku kalendářních dat** v seznamu **Datum** vyberte sloupec **Date** jako jedinečný identifikátor. Ve výchozím nastavení je obvykle vybraný. Klikněte na **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Co dále?
[Lekce 4: Vytvoření relací](../tutorials/aas-lesson-4-create-relationships.md)
  

