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
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 37d12209d28b73f0d7fc6d940ded344fbeae968d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Postup zobrazení souvisejících dat prostředky v Azure Data Catalog?
Azure Data Catalog umožňuje zobrazit datové prostředky související s vybraná data asset a zobrazení vztahy mezi nimi. 

## <a name="supported-data-sources"></a>Podporované zdroje dat 
Při registraci datových prostředků z následujících zdrojů dat Azure Data Catalog automaticky registruje metadata o relace typu join mezi vybraných datových prostředcích. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

> [!NOTE]
> Pro katalog Data Catalog pro import vztahu mezi dvěma datovými prostředky je nutné zaregistrovat i prostředky ve stejnou dobu. Pokud jste měli jeden z nich přidali samostatně, přidejte ho znovu a datový prostředek pro import vztahu mezi nimi.

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

## <a name="next-steps"></a>Další postup
- [Jak spravovat datové prostředky](data-catalog-how-to-manage.md)