<properties
    pageTitle="Začínáme s Android Apps Azure Mobile Engagementem"
    description="Naučte se používat Azure Mobile Engagement s analýzou a nabízenými oznámeními pro aplikace pro Android."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# Začínáme s Azure Mobile Engagementem pro aplikace pro Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikací a odesílat nabízená oznámení segmentovaným uživatelům aplikace pro Android.
Tento kurz představuje scénář jednoduchého vysílání přes Mobile Engagement. V něm můžete vytvořit prázdnou aplikaci pro Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).

## Požadavky

K dokončení tohoto kurzu budete potřebovat [vývojářské nástroje pro Android](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio a nejnovější platformu Android.

Také budete potřebovat sadu [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## Nastavení Mobile Engagementu pro vaši aplikaci pro Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

## Připojení aplikace k back-endu Mobile Engagementu

V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení. Pomocí Android Studia vytvoříte základní aplikaci, na které si tuto integraci předvedeme.

Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement Android SDK](mobile-engagement-android-sdk-overview.md).

### Vytvoření projektu Android

1. Spusťte **Android Studio** a v místní nabídce vyberte **Start a new Android Studio project** (Začít nový projekt Android Studio).

    ![][1]

2. Zadejte název aplikace a doménu firmy. Poznamenejte si, co vyplňujete, protože budete tyto údaje potřebovat později. Klikněte na **Další**.

    ![][2]

3. Zvolte cílový typ zařízení a úroveň API a klikněte na**Next**.

    >[AZURE.NOTE] Mobile Engagement vyžaduje API minimálně úrovně 10 (Android 2.3.3).

    ![][3]

4. Zde vyberte **Blank Activity** (Prázdná aktivita), která je jedinou obrazovkou pro tuto aplikaci, a klikněte na **Next**.

    ![][4]

5. Na závěr ponechte výchozí nastavení a klikněte na**Finish** (Dokončit).

    ![][5]

Android Studio nyní vytvoří ukázkovou aplikaci, do které integrujeme Mobile Engagement.

### Zahrnutí knihovny sady SDK do projektu

1. Stáhněte sadu [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).
2. Extrahujte soubor archivu do složky v počítači.
3. Identifikujte knihovnu .jar pro aktuální verzi této sady SDK a zkopírujte ji do schránky.

      ![][6]

4. Přejděte do sekce **Project** (Projekt) (1) a vložte .sloubor jar do složky libs (2).

      ![][7]

5. Pro načtení knihovny synchronizujte projekt.

      ![][8]

### Připojení aplikace k back-endu Mobile Engagementu pomocí připojovacího řetězce

1. Zkopírujte následující řádky kódu do vytváření aktivity (je třeba provést pouze na jednom místě aplikace, obvykle v hlavní aktivitě). V případě této ukázkové aplikace otevřete MainActivity pod složkou src -> main -> java a přidejte následující:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Vyřešte reference stisknutím Alt+Enter nebo přidáním následujících příkazů pro import:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Přejděte zpět na Azure Classic Portal na stránce **Connection Info** (Informace o připojení) vaší aplikace a zkopírujte obsah pole **Connection String** (Připojovací řetězec).

      ![][9]

4. Vložte jej do parametru `setConnectionString` a nahraďte celý zobrazený řetězec následujícím kódem:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### Přidání oprávnění a deklarace služby

1. Přidejte oprávnění do souboru Manifest.xml vašeho projektu těsně před značku `<application>` nebo za ni.

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Chcete-li deklarovat službu agenta, přidejte tento kód mezi značky `<application>` a `</application>`:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Ve vloženém kódu nahraďte `"<Your application name>"` v popisku, který se zobrazí v nabídce **Nastavení**, kde můžete zobrazit služeb spuštěné v zařízení. Do tohoto popisku můžete například přidat slovo „Service“.

### Odeslání obrazovky do Mobile Engagementu

Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.

Přejděte na **MainActivity.java** a přidejte následující kód, kterým nahradíte základní třídu **MainActivity** třídou **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Pokud vaše základní třída není *Activity*, podívejte se do [Rozšířených možností hlášení pro Android](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) na postupy, jak dědit z různých tříd.


Okomentujte následující řádek pro tento jednoduchý vzorový scénář:

    // setSupportActionBar(toolbar);

Pokud chcete zachovat `ActionBar` ve vaší aplikaci, zobrazte si část [Rozšířená sestava Android](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## Připojení aplikace se sledováním v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## Povolení nabízených oznámení a zasílání zpráv v aplikaci

Během kampaně vám Mobile Engagement umožňuje interagovat a KOMUNIKOVAT s uživateli pomocí nabízených oznámení a zpráv v aplikaci. Tento modul se na portálu Mobile Engagement nazývá REACH.
V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.

### Kopírování prostředků sady SDK do projektu

1. Přejděte zpět na obsah stažené sady SDK a zkopírujte složku **res**.

    ![][10]

2. Přejděte zpět na Android Studio, vyberte adresář **main** projektu a vložte do něj zkopírovanou složku res, abyste prostředky přidali do projektu.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## Další kroky

Podrobné informace o integraci sady SDK najdete v článku o integraci sady [Android SDK](mobile-engagement-android-sdk-overview.md).

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png



<!---HONumber=Aug16_HO4-->


