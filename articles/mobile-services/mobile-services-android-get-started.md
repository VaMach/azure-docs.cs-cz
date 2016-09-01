<properties
    pageTitle="Začněte používat Azure Mobile Services pro aplikace Android (back-end JavaScript)"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj pro Android (back-end JavaScript)."
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="reikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="ricksal"/>

# Začněte používat Azure Mobile Services pro Android (back-end JavaScript)

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi mobilní aplikace tohoto tématu naleznete v části [Vytvoření aplikace pro Android v Azure Mobile Apps](../app-service-mobile/app-service-mobile-android-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Android pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci **seznam úkolů**, která ukládá data aplikací v rámci nové mobilní služby.

> [AZURE.VIDEO mobile-get-started-android]

Zde je snímek obrazovky dokončené aplikace:

![](./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png)

## Požadavky

K dokončení tohoto kurzu budete potřebovat [vývojářské nástroje pro Android](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio a nejnovější platformu Android. Je požadována verze Android 4.2 nebo novější.

Stažený projekt quickstart obsahuje Azure Mobile Services SDK pro Android.

> [AZURE.IMPORTANT] K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28).


## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace Android

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části vytvoříte novou aplikaci pro Android, která je připojena k mobilní službě.

1.  Na portálu Azure Classic klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlé spuštění klikněte na tlačítko **Android** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Android**.

    ![](./media/mobile-services-android-get-started/mobile-portal-quickstart-android1.png)

    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Android připojené k mobilní službě.

    ![](./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png)

3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte nástroje [Android Developer Tools](https://go.microsoft.com/fwLink/p/?LinkID=280125) do svého místního počítače nebo virtuálního počítače.

4. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.


5. Teď aplikaci stáhněte stisknutím tlačítka **Stáhnout**.

## Spusťte svoji aplikaci pro Android

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]


## <a name="next-steps"> </a>Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Začínáme s daty]
  <br/>Další informace o ukládání a dotazování dat pomocí Mobile Services.

* [Začínáme s ověřováním]
  <br/>Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Začínáme s nabízenými oznámeními]
  <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]


<!-- URLs. -->
[Začínáme (Eclipse)]: mobile-services-android-get-started-ec.md
[Začínáme s daty]: mobile-services-android-get-started-data.md
[Začínáme s ověřováním]: mobile-services-android-get-started-users.md
[Začínáme s nabízenými oznámeními]: mobile-services-javascript-backend-android-get-started-push.md
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533




<!---HONumber=Aug16_HO4-->


