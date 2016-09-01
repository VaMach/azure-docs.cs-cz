<properties
    pageTitle="Vytvoření Univerzální platformy Windows (UPW) využívající Mobile Apps | Microsoft Azure"
    description="V tomto kurzu začnete pracovat s back-endy mobilní aplikace Azure při vývoji aplikací pro Univerzální platformu Windows (UPW) v jazycích C#, Visual Basic a JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="ggailey777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="glenga"/>

#Vytvoření aplikace pro Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Přehled

V tomto kurzu se dozvíte, jak do aplikace pro Univerzální platformu Windows (UPW) přidat cloudovou back-end službu. Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md). Zde jsou snímky obrazovek z hotové aplikace:

![Hotová desktopová aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Spuštěná na ploše 

![Hotová telefonní aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Spuštěná na telefonu

Ve všech dalších kurzech Mobile App pro aplikace UPW se předpokládá dokončení tohoto kurzu. 

##Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Community 2015] nebo novější verzi

>[AZURE.NOTE] Pokud chcete začít se službou Azure App Service, ještě než si zaregistrujete účet Azure, přejděte k [možnosti vyzkoušet si tuto službu](https://tryappservice.azure.com/?appServiceName=mobile). Tam si můžete v App Service hned vytvořit krátkodobou úvodní mobilní aplikaci – nepožaduje se žádná platební karta ani to s sebou nenese žádné závazky.

##Vytvoření nového back-endu mobilní aplikace Azure

Podle těchto pokynů vytvořte nový back-end mobilní aplikace:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. Dále si stáhnete serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.

## Konfigurace serverového projektu

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##Stažení a spuštění klientského projektu

Až nakonfigurujete svůj back-end mobilní aplikace, je možné buď vytvořit novou klientskou aplikaci, nebo upravit existující aplikaci tak, aby se připojovala k Azure. V této části si stáhnete projekt šablony aplikace UPW přizpůsobený tak, aby se připojoval k vašemu back-endu mobilní aplikace.

1. Po návratu do okna **Rychlý start** back-endu mobilní aplikace klikněte na **Vytvořit novou aplikaci** > **Stáhnout** a extrahujte komprimované soubory projektu do místního počítače.

    ![Stáhnutí projektu typu rychlý start pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Nepovinné) Přidejte projekt aplikace UPW do stejného řešení, v jakém se nachází serverový projekt. V tomtéž řešení nástroje Visual Studio bude snazší ladit a testovat jak aplikaci, tak back-end. Aby bylo možné projekt aplikace UPW do řešení přidat, je nutné používat Visual Studio 2015 nebo jeho novější verzi.

4. Nastavte aplikaci UPW jako spouštěný projekt, stiskněte klávesu F5, čímž aplikaci nasadíte a spustíte.

5. V aplikaci zadejte do textového pole pro **vložení úkolu** smysluplný text, například *Dokončit kurz*, a klikněte na **Uložit**.

    ![Dokončení desktopového projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Tím se odešle požadavek POST na nový back-end mobilní aplikace, jehož hostitelem je Azure.

6. (Volitelné) Ukončete aplikaci a restartujte ji na jiném zařízení nebo emulátoru mobilního zařízení.

    ![Dokončení telefonního projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Všimněte si, že po spuštění aplikace UPW se data uložená v předchozím kroku načetla z Azure. 

##Další kroky

* [Přidání ověřování do aplikace](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidání nabízených oznámení do aplikace](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Naučte se přidávat do aplikace podporu nabízených oznámení a konfigurovat back-end mobilní aplikace tak, aby k zasílání nabízených oznámení používal Azure Notification Hubs.

* [Povolení offline synchronizace u aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Naučte se, jak pomocí back-endu mobilní aplikace přidat do aplikace podporu offline režimu. Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací &mdash; zobrazovat, přidávat a upravovat data &mdash; i v případě, že nemají připojení k síti.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Sada SDK pro mobilní aplikace]: http://go.microsoft.com/fwlink/?LinkId=257545
[portál Azure]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!---HONumber=Aug16_HO4-->


