---
title: "Postup zobrazení souvisejících dat prostředky v Azure Data Catalog | Microsoft Docs"
description: "Tento článek vysvětluje postup zobrazení souvisejících dat majetku vybrané datové prostředky v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: 0623847ea1e79c3a45a8fbdcc1d3a551864aefd6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Postup zobrazení souvisejících dat prostředky v Azure Data Catalog?
Azure Data Catalog umožňuje zobrazit datové prostředky související s vybraná data asset a zobrazení vztahy mezi nimi. 

## <a name="supported-data-sources"></a>Podporované zdroje dat 
Při registraci datových prostředků z následujících zdrojů dat Azure Data Catalog automaticky registruje metadata o relace typu join mezi vybraných datových prostředcích. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Zobrazit související datových prostředků
Chcete-li zobrazit datové prostředky, které souvisí s vybranou datovou sadu, použijte **vztahy** kartě, jak je znázorněno na následujícím obrázku: 

![Azure Data Catalog – zobrazení související s datovým prostředkům](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

V tomto příkladu jsou dva vztahy pro vybranou **ProductSubcategory** datový prostředek: 

- ProductSubcategoryID sloupec tabulky produktu má relace cizího klíče se sloupcem ProductSubcategoryID vybrané ProductSubcategory tabulky. 
- ProductCategoryID sloupec tabulky ProductSubCategory má relace cizího klíče se sloupcem ProductCategoryID vybrané tabulky ProductCategory.

> [!NOTE]
> Všimněte si, směr na šipku v zobrazení stromu vztahů.  

Chcete-li zobrazit další podrobnosti, jako je plně kvalifikovaný název sloupce, najeďte myší a zobrazí místní okno podobně jako na následujícím obrázku: 

![Azure Data Catalog – místní relace](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

Chcete-li zahrnout vztahy mezi prostředky, které jsou již zaregistrovány, znovu zaregistrujte tyto prostředky.

## <a name="next-steps"></a>Další kroky
- [Jak spravovat datové prostředky](data-catalog-how-to-manage.md)