<properties
    pageTitle="Začínáme používat mobilní služby pro aplikace Windows Store v jazyce JavaScript | Azure Mobile Services"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj Windows Store v JavaScript."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="javascript"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="glenga"/>


# Začínáme používat mobilní služby

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace pro Windows pomocí mobilních aplikací](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).  

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do aplikace Windows Store jazyka JavaScript pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby. Vytvořená mobilní služba používá JavaScript pro serverovou obchodní logiku. 

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Visual Studio 2013 Express pro Windows]

## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace pro Windows Store

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace Windows Store 8.1 jazyka JavaScript, která se připojuje k mobilní službě.

1.  Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.


2. Na kartě Rychlé spuštění klikněte na tlačítko **Windows** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Windows Store**.

3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte nástroje [Visual Studio 2013][Visual Studio 2013 Express pro Windows] do svého místního počítače nebo virtuálního počítače.

4. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

5. V části **Stažení a spuštění aplikace** vyberte jazyk aplikace a pak klikněte na tlačítko **Stáhnout**.

    Tato akce stáhne projekt pro vzorovou aplikaci *Seznam úkolů*, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spuštění aplikace pro Windows

Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, rozbalte soubory do počítače a otevřete soubor řešení ve Visual Studiu.

2. Stisknutím klávesy **F5** projekt znovu sestavte a aplikaci spusťte.

3. V aplikaci zadejte smysluplný text, například *Dokončit kurz*, v části **Vložení úkolu** a klikněte na **Uložit**.

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v druhém sloupci aplikace se zobrazí data.

4. (Volitelné) Znovu spusťte aplikaci a všimněte si, že data uložená v předchozím kroku jsou načtena z mobilní služby po spuštění aplikace.
 
4. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku **TodoItems**.

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

>[AZURE.NOTE] Kód, který přistupuje k mobilní službě pro dotazování a vkládání dat a který se nachází v souboru default.js, můžete zkontrolovat.

## Další kroky
Teď, když jste dokončili rychlé spuštění se naučte pracovat s [klientem Mobile Services pro HTML/JavaScript](mobile-services-html-how-to-use-client-library.md). 

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Visual Studio 2013 Express pro Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[portál Azure Classic]: https://manage.windowsazure.com/



<!--HONumber=Sep16_HO3-->


