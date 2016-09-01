<properties
    pageTitle="Přizpůsobení portálu pro vývojáře ve službě Azure API Management | Microsoft Azure"
    description="Naučte se přizpůsobovat portál pro vývojáře ve službě Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/09/2016"
    ms.author="sdanie"/>

# Přizpůsobení portálu pro vývojáře ve službě Azure API Management

Tento průvodce vám ukáže, jak upravit vzhled portálu pro vývojáře ve službě Azure API Management, aby byl v souladu s vaší značkou.

## <a name="change-page-headers"> </a>Změna textu nebo loga v záhlaví stránky

Jedním z klíčových aspektů přizpůsobení portálu je nahrazení textu v horní části všech stránek názvem a logem vaší společnosti.

Obsah portálu pro vývojáře se mění prostřednictvím portálu vydavatele, na který můžete přistoupit pomocí portálu Azure Classic. Pokud se chcete dostat na rozhraní API portálu vydavatele, klikněte na portálu Azure Classic služby API Management na **Spravovat**.

![Portál vydavatele][api-management-management-console]

Portál pro vývojáře je postaven na systému CMS. Záhlaví, který se zobrazuje na každé stránce, je zvláštním typem obsahu, kterému se říká widget. Pokud chcete obsah takového widgetu upravit, klikněte v nabídce **Portál pro vývojáře** na levé straně na **Widgety** a potom v seznamu vyberte widget **Záhlaví**.

![Záhlaví widgetů][api-management-widgets-header]

Obsah záhlaví můžete upravovat v poli **Text**. Změňte text na „Portál pro vývojáře Fabrikam“ a potom v dolní části stránky klikněte na **Uložit**.

Nové záhlaví by se teď mělo zobrazovat na každé stránce portálu pro vývojáře.

> Pokud chcete portál pro vývojáře otevřít, když jste na portálu vydavatele, klikněte na horním panelu na **Portál pro vývojáře**.

## <a name="change-headers-styling"> </a>Změna stylu záhlaví

Barvy, písma, velikosti, mezery a další související prvky stylu libovolné stránky na portálu se definují pomocí pravidel stylu. Pokud chcete styly upravit, klikněte v nabídce **Portál pro vývojáře** (na portálu vydavatele) na **Vzhled**, potom klikněte na **Zahájit přizpůsobení** a povolte editor stylů.

Váš prohlížeč přepne na skrytou stránku v rámci portálu pro vývojáře, která obsahuje ukázkový obsah s příklady všech pravidel stylů používaných kdekoli na webu. Pokud chcete otevřít editor stylů, přesuňte kurzor na tenkou šedou čáru, která je umístěná svisle podél levého okraje stránky. Měl by se zobrazit panel nástrojů editoru.

![Přizpůsobení panelu nástrojů][api-management-customization-toolbar]

Existují dva hlavní režimy úprav pravidel stylů – **Upravit všechna pravidla** zobrazí seznam všech pravidel stylů použitých na libovolném místě a **Vybrat prvek** vám umožní vybrat prvek na aktuální stránce a zobrazí styly jenom tohoto prvku.

V této části chceme změnit styl jenom záhlaví. Na panelu nástrojů editoru stylů klikněte na možnost **Vybrat prvek** a potom klikněte na **Vybrat prvek k přizpůsobení**. Když najedete myší na jednotlivé prvky, tyto prvky se zvýrazní, aby označily, jaké styly prvku byste začali upravovat, pokud byste myší klikli. Najeďte myší na text, který představuje název společnosti v záhlaví („Portál pro vývojáře Fabrikam“, pokud jste postupovali podle pokynů v předchozí části), a klikněte na něj. V editoru stylů se zobrazí sada pojmenovaných a podle kategorií seřazených pravidel stylů.

Každé pravidlo představuje vlastnost stylu vybraného prvku. V případě výše vybraného textu záhlaví je například velikost textu určená v parametru @font-size-h1 a název písma s alternativami je uveden v parametru @headings-font-family.

> Pokud jste obeznámeni s architekturou [bootstrap][], tato pravidla jsou ve skutečnosti [proměnné LESS][] v motivu spuštění, který používá portál pro vývojáře.

Zkuste změnit barvu textu v nadpisu. Vyberte položku v poli **@headings-color** a zadejte **#000000**. To je šestnáctkový kód černé barvy. Když to uděláte, zobrazí se na konci textového pole čtvercový ukazatel barvy. Pokud na ukazatel kliknete, otevře se výběr barvy a umožní vám vybrat barvu.

![Výběr barvy][api-management-customization-toolbar-color-picker]

Po dokončení změn stylů vybraného prvku klikněte na **Náhled změn** a zobrazte si výsledky na obrazovce. V tuto chvíli je vidí jenom správci. Pokud chcete tyto změny zviditelnit pro všechny uživatele, klikněte v editoru stylů na tlačítko **Publikovat** a potvrďte změny.

![Nabídka Publikovat][api-management-customization-toolbar-publish-form]

> Pokud chcete pravidla stylu použít na jakýkoli další prvek na stránce, postupujte stejným způsobem jako v případě záhlaví. V editoru stylů klikněte na **Vybrat prvek**, vyberte požadovaný prvek a začněte upravovat hodnoty pravidel stylu, které vidíte na obrazovce.

## <a name="edit-page-contents"> </a>Úprava obsahu stránky

Portál pro vývojáře se skládá z automaticky vytvářených stránek, například stránek rozhraní API, produktů, aplikací, problémů a ručně psaného obsahu. Protože je postaven na systému CMS, můžete takový obsah vytvářet podle potřeby.

Pokud chcete zobrazit seznam všech existujících stránek s obsahem, klikněte v nabídce **Portál pro vývojáře** (na portálu vydavatele) na **Obsah**.

![Správa obsahu][api-management-customization-manage-content]

Pokud chcete upravit obsah zobrazený na domovské stránce portálu pro vývojáře, klikněte na stránku **Vítejte**. Proveďte požadované změny, v případě potřeby si zobrazte jejich náhled a potom je zpřístupněte všem uživatelům kliknutím na **Publikovat**.

> Domovská stránka používá zvláštní rozložení, které umožňuje zobrazit banner v její horní části. Tento banner nemůžete v části **Obsah** upravovat. Pokud chcete banner upravit, klikněte v nabídce **Portál pro vývojáře** na **Widgety**, v rozevíracím seznamu **Aktuální vrstva** vyberte **Domovská stránka** a potom v části **Vybraná část** otevřete položku **Banner**. Obsah tohoto widgetu můžete upravovat stejně jako jakoukoli jinou stránku.

## <a name="next-steps"> </a>Další kroky

-   Podívejte se i na další témata kurzu [Začínáme s pokročilou konfigurací rozhraní API][].

[Změna textu nebo loga v záhlaví stránek]: #change-page-headers
[Změna stylu záhlaví]: #change-headers-styling
[Úprava obsahu stránky]: #edit-page-contents
[Další kroky]: #next-steps

[Portál Azure Classic]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[Začínáme s pokročilou konfigurací rozhraní API]: api-management-get-started-advanced.md
[bootstrap]: http://getbootstrap.com/
[proměnné LESS]: http://getbootstrap.com/css/



<!---HONumber=Aug16_HO4-->


