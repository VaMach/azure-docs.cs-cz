---
title: "Webová služba mobilní aplikace Azure MFA Serveru | Dokumentace Microsoftu"
description: "Aplikace Microsoft Authenticator nabízí možnost dalšího ověření mimo pásmo.  To umožňuje serveru MFA používat nabízená oznámení pro uživatele."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 20afeb3ba290ddf728d2b52c076c7a57fadc77c6
ms.openlocfilehash: 4014bf0217e25ea9bc8473ef2383279e5eb79b87
ms.contentlocale: cs-cz
ms.lasthandoff: 02/28/2017

---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Povolení ověření přes mobilní aplikaci se serverem Azure Multi-Factor Authentication

Aplikace Microsoft Authenticator nabízí možnost dalšího ověření mimo pásmo. Namísto zahájení automatizovaného telefonního hovoru nebo odeslání SMS zprávy uživateli při přihlášení, doručí služba Azure Multi-Factor Authentication oznámení do aplikace Microsoft Authenticator na tablet nebo smartphone uživatele. Uživatel v aplikaci jednoduše dokončí přihlášení klepnutím na **Ověřit** (nebo zadá kód PIN a klepne na Ověřit).

Použití mobilní aplikace pro dvojstupňové ověřování se upřednostňuje v případě, že je telefonní příjem nespolehlivý. Pokud aplikaci používáte jako generátor tokenu OAUTH, nevyžaduje připojení k síti ani k internetu.

Instalace portálu uživatele na jiném serveru, než je Azure Multi-Factor Authentication Server, vyžaduje následující kroky:

1. Instalace sady SDK webové služby
2. Instalaci webové služby mobilní aplikace
3. Konfigurace serveru Azure Multi-Factor Authentication
4. Aktivace aplikace Microsoft Authenticator pro koncové uživatele

## <a name="requirements"></a>Požadavky

Pokud chcete použít aplikaci Microsoft Authenticator, je třeba splnit následující podmínky, aby aplikace mohla úspěšně komunikovat s webovou službou mobilní aplikace:

* Azure Multi-Factor Authentication Server verze&6;.0 nebo vyšší
* Instalace webové služby mobilní aplikace na internetovém webovém serveru s [Internetovou informační službou Microsoft® (IIS) IIS 7.x nebo vyšší](http://www.iis.net/)
* Nainstalované, registrované a povolené rozhraní ASP.NET v4.0.30319
* Požadované role služby zahrnují technologie ASP.NET a kompatibilitu metabáze služby IIS 6
* Webová služba mobilní aplikace je přístupná přes veřejnou adresu URL.
* Webová služba mobilní aplikace je zabezpečena certifikátem SSL.
* Instalace sady SDK webové služby Azure Multi-Factor Authentication ve službě IIS 7.x nebo vyšší na stejném serveru jako Azure Multi-Factor Authentication Server.
* Sada SDK webové služby Azure Multi-Factor Authentication je zabezpečená certifikátem SSL.
* Webová služba mobilní aplikace se může připojit k sadě SDK webové služby Azure Multi-Factor Authentication přes protokol SSL.
* Webová služba mobilní aplikace se může ověřit v sadě SDK webové služby Azure MFA pomocí přihlašovacích údajů účtu služby, který je členem skupiny zabezpečení „PhoneFactor Admins“. Tento účet služby a skupina existují ve službě Active Directory, pokud Azure Multi-Factor Authentication Server běží na serveru připojeném k doméně. Tento účet služby a skupina existují místně na Azure Multi-Factor Authentication Serveru, pokud není připojený k doméně.


## <a name="install-the-web-service-sdk"></a>Instalace sady SDK webové služby
Pokud sada SDK webové služby Azure Multi-Factor Authentication není na Azure Multi-Factor Authentication (MFA) Serveru nainstalovaná, přejděte k danému serveru a otevřete Azure MFA Server.

1. Klikněte na ikonu sady SDK webové služby.
2. Klikněte na **Nainstalovat sadu SDK webové služby** a postupujte podle zobrazených pokynů.

Sada SDK webové služby musí být zabezpečená certifikátem SSL. Pro tento účel stačí certifikát podepsaný svým držitelem. Importujte certifikát do úložiště „Důvěryhodné kořenové certifikační autority“ účtu místního počítače na webovém serveru portálu User Portal tak, aby byl v budoucnu certifikát důvěryhodný při navazování připojení SSL.

![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

## <a name="install-the-mobile-app-web-service"></a>Instalaci webové služby mobilní aplikace
Před instalací webové služby mobilní aplikace mějte na paměti následující:

* Pokud je portál Azure MFA User Portal již nainstalován na serveru s přístupem k internetu, uživatelské jméno, heslo a adresu URL pro sadu SDK webové služby lze zkopírovat ze souboru web.config portálu User Portal.
* Je užitečné otevřít webový prohlížeč na webovém serveru s přístupem k internetu a přejít na adresu URL sady Web Service SDK, která byla zadán do souboru web.config. Pokud se prohlížeč úspěšně dostane k webové službě, měla by se zobrazit výzva k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které jste zadali do souboru web.config, v naprosto stejném tvaru. Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.
* Pokud se před webovým serverem webové služby mobilní aplikace nachází reverzní proxy server nebo brána firewall a provádí snižování zátěže protokolu SSL, můžete upravit soubor web.config webové služby mobilní aplikace tak, aby webová služba mobilní aplikace mohla používat protokol http místo protokolu https. SSL je však stále zapotřebí ve směru z mobilní aplikace k reverznímu proxy serveru / bráně firewall. Do části \<appSettings\> přidejte následující klíč:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-service"></a>Instalace služby

1. Otevřete Průzkumníka Windows na Azure Multi-Factor Authentication Serveru a přejděte do složky, kde je nainstalován Azure MFA Server (obvykle C:\Program Files\Azure Multi-Factor Authentication). Zvolte 32bitovou nebo 64bitovou verzi instalačního souboru Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup. Zkopírujte instalační soubor na server s přístupem k internetu.

2. Na webovém serveru s přístupem k internetu spusťte instalační soubor s oprávněním správce. Otevřete příkazový řádek jako správce a přejděte do umístění, kam se zkopíroval instalační soubor.

3. Spusťte instalační soubor Multi-Factor AuthenticationMobileAppWebServiceSetup, změňte v případě potřeby web a změňte virtuální adresář na krátký název, například „PA“.

  Vzhledem k tomu, že budou muset uživatelé zadat adresu URL webové služby mobilní aplikace do mobilního zařízení během aktivace, doporučujeme použít krátký název virtuálního adresáře.

4. Po dokončení instalace Azure Multi-Factor AuthenticationMobileAppWebServiceSetup vyhledejte C:\inetpub\wwwroot\PA (nebo příslušný adresář na základě názvu virtuálního adresáře) a upravte soubor web.config.

5. Vyhledejte klíče WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD. Nastavte hodnoty na uživatelské jméno a heslo účtu služby, který je členem skupiny zabezpečení PhoneFactor Admins. Může se jednat o stejný účet, který se používá jako identita portálu pro uživatele Azure Multi-Factor Authentication, pokud byl dříve nainstalován. Nezapomeňte vložit uživatelské jméno a heslo mezi uvozovky na konci řádku (hodnota = "" / >). Použijte kvalifikované uživatelské jméno, například doména\uživatelské_jméno nebo počítač\uživatelské_jméno.  

6. Vyhledejte nastavení pfMobile App Web Service_pfwssdk_PfWsSdk. Změňte hodnotu z *http://localhost:4898/PfWsSdk.asmx* na adresu URL sady SDK webové služby, která běží na Azure Multi-Factor Authentication Serveru (např. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).

  Vzhledem k tomu, že se pro toto připojení používá protokol SSL, je třeba na sadu SDK webové služby odkazovat pomocí názvu serveru, a ne pomocí IP adresy. Certifikát SSL by byl vydán pro název serveru a použitá adresa URL se musí shodovat s názvem na certifikátu. Název serveru nemusí odpovídat IP adrese ze serveru s přístupem na internet. Pokud je to váš případ, přidejte do souboru hostitelů na tomto serveru záznam pro namapování Azure Multi-Factor Authentication Serveru na jeho IP adresu. Po provedení změn uložte soubor web.config.

7. Pokud web, na který byla nainstalována webová služba mobilní aplikace, nebyl dosud navázán na veřejně podepsaný certifikát, nainstalujte certifikát na server, otevřete Správce služby IIS a vytvořte vazbu certifikátu k webu.

8. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam byla nainstalována webová služba mobilní aplikace (např. https://www.publicwebsite.com/PA). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace serveru Azure Multi-Factor Authentication
Teď, když je instalována webová služba mobilní aplikace, musíte nakonfigurovat server Azure Multi-Factor Authentication pro práci s portálem.

1. V Azure MFA Serveru klikněte na ikonu User Portal. Pokud mají uživatelé povolení řídit své metody ověřování, zaškrtněte možnost **Mobilní aplikace** na kartě Nastavení v části **Povolit uživatelům výběr metody**. Bez povolení této funkce bude nutné kontaktovat podporu a požádat o úplnou aktivaci mobilní aplikace.
2. Zaškrtněte políčko **Povolit uživatelům aktivaci mobilní aplikace**.
3. Zaškrtněte políčko **Povolit zápis uživatele**.
4. Klikněte na ikonu Mobilní aplikace.
5. Zadejte adresu URL používanou s virtuálním adresářem, který byl vytvořen při instalaci Azure Multi-Factor AuthenticationMobileAppWebServiceSetup. Název účtu může být zapsán v poskytnutém prostoru. Tento název společnosti se zobrazí v mobilní aplikaci. Pokud nebude zadán, zobrazí se název poskytovatele služby Multi-Factor Auth vytvořený na portálu Azure Classic.

<center>![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>

