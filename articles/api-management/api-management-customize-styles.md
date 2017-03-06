---
title: "Přizpůsobení stylů na portálu pro vývojáře ve službě Azure API Management | Dokumentace Microsoftu"
description: "Zjistěte, jak změnit styly použité pro libovolnou stránky na portálu pro vývojáře ve službě Azure API Management."
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
ms.sourcegitcommit: 336d4f80f0357796fb29eb9314c11edfce831a69
ms.openlocfilehash: bd08eb476a4bd7298c5650977b88ba0b24deddec
ms.lasthandoff: 02/23/2017


---
# <a name="customize-the-styling-of-the-developer-portal-in-azure-api-management"></a>Přizpůsobení stylů na portálu pro vývojáře ve službě Azure API Management
Existují tři základní způsoby, kterými jde přizpůsobit portál pro vývojáře ve službě Azure API Management:

* [Úprava obsahu statických stránek a elementů rozložení stránek][modify-content-layout]
* [Aktualizace stylů použitých pro elementy stránek napříč portálem pro vývojáře][customize-styles] (vysvětlení obsahuje tento průvodce)
* [Úprava šablon použitých pro stránky generované portálem][portal-templates] (dokumentace rozhraní API, produkty, ověřování uživatelů atd.)

## <a name="change-headers-styling"> </a>Změna stylu prvků na stránce

Barvy, písma, velikosti, mezery a další související prvky stylu libovolné stránky na portálu se definují pomocí pravidel stylu. 

K úpravě stylů se používá **portál pro vývojáře**, když jste přihlášení jako správce. Otevřete ho tak, že nejdřív otevřete Azure Portal a potom na panelu nástrojů instance služby API Management kliknete na **Portál pro vydavatele**.

![Portál vydavatele][api-management-management-console]

Potom vpravo nahoře klikněte na **Portál pro vývojáře**. 

![Odkaz na portál pro vývojáře na portálu pro vydavatele][api-management-pp-dp-link]

Pokud chcete otevřít panel nástrojů pro přizpůsobení, najeďte myší na ikonu přizpůsobení (nebo ji vyberte) a potom na panelu nástrojů klikněte na Styly.

![Tlačítko panelu nástrojů pro přizpůsobení][api-management-customization-toolbar-button]

Existují dva hlavní způsoby úpravy pravidel stylů: Můžete projít seznam všech použitých pravidel stylů, který se standardně zobrazí, a podle potřeby styl upravit, nebo můžete zvolit **Vybrat prvek na stránce** a pak kliknout na libovolné místo stránky a zobrazit jenom styly tohoto prvku.

![Přizpůsobení panelu nástrojů][api-management-customization-toolbar]

V tomto příkladu klikněte na možnost **Vybrat prvek na stránce**.  Když najedete myší na jednotlivé prvky, tyto prvky se zvýrazní, aby označily, jaké styly prvku byste začali upravovat, pokud byste myší klikli. Najeďte myší na text v záhlaví (obvykle je tam název společnosti) a klikněte na něj. V editoru stylů se zobrazí sada pojmenovaných a podle kategorií seřazených pravidel stylů. Každé pravidlo představuje vlastnost stylu vybraného prvku. V případě výše vybraného textu záhlaví je například velikost textu určená v parametru @font-size-h1 a název písma s alternativami je uvedený v parametru @headings-font-family.

> Pokud jste obeznámeni s architekturou [bootstrap][bootstrap], tato pravidla jsou ve skutečnosti [proměnné LESS][LESS variables] v motivu spuštění, který používá portál pro vývojáře.
> 
> 

Zkuste změnit barvu textu v nadpisu. Vyberte položku v poli **@headings-color** a zadejte **#000000**. To je šestnáctkový kód černé barvy. Když to uděláte, zobrazí se na konci textového pole čtvercový ukazatel barvy. Pokud na ukazatel kliknete, otevře se výběr barvy a umožní vám vybrat barvu.

![Výběr barvy][api-management-customization-toolbar-color-picker]

Změny se zobrazují v reálném čase, jak je provádíte, ale jsou viditelné pouze správcům. Pokud chcete tyto změny zviditelnit pro všechny uživatele, klikněte v editoru stylů na tlačítko **Publikovat** a potvrďte změny.

![Nabídka Publikovat][api-management-customization-toolbar-publish-form]

> Pokud chcete pravidla stylu použít na jakýkoli další prvek na stránce, postupujte stejným způsobem jako v případě záhlaví. V editoru stylů klikněte na **Vybrat prvek na stránce**, vyberte požadovaný prvek a začněte upravovat hodnoty pravidel stylu, které vidíte na obrazovce.
> 
> 


## <a name="next-steps"> </a>Další kroky
* Zjistěte, jak můžete přizpůsobit obsah stránek portálu pro vývojáře pomocí [šablon portálu pro vývojáře](api-management-developer-portal-templates.md).

[Change the styling of the headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/

