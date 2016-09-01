<properties 
    pageTitle="Nasazení uživatelského portálu pro Azure Multi-Factor Authentication Server" 
    description="Toto je stránka Vícefaktorového ověřování Azure, která nabízí úvod do používání Azure MFA a uživatelského portálu." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="femila" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="billmath"/>

# Nasazení uživatelského portálu pro Azure Multi-Factor Authentication Server

Uživatelský portál umožňuje správci nainstalovat a konfigurovat Azure Multi-Factor Authentication User Portal. User Portal je webová stránka IIS, která uživatelům umožní se zapsat do ověření Azure Multi-Factor Authentication a spravovat své účty. Uživatel může změnit svoje telefonní číslo, změnit PIN nebo obejít ověření Azure Multi-Factor Authentication při příštím přihlášení. 

Uživatelé se do portálu User Portal budou přihlašovat svým normálním přihlašovacím jménem a heslem a buďto dokončí volání ověření Azure Multi-Factor Authentication, nebo se ověří tak, že odpoví na bezpečnostní otázky. Pokud je povolený zápis uživatelů, uživatel si při prvním přihlášení do portálu User Portal nastaví telefonní číslo a PIN. 

Správci portálu User Portal můžou vytvořit a udělit oprávnění vytvářet nové uživatele a aktualizovat existující uživatele. 

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## Nasazení uživatelského portálu na stejném serveru jako je Azure Multi-Factor Authentication Server

Pro instalaci portálu User Portal na stejném serveru jako je Azure Multi-Factor Authentication Server je potřeba splnit tyto podmínky:

- IIS se musí nainstalovat včetně kompatibility s asp.net a IIS 6 meta base (pro IIS 7 a novější). 
- Přihlášený uživatel musí mít práva správce počítače a případně domény.  To je potřeba kvůli tomu, že účet potřebuje oprávnění k vytvoření skupin zabezpečení Active Directory.

### Nasazení uživatelského portálu pro Azure Multi-Factor Authentication Server

1. V Azure Multi-Factor Authentication Serveru: klikněte na ikonu User Portal, klikněte na tlačítko Nainstalovat portál User Portal. 
1. Klikněte na Další.
1. Klikněte na Další.
1. Pokud je počítač připojený k doméně a konfigurace Active Directory pro zabezpečení komunikace mezi portálem User Portal a službou Azure Multi-Factor Authentication není kompletní, zobrazí se krok Active Directory. Když kliknete na tlačítko Další, tato konfigurace se automaticky dokončí.
1. Klikněte na Další.
1. Klikněte na Další.
1. Klikněte na Zavřít.
1. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam se nainstaloval User Portal (např. https://www.verejnyweb.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## Nasazení portálu User Portal pro Azure Multi-Factor Authentication Server na samostatném serveru

Chcete-li používat aplikaci Azure Multi-Factor Authentication, je třeba splnit následující podmínky, aby aplikace mohla úspěšně komunikovat s portálem User Portal: 

Hardwarové a softwarové požadavky naleznete v části Požadavky na hardware a software:

- Musíte použít Azure Multi-Factor Authentication Server verze 6.0 nebo vyšší
- User Portal musí být nainstalovaný na internetovém webovém serveru s Microsoft® Internetovou informační službou (IIS) 6.x, IIS 7.x nebo vyšší.
- Při použití IIS 6.x se ujistěte, že je nainstalované ASP.NET v2.0.50727, že je zaregistrované a nastavené na Povoleno.
- Požadované služby rolí při použití IIS 7.x nebo vyšší obsahují kompatibilitu s ASP.NET a Metabází IIS 6.
- User Portal musí být zabezpečený certifikátem SSL.
- Sada SDK webové služby Azure Multi-Factor Authentication musí být nainstalovaná ve službě IIS 6.x, IIS 7.x nebo vyšší na serveru, na kterém je nainstalovaný Azure Multi-Factor Authentication Server.
- Sada SDK webové služby Azure Multi-Factor Authentication musí být zabezpečená certifikátem SSL.
- User Portal se musí dokázat připojit k sadě SDK webové služby Azure Multi-Factor Authentication přes protokol SSL
- User Portal se musí dokázat ověřit pro sadu SDK webové služby Azure Multi-Factor Authentication pomocí pověření účtu služby, který je členem skupiny zabezpečení s názvem „PhoneFactor Admins“. Tento účet služby a skupina existují ve službě Active Directory, pokud Azure Multi-Factor Authentication Server běží na serveru připojeném k doméně. Tento účet služby a skupina existují místně na Azure Multi-Factor Authentication Serveru, pokud není připojený k doméně.

Instalace portálu uživatele na jiném serveru než je Azure Multi-Factor Authentication Server vyžaduje následující tři kroky:

1. Instalace sady SDK webové služby
2. Instalace uživatelského portálu
3. Konfigurace nastavení portálu User Portal v Azure Multi-Factor Authentication Serveru


### Instalace sady SDK webové služby

Pokud sada SDK webové služby Azure Multi-Factor Authentication není na Azure Multi-Factor Authentication Serveru nainstalovaná, přejděte k danému serveru a otevřete Azure Multi-Factor Authentication Server. Klikněte na ikonu sady SDK webové služby, klikněte na tlačítko Instalovat sadu SDK webové služby... a postupujte podle zobrazených pokynů. Sada SDK webové služby musí být zabezpečená certifikátem SSL. Certifikát podepsaný svým držitelem lze pro tento účel použít, ale musí být importován do úložiště „Důvěryhodné kořenové certifikační autority“ účtu místního počítače na webovém serveru portálu pro uživatele tak, aby byl v budoucnu certifikát důvěryhodný při zahájení připojení SSL. 

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### Instalace uživatelského portálu

Než začnete instalovat uživatelský portál na samostatný server, ujistěte se, že znáte následující informace:

- Je užitečné otevřít webový prohlížeč na webovém serveru s přístupem k internetu a přejít na adresu URL sady SDK webové služby, která je zadaná v souboru web.config. Pokud se prohlížeč úspěšně dostane k webové službě, měla by se zobrazit výzva k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které jste zadali do souboru web.config, v naprosto stejném tvaru. Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.
- Pokud se před webovým serverem portálu User Portal nachází reverzní proxy server nebo brána firewall a provádí snižování zátěže protokolu SSL, můžete upravit soubor web.config portálu User Portal a přidat následující klíč do části <appSettings> tak, aby User Portal mohl používat protokol http místo protokolu https. <add key="SSL_REQUIRED" value="false"/>

#### Instalace uživatelského portálu

1. Otevřete Průzkumníka Windows na serveru Azure Multi-Factor Authentication Server a přejděte do složky, kde je nainstalovaný server Azure Multi-Factor Authentication (například C:\Program Files\Multi-Factor Authentication Server). Zvolte 32bitovou nebo 64bitovou verzi instalačního souboru MultiFactorAuthenticationUserPortalSetup v závislosti na serveru, na který se User Portal nainstaluje. Zkopírujte instalační soubor na server s přístupem k internetu.
2. Na webovém serveru s přístupem k internetu je třeba instalační soubor spustit s oprávněním správce. Nejjednodušším způsobem je otevřít příkazový řádek jako správce a přejít do umístění, kam se zkopíroval instalační soubor.
3. Spusťte instalační soubor MultiFactorAuthenticationUserPortalSetup64, v případě potřeby změňte název Webu a Virtuálního adresáře.
4. Po dokončení instalace portálu User Portal přejděte do C:\inetpub\wwwroot\MultiFactorAuth (nebo odpovídajícího adresáře podle názvu virtuálního adresáře) a upravte soubor web.config.
5. Vyhledejte klíč USE_WEB_SERVICE_SDK a změňte hodnotu z false na true. Vyhledejte klíče WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD a nastavte hodnoty na uživatelské jméno a heslo účtu služby, který je členem skupiny zabezpečení PhoneFactor Admins (viz část Požadavky výše). Nezapomeňte vložit uživatelské jméno a heslo mezi uvozovky na konci řádku (hodnota = "" / >). Doporučujeme použít kvalifikované uživatelské jméno (například doména\uživatelské jméno nebo počítač\uživatelské jméno).
6. Vyhledejte nastavení pfup_pfwssdk_PfWsSdk a změňte hodnotu z „http://localhost:4898/PfWsSdk.asmx“ na adresu URL sady Web Service SDK, která běží na serveru Azure Multi-Factor Authentication (např. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Vzhledem k tomu, že se pro toto připojení používá protokol SSL, musíte odkázat na sadu Web Service SDK pomocí názvu serveru a ne IP adresy, protože certifikát SSL byl vydán pro název serveru a použitá adresa URL se musí shodovat s názvem na certifikátu. Pokud název serveru neodpovídá IP adrese ze serveru s přístupem na internet, přidejte záznam do souboru hostitelů na tomto serveru pro namapování názvu serveru Azure Multi-Factor Authentication na jeho IP adresu. Po provedení změn uložte soubor web.config.
7. Pokud web, na který se nainstaloval User Portal (například výchozí web), ještě není navázaný na veřejně podepsaný certifikát, nainstalujte certifikát na server, otevřete správce služby IIS a vytvořte vazbu certifikátu k webu.
8. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam se nainstaloval User Portal (např. https://www.verejnyweb.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.



## Konfigurace nastavení uživatelského portálu v Azure Multi-Factor Authentication Serveru
Teď, když je portál nainstalovaný, musíte nakonfigurovat Azure Multi-Factor Authentication Server pro práci s portálem.

Server Azure Multi-Factor Authentication poskytuje pro uživatelský portál několik možností.  V následující tabulce je seznam těchto možností a vysvětlení, k čemu se používají.

Nastavení uživatelského portálu|Popis|
:------------- | :------------- | 
Adresa URL portálu User Portal| Umožňuje vám zadat adresu URL, kde se portál hostuje.
Primární ověření| Umožní vám vybrat typ ověření, které se má použít pro přihlášení k portálu.  Ověření Windows, Radius nebo LDAP.
Umožnit uživatelům přihlášení|Umožní uživatelům zadat uživatelské jméno a heslo na přihlašovací stránce uživatelského portálu.  Pokud tato možnost není zapnutá, pole nebudou aktivní.
Povolit zápis uživatele|Umožní uživateli se zapsat do vícefaktorového ověřování na stránce nastavení, která ho požádá o další informace, jako je třeba telefonní číslo.  Vyzvat k zadání záložního telefonu umožní uživatelům na telefonu zadat záložní telefonní číslo.  Vyzvat k zadání tokenu OATH jiných dodavatelů umožní uživatelům zadat token OATH od jiných výrobců.
Povolit uživatelům zahájení jednorázového přihlášení| Umožní uživatelům jednorázově obejít přihlášení.  Pokud toto uživatel zapne, projeví se to při jeho příštím přihlášení.  Vyzvat k zadání počtu sekund jednorázového přihlášení zobrazí uživateli pole pro změnu výchozí hodnoty 300 sekund.  Jinak bude přeskočení přihlášení platné jen na 300 sekund.
Povolit uživatelům výběr metody| Umožní uživatelům vybrat primární metodu kontaktu.  Může to být telefonní hovor, SMS, mobilní aplikace nebo token OATH.
Povolit uživatelům výběr jazyka|  Umožní uživatelům změnit jazyk telefonního hovoru, zprávy SMS, mobilní aplikace nebo tokenu OATH.
Povolit uživatelům aktivaci mobilní aplikace| Umožní uživatelům vygenerovat aktivační kód pro dokončení procesu aktivace mobilní aplikace, který se použije se serverem.  Můžete taky nastavit počet zařízení, na kterých se tato funkce může aktivovat.  1 až 10.
V nouzové situaci použít bezpečnostní otázky|Umožní použití bezpečnostních otázek v případě, že vícefaktorové ověřování selže.  Můžete zadat počet bezpečnostních otázek, které je potřeba správně zodpovědět.
Povolit uživatelům přidružení tokenu OATH jiného výrobce| Povolí uživatelům zadat token OATH jiného výrobce.
V nouzové situaci použít token OATH|Umožní použít token OATH v situaci, kdy se vícefaktorové ověřování nepovede.  Můžete taky zadat časový limit relace v minutách.
Povolit protokolování|Povolí protokolování uživatelského portálu.  Protokoly se ukládají do: C:\Program Files\Multi-Factor Authentication Server\Logs.

Většina těchto nastavení je pro uživatele viditelná, když jsou zapnuté a uživatel se přihlásí k uživatelskému portálu.

![Nastavení uživatelského portálu](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### Konfigurace nastavení uživatelského portálu v Azure Multi-Factor Authentication Serveru




1. Na Azure Multi-Factor Authentication Serveru klikněte na ikonu uživatelského portálu. Na kartě Nastavení zadejte adresu URL portálu User Portal do pole Adresa URL portálu User Portal. Tato adresa URL se vloží do e-mailů, které se uživatelům pošlou po jejich importování do Azure Multi-Factor Authentication Serveru, pokud je zapnutá funkce e-mailu.
2. Vyberte nastavení, které chcete v portálu User Portal použít. Pokud mají například uživatelé povolení řídit své metody ověřování, zkontrolujte, že je vedle metod, ze kterých si můžou vybrat, zaškrtnutá možnost Povolit uživatelům výběr metody.
3. Vysvětlení každého zobrazeného nastavení můžete zobrazit kliknutím na odkaz Nápověda v pravém horním rohu.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## Karta Správci
Tato karta vám jednoduše možní přidávat uživatele, kteří budou mít oprávnění správce.  Při přidání uživatele můžete podrobně upravit oprávnění, která bude mít.  Můžete si tak být jistí, že mu udělíte jen ta oprávnění, které bude jako správce potřebovat.  Jednoduše klikněte na tlačítko Přidat, a pak vyberte uživatele a jeho oprávnění a klikněte na Přidat.

![Správci uživatelského portálu](./media/multi-factor-authentication-get-started-portal/admin.png)


## Bezpečnostní otázky
Tato karta vám umožní zadat bezpečnostní otázky, které budou uživatelé muset zodpovědět, pokud je vybraná možnost V nouzové situaci použít bezpečnostní otázky.  Azure Multi-Factor Authentication Server má několik výchozích otázek, které můžete použít.  Můžete taky změnit pořadí otázek nebo přidat svoje vlastní.  Pokud přidáváte vlastní otázky, můžete taky vybrat jazyk, ve kterém chcete, aby se otázka taky zobrazila.

![Bezpečnostní otázky uživatelského portálu](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## Úspěšné relace

## SAML
Umožní vám nastavit uživatelský portál tak, aby přijímal deklarace identity od zprostředkovatelů identity pomocí SAML.  Můžete zadat časový limit relace, ověřovací certifikát a Adresu URL pro přesměrování při odhlášení.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## Důvěryhodné IP adresy
Tato karta vám umožní zadat jednotlivé IP adresy nebo rozsahy IP adres, ze kterých pokud se uživatel přihlašuje, tak se vícefaktorové ověřování přeskočí. 

![Důvěryhodné IP adresy uživatelského portálu](./media/multi-factor-authentication-get-started-portal/trusted.png)

## Povolit zápis uživatele k samoobslužné službě
Pokud chcete, aby se uživatelé přihlásili a zapsali, musíte zapnout možnosti Umožnit uživatelům přihlášení a Povolit zápis uživatele. Nezapomeňte, že nastavení, které vyberete, bude mít vliv na proces přihlášení.

Pokud se třeba uživatel přihlásí do portálu User Portal a klikne na tlačítko Přihlásit, zobrazí se stránka Azure Multi-Factor Authentication – Nastavení uživatele.  Podle toho, jak jste nakonfigurovali ověřování Azure Multi-Factor Authentication si uživatel možná bude moci vybrat metodu ověření.  

Pokud si vybere možnost Hlasový hovor nebo je tato možnost předem nastavená, na stránce se zobrazí výzva k zadání svého hlavního telefonního čísla a případně linky.  Možná budou moct zadat záložní telefonní číslo.  

![Důvěryhodné IP adresy uživatelského portálu](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Pokud musí uživatel při ověření zadat PIN, zobrazí se na stránce výzva k zadání čísla PIN.  Po zadání telefonního čísla nebo telefonních čísel a čísla PIN (pokud je potřeba) uživatel klikne na tlačítko Zavolat mi nyní a ověřit mě.  Azure Multi-Factor Authentication provede ověření tím, že zavolá na hlavní telefonní číslo uživatele.  Uživatel musí hovor přijmout, zadat PIN (pokud je tato funkce aktivní) a stisknout #, tím přejde k dalšímu kroku vlastního zápisu.   

Pokud si uživatel vybere možnost SMS zpráva nebo je tato možnost předem nastavená, na stránce se zobrazí výzva k zadání telefonního čísla na mobilní telefon.  Pokud musí uživatel při ověření zadat PIN, zobrazí se na stránce výzva k zadání čísla PIN.  Po zadání telefonního čísla a čísla PIN (pokud je potřeba) uživatel klikne na tlačítko Poslat mi nyní zprávu SMS a ověřit mě.  Azure Multi-Factor Authentication provede ověření tím, že na mobilní telefonní číslo uživatele pošle SMS zprávu.  Uživatel musí přijmout zprávu SMS, která obsahuje jednorázový kód (OTP), a odpovědět na ni s tímto OTP a číslem PIN (pokud je potřeba), aby mohl přejít k dalšímu kroku vlastního zápisu. 

![SMS uživatelského portálu](./media/multi-factor-authentication-get-started-portal/text.png)   

Pokud si uživatel vybere ověření přes mobilní aplikaci nebo je tato možnost předem nastavená, na stránce se zobrazí výzva k instalaci aplikace Azure Multi-Factor Authentication na mobilní zařízení a vygenerování aktivačního kódu na tomto mobilním zařízení.  Když uživatel nainstaluje aplikaci Azure Multi-Factor Authentication klikne pak na tlačítko Generovat aktivační kód.    

>[AZURE.NOTE]Pro používání aplikace Azure Multi-Factor Authentication je potřeba, aby měl uživatel ve svém mobilním zařízení zapnutý příjem nabízených oznámení. 

Na stránce se pak zobrazí aktivační kód, adresa URL a čárový kód.  Pokud musí uživatel při ověření zadat PIN, zobrazí se na stránce výzva k zadání čísla PIN.  Uživatel zadá aktivační kód a adresu URL do aplikace Azure Multi-Factor Authentication nebo pomocí čtečky čárových kódů naskenuje čárový kód, pak klikne na tlačítko Aktivovat.    

Po dokončení aktivace uživatel klikne na tlačítko Ověřit mě nyní.  Azure Multi-Factor Authentication provede ověření přes mobilní aplikaci uživatele.  Uživatel musí zadat PIN (pokud je tato funkce aktivní) a stisknout tlačítko Ověřit, tím přejde k dalšímu kroku vlastního zápisu.  


Pokud správci nastavili Azure Multi-Factor Authentication Server tak, aby shromažďoval bezpečnostní otázky a odpovědi, přesměruje uživatele na stránku s bezpečnostními otázkami.  Uživatel musí vybrat čtyři bezpečnostní otázky a zadat odpovědi na tyto otázky.    

![Bezpečnostní otázky uživatelského portálu](./media/multi-factor-authentication-get-started-portal/secq.png)  

Samoobslužný zápis je teď dokončený a uživatel je přihlášený do portálu User Portal.  V budoucnu se uživatel může kdykoli znovu přihlásit a pokud k tomu má oprávnění od správců, bude moct změnit svoje telefonní čísla, čísla PIN, metody ověření a bezpečnostní otázky.

 



<!--HONumber=Aug16_HO4-->


