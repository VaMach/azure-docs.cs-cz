---
title: "Začínáme se službou Azure AD Android | Microsoft Docs"
description: "Jak vytvářet aplikace platformy Android, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný pomocí OAuth2.0 rozhraní API."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 619334b3ca65654fd845a62c2fc068156d94d6fc
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="azure-ad-android-getting-started"></a>Začínáme se službou Azure AD Android
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Pokud vyvíjíte aplikace pracovní plochy, Azure Active Directory (Azure AD) je jednoduchá a přímočará pro vás k ověřování uživatelů pomocí jejich místních účtů služby Active Directory. Taky umožňuje vaší aplikaci bezpečně využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.

Pro Android klientů, kteří potřebují přístup k chráněným prostředkům Azure AD poskytuje službě Active Directory Authentication Library (ADAL). Jediný účel ADAL je snadno pro aplikaci, kterou chcete získat přístupové tokeny. K předvedení, jak je snadné, jsme budete sestavit Android seznam úkolů aplikaci, která:

* Získá přístup k tokeny pro volání rozhraní API seznamu úkolů pomocí [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Získá seznam úkolů uživatele.
* Provede odhlášení uživatele.

Abyste mohli začít, musíte klienta služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci. Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Krok 1: Stáhněte a spusťte ukázkový server Node.js REST API se seznamem úkolů
Ukázka TODO rozhraní API REST Node.js se zapíše speciálně pro práci s naše existující vzorek pro vytváření jednoho klienta úkolů REST API pro Azure AD. Toto je předpokladem pro rychlý Start.

Informace o tom, jak nastavit tuto možnost najdete v tématu naše stávající ukázky v [Microsoft Azure Active Directory ukázka REST API Service pro Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Krok 2: Registrace webového rozhraní API s klientovi Azure AD
Služba Active Directory podporuje přidání dva typy aplikací:

- Webové rozhraní API, které nabízejí služby pro uživatele
- Aplikace (běžící na webu nebo v zařízení), které ty přístup k webovým rozhraním API

V tomto kroku se registrace webové rozhraní API, kterou používáte místně pro testování této ukázce. Za normálních okolností tomuto webovému rozhraní API je služba REST, která je funkce, které chcete aplikaci pro přístup do nabídky. Azure AD můžete chránit libovolný koncový bod.

Jsme se za předpokladu, že jste registrace rozhraní REST API TODO odkazuje dříve. Ale tento postup funguje pro všechny webové rozhraní API, které chcete Azure Active Directory k ochraně.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na horním panelu klikněte na váš účet. V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Zadejte popisný název pro aplikaci (například **TodoListService**), vyberte **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko **Další**.
6. Přihlášení adresy URL zadejte základní adresu URL pro vzorovou. Ve výchozím nastavení je to `https://localhost:8080`.
7. Klikněte na tlačítko **OK** k dokončení registrace.
8. Během portál Azure, přejděte na stránku aplikace, najít hodnotu ID aplikace a zkopírujte jej. Budete potřebovat to později při konfiguraci vaší aplikace.
9. Z **nastavení** -> **vlastnosti** stránky, aktualizovat identifikátor ID URI aplikace – zadejte `https://<your_tenant_name>/TodoListService`. Nahraďte `<your_tenant_name>` s názvem vašeho klienta Azure AD.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Krok 3: Registrace ukázkovou aplikaci Android Native Client
V této ukázce je nutné zaregistrovat webové aplikace. To umožňuje aplikacím komunikovat se právě zaregistrován webové rozhraní API. Azure AD bude odmítnout i, aby vaše aplikace a požádejte o přihlášení, pokud je zaregistrována. Který je součástí modelu zabezpečení.

Jsme se za předpokladu, že registrace ukázkovou aplikaci odkazuje dříve. Ale tento postup funguje pro každou aplikaci, která jste vývoj.

> [!NOTE]
> Může vás zajímat, proč jste uvedení aplikace i webové rozhraní API v jednoho klienta. Jak vám může mít uhádnout, můžete vytvořit aplikaci, která přistupuje k externí rozhraní API, která je zaregistrovaná ve službě Azure AD z jiného klienta. Pokud tak učiníte, vaši zákazníci budou vyzváni k souhlas s použitím rozhraní API v aplikaci. Knihovna ověřování Active Directory pro iOS postará svůj souhlas pro vás. Jak jsme prozkoumat nabízí vyspělejší funkce, uvidíte, že se jedná o důležitou součástí práce potřebné pro přístup k sadě Microsoft APIs z Azure a Office, stejně jako ostatní poskytovatele služeb. Teď protože jste zaregistrovali webového rozhraní API a aplikace v rámci stejné klienta, neuvidíte zobrazování výzev k vyjádření souhlasu. Je tomu tak obvykle Pokud vyvíjíte aplikaci pouze pro vaši vlastní společnost používat.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na horním panelu klikněte na váš účet. V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Zadejte popisný název pro aplikaci (například **TodoListClient Android**), vyberte **nativní klientská aplikace**a klikněte na tlačítko **Další**.
6. Identifikátor URI přesměrování, zadejte `http://TodoListClient`. Klikněte na **Dokončit**.
7. Na stránce aplikace najít hodnotu ID aplikace a zkopírujte ho. Budete potřebovat to později při konfiguraci vaší aplikace.
8. Z **nastavení** vyberte **požadovaných oprávnění** a vyberte **přidat**.  Vyhledejte a vyberte TodoListService, přidejte **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a klikněte na tlačítko **provádí**.

Pokud chcete vytvořit s Maven, můžete použít pom.xml na nejvyšší úrovni:

1. Klonování tohoto úložiště do adresáře podle vašeho výběru:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Postupujte podle kroků v [požadavky pro nastavení prostředí Maven pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Nastavte emulátoru s SDK 19.
4. Přejděte do kořenové složky, které jste naklonovali úložiště.
5. Spusťte tento příkaz:`mvn clean install`
6. Změňte adresář na ukázkové rychlý Start:`cd samples\hello`
7. Spusťte tento příkaz:`mvn android:deploy android:run`

   Měli byste vidět, spuštění aplikace.
8. Zadejte přihlašovací údaje testovacího uživatele a zkuste to.

Balíčky JAR bude odeslána vedle balíček AAR.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Krok 4: Stáhnout Android ADAL a přidejte ji do pracovního prostoru Eclipse
Provedli jsme ho snadno, mají několik možností použití knihovny ADAL v projektu Android:

* Zdrojový kód můžete použít k importu této knihovny do Eclipse a propojení k vaší aplikaci.
* Pokud používáte Android Studio, můžete použít formát balíčku AAR a odkazovat na binární soubory.

### <a name="option-1-source-zip"></a>Možnost 1: Zdroj Zip
Chcete-li stáhnout kopii zdrojového kódu, klikněte na tlačítko **stáhnout ZIP** na pravé straně stránky. Nebo můžete [stáhnout z webu GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Možnost 2: Zdroj pomocí Gitu
Chcete-li získat zdrojový kód sady SDK prostřednictvím Git, zadejte:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Možnost 3: Binární soubory přes Gradle
Binární soubory můžete získat z centrální úložiště Maven. Balíček AAR můžou být takto součástí projekt v Android Studio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Možnost 4: AAR prostřednictvím Maven
Pokud používáte modul plug-in M2Eclipse, v souboru pom.xml můžete určit závislost:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a>Možnost 5: JAR balíčku naleznete ve složce knihovny
Můžete získat na soubor JAR z úložiště Maven a umístěte jej do **knihovny** složku ve vašem projektu. Budete muset zkopírovat požadované prostředky do projektu je také možné, protože balíčky JAR neobsahují.

## <a name="step-5-add-references-to-android-adal-to-your-project"></a>Krok 5: Přidejte odkazy na Android ADAL do projektu
1. Přidat odkaz na projekt a zadejte jej jako knihovna pro Android. Pokud si nejste jisti, jak na to, získáte další informace [lokality Android Studio](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Přidáte závislost projektu ladění do nastavení projektu.
3. Aktualizace souboru AndroidManifest.xml vašeho projektu:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Vytvoření instance kontextu AuthenticationContext v hlavní aktivitě. Podrobnosti o toto volání jsou nad rámec tohoto tématu, ale můžete začít funkčním prohlížením [ukázka Android Native Client](https://github.com/AzureADSamples/NativeClient-Android). V následujícím příkladu je SharedPreferences výchozí mezipaměti a autorita ve formě `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Zkopírujte tento blok kódu pro zpracování konec AuthenticationActivity poté, co uživatel zadá přihlašovací údaje a přijímá autorizační kód:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. Chcete-li požádat o token, definujte zpětné volání:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Nakonec požádejte o token pomocí že zpětné volání:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Následuje vysvětlení parametrů:

* *prostředek* je povinná a je prostředků, které se snažíte získat přístup.
* *ClientID* je povinná a pochází z Azure AD.
* *RedirectUri* není nutné je třeba zadat acquireToken volání. Můžete ho nastavit tak jako název balíčku.
* *PromptBehavior* umožňuje požádat o přihlašovací údaje tak, aby přeskočil mezipaměti a souboru cookie.
* *zpětné volání* je volána po autorizační kód se vyměňují pro token. Obsahuje objekt AuthenticationResult, který obsahuje přístupový token, datum vypršela platnost a ID informace o tokenu.
* *acquireTokenSilent* je volitelný. Můžete ji volat popisovač ukládání do mezipaměti a token obnovení. Nabízí taky verze synchronizace. Přijímá *userId* jako parametr.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Pomocí tohoto návodu, byste měli mít, co je potřeba úspěšně integraci se službou Azure Active Directory. Další příklady tato práce, najdete v článku AzureADSamples nebo úložišti na Githubu.

## <a name="important-information"></a>Důležité informace
### <a name="customization"></a>Přizpůsobení
Vaše prostředky aplikace můžete přepsat projektu prostředky knihovny. To se stane, když sestavuje vaší aplikace. Z tohoto důvodu můžete přizpůsobit ověřování aktivity rozložení požadovaným způsobem. Ujistěte se, že zachovat ID ovládacích prvků, že používá ADAL (webového zobrazení).

### <a name="broker"></a>Zprostředkovatel
Aplikace portál společnosti Microsoft Intune poskytuje komponentu zprostředkovatele. Účet je vytvořen v AccountManager. Typ účtu je "com.microsoft.workaccount." AccountManager umožňuje jenom jeden účet jednotné přihlašování. Vytvoří soubor cookie jednotného přihlašování pro uživatele po dokončení zařízení výzvu pro jednu z aplikací.

ADAL používá zprostředkovatele účtu, pokud jeden uživatelský účet je vytvořen v tato ověřovací data a můžete zvolit nepoužívání přeskočit ho. Můžete přeskočit zprostředkovatele uživatele pomocí:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Je třeba zaregistrovat speciální RedirectUri pro použití zprostředkovatele. RedirectUri je ve formátu `msauth://packagename/Base64UrlencodedSignature`. Vaše RedirectUri pro vaši aplikaci můžete získat pomocí skriptu brokerRedirectPrint.ps1 nebo mContext.getBrokerRedirectUri volání rozhraní API. Podpis se vztahuje k podepisování certifikátů.

Aktuální model zprostředkovatele je pro jednoho uživatele. Kontextu AuthenticationContext poskytuje metodu API získat zprostředkovatele uživatele.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Manifest aplikace by měla mít následující oprávnění k používání AccountManager účtů. Podrobnosti najdete v tématu [AccountManager informace na webu Android](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Adresa URL autority a AD FS
Active Directory Federation Services (AD FS) není rozpoznán jako produkční služby tokenů zabezpečení, takže budete muset zapnout zjišťování instance a předá hodnotu false v konstruktoru kontextu AuthenticationContext.

Adresa URL autority potřebuje instance služby tokenů zabezpečení a [název klienta](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Dotazování na položky mezipaměti
ADAL poskytuje výchozí mezipaměti SharedPreferences s některé jednoduché mezipaměti funkce dotazování. Aktuální mezipaměť můžete získat z kontextu AuthenticationContext s použitím:

    ITokenCacheStore cache = mContext.getCache();

Pokud chcete přizpůsobit, můžete zadat taky implementaci mezipaměti.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Chování výzvy
ADAL nabízí možnost, chcete-li určit chování výzvy. PromptBehavior.Auto se zobrazí uživatelské rozhraní, pokud token obnovení je neplatný a jsou vyžadována pověření uživatele. PromptBehavior.Always bude přeskočit využití mezipaměti a vždy zobrazí uživatelské rozhraní.

### <a name="silent-token-request-from-cache-and-refresh"></a>Tichou žádosti o token z mezipaměti a aktualizace
Tichou žádosti o token nepoužívá místní uživatelské rozhraní a nevyžaduje aktivitu. Vrátí token z mezipaměti, pokud je k dispozici. Pokud je platnost tokenu, pokusí se tato metoda jej aktualizovat. Pokud token obnovení vyprší nebo se nezdařilo, vrátí authenticationexception –.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Můžete provést také synchronizace volat pomocí této metody. Můžete nastavit hodnotu null pro zpětné volání nebo použít acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostika
Toto jsou primární zdroje informací pro diagnostiku problémů:

* Výjimky
* Logs
* Trasování sítě

Všimněte si, že jsou ID korelace centrální diagnostiku v knihovně. Můžete nastavit vaše korelace požadavku ID na základě požadavků, pokud chcete ke korelaci ADAL s dalšími operacemi v kódu. Pokud není nastavený ID korelace, ADAL vygeneruje náhodné. Všechny zprávy protokolu a volání síť pak bude být označený ID korelace. Vygenerovaný sám sebou ID změny na každý požadavek.

#### <a name="exceptions"></a>Výjimky
Výjimky jsou první diagnostiky. Pokusíme se poskytují užitečné chybové zprávy. Pokud zjistíte, ten, který není užitečné, oznamte problém a dejte nám vědět. Zahrnují informace o zařízení, jako je například modelu a číslo SDK.

#### <a name="logs"></a>Logs
Můžete nakonfigurovat knihovně k vygenerování zprávy protokolu, které můžete použít pro usnadnění diagnostiky problémů. Konfigurace protokolování tím, že provedete následující volání pro zpětné volání, které ADAL použije k ručně vypnout každé zprávě protokolu je generovaná konfigurace.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Jak je znázorněno v následujícím kódu, mohou být zprávy zapisovány do vlastního souboru protokolu. Bohužel neexistuje žádný standardní způsob získání protokolů ze zařízení. Existují některé služby, které vám mohou pomoci s to. Vám může také vytvořte vlastní, například odeslání souboru na server.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Toto jsou úrovní protokolování:
* Chyba (výjimek)
* Warn (upozornění)
* Informace o (informační účely)
* Verbose (podrobnosti)

Můžete nastavit úroveň protokolu takto:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Všechny zprávy protokolu jsou odesílány logcat, kromě všech zpětná volání vlastního protokolu.
Můžete získat protokolu do souboru logcat následujícím způsobem:

    adb logcat > "C:\logmsg\logfile.txt"

 Podrobnosti o adb příkazů najdete v tématu [logcat informace na webu Android](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Trasování sítě
Různé nástroje můžete zaznamenávat provoz protokolu HTTP, který generuje ADAL.  To je velmi užitečné, pokud jste obeznámeni s protokolem OAuth, nebo pokud potřebujete poskytovat diagnostické informace do společnosti Microsoft nebo jiné kanály podpory.

Fiddler je nejjednodušší nástroj trasování HTTP. Pomocí následujících odkazů pro ni nastavit až správně záznamů ADAL síťový provoz. Pro trasování nástroje, jako je Fiddler nebo Charlese být užitečné je nutné nakonfigurovat ji k zaznamenání nešifrované přenosy protokolu SSL.  

> [!NOTE]
> Trasování generované tímto způsobem může obsahovat vysoce důvěrné informace, například přístupové tokeny, uživatelská jména a hesla. Pokud používáte produkční účty, nesdílí toto trasování s třetími stranami. Pokud budete muset zadat trasování uživateli, aby bylo možné získat podporu, reprodukujte problém pomocí dočasného účtu s uživatelských jmen a hesel, která vás sdílení.

* Z webu Telerik webu: [nastavení si aplikaci Fiddler pro Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Z Githubu: [nakonfigurovat aplikaci Fiddler pravidla pro ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Dialogové okno režimu
Metoda acquireToken bez aktivity podporuje řádku dialogové okno.

### <a name="encryption"></a>Šifrování
ADAL šifruje tokeny a úložiště v SharedPreferences ve výchozím nastavení. Můžete si prohlédnout třídy StorageHelper a zobrazit podrobnosti. Android zavedl Android úložiště klíčů pro 4.3 (rozhraní API 18) bezpečné uložení privátního klíče. ADAL použije tento 18 rozhraní API a vyšší. Pokud chcete pomocí knihovny ADAL pro nižší verze sady SDK, budete muset zadat tajný klíč v AuthenticationSettings.INSTANCE.setSecretKey.

### <a name="oauth2-bearer-challenge"></a>Výzvy nosiče OAuth2
Třída AuthenticationParameters poskytuje funkci k získání authorization_uri z výzvy nosiče OAuth2.

### <a name="session-cookies-in-webview"></a>Soubory cookie relace ve webovém zobrazení
Android webové zobrazení nevymaže soubory cookie relace po zavření aplikace. Která dokáže zpracovat pomocí ukázkový kód:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Podrobnosti o souborech cookie najdete v tématu [CookieSyncManager informace na webu Android](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Přepsání prostředků
Knihovna ADAL zahrnuje anglické řetězce pro zprávy ProgressDialog. Aplikace je měli přepsat, pokud chcete, aby lokalizovaných řetězců.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>Dialogové okno protokolu NTLM
ADAL verze 1.1.0 podporuje dialogu protokolu NTLM, který zpracovává se pomocí onReceivedHttpAuthRequest událost z WebViewClient. Můžete přizpůsobit rozložení a řetězce pro dialogové okno.

### <a name="cross-app-sso"></a>Jednotné přihlašování napříč aplikacemi
Další informace [postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
