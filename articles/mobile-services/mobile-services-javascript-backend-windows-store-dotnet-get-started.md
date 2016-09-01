<properties
    pageTitle="Začínáme používat Mobile Services pro aplikace Windows Store (C#) | Microsoft Azure"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj Windows Store v C#."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="get-started-article" 
    ms.date="07/21/2016"
    ms.author="glenga"/>

# <a name="getting-started"> </a>Začínáme používat mobilní služby

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace pro Windows](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do univerzální aplikace pro Windows pomocí Azure Mobile Services. Univerzální řešení aplikací pro Windows zahrnuje projekty pro aplikace Windows Store 8.1 a Windows Phone Store 8.1 a běžně sdílený projekt. Další informace naleznete v tématu [Sestavení univerzálních aplikací pro Windows, které se zaměřují na Windows a Windows Phone](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx).

V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby. Vytvořená mobilní služba používá JavaScript pro serverovou obchodní logiku. Vytvoření mobilní služby, který umožňuje zapisovat serverovou obchodní logiku v podporovaných jazycích .NET pomocí Visual Studia naleznete v části Back-end verze jazyka .NET v tomto tématu.

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Visual Studio 2013 Express pro Windows]

## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové univerzální aplikace pro Windows

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové univerzální aplikace pro Windows, nebo úpravě stávající aplikace Windows Store nebo Windows Phone pro připojení k mobilní službě.

V této části vytvoříte novou univerzální aplikaci pro Windows, která je připojena k mobilní službě.

1.  Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.


2. Na kartě Rychlé spuštění klikněte na tlačítko **Windows** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Windows Store**.

    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Windows Store připojené k mobilní službě.

    ![Postup rychlého spuštění Mobile Services](./media/mobile-services-javascript-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte nástroje [Visual Studio 2013 Express pro Windows] do svého místního počítače nebo virtuálního počítače.

4. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

5. V části **Stažení a spuštění aplikace** vyberte jazyk aplikace a pak klikněte na tlačítko **Stáhnout**.

    Tato akce stáhne projekt pro vzorovou aplikaci *Seznam úkolů*, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spuštění aplikace pro Windows

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]

>[AZURE.NOTE]Kód, který přistupuje k mobilní službě pro dotazování a vkládání dat a který se nachází v souboru MainPage.xaml.cs, můžete zkontrolovat.

## Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Začínáme s offline synchronizací dat] Zjistěte, jak můžete používat synchronizaci dat offline k vytvoření fungující a robustní aplikace.

* [Přidat ověřování do aplikace pro Mobile Services ][Začínáme s ověřováním]  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidat nabízená oznámení do aplikace][Začínáme s nabízenými oznámeními]  
  Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

* [Používání klientské knihovny .NET](mobile-services-dotnet-how-to-use-client-library.md)  
 Zjistěte, jak odesílat dotazy na mobilní službu, pracovat s daty a přistupovat k vlastnímu rozhraní API.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->



<!-- URLs. -->
[Začínáme s offline synchronizací dat]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Začínáme s ověřováním]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md
[Začínáme s nabízenými oznámeními]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md
[Visual Studio 2013 Express pro Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[portál Azure Classic]: https://manage.windowsazure.com/
 


<!--HONumber=Aug16_HO4-->


