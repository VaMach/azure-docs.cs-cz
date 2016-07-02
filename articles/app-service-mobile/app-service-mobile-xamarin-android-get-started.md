<properties
    pageTitle="Začínáme s Azure Mobile Apps u aplikací na platformě Xamarin.Android"
    description="V tomto kurzu začnete používat Azure Mobile Apps pro vývoj na platformě Xamarin.Android."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="glenga" />

#Vytvoření aplikace Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Přehled

V tomto kurzu se dozvíte, jak do aplikace Xamarin.Android přidat cloudovou back-end službu. Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md).

Zde je snímek obrazovky dokončené aplikace:

![][0]

Ve všech dalších kurzech k používání funkce Mobile Apps pro Xamarin.Android se předpokládá dokončení tohoto kurzu.

##Předpoklady

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarinem. Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).  
 
>[AZURE.NOTE] Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://tryappservice.azure.com/?appServiceName=mobile), kde si můžete hned vytvořit krátkodobou úvodní mobilní aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.

## Vytvoření nového back-endu mobilní aplikace Azure

Podle těchto pokynů vytvořte nový back-end mobilní aplikace:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. Dále si stáhnete serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.

## Konfigurace serverového projektu

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Stáhnutí a spuštění aplikace Xamarin.Android

1. V části **Stáhnout a spustit projekt Xamarin.Android** klikněte na tlačítko **Stáhnout**.

    Tím se stáhne projekt, který obsahuje klientskou aplikaci připojenou k mobilní aplikaci. Uložte komprimovaný soubor projektu na místní počítač a poznamenejte si, kam jste jej uložili.

2. Stiskněte klávesu **F5**, aby se projekt sestavil a spustil aplikaci.

3. V aplikaci zadejte smysluplný text, například _Dokončit kurz_, a klikněte na tlačítko **Přidat**.

    ![][10]

    Tím se pošle požadavek POST na nový back-end mobilní aplikace hostovaný v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce budou vráceny back-endu mobilní aplikace a v seznamu se objeví data.

    > [AZURE.NOTE] Na kód, který přistupuje k back-endu mobilní aplikace pro dotazování a vkládání dat, se můžete podívat v souboru C# ToDoActivity.cs.

##Další kroky

* [Přidání ověřování do aplikace ](app-service-mobile-xamarin-android-get-started-users.md)  
Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Přidání nabízených oznámení do aplikace Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)  
Naučte se přidávat nabízená oznámení do aplikace.
* [Jak používat spravovaného klienta pro Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Naučte se pracovat se spravovanou klientskou sadou SDK v aplikaci Xamarin. 


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Portál Azure]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!--HONumber=Jun16_HO2-->


