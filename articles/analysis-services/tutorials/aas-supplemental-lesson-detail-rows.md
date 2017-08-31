---
title: "Kurz služby Azure Analysis Services – Doplňková lekce: Řádky podrobností | Dokumentace Microsoftu"
description: "Popisuje, jak vytvořit výraz řádků podrobností v kurzu služby Azure Analysis Services."
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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: fde5cd9a9efc3a13e731a91962ced5c086a72355
ms.contentlocale: cs-cz
ms.lasthandoff: 06/03/2017

---
# <a name="supplemental-lesson---detail-rows"></a>Doplňková lekce – Řádky podrobností

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této doplňkové lekci použijete Editor DAX k definici vlastního výrazu řádků podrobností. Výraz řádků podrobností je vlastnost míry, která koncovým uživatelům poskytuje další informace o agregovaných výsledcích míry. 
  
Odhadovaný čas dokončení této lekce: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma doplňkové lekce je součástí kurzu tabulkového modelování. Než začnete provádět úkoly v této doplňkové lekci, měli byste mít dokončené všechny předchozí lekce nebo mít dokončený ukázkový projekt modelu Adventure Works Internet Sales.  
  
## <a name="what-do-we-need-to-solve"></a>Co je potřeba vyřešit?
Než přidáme výraz řádků podrobností, podívejme se na podrobnosti naší míry InternetTotalSales.

1.  V sadě SSDT klikněte na nabídku **Model** > **Analyzovat v aplikaci Excel**, otevřete Excel a vytvořte prázdnou kontingenční tabulku.
  
2.  V části **Pole kontingenční tabulky** přidejte míru **InternetTotalSales** z tabulky FactInternetSales do **Hodnoty**, **CalendarYear** z tabulky DimDate do **Sloupce** a **EnglishCountryRegionName** do **Řádky**. Naše kontingenční tabulka nám teď poskytuje výsledky míry InternetTotalSales podle oblastí a roku. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. V kontingenční tabulce poklikejte na agregovanou hodnotu pro rok a název oblasti. My jsme poklikali na hodnotu pro Austrálii a rok 2014. Otevře se nový list, který obsahuje data. Tato data ale nejsou moc užitečná.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Rádi bychom viděli tabulku obsahující sloupce a řádky dat přispívající k agregovanému výsledku naší míry InternetTotalSales. Za tímto účelem můžeme přidat výraz řádků podrobností jako vlastnost míry.

## <a name="add-a-detail-rows-expression"></a>Přidání výrazu řádků podrobností

#### <a name="to-create-a-detail-rows-expression"></a>Vytvoření výrazu řádků podrobností 
  
1. V sadě SSDT klikněte v mřížce měr tabulky FactInternetSales na míru **InternetTotalSales**. 

2. V části **Vlastnosti** > **Výraz řádků podrobností** klikněte na tlačítko editoru a otevřete Editor DAX.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. V Editoru DAX zadejte následující výraz:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Tento výraz určuje názvy a sloupce a výsledky měr z tabulky FactInternetSales a souvisejících tabulky se vrátí, když uživatel pokliká na agregovaný výsledek v kontingenční tabulce nebo sestavě.

4. Zpět v aplikaci Excel odstraňte list vytvořený v kroku 3, pak poklikejte na agregovanou hodnotu. Teď s nadefinovanou vlastností výrazu řádků vlastností pro míru se otevře nový list obsahující užitečnější data.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Znovu nasaďte model.

  
## <a name="see-also"></a>Viz také  
[Funkce SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Doplňková lekce – Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  

