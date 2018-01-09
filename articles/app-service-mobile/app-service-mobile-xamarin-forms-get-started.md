---
title: "Začínáme s funkcí Mobile Apps na platformě Xamarin.Forms"
description: "V tomto kurzu začnete používat funkci Mobile Apps pro vývoj s Xamarin.Forms."
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
ms.openlocfilehash: 12c7eb78b5049b385ee34c7ac8e3574b064d7ecf
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-xamarinforms-app"></a>Vytvoření aplikace na platformě Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
V tomto kurzu se dozvíte, jak přidat cloudovou službu back-end do mobilní aplikace na platformě Xamarin.Forms pomocí funkce Mobile Apps služby Azure App Service jako back-endu. Vytvoříte jak nový back-end Mobile Apps, tak jednoduchou aplikaci Xamarin.Forms, která bude představovat seznam úkolů a ukládat data do Azure.

Ve všech dalších kurzech k Mobile Apps týkajících se Xamarin.Forms se předpokládá dokončení tohoto kurzu.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období. Další informace najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarinem. Informace najdete na stránce o [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).

* Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community. Informace najdete v článcích věnovaných [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx) a [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Vytvoření nového back-endu Mobile Apps

Pokud chcete vytvořit nový back-end Mobile Apps, postupujte následovně:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Právě jste nastavili back-end Mobile Apps, který můžou používat vaše mobilní klientské aplikace. Dále si stáhnete serverový projekt pro jednoduchý back-end seznamu úkolů a pak ho publikujete v Azure.

## <a name="configure-the-server-project"></a>Konfigurace serverového projektu

Pokud chcete nakonfigurovat serverový projekt tak, aby používal buď back-end v Node.js, nebo v .NET, postupujte následovně:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Stáhnutí a spuštění řešení Xamarin.Forms

Řešení můžete stáhnout dvěma způsoby. Můžete si ho stáhnout na Mac a otevřít ho v nástroji Xamarin Studio nebo si ho můžete stáhnout na počítač s Windows a otevřít v sadě Visual Studio pomocí síťově připojeného počítače Mac, abyste mohli sestavit aplikaci pro iOS. Další informace najdete v článku věnovaném [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).

Na počítači Mac nebo Windows postupujte takto:

1. Přejděte na [portál Azure].

2. V okně nastavení mobilní aplikace klikněte na **Rychlý start** (v části Nasazení) > **Xamarin.Forms**. V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.  Pak klikněte na tlačítko **Stáhnout**.

   Touto akcí se stáhne projekt, který obsahuje klientskou aplikaci připojenou k mobilní aplikaci. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

3. Extrahujte projekt, který jste stáhli, a otevřete ho v nástroji Xamarin Studio (Mac) nebo v sadě Visual Studio (Windows).

   ![Extrahovaný projekt v nástroji Xamarin Studio][9]

   ![Extrahovaný projekt v sadě Visual Studio][8]

## <a name="optional-run-the-ios-project"></a>(Nepovinné) Spuštění projektu pro iOS
V této části spustíte projekt Xamarin iOS pro zařízení s iOS. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio
1. Klikněte pravým tlačítkem na projekt pro iOS a pak vyberte **Nastavit jako spouštěný projekt**.

2. V nabídce **Spustit** vyberte **Spustit ladění**, aby se projekt sestavil a aplikace se spustila v emulátoru iPhonu.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio
1. Klikněte pravým tlačítkem na projekt pro iOS a pak vyberte **Nastavit jako spouštěný projekt**.

2. V nabídce **Sestavení** (Build) vyberte **Správce konfigurace**.

3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** vedle projektu pro iOS.

4. Pokud chcete projekt sestavit a spustit aplikaci v emulátoru iPhonu, stiskněte klávesu **F5**.

   > [!NOTE]
   > Pokud máte se sestavením projektu problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Aktualizace projektů Rychlý start na nejnovější verze může být pomalá.    
   >
   >

5. Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a pak vyberte symbol plus (**+**).

    ![][10]

    Touto akcí se odešle požadavek POST na nový back-end Mobile Apps, jehož hostitelem je Azure. Data z požadavku se vloží do tabulky TodoItem. Back-end Mobile Apps vrátí položky uložené v tabulce a data se zobrazí v seznamu.

    > [!NOTE]
    > Kód, který získává přístup k back-endu Mobile Apps, najdete v souboru jazyka C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
    >
    >

## <a name="optional-run-the-android-project"></a>(Nepovinné) Spuštění projektu pro Android
V této části spustíte projekt Xamarin Android pro zařízení s Androidem. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio

1. Klikněte pravým tlačítkem na projekt pro Android a pak vyberte **Nastavit jako spouštěný projekt**.

2. V nabídce **Spustit** vyberte **Spustit ladění**, aby se projekt sestavil a aplikace se spustila v emulátoru Androidu.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio

1. Klikněte pravým tlačítkem na projekt pro Android (Droid) a pak vyberte **Nastavit jako spouštěný projekt**.

2. V nabídce **Sestavení** (Build) vyberte **Správce konfigurace**.

3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** vedle projektu pro Android.

4. Pokud chcete projekt sestavit a spustit aplikaci v emulátoru Androidu, stiskněte klávesu **F5**.

   > [!NOTE]
   > Pokud máte se sestavením projektu problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Aktualizace projektů Rychlý start na nejnovější verze může být pomalá.    
   >
   >

5. Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a pak vyberte symbol plus (**+**).

    ![][11]
    
    Touto akcí se odešle požadavek POST na nový back-end Mobile Apps, jehož hostitelem je Azure. Data z požadavku se vloží do tabulky TodoItem. Back-end Mobile Apps vrátí položky uložené v tabulce a data se zobrazí v seznamu.
    
    > [!NOTE]
    > Kód, který získává přístup k back-endu Mobile Apps, najdete v souboru jazyka C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
    >
    >

## <a name="optional-run-the-windows-project"></a>(Nepovinné) Spuštění projektu pro Windows

V této části spustíte projekt Xamarin WinApp pro zařízení s Windows. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio

1. Klikněte pravým tlačítkem na kterýkoli z projektů pro Windows a pak vyberte **Nastavit jako spouštěný projekt**.

2. V nabídce **Sestavení** (Build) vyberte **Správce konfigurace**.

3. V dialogovém okně **Správce konfigurace** zaškrtněte políčka **Sestavení** (Build) a **Nasazení** vedle vámi zvoleného projektu pro Windows.

4. Pokud chcete projekt sestavit a spustit aplikaci v emulátoru Windows, stiskněte klávesu **F5**.

   > [!NOTE]
   > Pokud máte se sestavením projektu problémy, spusťte správce balíčků NuGet a proveďte aktualizaci na nejnovější verzi podpůrných balíčků Xamarin. Aktualizace projektů Rychlý start na nejnovější verze může být pomalá.    
   >
   >

5. Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a pak vyberte symbol plus (**+**).

    Touto akcí se odešle požadavek POST na nový back-end Mobile Apps, jehož hostitelem je Azure. Data z požadavku se vloží do tabulky TodoItem. Back-end Mobile Apps vrátí položky uložené v tabulce a data se zobrazí v seznamu.
    
    ![][12]
    
    > [!NOTE]
    > Kód, který získává přístup k back-endu Mobile Apps, najdete v souboru jazyka C# TodoItemManager.cs v projektu knihovny přenosných tříd pro vaše řešení.
    >
    >

## <a name="next-steps"></a>Další kroky

* [Přidání ověřování do aplikace](app-service-mobile-xamarin-forms-get-started-users.md)  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidání nabízených oznámení do aplikace](app-service-mobile-xamarin-forms-get-started-push.md)  
  Zjistěte, jak do aplikace přidat podporu nabízených oznámení a jak nakonfigurovat back-end Mobile Apps tak, aby k zasílání nabízených oznámení používal službu Azure Notification Hubs.

* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Zjistěte, jak pomocí back-endu Mobile Apps přidat do aplikace podporu offline režimu. Offline synchronizace umožňuje zobrazovat, přidávat nebo upravovat data mobilní aplikace i když nemá připojení k síti.

* [Použití spravovaného klienta pro funkci Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Naučte se pracovat se spravovanou klientskou sadou SDK v aplikaci Xamarin.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


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
[portál Azure]: https://portal.azure.com/
