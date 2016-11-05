---
title: Začínáme s webovou službou mobilní aplikace serveru MFA
description: Aplikace Azure Multi-Factor Authentication nabízí možnost dalšího ověření mimo pásmo.  To umožňuje serveru MFA používat nabízená oznámení pro uživatele.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtland

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban

---
# Začínáme s webovou službou mobilní aplikace serveru MFA
Aplikace Azure Multi-Factor Authentication nabízí možnost dalšího ověření mimo pásmo. Namísto zahájení automatizovaného telefonního hovoru nebo SMS uživateli při přihlášení, doručí Azure Multi-Factor Authentication oznámení do aplikace Azure Multi-Factor Authentication na tablet nebo smartphone uživatele. Uživatel jednoduše klepne na položku „Ověřit“ (nebo zadá kód PIN a klepne na „Ověřit“) v aplikaci k přihlášení.

Chcete-li používat aplikaci Azure Multi-Factor Authentication, je třeba splnit následující podmínky, aby aplikace mohla úspěšně komunikovat s webovými službami mobilní aplikace:

* Hardwarové a softwarové požadavky naleznete v části Požadavky na hardware a software
* Je třeba použít server Azure Multi-Factor Authentication verze 6.0 nebo vyšší
* Webové služby mobilní aplikace musí být nainstalovány na internetovém webovém serveru s internetovými informačními službami Microsoft® (IIS) IIS 7.x nebo vyšší.  Další informace o službě IIS najdete v části [IIS.NET](http://www.iis.net/).
* Ujistěte se, že je nainstalováno, registrováno a povoleno rozhraní ASP.NET v4.0.30319
* Požadované role služby zahrnují technologie ASP.NET a kompatibilitu metabáze služby IIS 6
* Webové služby mobilní aplikace musí být přístupné přes veřejnou adresu URL
* Webové služby mobilní aplikace musí být zabezpečeny certifikátem SSL.
* Sada Web Service SDK Azure Multi-Factor Authentication musí být nainstalován ve službě IIS 7.x nebo vyšší na serveru Azure Multi-Factor Authentication
* Sada Web Service SDK Azure Multi-Factor Authentication musí být zabezpečená certifikátem SSL.
* Webové služby mobilní aplikace musí být schopni se připojit k sadě Web Service SDK Azure Multi-Factor Authentication přes protokol SSL
* Webové služby mobilní aplikace musí mít k ověření sady Web Service SDK Azure Multi-Factor Authentication pomocí pověření účtu služby, který je členem skupiny zabezpečení s názvem „PhoneFactor Admins“. Tento účet služby a skupina existují ve službě Active Directory, pokud server Azure Multi-Factor Authentication běží na serveru připojeném k doméně. Tento účet služby a skupina existují místně na Azure Multi-Factor Authentication Serveru, pokud není připojený k doméně.

Instalace portálu uživatele na jiném serveru než je Azure Multi-Factor Authentication Server vyžaduje následující tři kroky:

1. Instalaci sady SDK webové služby
2. Instalaci webové služby mobilní aplikace
3. Konfigurace serveru Azure Multi-Factor Authentication
4. Aktivace aplikace Azure Multi-Factor Authentication pro koncové uživatele

## Instalace sady SDK webové služby
Pokud sada SDK webové služby Azure Multi-Factor Authentication není na Azure Multi-Factor Authentication Serveru nainstalovaná, přejděte k danému serveru a otevřete Azure Multi-Factor Authentication Server. Klikněte na ikonu sady SDK webové služby, klikněte na tlačítko Instalovat sadu SDK webové služby... a postupujte podle zobrazených pokynů. Sada SDK webové služby musí být zabezpečená certifikátem SSL. Certifikát podepsaný svým držitelem lze pro tento účel použít, ale musí být importován do úložiště „Důvěryhodné kořenové certifikační autority“ účtu místního počítače na webovém serveru portálu pro uživatele tak, aby byl v budoucnu certifikát důvěryhodný při zahájení připojení SSL.

<center>![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## Instalaci webové služby mobilní aplikace
Před instalací webové služby mobilní aplikace si uvědomte skutečnosti:

* Pokud je portál pro uživatele Azure Multi-Factor Authentication již nainstalován na internetovém serveru, uživatelské jméno, heslo a adresu URL pro sadu Web Service SDK lze zkopírovat ze souboru web.config rozhraní portálu pro uživatele.
* Je užitečné otevřít webový prohlížeč na webovém serveru s přístupem k internetu a přejít na adresu URL sady Web Service SDK, která byla zadán do souboru web.config. Pokud se prohlížeč úspěšně dostane k webové službě, měla by se zobrazit výzva k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které jste zadali do souboru web.config, v naprosto stejném tvaru. Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.
* Pokud se před webovým serverem webové služby mobilní aplikace nachází reverzní proxy server nebo brána firewall a provádí snižování zátěže protokolu SSL, můžete upravit soubor web.config webové služby mobilní aplikace a přidat následující klíč do části <appSettings> tak, aby webové služby mobilní aplikace mohly používat protokol http místo protokolu https. SSL je však stále zapotřebí ve směru z mobilní aplikace k reverznímu proxy serveru / bráně firewall. <add key="SSL_REQUIRED" value="false"/>

### Instalace webové služby mobilní aplikace
<ol>

<li>Otevřete Průzkumníka Windows na serveru Azure Multi-Factor Authentication a přejděte do složky, kde je nainstalován server Azure Multi-Factor Authentication (například C:\Program Files\Azure Multi-Factor Authentication). Zvolte 32bitovou nebo 64bitovou verzi instalačního souboru Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup v závislosti na serveru, na který bude webová služba mobilní aplikace nainstalována. Zkopírujte instalační soubor na server s přístupem k internetu.</li>

<li>Na webovém serveru s přístupem k internetu je třeba instalační soubor spustit s oprávněním správce. Nejjednodušším způsobem je otevřít příkazový řádek jako správce a přejít do umístění, kam byl zkopírován instalační soubor.</li>  

<li>Spusťte instalační soubor Multi-Factor AuthenticationMobileAppWebServiceSetup, změňte v případě potřeby web a změňte virtuální adresář na krátký název, například „PA“. Vzhledem k tomu, že budou muset uživatelé zadat adresu URL webové služby mobilní aplikace do mobilního zařízení během aktivace, doporučujeme použít krátký název virtuálního adresáře.</li>

<li>Po dokončení instalace Azure Multi-Factor AuthenticationMobileAppWebServiceSetup vyhledejte C:\inetpub\wwwroot\PA (nebo příslušný adresář na základě názvu virtuálního adresáře) a upravte soubor web.config.</li>  

<li>Vyhledejte klíče WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD a nastavte hodnoty na uživatelské jméno a heslo účtu služby, který je členem skupiny zabezpečení PhoneFactor Admins (viz část Požadavky výše). Může se jednat o stejný účet, který se používá jako identita portálu pro uživatele Azure Multi-Factor Authentication, pokud byl dříve nainstalován. Nezapomeňte vložit uživatelské jméno a heslo mezi uvozovky na konci řádku (hodnota = "" / >). Doporučujeme použít kvalifikované uživatelské jméno (například domain\username nebo machine\username).</li>  

<li>Vyhledejte nastavení pfMobile App Web Service_pfwssdk_PfWsSdk a změňte hodnotu z „http://localhost:4898/PfWsSdk.asmx“ na adresu URL sady Web Service SDK, která běží na serveru Azure Multi-Factor Authentication (např. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Vzhledem k tomu, že se pro toto připojení používá protokol SSL, musíte odkázat na sadu Web Service SDK pomocí názvu serveru a ne IP adresy, protože certifikát SSL byl vydán pro název serveru a použitá adresa URL se musí shodovat s názvem na certifikátu. Pokud název serveru neodpovídá IP adrese ze serveru s přístupem na internet, přidejte záznam do souboru hostitelů na tomto serveru pro namapování názvu serveru Azure Multi-Factor Authentication na jeho IP adresu. Po provedení změn uložte soubor web.config.</li>  

<li>Pokud web, na který byly nainstalovány webové služby mobilní aplikace (například výchozí web), nebyl dosud navázán na veřejně podepsaný certifikát, nainstalujte certifikát na server, otevřete správce služby IIS a vytvořte vazbu certifikátu k webu.</li>  

<li>Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam byly nainstalovány webové služby mobilní aplikace (např. https://www.publicwebsite.com/PA). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.</li>

### Konfigurace serveru Azure Multi-Factor Authentication
Teď, když je instalována webová služba mobilní aplikace, musíte nakonfigurovat server Azure Multi-Factor Authentication pro práci s portálem.

#### Konfigurace mobilní aplikace na serveru Azure Multi-Factor Authentication
1. Na serveru Azure Multi-Factor Authentication klikněte na ikonu portálu pro uživatele. Pokud mají uživatelé povolení řídit své metody ověřování, na kartě Nastavení v části Povolit uživatele, zaškrtněte možnost Mobilní aplikace. Bez povolení této funkce bude nutné kontaktovat podporu a požádat o úplnou aktivaci mobilní aplikace.
2. Zkontrolujte možnost Povolit uživatele pro aktivaci pole Mobilní aplikace.
3. Zaškrtněte políčko Povolit zápis uživatele.
4. Klikněte na ikonu Mobilní aplikace.
5. Zadejte adresu URL používanou s virtuálním adresářem, který byl vytvořen při instalaci Azure Multi-Factor AuthenticationMobileAppWebServiceSetup. Název účtu může být zapsán v poskytnutém prostoru. Tento název společnosti se zobrazí v mobilní aplikaci. Pokud nebude zadáno, zobrazí se název poskytovatele služby Multi-Factor Auth vytvořený na portálu pro správu Azure.

<center>![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>



<!--HONumber=Sep16_HO3-->


