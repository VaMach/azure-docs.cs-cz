<properties
    pageTitle="Začínáme používat Azure Mobile Services pro aplikace PhoneGap/cordova | Microsoft Azure"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj aplikací PhoneGap pro iOS, Android a Windows Phone."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="multiple"
    ms.topic="get-started-article" 
    ms.date="07/21/2016"
    ms.author="ggailey777"/>

# Začínáme používat mobilní služby

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci _Seznam úkolů_, která ukládá data aplikací v rámci nové mobilní služby.

Zde je snímek obrazovky dokončené aplikace:

![][3]

### Další požadavky

K dokončení tohoto kurzu budete potřebovat následující:

+ Nástroje PhoneGap (v3.2+ je nutná pro projekty Windows Phone 8).

+ Aktivní účet Microsoft Azure.

+ PhoneGap podporuje vývoj pro různé platformy. Navíc k samotnému nástroji PhoneGap musíte nainstalovat nástroje pro každou platformu, na kterou cílíte:

    - Windows Phone: Nainstalujte [Visual Studio 2012 Express pro Windows Phone](https://go.microsoft.com/fwLink/p/?LinkID=268374)
    - iOS: Nainstalujte [Xcode] (požadována verze 4.4+)
    - Android: Nainstalujte [Nástroje pro vývojáře pro Android][Android SDK]
        <br/>(Mobile Services SDK pro Android podporuje aplikace pro Android 2.2 nebo vyšší verze. Android 4.2 nebo novější je nutný ke spouštění aplikací s rychlým startem.)

## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace PhoneGap

V této části vytvoříte novou aplikaci PhoneGap, která je připojena k mobilní službě.

1.  Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlé spuštění klikněte na tlačítko **PhoneGap** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci PhoneGap**.

    ![][0]

    Zobrazí se tří jednoduché kroky k vytvoření aplikace PhoneGap připojené k mobilní službě.

    ![][1]

3. Pokud jste tak již neučinili, stáhněte a nainstalujte PhoneGap a alespoň jeden z vývojových nástrojů platformy (Windows Phone, iOS nebo Android).

4. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

5. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**.

    Tato akce stáhne projekt pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě společně s Mobile Services JavaScript SDK. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spuštění nové aplikace PhoneGap

Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1.  Přejděte do umístění, kam jste uložili komprimované soubory projektu a rozbalte soubory do počítače.

2.  Otevřete a spusťte projekt podle pokynů níže pro každou platformu.

    + **Windows Phone 8**

        1. Windows Phone 8: Otevřete soubor .sln ve složce **platforms\wp8** v aplikaci Visual Studio 2012 Express for Windows Phone.

        2. Stisknutím klávesy **F5** projekt znovu sestavte a aplikaci spusťte.

        ![][2]

    + **iOS**

        1. Otevřete projekt ve složce **platforms/ios** v Xcode.

        2. Stiskněte tlačítko **Spustit** pro sestavení projektu a spusťte aplikaci v emulátoru iPhonu, který je výchozí pro tento projekt.

        ![][3]

    + **Android**

        1. V Eclipse klikněte na tlačítko **Soubor**, pak **Import**, rozbalte položku **Android**, klikněte na tlačítko **Existující kód Android do pracovního prostoru** a pak klikněte na tlačítko **Další.**

        2. Klikněte na tlačítko **Procházet**, přejděte do umístění rozbalených souborů projektu, klikněte na tlačítko **OK**, ujistěte se, zda je zaškrtnuto políčko projektu TodoActivity a pak klikněte na tlačítko **Dokončit**. <p>Tento postup importuje soubory projektu do aktuálního pracovního prostoru.</p>

        3. Z nabídky **Spustit** klikněte na tlačítko **Spustit** a spusťte projekt v emulátoru systému Android.

            ![][4]

        >[AZURE.NOTE]Abyste mohli spustit projekt v Android emulátoru, musíte definovat minimálně jedno virtuální zařízení Android (AVD). Použijte správce AVD k vytvoření a správě těchto zařízení.


3. Po spuštění aplikace v jednom z výše uvedených emulátorů, zadejte text do textového pole a pak klikněte na tlačítko **Přidat**.

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky **TodoItem**. Položky uložené v tabulce se vrátí pomocí mobilní služby a v seznamu se zobrazí data.

    > [AZURE.IMPORTANT] Změny tohoto projektu platformy budou přepsány, pokud je hlavní projekt znovu sestaven pomocí nástrojů PhoneGap. Místo toho proveďte změny v kořenovém adresáři www projektu jak je uvedeno v následující části.

4. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku **TodoItem**.

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-tab.png)

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-browse.png)


## Proveďte aktualizace aplikací a znovu sestavte projekty pro každou platformu

1. Proveďte změny souborů kódu v adresáři ´www´, který je v tomto případě na adrese „todolist/www“.

2. Ověřte, zda jsou v systémové cestě přístupné všechny nástroje cílové platformy.

2. Otevřete příkazový řádek v kořenovém adresáři projektu a spusťte následující specifické příkazy platformy:

    + **Windows Phone**

        Následující příkaz spusťte z příkazového řádku aplikace Visual Studio Developer:

            phonegap local build wp8

    + **iOS**

        Otevřete terminál a spusťte následující příkaz:

            phonegap local build ios

    + **Android**

        Otevřete příkazový řádek nebo okno terminálu a spusťte následující příkaz.

            phonegap local build android

4. Každý projekt se otevře v příslušném vývojovém prostředí, jak je uvedeno v předchozí části.

>[AZURE.NOTE]Kód, který přistupuje k mobilní službě pro dotazování a vkládání dat a který se nachází v souboru js/index.js, můžete zkontrolovat.

## Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* **[Přidání ověřování do aplikace]**  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.  

* **[Přidání nabízených oznámení do aplikace](https://msdn.microsoft.com/magazine/dn879353.aspx)**  
  Naučte se registrovat a odesílat nabízená oznámení do aplikace.

* **[Reference konceptu postupu u Mobile Services HTML/JavaScript](mobile-services-html-how-to-use-client-library.md)**  
  Další informace o používání knihovny JavaScript klienta pro přístup k datům, volání vlastním rozhraním API a provádění ověřování.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Images. -->
[0]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot1.png
[1]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot2.png
[2]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-wp8.png
[3]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-ios.png
[4]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-android.png

<!-- URLs. -->
[Přidání ověřování do aplikace]: mobile-services-html-get-started-users.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[portál Azure Classic]: https://manage.windowsazure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio 2012 Express pro Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
 


<!---HONumber=Aug16_HO4-->


