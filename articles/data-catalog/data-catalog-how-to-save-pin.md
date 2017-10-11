---
title: "Uložte hledání a připnout datových prostředků v Azure Data Catalog | Microsoft Docs"
description: "Postupy: článek zvýraznění možnosti v Azure Data Catalog pro ukládání zdroje dat a datových prostředků pro pozdější použití."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 8c319d0dcbe8b95af11b8be2368a9348b260446c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Uložte hledání a kód pin datových prostředků v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog poskytuje možnosti pro zjišťování zdroje. Můžete rychle vyhledat a filtrování katalogu pro vyhledání zdroje dat a pochopení jejich zamýšlený účel, snadněji najít správná data pro úlohu po ruce.

Ale co když je nutné pravidelně pracovat se stejnými daty? A co když vy a ostatní uživatelé pravidelně přispívat vašich znalostí do stejného zdroje dat v katalogu? V těchto situacích může být bylo nutné opakovaně vystavovat stejné hledání neefektivní. Toto je, kde může pomoci uložené hledání a definovaného datových prostředků.

## <a name="saved-searches"></a>Uložená hledání
Uložené hledání v katalogu Data Catalog je opakovaně využívat, definice vyhledávání na uživatele. Můžete definovat hledání, včetně podmínek vyhledávání, značky a ostatní filtry a pak ho uložte. Uložené hledání definici později vrátit všechny datových prostředků, které odpovídají jeho kritéria vyhledávání můžete znovu spustit.

### <a name="create-a-saved-search"></a>Vytvoření uloženého hledání
K vytvoření uloženého hledání, postupujte takto:
1. Na portálu Azure Data Catalog v **aktuální vyhledávání** okně klikněte na tlačítko **Uložit**. 

    ![Aktuální odkaz Uložit nastavení vyhledávání](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Zadejte kritéria hledání, které chcete znovu použít a pak klikněte na tlačítko **Uložit**.

    ![Aktuální nastavení vyhledávání uložit název vyhledávání](./media/data-catalog-how-to-save-pin/02-name.png)

3. Když se zobrazí výzva, zadejte název pro uložené hledání. Vyberte název, který má smysl a která popisuje datové prostředky, které vrátí vyhledávání.

### <a name="manage-saved-searches"></a>Spravovat uložená hledání
Po uložení jeden nebo více hledání **uložená hledání** možnost se zobrazí pod **aktuální vyhledávání** pole. Po rozbalení seznamu se zobrazí všechna uložená hledání.

 ![Seznam uložených hledání](./media/data-catalog-how-to-save-pin/03-list.png)

Proveďte jeden z následujících akcí:

* Provést hledání, vyberte v seznamu uloženého hledání.

* Chcete-li zobrazit seznam možností správy pro uložené hledání, klikněte na šipku dolů vedle názvu vyhledávání.

    ![Možnosti pro správu uložených hledání](./media/data-catalog-how-to-save-pin/04-managing.png)

* Pokud chcete zadat nový název pro uložené hledání, vyberte **přejmenovat**. Definice vyhledávání se nezmění.

* Chcete-li odebrat uloženého hledání ze seznamu, vyberte **odstranit**a poté potvrďte odstranění.

* Chcete-li označit uloženého hledání jako výchozí hledání, vyberte **uložit jako výchozí**. Pokud provádíte "prázdný" vyhledávání z domovské stránky Azure Data Catalog, se spustí vyhledávání výchozí. Kromě toho vyhledávání, který je označený jako výchozí nastavení hledání se zobrazí v horní části **uložená hledání** seznamu.

### <a name="organizational-saved-searches"></a>Organizační uložených hledání
Všechny uživatele ve vaší organizaci můžete uložit hledání pro vlastní použití. Správci katalogu dat můžete také uložit hledání pro všechny uživatele v rámci organizace. Když správci uložte hledání, že máte na výběr **sdílenou složku v rámci společnosti** možnost. Výběrem této možnosti sdílí uloženého hledání pro všechny uživatele v organizaci.

 ![Organizační uložených hledání](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Definovaného datových prostředků
S uložená hledání můžete uložit a opakovaně používat definice vyhledávání. Časem jako obsah katalogu změny může změnit datové prostředky, které se vrátí pomocí vyhledávání. Pokud připnete datových prostředků, můžete explicitně identifikovat konkrétní datové prostředky se mají usnadňuje jejich přístup bez nutnosti použít vyhledávání.

Připnutí datový prostředek je jednoduchá. Chcete-li přidat datový prostředek do definovaného seznamu, klepněte na tlačítko **pin** ikonu. Na ikonu se zobrazí v horním rohu na dlaždici asset v dlaždici zobrazení a ve sloupci nejvíce vlevo v zobrazení seznamu na portálu Azure Data Catalog.

![Na ikonu Připnutí datový prostředek](./media/data-catalog-how-to-save-pin/05-pinning.png)

Odepnutí datový prostředek je stejně jednoduché. Jednoduše klikněte **Odepnout** ikonu k přepnutí nastavení pro vybraný prostředek.

![Data-asset Odepnout ikonu](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>V části Moje prostředky
Zahrnuje domovské stránky portálu katalogu Data Catalog **Moje prostředky** oddíl, který zobrazuje prostředky, které vás zajímají s aktuálním uživatelem. Tato část zahrnuje obě definovaného prostředky a uložená hledání.

![V části Moje prostředky na domovské stránce](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Souhrn
Azure Data Catalog poskytuje možnosti, které usnadňují zjistit zdroje dat, které budete potřebovat, takže vám i ostatním členům organizace může trávit méně času, hledá dat a další čas pracovat s ním. Uložená hledání a připnuté data, která prostředky sestavení na tyto základní možnosti, aby uživatelé mohli snadno identifikovat zdrojů dat, které pracují s, opakovaně.
