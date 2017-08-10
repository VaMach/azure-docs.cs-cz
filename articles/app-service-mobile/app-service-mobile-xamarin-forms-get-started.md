---
title: "Začínáme s Mobile Apps na platformě Xamarin.Forms"
description: "V tomto kurzu začnete používat Azure Mobile Apps pro vývoj s Xamarin.Forms."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: cb959867ccfc85993694bd810f08e2f8150b44f3
ms.contentlocale: cs-cz
ms.lasthandoff: 08/05/2017

---
# <a name="create-a-xamarinforms-app"></a>Vytvoření aplikace na platformě Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace na platformě Xamarin.Forms pomocí back-endu mobilní aplikace Azure. Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Xamarin.Forms, která bude představovat *seznam úkolů* a ukládat data do Azure.

Ve všech dalších kurzech k Mobile Apps týkajících se Xamarin.Forms se předpokládá dokončení tohoto kurzu.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio s Xamarinem. Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community. Přečtěte si témata o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a o [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-azure-mobile-app-backend"></a>Vytvoření nového back-endu mobilní aplikace Azure
Podle těchto pokynů vytvořte nový back-end mobilní aplikace:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. Dále si stáhnete serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.

## <a name="configure-the-server-project"></a>Konfigurace serverového projektu
Podle pokynů níže nakonfigurujte serverový projekt tak, aby používal buď back-end Node.js, nebo .NET.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Stáhnutí a spuštění řešení Xamarin.Forms
Zde máte několik možností. Řešení si můžete stáhnout na Mac a otevřít ho v nástroji Xamarin Studio nebo si ho můžete stáhnout do počítače s Windows a otevřít v nástroji Visual Studio pomocí síťově připojeného počítače Mac, abyste mohli sestavit aplikaci pro iOS. Pokud potřebujete podrobnější pokyny ke scénářům instalace nástroje Xamarin, nahlédněte do tématu o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

Můžeme pokračovat:

1. Na počítači Mac nebo počítači s Windows otevřete [Azure Portal] v okně prohlížeče.
2. V okně nastavení mobilní aplikace klikněte na **Začínáme** (v části pro mobilní řešení) > **Xamarin.Forms**. V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.  Pak klikněte na tlačítko **Stáhnout**.

   Tím se stáhne projekt, který obsahuje klientskou aplikaci připojenou k mobilní aplikaci. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.
3. Extrahujte projekt, který jste stáhli, a otevřete jej v nástroji Xamarin Studio nebo v nástroji Visual Studio.

   ![][9]

   ![][8]

## <a name="optional-run-the-ios-project"></a>(Nepovinné) Spuštění projektu pro iOS
Tato část se týká spuštění projektu Xamarin iOS pro zařízení s iOS. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio
1. Klikněte pravým tlačítkem myši na projekt pro iOS a pak klikněte na **Nastavit jako spouštěný projekt**.
2. V nabídce **Spustit** klikněte na **Spustit ladění**, aby se projekt sestavil a aplikace se spustila v emulátoru iPhonu.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio
1. Klikněte pravým tlačítkem myši na projekt pro iOS a pak klikněte na **Nastavit jako spouštěný projekt**.
2. V nabídce **Sestavení** (Build) klikněte na **Správce konfigurace**.
3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** u projektu pro iOS.
4. Stiskněte klávesu **F5**, aby se projekt sestavil a aplikace se spustila v emulátoru iPhonu.

   > [!NOTE]
   > Pokud máte se sestavením problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Někdy se u projektů typu Rychlý start může aktualizace na nejnovější verzi opozdit.    
   >
   >

Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a klikněte na tlačítko **+**.

![][10]

Tím se pošle požadavek POST na nový back-end mobilní aplikace hostovaný v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí back-endu mobilní aplikace a v seznamu se zobrazí data.

> [!NOTE]
> Kód, který získává přístup k back-endu mobilní aplikace, naleznete v souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
>
>

## <a name="optional-run-the-android-project"></a>(Nepovinné) Spuštění projektu pro Android
Tato část se týká spuštění projektu Xamarin Android pro zařízení s Androidem. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio
1. Klikněte pravým tlačítkem myši na projekt pro Android a pak klikněte na **Nastavit jako spouštěný projekt**.
2. V nabídce **Spustit** klikněte na **Spustit ladění**, aby se projekt sestavil a aplikace se spustila v emulátoru Androidu.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio
1. Klikněte pravým tlačítkem myši na projekt pro Android (Droid) a pak klikněte na **Nastavit jako spouštěný projekt**.
2. V nabídce **Sestavení** (Build) klikněte na **Správce konfigurace**.
3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** u projektu pro Android.
4. Stiskněte klávesu **F5**, aby se projekt sestavil a aplikace se spustila v emulátoru Androidu.

   > [!NOTE]
   > Pokud máte se sestavením problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Někdy se u projektů typu Rychlý start může aktualizace na nejnovější verzi opozdit.    
   >
   >

Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a klikněte na tlačítko **+**.

![][11]

Tím se pošle požadavek POST na nový back-end mobilní aplikace hostovaný v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí back-endu mobilní aplikace a v seznamu se zobrazí data.

> [!NOTE]
> Kód, který získává přístup k back-endu mobilní aplikace, naleznete v souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
>
>

## <a name="optional-run-the-windows-project"></a>(Nepovinné) Spuštění projektu pro Windows
Tato část se týká spuštění projektu Xamarin WinApp pro zařízení s Windows. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio
1. Klikněte pravým tlačítkem myši na kterýkoli z projektů pro Windows a pak klikněte na **Nastavit jako spouštěný projekt**.
2. V nabídce **Sestavení** (Build) klikněte na **Správce konfigurace**.
3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** u vámi zvoleného projektu pro Windows.
4. Stiskněte klávesu **F5**, aby se projekt sestavil a spustil aplikaci v emulátoru Windows.

   > [!NOTE]
   > Pokud máte se sestavením problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Někdy se u projektů typu Rychlý start může aktualizace na nejnovější verzi opozdit.    
   >
   >

Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a klikněte na tlačítko **+**.

Tím se pošle požadavek POST na nový back-end mobilní aplikace hostovaný v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí back-endu mobilní aplikace a v seznamu se zobrazí data.

![][12]

> [!NOTE]
> Kód, který získává přístup k back-endu mobilní aplikace, naleznete v souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
>
>

## <a name="next-steps"></a>Další kroky
* [Přidání ověřování do aplikace](app-service-mobile-xamarin-forms-get-started-users.md)  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Přidání nabízených oznámení do aplikace](app-service-mobile-xamarin-forms-get-started-push.md)  
  Naučte se přidávat do aplikace podporu nabízených oznámení a konfigurovat back-end mobilní aplikace tak, aby k zasílání nabízených oznámení používal Azure Notification Hubs.
* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Naučte se, jak pomocí back-endu mobilní aplikace přidat do aplikace podporu offline režimu. Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací &mdash; zobrazovat, přidávat a upravovat data &mdash; i v případě, že nemají připojení k síti.
* [Jak používat spravovaného klienta pro Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Naučte se pracovat se spravovanou klientskou sadou SDK v aplikaci Xamarin.

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/

