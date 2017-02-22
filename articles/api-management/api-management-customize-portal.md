---
title: "Přizpůsobení portálu pro vývojáře ve službě Azure API Management | Dokumentace Microsoftu"
description: "Naučte se přizpůsobovat portál pro vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 13431648e87d085161ad278dc991d49f7872be34
ms.openlocfilehash: 60213f885020a5ba36d6ada0812f755e06b3c48b


---
# <a name="customize-the-developer-portal-in-azure-api-management"></a>Přizpůsobení portálu pro vývojáře ve službě Azure API Management
Tento průvodce vám ukáže, jak upravit vzhled portálu pro vývojáře ve službě Azure API Management, aby byl v souladu s vaší značkou.

## <a name="change-page-headers"> </a>Změna textu nebo loga v záhlaví stránky
Jedním z klíčových aspektů přizpůsobení portálu je nahrazení textu v horní části všech stránek názvem a logem vaší společnosti.

Obsah portálu pro vývojáře se mění prostřednictvím portálu vydavatele, který je přístupný z webu Azure Portal. Otevřete jej tak, že na panelu nástrojů služby svojí instance služby API Management kliknete na **Portál vydavatele**.

![Portál vydavatele][api-management-management-console]

Portál pro vývojáře je postaven na systému CMS. Záhlaví, který se zobrazuje na každé stránce, je zvláštním typem obsahu, kterému se říká widget. Pokud chcete obsah takového widgetu upravit, klikněte v nabídce **Portál pro vývojáře** na levé straně na **Widgety** a potom v seznamu vyberte widget **Záhlaví**.

![Záhlaví widgetů][api-management-widgets-header]

Obsah záhlaví můžete upravovat v poli **Text**. Změňte text na „Portál pro vývojáře Fabrikam“ a potom v dolní části stránky klikněte na **Uložit**.

Nové záhlaví by se teď mělo zobrazovat na každé stránce portálu pro vývojáře.

> Pokud chcete portál pro vývojáře otevřít, když jste na portálu vydavatele, klikněte na horním panelu na **Portál pro vývojáře**.
> 
> 

## <a name="change-headers-styling"> </a>Změna stylu záhlaví
Barvy, písma, velikosti, mezery a další související prvky stylu libovolné stránky na portálu se definují pomocí pravidel stylu. Pokud chcete upravit styly během činnosti v **portálu pro vývojáře**, otevřete panel nástrojů pro přizpůsobení na levé straně přesunutím ukazatele myši nad ikonu přizpůsobení a vyberte z panelu nástrojů položku Styly.

![Tlačítko panelu nástrojů pro přizpůsobení][api-management-customization-toolbar-button]

Existují dva hlavní způsoby úpravy pravidel stylů: Můžete projít seznam všech použitých pravidel stylů, který se standardně zobrazí, a podle potřeby styl upravit, nebo můžete zvolit **Vybrat prvek na stránce** a pak kliknout na libovolné místo stránky a zobrazit jenom styly tohoto prvku.

V této části chceme změnit styl jenom záhlaví. Klikněte na možnost **Vybrat prvek na stránce** z panelu nástrojů editoru stylů. 

![Přizpůsobení panelu nástrojů][api-management-customization-toolbar]

Když najedete myší na jednotlivé prvky, tyto prvky se zvýrazní, aby označily, jaké styly prvku byste začali upravovat, pokud byste myší klikli. Najeďte myší na text, který představuje název společnosti v záhlaví („Portál pro vývojáře Fabrikam“, pokud jste postupovali podle pokynů v předchozí části), a klikněte na něj. V editoru stylů se zobrazí sada pojmenovaných a podle kategorií seřazených pravidel stylů. Každé pravidlo představuje vlastnost stylu vybraného prvku. V případě výše vybraného textu záhlaví je například velikost textu určená v parametru @font-size-h1 a název písma s alternativami je uvedený v parametru @headings-font-family.

> Pokud jste obeznámeni s architekturou [bootstrap][bootstrap], tato pravidla jsou ve skutečnosti [proměnné LESS][LESS variables] v motivu spuštění, který používá portál pro vývojáře.
> 
> 

Zkuste změnit barvu textu v nadpisu. Vyberte položku v poli **@headings-color** a zadejte **#000000**. To je šestnáctkový kód černé barvy. Když to uděláte, zobrazí se na konci textového pole čtvercový ukazatel barvy. Pokud na ukazatel kliknete, otevře se výběr barvy a umožní vám vybrat barvu.

![Výběr barvy][api-management-customization-toolbar-color-picker]

Změny se zobrazují v reálném čase, jak je provádíte, ale jsou viditelné pouze správcům. Pokud chcete tyto změny zviditelnit pro všechny uživatele, klikněte v editoru stylů na tlačítko **Publikovat** a potvrďte změny.

![Nabídka Publikovat][api-management-customization-toolbar-publish-form]

> Pokud chcete pravidla stylu použít na jakýkoli další prvek na stránce, postupujte stejným způsobem jako v případě záhlaví. V editoru stylů klikněte na **Vybrat prvek**, vyberte požadovaný prvek a začněte upravovat hodnoty pravidel stylu, které vidíte na obrazovce.
> 
> 

## <a name="edit-page-contents"> </a>Úprava obsahu stránky
Portál pro vývojáře se skládá z automaticky vytvářených stránek, například stránek rozhraní API, produktů, aplikací, problémů a ručně psaného obsahu. Protože je postaven na systému CMS, můžete takový obsah vytvářet podle potřeby.

Pokud chcete zobrazit seznam všech existujících stránek s obsahem, klikněte v nabídce **Portál pro vývojáře** (na portálu vydavatele) na **Obsah**.

![Správa obsahu][api-management-customization-manage-content]

Pokud chcete upravit obsah zobrazený na domovské stránce portálu pro vývojáře, klikněte na stránku **Vítejte**. Proveďte požadované změny, v případě potřeby si zobrazte jejich náhled a potom je zpřístupněte všem uživatelům kliknutím na **Publikovat**.

> Domovská stránka používá zvláštní rozložení, které umožňuje zobrazit banner v její horní části. Tento banner nemůžete v části **Obsah** upravovat. Pokud chcete banner upravit, klikněte v nabídce **Portál pro vývojáře** na **Widgety**, v rozevíracím seznamu **Aktuální vrstva** vyberte **Domovská stránka** a potom v části **Vybraná část** otevřete položku **Banner**. Obsah tohoto widgetu můžete upravovat stejně jako jakoukoli jinou stránku.
> 
> 

## <a name="next-steps"> </a>Další kroky
* Zjistěte, jak můžete přizpůsobit obsah stránek portálu pro vývojáře pomocí [šablon portálu pro vývojáře](api-management-developer-portal-templates.md).

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-portal/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/



<!--HONumber=Feb17_HO1-->


