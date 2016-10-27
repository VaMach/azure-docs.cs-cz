
###<a name="update-manifest-file-to-enable-notifications"></a>Aktualizace souboru manifest pro povolení oznámení

Zkopírujte níže uvedené prostředky zasílání zpráv v aplikaci do souboru Manifest.xml mezi značky `<application>` a `</application>`.

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

###<a name="specify-an-icon-for-notifications"></a>Určení ikony pro oznámení

Vložte následující fragment kódu XML do souboru Manifest.xml mezi značky `<application>` a `</application>`.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Tím se určuje ikona, která se bude zobrazovat v oznámeních v systému i aplikaci. Pro oznámení v aplikaci je ikona volitelná, ale pro systémová oznámení povinná. Android odmítá systémová oznámení s neplatnými ikonami.

Ujistěte se, že použijete ikonu, která existuje v některé ze složek **drawable** (třeba ``engagement_close.png``). Není dostupná podpora složky **mipmap**.

>[AZURE.NOTE] Neměli byste používat ikonu **launcher**. Má jiné rozlišení a většinou se nachází ve složkách mipmap, které nejsou podporované.

Ve skutečných aplikacích můžete použít ikonu vhodnou pro oznámení podle [pokynů pro návrh Androidu](http://developer.android.com/design/patterns/notifications.html).

>[AZURE.TIP] Pokud chcete mít jistotu, že používáte správné rozlišení ikon, můžete se podívat na [tyto příklady](https://www.google.com/design/icons).
Přejděte dolů do části **Oznámení** a kliknutím na `PNGS` stáhněte sadu drawable ikony. Zjistíte, které složky drawable s jakým rozlišením se mají pro každou verzi ikony použít.

###<a name="enable-your-app-to-receive-gcm-push-notifications"></a>Povolení přijímání nabízených oznámení GCM v aplikaci

1. Až nahradíte **** získané z konzoly projektu Firebase, vložte následující položky do souboru Manifest.xml mezi značky `<application>` a `</application>`. Položka \n je úmyslná, proto se ujistěte, že se nachází na konci čísla projektu.

        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />

2. Vložte následující kód do souboru Manifest.xml mezi značky `<application>` a `</application>`. Nahraďte název balíčku <Your package name>.

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>

3. Přidejte poslední sadu oprávnění, která je zvýrazněná, před značku `<application>`. Nahraďte `<Your package name>` skutečným názvem balíčku aplikace.

        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />






<!--HONumber=Oct16_HO3-->


