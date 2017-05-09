---
title: "Portál User Portal pro Azure MFA Server | Dokumentace Microsoftu"
description: "Toto je stránka Vícefaktorového ověřování Azure, která nabízí úvod do používání Azure MFA a uživatelského portálu."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/24/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 97845748ef44469d1ec6a91ff8184288dc6350d9
ms.contentlocale: cs-cz
ms.lasthandoff: 04/28/2017


---
# <a name="deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Nasazení portálu User Portal pro Azure Multi-Factor Authentication Server
User Portal je web služby IIS, který uživatelům umožňuje se zapsat do služby Azure Multi-Factor Authentication a spravovat své účty. Uživatel může změnit svoje telefonní číslo, změnit PIN kód nebo obejít dvoustupňové ověřování při příštím přihlášení.

Uživatelé se k portálu User Portal přihlašují svým normálním uživatelským jménem a heslem a buďto dokončí dvoustupňový ověřovací hovor, nebo se ověří tak, že odpoví na bezpečnostní otázky. Pokud je povolený zápis uživatelů, uživatelé si při prvním přihlášení na portálu User Portal nastaví telefonní číslo a PIN kód.

Správci portálu User Portal můžou vytvořit a udělit oprávnění vytvářet nové uživatele a aktualizovat existující uživatele.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/install.png)</center>

>[!NOTE] 
>Portál User Portal je k dispozici pouze v Azure Multi-Factor Authentication Serveru. Pokud používáte službu Multi-Factor Authentication v cloudu, odkažte své uživatele na témata [Nastavení účtu pro dvoustupňové ověřování](./end-user/multi-factor-authentication-end-user-first-time.md) nebo [Správa nastavení pro dvoustupňové ověřování](./end-user/multi-factor-authentication-end-user-manage-settings.md).


## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Nasazení portálu User Portal na stejném serveru, jako je Azure Multi-Factor Authentication Server
Pro instalaci portálu User Portal na stejném serveru, jako je Azure Multi-Factor Authentication Server, se vyžaduje následující:

* Služba IIS, včetně kompatibility s asp.net a IIS 6 meta base (pro IIS 7 a novější).
* Účet s právy správce počítače a případně domény. Tento účet musí mít oprávnění k vytvoření skupin zabezpečení Active Directory.

Pokud chcete nasadit portál User Portal, postupujte takto:

1. V Azure Multi-Factor Authentication Serveru klikněte v nabídce vlevo na ikonu **User Portal** a potom klikněte na **Nainstalovat portál User Portal**.
2. Klikněte na **Další**.
3. Klikněte na **Další**.
4. Pokud je počítač připojený k doméně a služba Active Directory není nakonfigurovaná pro zabezpečení komunikace mezi portálem User Portal a službou Azure Multi-Factor Authentication, zobrazí se krok Active Directory. Když kliknete na tlačítko **Další**, tato konfigurace se automaticky dokončí.
5. Klikněte na **Další**.
6. Klikněte na **Další**.
7. Klikněte na **Zavřít**.
8. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam se nainstaloval portál User Portal (např. https://www.verejnyweb.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploy-the-user-portal-on-a-separate-server"></a>Nasazení portálu User Portal na samostatný server
Pokud server, na kterém je spuštěný Azure Multi-Factor Authentication Server, nemá přístup k internetu, měli byste portál User Portal nainstalovat na samostatný server s přístupem k internetu. 

Pokud vaše organizace jako jednu z metod ověřování používá aplikaci Microsoft Authenticator a chcete nasadit portál User Portal na vlastním serveru, splňte následující požadavky: 

* Použijte Azure Multi-Factor Authentication Server verze 6.0 nebo vyšší.
* Nainstalujte portál User Portal na webovém serveru s přístupem k internetu a s Internetovou informační službou (IIS) verze 6.x nebo vyšší.
* Pokud používáte IIS 6.x, ujistěte se, že je nainstalované ASP.NET v2.0.50727, že je zaregistrované a nastavené na **Povoleno**.
* Při použití služby IIS 7.x nebo vyšší se vyžadují služby rolí ASP.NET a Kompatibilita metabáze služby IIS 6.
* Zabezpečte portál User Portal certifikátem SSL.
* Zabezpečte sadu SDK webové služby Azure Multi-Factor Authentication certifikátem SSL.
* Ověřte, že se portál User Portal dokáže připojit k sadě SDK webové služby Azure Multi-Factor Authentication přes protokol SSL.
* Ověřte, že se portál User Portal dokáže ověřit pro sadu SDK webové služby Azure Multi-Factor Authentication pomocí přihlašovacích údajů účtu služby ve skupině zabezpečení PhoneFactor Admins. Tento účet služby a skupina existují ve službě Active Directory, pokud server Azure Multi-Factor Authentication běží na serveru připojeném k doméně. Tento účet služby a skupina existují místně na Azure Multi-Factor Authentication Serveru, pokud není připojený k doméně.

Instalace portálu uživatele na jiném serveru než je Azure Multi-Factor Authentication Server vyžaduje následující tři kroky:

1. Instalace sady SDK webové služby na stejném serveru, jako je Azure Multi-Factor Authentication Server
2. Instalace uživatelského portálu
3. Konfigurace nastavení portálu User Portal v Azure Multi-Factor Authentication Serveru

### <a name="step-1-install-the-web-service-sdk"></a>Krok 1: Instalace sady SDK webové služby
Pokud sada SDK webové služby Azure Multi-Factor Authentication není na Azure Multi-Factor Authentication Serveru nainstalovaná, přejděte k danému serveru a otevřete Azure Multi-Factor Authentication Server. Klikněte na ikonu **Sada SDK webové služby** a potom na **Instalovat sadu SDK webové služby**. Postupujte podle zobrazených pokynů. 

Sada SDK webové služby musí být zabezpečená certifikátem SSL. Pro tento účel je možné použít i certifikát podepsaný svým držitelem, ale musí být importován do úložiště „Důvěryhodné kořenové certifikační autority“ účtu místního počítače na serveru. Nyní může sada SDK webové služby tomuto certifikátu důvěřovat při navazování připojení protokolem SSL.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="step-2-install-the-user-portal"></a>Krok 2: Instalace portálu User Portal
Než začnete instalovat portál User Portal na samostatný server, mějte na paměti následující osvědčené postupy:

* Je užitečné otevřít webový prohlížeč na webovém serveru s přístupem k internetu a přejít na adresu URL sady SDK webové služby, která byla zadána do souboru web.config. Pokud se prohlížeč úspěšně dostane k webové službě, měla by se zobrazit výzva k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které jste zadali do souboru web.config, v naprosto stejném tvaru. Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.
* Pokud se před webovým serverem portálu User Portal nachází reverzní proxy server nebo brána firewall a provádí snižování zátěže protokolu SSL, můžete upravit soubor web.config portálu User Portal a přidat následující klíč do části `<appSettings>` tak, aby User Portal mohl používat protokol http místo protokolu https:

    `<add key="SSL_REQUIRED" value="false"/>`

#### <a name="to-install-the-user-portal"></a>Instalace uživatelského portálu
1. Na serveru s Azure MFA Serverem otevřete Průzkumníka Windows a přejděte do složky, kde je Azure Multi-Factor Authentication Server nainstalován (např. C:\Program Files\Multi-Factor Authentication Server). Zkopírujte na server s přístupem k internetu 32bitovou nebo 64bitovou verzi instalačního souboru MultiFactorAuthenticationUserPortalSetup.
2. Na webovém serveru s přístupem k internetu spusťte tento soubor s oprávněním správce. Nejjednodušším způsobem je otevřít příkazový řádek jako správce a přejít do umístění, kam se zkopíroval instalační soubor.
3. Spusťte instalační soubor MultiFactorAuthenticationUserPortalSetup64, v případě potřeby změňte název Webu a Virtuálního adresáře.
4. Po dokončení instalace portálu User Portal přejděte do C:\inetpub\wwwroot\MultiFactorAuth (nebo odpovídajícího adresáře podle názvu virtuálního adresáře) a upravte soubor web.config.
5. Vyhledejte klíč USE_WEB_SERVICE_SDK a změňte hodnotu z **false** na **true**. 
6. Vyhledejte klíč WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a nastavte hodnotu na uživatelské jméno účtu služby ve skupině zabezpečení PhoneFactor Admins. Použijte kvalifikované uživatelské jméno, například doména\uživatelské_jméno nebo počítač\uživatelské_jméno. 
7. Vyhledejte klíč WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD a nastavte hodnotu na heslo účtu služby ve skupině zabezpečení PhoneFactor Admins.
8. Vyhledejte nastavení pfup_pfwssdk_PfWsSdk a změňte hodnotu z „http://localhost:4898/PfWsSdk.asmx“ na adresu URL sady Web Service SDK, která běží na serveru Azure Multi-Factor Authentication (např. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Vzhledem k tomu, že se pro toto připojení používá protokol SSL, odkazujte na sadu SDK webové služby názvem serveru, a ne IP adresou, protože certifikát SSL byl vydaný pro název serveru. Pokud název serveru neodpovídá IP adrese ze serveru s přístupem k internetu, přidejte záznam do souboru hostitelů na tomto serveru pro namapování názvu Azure Multi-Factor Authentication Serveru na jeho IP adresu. Po provedení změn uložte soubor web.config.

    Další podrobnosti o úpravě konfiguračního souboru najdete v části [Zabezpečení prostředků pomocí Azure Multi-Factor Authentication Serveru se službou AD FS](multi-factor-authentication-get-started-adfs-w2k12.md#edit-the-multifactorauthenticationadfsadapterconfig-file).

7. Pokud web, na který se nainstaloval portál User Portal (například výchozí web), ještě není navázaný na veřejně podepsaný certifikát, nainstalujte certifikát na server, otevřete Správce služby IIS a vytvořte vazbu certifikátu k webu.
8. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL, kam se nainstaloval portál User Portal (např. https://www.verejnyweb.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

### <a name="step-3-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Krok 3: Konfigurace nastavení portálu User Portal v Azure Multi-Factor Authentication Serveru
Teď, když je portál User Portal nainstalovaný, musíte nakonfigurovat Azure Multi-Factor Authentication Server pro práci s portálem.

Server Azure Multi-Factor Authentication poskytuje pro uživatelský portál několik možností.  V následující tabulce je seznam těchto možností a vysvětlení, k čemu se používají.

| Nastavení uživatelského portálu | Popis |
|:--- |:--- |
| Adresa URL portálu User Portal | Zadejte adresu URL, na které se portál hostuje. |
| Primární ověření | Zadejte typ ověřování, které se má použít pro přihlašování k portálu.  Ověření Windows, Radius nebo LDAP. |
| Umožnit uživatelům přihlášení | Umožní uživatelům zadat uživatelské jméno a heslo na přihlašovací stránce portálu User Portal.  Pokud tato možnost není zapnutá, pole budou zobrazena šedě. |
| Povolit zápis uživatele | Umožní uživateli se zapsat do služby Multi-Factor Authentication na stránce nastavení, která ho požádá o další informace, jako je třeba telefonní číslo. Vyzvat k zadání záložního telefonu umožní uživatelům na telefonu zadat záložní telefonní číslo. Výzva k zadání tokenu OATH jiných výrobců umožní uživatelům zadat token OATH od jiného výrobce. |
| Povolit uživatelům zahájení jednorázového přihlášení | Umožní uživatelům zahájení jednorázového přihlášení.  Pokud toto uživatel nastaví, projeví se to při jeho příštím přihlášení. Vyzvat k zadání počtu sekund jednorázového přihlášení zobrazí uživateli pole pro změnu výchozí hodnoty 300 sekund. Jinak bude přeskočení přihlášení platné jen na 300 sekund. |
| Povolit uživatelům výběr metody | Umožní uživatelům vybrat primární způsob kontaktu.  Může to být telefonní hovor, SMS, mobilní aplikace nebo token OATH. |
| Povolit uživatelům výběr jazyka | Umožní uživatelům změnit jazyk telefonního hovoru, zprávy SMS, mobilní aplikace nebo tokenu OATH. |
| Povolit uživatelům aktivaci mobilní aplikace | Umožní uživatelům vygenerovat aktivační kód pro dokončení procesu aktivace mobilní aplikace, který se použije se serverem.  Můžete také nastavit počet zařízení, na kterých mohu aplikaci aktivovat, v rozmezí 1 až 10. |
| V nouzové situaci použít bezpečnostní otázky | Povolí bezpečnostní otázky, pokud se nezdaří dvoustupňové ověřování.  Můžete zadat počet bezpečnostních otázek, které je potřeba správně zodpovědět. |
| Povolit uživatelům přidružení tokenu OATH jiného výrobce | Povolí uživatelům zadat token OATH jiného výrobce. |
| V nouzové situaci použít token OATH | Umožní použití tokenu OAUTH, pokud se nezdaří dvoustupňové ověřování. Můžete taky zadat časový limit relace v minutách. |
| Povolit protokolování | Povolí protokolování na portálu User Portal. Protokoly se ukládají do: C:\Program Files\Multi-Factor Authentication Server\Logs. |

Většina těchto nastavení je pro uživatele viditelná, jakmile jsou povoleni a přihlášeni na portálu User Portal.

![Nastavení uživatelského portálu](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace nastavení uživatelského portálu v Azure Multi-Factor Authentication Serveru
1. V Azure Multi-Factor Authentication Serveru klikněte na ikonu **User Portal**. Na kartě Nastavení zadejte adresu URL portálu User Portal do pole **Adresa URL portálu User Portal**. Pokud je zapnutá funkce e-mailu, tato adresa URL se vloží do e-mailů, které se uživatelům pošlou po jejich importování do Azure Multi-Factor Authentication Serveru.
2. Vyberte nastavení, které chcete v portálu User Portal použít. Pokud si například uživatelé můžou zvolit metodu ověřování, zkontrolujte, že je kromě metod, ze kterých si mohou vybrat, zaškrtnutá i možnost **Povolit uživatelům výběr metody**.
3. Vysvětlení každého zobrazeného nastavení můžete zobrazit kliknutím na odkaz **Nápověda** v pravém horním rohu.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Karta Správci
Na kartě **Správci** můžete přidávat uživatele, kteří budou mít oprávnění správce.  Při přidávání správce můžete podrobně upravit oprávnění, která bude mít. Klikněte na tlačítko **Přidat**, vyberte uživatele a jeho oprávnění a klikněte na **Přidat**.

![Správci uživatelského portálu](./media/multi-factor-authentication-get-started-portal/admin.png)

## <a name="security-questions"></a>Bezpečnostní otázky
Na kartě **Bezpečnostní otázky** můžete zadat bezpečnostní otázky, které budou uživatelé muset zodpovědět, pokud je vybraná možnost **V nouzové situaci použít bezpečnostní otázky**.  Azure Multi-Factor Authentication Server má několik výchozích otázek, které můžete použít. Můžete změnit pořadí otázek nebo přidat svoje vlastní.  Pokud přidáváte vlastní otázky, můžete také vybrat jazyk, ve kterém chcete, aby se otázka zobrazila.

![Bezpečnostní otázky uživatelského portálu](./media/multi-factor-authentication-get-started-portal/secquestion.png)

## <a name="saml"></a>SAML
Na kartě **SAML** můžete nakonfigurovat portál User Portal, aby přijímal deklarace identity od zprostředkovatelů identity pomocí SAML.  Můžete zadat časový limit relace, ověřovací certifikát a Adresu URL pro přesměrování při odhlášení.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Na kartě **Důvěryhodné IP adresy** můžete zadat jednotlivé IP adresy nebo rozsahy IP adres, aby uživatelé, kteří se z těchto adres přihlašují, nemuseli dokončovat dvoustupňové ověřování.

![Důvěryhodné IP adresy uživatelského portálu](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Povolit zápis uživatele k samoobslužné službě
Pokud chcete, aby se uživatelé přihlásili a zapsali, musíte zapnout možnosti **Umožnit uživatelům přihlášení** a **Povolit zápis uživatele** na kartě Nastavení. Nezapomeňte, že nastavení, které vyberete, bude mít vliv přihlašovací prostředí uživatelů.

Pokud se například uživatel přihlásí k portálu User Portal poprvé, bude přesměrován na stránku Azure Multi-Factor Authentication – Nastavení uživatele.  Podle toho, jak jste nakonfigurovali ověřování Azure Multi-Factor Authentication si uživatel možná bude moci vybrat metodu ověření.  

Pokud si vybere metodu ověření hlasovým hovorem nebo je tato možnost předem nastavená, na stránce se zobrazí výzva k zadání primárního telefonního čísla a případně linky.  Možná budou moct zadat záložní telefonní číslo.  

![Důvěryhodné IP adresy uživatelského portálu](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Pokud musí uživatel při ověřování zadat PIN kód, zobrazí se na stránce také výzva k vytvoření PIN kódu.  Po zadání telefonního čísla nebo telefonních čísel a PIN kódu (pokud je potřeba) uživatel klikne na tlačítko **Zavolat mi nyní a ověřit mě**.  Azure Multi-Factor Authentication provede ověření zavoláním na primární telefonní číslo uživatele.  Uživatel musí hovor přijmout, zadat PIN (pokud je tato funkce aktivní) a stisknout #, tím přejde k dalšímu kroku vlastního zápisu.   

Pokud si uživatel vybere metodu ověření SMS zprávou nebo je tato možnost předem nastavená, na stránce se zobrazí výzva k zadání čísla mobilního telefonu.  Pokud musí uživatel při ověření zadat PIN, zobrazí se na stránce výzva k zadání čísla PIN.  Po zadání telefonního čísla a PIN kódu (pokud je potřeba) uživatel klikne na tlačítko **Poslat mi nyní zprávu SMS a ověřit mě**.  Azure Multi-Factor Authentication provede ověření tím, že na mobilní telefon uživatele pošle zprávu SMS.  Uživatel obdrží zprávu SMS s jednorázovým heslem a odpoví na zprávu s použitím jednorázového hesla spolu s PIN kódem (pokud je potřeba).

![SMS uživatelského portálu](./media/multi-factor-authentication-get-started-portal/text.png)   

Pokud si uživatel vybere metodu ověření pomocí mobilní aplikace, na stránce se zobrazí výzva k instalaci aplikace Microsoft Authenticator na mobilní zařízení a vygenerování aktivačního kódu.  Po nainstalování aplikace uživatel klikne na tlačítko Generovat aktivační kód.    

> [!NOTE]
> Pro používání aplikace Microsoft Authenticator je potřeba, aby měl uživatel ve svém zařízení zapnutý příjem nabízených oznámení.

Na stránce se pak zobrazí aktivační kód, adresa URL a čárový kód.  Pokud musí uživatel při ověření zadat PIN, zobrazí se na stránce výzva k zadání čísla PIN.  Uživatel zadá aktivační kód a adresu URL do aplikace Microsoft Authenticator nebo pomocí čtečky čárových kódů naskenuje čárový kód a pak klikne na tlačítko Aktivovat.    

Po dokončení aktivace uživatel klikne na tlačítko **Ověřit mě nyní**.  Azure Multi-Factor Authentication provede ověření přes mobilní aplikaci uživatele.  Uživatel musí zadat PIN (pokud je tato funkce aktivní) a stisknout tlačítko Ověřit, tím přejde k dalšímu kroku vlastního zápisu.  

Pokud správci nastavili Azure Multi-Factor Authentication Server tak, aby shromažďoval bezpečnostní otázky a odpovědi, přesměruje uživatele na stránku s bezpečnostními otázkami.  Uživatel musí vybrat čtyři bezpečnostní otázky a zadat odpovědi na tyto otázky.    

![Bezpečnostní otázky uživatelského portálu](./media/multi-factor-authentication-get-started-portal/secq.png)  

Samoobslužný zápis je teď dokončený a uživatel je přihlášený k portálu User Portal.  V budoucnu se uživatel může kdykoli znovu přihlásit a pokud k tomu má oprávnění od správce, bude moci změnit svá telefonní čísla, PIN kódy, metody ověřování a bezpečnostní otázky.


