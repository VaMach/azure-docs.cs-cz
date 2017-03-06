---
title: "Úprava obsahu stránek na portálu pro vývojáře ve službě Azure API Management | Dokumentace Microsoftu"
description: "Naučte se upravovat obsah stránek na portálu pro vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
translationtype: Human Translation
ms.sourcegitcommit: ecae1da20551d8372331124b07c4aca2e15f55bb
ms.openlocfilehash: 708c803c36c182ed90e04731b12d4ade00ae7ffb
ms.lasthandoff: 02/23/2017


---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>Úprava obsahu a rozložení stránek na portálu pro vývojáře ve službě Azure API Management
Existují tři základní způsoby, kterými jde přizpůsobit portál pro vývojáře ve službě Azure API Management:

* [Úprava obsahu statických stránek a elementů rozložení stránek][modify-content-layout] (vysvětlení obsahuje tento průvodce)
* [Aktualizace stylů použitých pro elementy stránek napříč portálem pro vývojáře][customize-styles]
* [Úprava šablon použitých pro stránky generované portálem][portal-templates] (dokumentace rozhraní API, produkty, ověřování uživatelů atd.)

## <a name="page-structure"> </a>Struktura stránek portálu pro vývojáře

Portál pro vývojáře je založený na systému CMS (Content Management System). Rozložení jednotlivých stránek je založené na základě sady malých elementů stránek, které se označují jako widgety:

![Struktura stránek portálu pro vývojáře][api-management-customization-widget-structure]

Všechny widgety se dají upravovat. 
* Základní obsah specifický pro každou jednotlivou stránku se nachází ve widgetu Contents. Úprava stránky znamená úpravy obsahu tohoto widgetu.
* Všechny prvky rozložení stránky jsou obsažené v ostatních widgetech. Změny provedené v těchto widgetech budou platit pro všechny stránky. Budeme je označovat jako widgety rozložení.

Při běžných úpravách stránek se obvykle mění jenom widget Content, který má pro každou jednotlivou stránku jiný obsah.

## <a name="modify-layout-widget"> </a>Změně obsahu widgetu rozložení

Obsah portálu pro vývojáře se mění prostřednictvím portálu vydavatele, který je přístupný z webu Azure Portal. Otevřete jej tak, že na panelu nástrojů služby svojí instance služby API Management kliknete na **Portál vydavatele**.

![Portál vydavatele][api-management-management-console]

Pokud chcete obsah takového widgetu upravit, klikněte v nabídce **Portál pro vývojáře** na levé straně na **Widgety**. V tomto příkladu upravíme obsah widgetu Header. V seznamu vyberte widget **Header**.

![Záhlaví widgetů][api-management-widgets-header]

Obsah záhlaví můžete upravovat v poli **Text**. Změňte text podle potřeby a potom v dolní části stránky klikněte na **Uložit**.

Nové záhlaví by se teď mělo zobrazovat na každé stránce portálu pro vývojáře.

> Pokud chcete portál pro vývojáře otevřít, když jste na portálu vydavatele, klikněte na horním panelu na **Portál pro vývojáře**.
> 
> 

## <a name="edit-page-contents"> </a>Úprava obsahu stránky

Pokud chcete zobrazit seznam všech existujících stránek s obsahem, klikněte v nabídce **Portál pro vývojáře** (na portálu vydavatele) na **Obsah**.

![Správa obsahu][api-management-customization-manage-content]

Pokud chcete upravit obsah zobrazený na domovské stránce portálu pro vývojáře, klikněte na stránku **Vítejte**. Proveďte požadované změny, v případě potřeby si zobrazte jejich náhled a potom je zpřístupněte všem uživatelům kliknutím na **Publikovat**.

> Domovská stránka používá zvláštní rozložení, které umožňuje zobrazit banner v její horní části. Tento banner nemůžete v části **Obsah** upravovat. Pokud chcete banner upravit, klikněte v nabídce **Portál pro vývojáře** na **Widgety**, v rozevíracím seznamu **Aktuální vrstva** vyberte **Domovská stránka** a potom v části **Vybraná část** otevřete položku **Banner**. Obsah tohoto widgetu můžete upravovat stejně jako jakoukoli jinou stránku.
> 
> 

## <a name="next-steps"> </a>Další kroky
* [Aktualizace stylů použitých pro elementy stránek napříč portálem pro vývojáře][customize-styles]
* [Úprava šablon použitých pro stránky generované portálem][portal-templates] (dokumentace rozhraní API, produkty, ověřování uživatelů atd.)

[Structure of developer portal pages]: #page-structure
[Modifying the contents of a layout widget]: #modify-layout-widget
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png

