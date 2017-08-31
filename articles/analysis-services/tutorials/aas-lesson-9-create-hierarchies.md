---
title: "Kurz služby Azure Analysis Services – Lekce 9: Vytvoření hierarchií | Dokumentace Microsoftu"
description: 
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
ms.openlocfilehash: d628dc621335acf231342a6d9186079de16e85f4
ms.contentlocale: cs-cz
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-9-create-hierarchies"></a>Lekce 9: Vytvoření hierarchií

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte hierarchie. Hierarchie jsou skupiny sloupců uspořádané do úrovní, například hierarchie Geografie může mít podúrovně pro zemi, stát, kraj a město. Hierarchie se může zobrazit odděleně od ostatních sloupců v seznamu polí klientských aplikací pro vytváření sestav. Další informace najdete v tématu [Hierarchie](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular).
  
K vytvoření hierarchií použijte návrháře modelů v *zobrazení diagramu*. Vytváření a správa hierarchií se v zobrazení dat nepodporuje.  
  
Odhadovaný čas dokončení této lekce: **20 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabulkového modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 8: Vytvoření perspektiv](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Vytvoření hierarchií  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a>Postup vytvoření hierarchie Category v tabulce DimProduct  
  
1.  V návrháři modelů (zobrazení diagramu) klikněte pravým tlačítkem myši na tabulku **DimProduct** > **Vytvořit hierarchii**. V dolní části okna tabulky se zobrazí nová hierarchie. Přejmenujte hierarchii na **Category**.  
  
2.  Klikněte na sloupec **ProductCategoryName** a přetáhněte ho do nové hierarchie **Category**.  
  
3.  V hierarchii **Category** klikněte pravým tlačítkem myši na **ProductCategoryName** > **Přejmenovat**a pak zadejte **Category**.  
  
    > [!NOTE]  
    > Přejmenováním sloupce v hierarchii nedojde k přejmenování tohoto sloupce v tabulce. Sloupec v hierarchii je pouze reprezentací sloupce v tabulce.  
  
4.  Klikněte na sloupec **ProductSubcategoryName** a přetáhněte ho do hierarchie **Category**. Přejmenujte ho na **Subcategory**. 
  
5.  Pravým tlačítkem myši klikněte na sloupec **ModelName** > **Přidat do hierarchie** a potom vyberte **Category**. Přejmenujte ho na **Model**.

6.  Nakonec přidejte **EnglishProductName** do hierarchie Category. Přejmenujte ho na **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a>Postup vytvoření hierarchií v tabulce DimDate  
  
1.  V tabulce **DimDate** vytvořte hierarchii s názvem **Calendar**.  
  
3.  Přidejte následující sloupce v daném pořadí:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  V tabulce **DimDate** vytvořte hierarchii**Fiscal**. Zahrňte následující sloupce v daném pořadí:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Nakonec v tabulce **DimDate** vytvořte hierarchii **ProductionCalendar**. Zahrňte následující sloupce v daném pořadí:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Co dále?
[Lekce 10: Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md) 
  
  

