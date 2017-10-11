---
title: "Centra oznámení nejnovější zprávy přes kurz - Android"
description: "Naučte se používat Azure Service Bus Notification Hubs k odesílání oznámení o aktuálních zprávách do zařízení se systémem Android."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 76ec01c874fceedab7d76b2ef58e4b45b5489f58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Používání centra oznámení k odesílání novinek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do aplikace pro Android. Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení. Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení. Protože značky jsou jednoduše řetězce, nemají být předem zřízená. Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Přidat výběru kategorie do aplikace
Prvním krokem je přidání prvky uživatelského rozhraní pro vaše stávající hlavní aktivitu, který uživateli umožňuje výběr kategorií k registraci. Kategorie, které uživatel jsou uloženy v zařízení. Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.

1. Otevřete soubor res/layout/activity_main.xml a nahraďte obsah s následujícími službami:
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. Otevřete soubor res/values/strings.xml a přidejte následující řádky:
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    Grafické rozložení main_activity.xml by měl nyní vypadat takto:
   
    ![][A1]
3. Teď vytvořte třídu **oznámení** ve stejném balíčku jako vaše **MainActivity** třídy.
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    Tato třída používá místní úložiště k ukládání kategorie příspěvků, který toto zařízení má přijmout. Obsahuje také metody pro registraci pro tyto kategorie.
4. Ve vaší **MainActivity** třída odebrat vaší privátní pole pro **NotificationHub** a **GoogleCloudMessaging**, a přidejte pole pro **oznámení**:
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. Potom v **onCreate** metoda, odeberte inicializace **rozbočovače** pole a **registerWithNotificationHubs** metoda. Pak přidejte následující řádky, které inicializovat instanci **oznámení** třídy. 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`a `HubListenConnectionString` by měl být již nastaven s `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.

    > [AZURE.NOTE] Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace. Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.


1. Pak přidejte následující importy a `subscribe` metodu ke zpracování na tlačítko přihlásit k odběru, klikněte na událost:
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení. Při změně kategorií, registrace se znovu vytvoří se nové kategorie.

Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.

> [!NOTE]
> Protože registrationId přiřazené pomocí zasílání zpráv cloudu Google (GCM) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení. V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace. Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.
> 
> 

1. Přidejte následující kód na konci **onCreate** metoda v **MainActivity** třídy:
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registrace pro tyto kategorie. 
2. Aktualizujte `onStart()` metodu `MainActivity` třídy následujícím způsobem:
   
    @Overridechráněné void onStart() {
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }
   
    Tím se aktualizuje v hlavní aktivitě na základě stavu dříve uloženou kategorií.

Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie. V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.

## <a name="sending-tagged-notifications"></a>Odesílání oznámení s příznakem
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat upozornění
1. V nástroji Android Studio sestavení aplikace a spusťte jej na emulátoru nebo zařízení.
   
    Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.
2. Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.
   
    Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení. Registrovaný kategorie jsou vráceny a zobrazeny v oznámení s informační zprávou.
3. Nové oznámení odesílat spuštěním aplikace konzoly .NET.  Alternativně můžete odeslat oznámení s příznakem šablony pomocí karty ladění centra oznámení v [portálu Azure Classic].
   
    Oznámení pro vybrané kategorie se zobrazí jako informační zprávy.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jsme zjistili, jak k vysílání novinek podle kategorie. Vezměte v úvahu dokončení jednu z následujících kurzů upozorňující na jiné pokročilé scénáře centra oznámení:

* [Použití centra oznámení k vysílání lokalizované novinek]
  
    Zjistěte, jak rozšířit aplikace nejnovější zprávy k povolení odesílání lokalizované upozornění.

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Použití centra oznámení k vysílání lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[portálu Azure Classic]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
