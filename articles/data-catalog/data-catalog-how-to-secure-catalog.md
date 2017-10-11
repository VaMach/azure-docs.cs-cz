---
title: "Jak zabezpečit přístup k Azure Data Catalog | Microsoft Docs"
description: "Tento článek vysvětlují, jak zabezpečit katalog data catalog a jeho datové prostředky."
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
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: 9664a7bc8493b08c8e0797ac6f1b212079829833
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Jak zabezpečit přístup do katalogu data catalog a datových prostředků
> [!IMPORTANT]
> Tato funkce je k dispozici pouze v edici standard systému Azure Data Catalog.

Azure Data Catalog umožňuje určit, kdo má přístup k katalogu dat a jaké operace (zaregistrovat, opatřit poznámkami, převzít vlastnictví) můžete provádět na metadata v katalogu. 

## <a name="catalog-users-and-permissions"></a>Uživatelé katalogu a oprávnění
Zadejte uživatele nebo skupinu přístup do katalogu data catalog a nastavit oprávnění:

1. Na [domovskou stránku katalogu dat](http://www.azuredatacatalog.com), klikněte na tlačítko **nastavení** na panelu nástrojů.

    ![data catalog – nastavení](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Na stránce nastavení, rozbalte **uživatelé katalogu** části.
    ![Katalogu uživatelé – přidat](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Klikněte na tlačítko **Přidat**.
4. Zadejte plně kvalifikovaný **uživatelské jméno** nebo název **skupiny zabezpečení** v Azure Active Directory (AAD) přidružené k katalogu. Čárkou (', ') jako oddělovač, chcete-li přidat více než jeden uživatel nebo skupina.
    ![Uživatelé katalogu - uživatele nebo skupiny](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Stiskněte klávesu **ENTER** nebo **KARTĚ** mimo textové pole. 
6.  Potvrďte, že všechna oprávnění (**Poznámka**, **zaregistrovat**, a **převzít vlastnictví**) jsou přiřazeny pro tyto uživatele nebo skupiny ve výchozím nastavení. To znamená, může uživatel nebo skupina [zaregistrovat datových prostředků]( data-catalog-how-to-register.md), [opatřit poznámkami datové prostředky]( data-catalog-how-to-annotate.md), a [převzetí vlastnictví datových prostředků]( data-catalog-how-to-manage.md). 
    ![Uživatelé katalogu – výchozí oprávnění](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Chcete-li poskytnout uživatele nebo skupinu pouze přístup pro čtení do katalogu, zrušte **opatřit poznámkami** možnost pro tohoto uživatele nebo skupinu. Pokud tak učiníte, uživatele nebo skupinu nelze opatřit poznámkami datové prostředky v katalogu, ale je mohou zobrazit. 
8.  Chcete-li odepřít uživatele nebo skupinu ze registrace datových prostředků, zrušte **zaregistrovat** možnost pro tohoto uživatele nebo skupinu.
9.  Chcete-li odepřít uživatele přebírat vlastnictví datový prostředek, zrušte **převzít vlastnictví** možnost pro tohoto uživatele nebo skupinu. 
10. Pokud chcete odstranit uživatele nebo skupiny z uživatelé katalogu, klikněte na tlačítko **x** pro uživatele nebo skupiny v dolní části seznamu. 
    ![Uživatelé katalogu – odstranit uživatele](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Doporučujeme přidat skupiny zabezpečení přímo katalogu uživatelů místo přidání uživatelů a přiřaďte oprávnění. Potom přidání uživatelů do skupiny zabezpečení, které odpovídají jejich rolí a jejich požadovaný přístup do katalogu.

## <a name="special-considerations"></a>Zvláštní upozornění

- Oprávnění přiřazená uživateli skupiny zabezpečení jsou aditivní. Vyslovte uživatel je ve dvou skupinách. Jedna skupina má opatřit poznámkami oprávnění a jiné skupiny není mají opatřit poznámkami oprávnění. Potom uživatel má opatřit poznámkami oprávnění. 
- Oprávnění přiřazená uživateli explicitně přepsat oprávnění přiřazená do skupin, do kterých uživatel patří. V předchozím příkladu Řekněme, můžete explicitně přidat uživatele katalogu uživatelů a provádět není přiřazení opatřit poznámkami oprávnění. Uživatele nelze opatřit poznámkami datové prostředky, i když je uživatel členem skupiny, která nemá opatřit poznámkami oprávnění.

## <a name="next-steps"></a>Další kroky
- [Začínáme s Azure Data Catalogem](data-catalog-get-started.md)

