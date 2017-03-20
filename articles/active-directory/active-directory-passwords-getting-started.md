---
title: "Začínáme: Správa hesel služby Azure AD | Dokumentace Microsoftu"
description: "Umožněte uživatelům resetovat svá hesla, přečtěte si o požadavcích resetování hesel a povolte zpětný zápis hesla ke správě místních hesel ve službě Active Directory."
services: active-directory
keywords: "Správa hesel služby Active Directory, správa hesel, resetování hesla Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: c40fca54b02f2673194ab16c41314f1e50be12be
ms.lasthandoff: 03/06/2017


---
# <a name="getting-started-with-password-management"></a>Začínáme se správou hesel
> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).
>
>

Pokud chcete uživatelům umožnit spravovat jejich vlastní hesla ke cloudové službě Azure Active Directory nebo místní službě Active Directory, stačí provést několik jednoduchých kroků. Po splnění několika jednoduchých požadavků a provedení několika snadných kroků bude aktivace funkce změny a resetování hesel ve vaší organizaci hned hotová. V tomto článku najdete následující informace:

* [**Jak uživatelům umožnit resetování hesel služby Azure Active Directory**](#enable-users-to-reset-their-azure-ad-passwords)
  * [Požadavky funkce samoobslužného resetování hesel](#prerequisites)
  * [Krok 1: Konfigurace zásad resetování hesel](#step-1-configure-password-reset-policy)
  * [Krok 2: Přidání kontaktních údajů testovacího uživatele](#step-2-add-contact-data-for-your-test-user)
  * [Krok 3: Resetování hesla z pohledu uživatele](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Jak uživatelům umožnit resetování nebo změnu hesel místní služby Azure Active Directory**](#enable-users-to-reset-or-change-their-ad-passwords)
  * [Požadavky pro zpětný zápis hesla](#writeback-prerequisites)
  * [Krok 1: Stažení nejnovější verze služby Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
  * [Krok 2: Povolení zpětného zápisu hesla ve službě Azure AD Connect prostřednictvím uživatelského rozhraní nebo prostředí PowerShell a jeho ověření](#step-2-enable-password-writeback-in-azure-ad-connect)
  * [Krok 3: Konfigurace brány firewall](#step-3-configure-your-firewall)
  * [Krok 4: Nastavení příslušných oprávnění](#step-4-set-up-the-appropriate-active-directory-permissions)
  * [Krok 5: Resetování hesla ke službě AD v roli uživatele a jeho ověření](#step-5-reset-your-ad-password-as-a-user)

## <a name="enable-users-to-reset-their-azure-ad-passwords"></a>Umožnění resetování hesel služby Azure AD uživatelům
V této části najdete informace o postupu povolení samoobslužného resetování hesel cloudového adresáře služby AAD, registrace uživatelů k samoobslužnému resetování hesel a provedení testovacího samoobslužného resetování hesla v roli uživatele.

* [Požadavky funkce samoobslužného resetování hesel](#prerequisites)
* [Krok 1: Konfigurace zásad resetování hesel](#step-1-configure-password-reset-policy)
* [Krok 2: Přidání kontaktních údajů testovacího uživatele](#step-2-add-contact-data-for-your-test-user)
* [Krok 3: Resetování hesla z pohledu uživatele](#step-3-reset-your-azure-ad-password-as-a-user)

### <a name="prerequisites"></a>Požadavky
Aby bylo možné povolit a používat samoobslužné resetování hesel, je nutné nejprve splnit následující požadavky:

* Vytvořte klienta služby AAD. Další informace najdete v článku [Začínáme se službou Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/)
* Pořiďte si předplatné Azure. Další informace najdete v článku [Ce je klient služby Azure AD?](active-directory-administer.md#what-is-an-azure-ad-tenant).
* Asociujte klienta služby AAD s předplatným Azure. Další informace najdete v článku [Asociování předplatných Azure se službou Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).
* Upgradujte na službu Azure AD Premium nebo Basic nebo použijte placené licence služby O365. Další informace najdete v článku [Edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

  > [!NOTE]
  > Pokud chcete povolit samoobslužné resetování hesel uživatelům cloudu, je nutné upgradovat na službu Azure AD Premium nebo Azure AD Basic nebo získat placenou licenci ke službě O365.  Pokud chcete povolit samoobslužné resetování hesel místním uživatelům, je nutné upgradovat na službu Azure AD Premium. Další informace najdete v článku [Edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). V těchto článcích najdete podrobné pokyny k registraci služeb Azure AD Premium a Basic, aktivaci plánu licencování a přístupu ke službě Azure AD a přiřazení přístupu účtům uživatelů a správců.
  >
  >
* V adresáři služby AAD vytvořte alespoň jeden účet správce a jeden účet uživatele.
* K vytvořeným účtům správce a uživatele přiřaďte placenou licenci služby AAD Premium, Basic nebo O365.

### <a name="step-1-configure-password-reset-policy"></a>Krok 1: Konfigurace zásad resetování hesel
Zásady konfigurace resetování hesel uživatelů nastavte pomocí následujících kroků:

1. Otevřete libovolný prohlížeč a přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. Na [portálu Azure Classic](https://manage.windowsazure.com) v navigačním panelu vlevo vyhledejte **Rozšíření Active Directory**.

   ![Správa hesel ve službě Azure AD][001]
3. Na kartě **ADRESÁŘ** klikněte na adresář, ve kterém chcete konfigurovat zásady změn hesel uživateli, například na adresář Wingtip Toys.

    ![][002]
4. Klikněte na kartu **KONFIGUROVAT**.

   ![][003]

5. Na kartě **Konfigurovat** přejděte dolů k části **zásady resetování hesel uživatelů**.  Zde se konfigurují všechny aspekty zásad resetování hesel uživatelů pro daný adresář. *Pokud kartu Konfigurovat nevidíte, zkontrolujte, jestli jste si zaregistrovali službu Azure Active Directory Premium nebo Basic a __přiřadili licenci__ účtu správce, který tuto funkci konfiguruje.*  

   > [!NOTE]
   > **Zásady, které nastavíte, platí jenom pro koncové uživatele ve vaší organizaci, a ne pro správce**. Zásady resetování hesel správců z bezpečnostních důvodů monitoruje Microsoft. Aktuální zásady pro správce vyžadují dva údaje – mobilní telefon a e-mailovou adresu.

   ![][004]
6. Abyste mohli konfigurovat zásady resetování hesel uživateli, přepněte přepínač **uživatelé mohou resetovat heslo** do polohy **ano**.  Zobrazí se několik dalších ovládacích prvků, které vám umožní konfigurovat, jak bude tato funkce ve vašem adresáři fungovat.  Funkci resetování hesla si pomocí nastavení přizpůsobte podle svých potřeb.  Další informace o tom, jak fungují jednotlivé zásady pro resetování hesel, najdete v článku [Přizpůsobení: Správa hesel služby Azure AD](active-directory-passwords-customize.md).

   ![][005]
7. Po provedení konfigurace zásad resetování hesel uživateli podle požadavků pro svého klienta klikněte na tlačítko **Uložit** v dolní části obrazovky.

   > [!NOTE]
   > Doporučuje se použít dvoustupňové zásady resetování hesel, abyste viděli, jak funkce funguje v komplikovanějších případech.
   >
   >

   ![][006]

   > [!NOTE]
   > **Zásady, které nastavíte, platí jenom pro koncové uživatele ve vaší organizaci, a ne pro správce**. Zásady resetování hesel správců z bezpečnostních důvodů monitoruje Microsoft. Aktuální zásady pro správce vyžadují dva údaje – mobilní telefon a e-mailovou adresu.
   >
   >

### <a name="step-2-add-contact-data-for-your-test-user"></a>Krok 2: Přidání dat kontaktu testovacího uživatele
Máte několik možností, jak můžete zadat data uživatelů ve své organizaci, která se budou používat k resetování hesel.

* Uživatele můžete upravovat na [portálu Azure Classic](https://manage.windowsazure.com) nebo na [Portálu pro správu Office 365](https://portal.microsoftonline.com).
* Vlastnosti uživatelů můžete z místní domény služby Active Directory synchronizovat do služby Azure AD pomocí služby AAD Connect.
* Vlastnosti uživatelů můžete upravit pomocí PowerShellu.
* Uživatelům můžete umožnit, aby si zaregistrovali svá vlastní data na portálu registrace na adrese [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup).
* Od uživatelů můžete požadovat, aby si zaregistrovali funkci resetování hesla při přihlášení k přístupovému panelu na adrese [http://myapps.microsoft.com](http://myapps.microsoft.com) nastavením možnosti **Vyžadovat od uživatelů registraci při přihlašování k přístupovému panelu** na **Ano**.

Další informace o tom, jaká data používá funkce resetování hesla a jaké jsou požadavky na jejich formátování naleznete v tématu [Jaká data používá funkce resetování hesla?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

#### <a name="to-add-user-contact-data-via-the-user-registration-portal"></a>Přidání kontaktních údajů uživatele pomocí portálu pro registraci uživatelů
1. Aby mohli uživatelé ve vaší organizaci použít registrační portál pro resetování hesla, musíte jim poskytnout odkaz na tuto stránku  ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) nebo zapnout možnost vyžadující automatickou registraci uživatelů.  Po kliknutí na tento odkaz jsou vyzváni k přihlášení pomocí účtu své organizace.   Po přihlášení se jim zobrazí následující stránka:

   ![][007]
2. Zde mohou uživatelé zadat a ověřit své mobilní telefonní číslo, alternativní e-mailovou adresu a bezpečnostní otázky.  Ověřování čísla mobilního telefonu vypadá takto:

   ![][008]
3. Když uživatel tyto informace zadá, stránka se aktualizuje a indikuje, že jsou platné (na obrázku níže jsou údaje začerněny).  Po kliknutí na tlačítko **dokončit** nebo **storno** je uživatel přesměrován zpět na přístupový panel.

   ![][009]
4. Když uživatel informace potvrdí, jeho profil se aktualizuje zadanými daty.  V tomto případě byl ručně zadáno **telefonní číslo do kanceláře**, takže uživatel může při resetování hesla využít i tento komunikační kanál.

   ![][010]

### <a name="step-3-reset-your-azure-ad-password-as-a-user"></a>Krok 3: Resetování hesla ke službě Azure AD v roli uživatele
Nyní jsou konfigurované zásady resetování hesel a zadány kontaktní údaje uživatele, a uživatel tedy může provést samoobslužné resetování hesla.

#### <a name="to-perform-a-self-service-password-reset"></a>Provedení samoobslužného resetování hesla
1. Když přejdete na web, jako je například [**portal.microsoftonline.com**](http://portal.microsoftonline.com), uvidíte přihlašovací obrazovku podobnou obrazovce níže.  Kliknutím na odkaz **Nezdařil se přístup k účtu?** otestujte uživatelské rozhraní pro resetování hesla.

   ![][011]
2. Po kliknutí na odkaz **Nezdařil se přístup k účtu?** budete přesměrováni na novou stránku s výzvou k zadání **ID uživatele**, jehož heslo chcete resetovat.  Zadejte testovací **ID uživatele**, splňte podmínky testu CAPTCHA a klikněte na **další**.

   ![][012]
3. Protože v tomto případě uživatel zadal **telefonní číslo do kanceláře**, **mobilní telefonní číslo** a **alternativní e-mailovou adresu**, uvidíte, že jsou mu v prvním kroku nabídnuty všechny tyto možnosti.

   ![][013]
4. V tomto případě vyberte nejprve možnost **Zavolat** **mi na telefon do kanceláře**.  Všimněte si, že při zadání metody založené na telefonu jsou uživatelé předtím, než mohou resetovat svá hesla, vyzváni k **ověření svého telefonního čísla**.  Účelem tohoto kroku je zabránit jednotlivcům se zlými úmysly ve zneužití telefonních čísel uživatelů ve vaší organizaci.

   ![][014]
5. Když uživatelé své telefonní číslo potvrdí, po kliknutí na tlačítko volání se zobrazí ukazatel průběhu a jejich telefon začne zvonit.  Po přijetí hovoru se uživatelům přehraje zpráva s výzvou, **aby stisknutím křížku** potvrdili svůj účet.  Stisknutím tohoto tlačítka uživatelé automaticky splní první krok a přejdou k druhému kroku ověření.

   ![][015]
6. Po splnění prvního kroku je uživatelské rozhraní automaticky aktualizováno a použitá možnost je odebrána ze seznamu nabízených možností.  V tomto případě, protože jste nejdřív použili svoje **telefonní číslo do kanceláře**, zůstanou jako možnosti ověření v druhém kroku aktivní pouze možnosti **Mobilní telefon** a **Alternativní e-mailová adresa**.  Klikněte na **Zaslat e-mail na mou alternativní e-mailovou adresu**.  Když nyní kliknete na zobrazené tlačítko odeslání e-mailu, odešle se na alternativní e-mailovou adresu e-mail.

   ![][016]
7. Zde je ukázka e-mailu, který uživatel dostane:

   ![][017]
8. Když je e-mail doručen, stránka se aktualizuje a do vstupního pole dole budete moct zadat ověřovací kód, který najdete v e-mailu.  Po zadání správného kódu se aktivuje tlačítko Další a budete moci splnit druhý krok ověření.

   ![][018]
9. Po splnění požadavků zásad organizace si můžete zvolit nové heslo.  Na základě požadavků služby AAD na sílu hesel je vyhodnocena síla hesla (viz [Zásady hesel ve službě Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx)) a zobrazí se validátor síly, který značí zda zadané heslo splňuje požadavky zásad.

   ![][019]
10. Po zadání dvou stejných hesel splňujících zásady organizace je vaše heslo resetováno a můžete se okamžitě přihlásit pomocí nového hesla.

    ![][020]

## <a name="enable-users-to-reset-or-change-their-ad-passwords"></a>Umožnění resetování a změn hesel služby AD uživatelům
V této části najdete postup konfigurace funkce resetování hesel k jejich zpětnému zápisu do místní služby Active Directory.

* [Požadavky pro zpětný zápis hesla](#writeback-prerequisites)
* [Krok 1: Stažení nejnovější verze služby Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
* [Krok 2: Povolení zpětného zápisu hesla ve službě Azure AD Connect prostřednictvím uživatelského rozhraní nebo prostředí PowerShell a jeho ověření](#step-2-enable-password-writeback-in-azure-ad-connect)
* [Krok 3: Konfigurace brány firewall](#step-3-configure-your-firewall)
* [Krok 4: Nastavení příslušných oprávnění](#step-4-set-up-the-appropriate-active-directory-permissions)
* [Krok 5: Resetování hesla ke službě AD v roli uživatele a jeho ověření](#step-5-reset-your-ad-password-as-a-user)

### <a name="writeback-prerequisites"></a>Požadavky pro zpětný zápis
Aby bylo možné povolit a používat zpětný zápis hesla, je nutné nejprve splnit následující požadavky:

* Je nutné mít klienta služby Azure AD s aktivovaným předplatným Azure AD Premium.  Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).
* V klientu je nutné konfigurovat a povolit resetování hesel.  Další informace najdete v tématu [Umožnění resetování hesel služby Azure AD uživatelům](#enable-users-to-reset-their-azure-ad-passwords)
* Je nutné mít alespoň jeden účet správce a jeden testovací uživatelský účet s licencí Azure AD Premium, který použijete k otestování této funkce.  Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

  > [!NOTE]
  > Ujistěte se, že účet správce, který používáte k povolení zpětného zápisu hesla, je cloudový účet správce (vytvořený ve službě Azure AD), nikoliv federovaný účet (vytvořený v místní službě AD a synchronizovaný se službou Azure AD).
  >
  >
* Je nutné mít místní nasazení jednoduché nebo vícenásobné doménové struktury se systémem Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 nebo Windows Server 2012 R2 s nejnovějšími aktualizacemi Service Pack.

  > [!NOTE]
  > Pokud používáte starší verzi systému Windows Server 2008 nebo 2008 R2, můžete tuto funkci také využívat, ale aby bylo možné vynutit místní zásady hesel služby AD v cloudu, bude nejprve nutné [stáhnout a nainstalovat komponenty popsané v článku znalostní báze KB 2386717](https://support.microsoft.com/kb/2386717).
  >
  >
* Je nutné mít nainstalovaný nástroj Azure AD Connect a prostředí služby AD musí být připravené na synchronizaci s cloudem.  Další informace najdete v tématu [Použití místní infrastruktury identity v cloudu](connect/active-directory-aadconnect.md).

  > [!NOTE]
  > Před otestováním zpětného zápisu hesla je nutné nejprve dokončit úplný import a synchronizaci ze služeb AD a Azure AD ve službě Azure AD Connect.
  >
  >
* Pokud používáte odchozí připojení nástroje Azure AD Sync nebo služby Azure AD Connect, port **TCP 443** (a v některých případech **TCP 9350-9354**) musí být otevřený.  Další informace najdete v části [Krok 3: Konfigurace brány firewall](#step-3-configure-your-firewall). Použití nástroje DirSync v tomto scénáři není nadále podporováno.  Pokud stále používáte nástroj DirSync, před nasazením zpětného zápisu hesla prosím upgradujte na nejnovější verzi služby Azure AD Connect.

  > [!NOTE]
  > Všem uživatelům používajícím nástroje Azure AD Sync nebo DirSync důrazně doporučujeme, aby k dosažení nejlepší funkce služby a k získávání nových funkcí při jejich vydání upgradovali na nejnovější verzi služby Azure AD Connect.
  >
  >

### <a name="step-1-download-the-latest-version-of-azure-ad-connect"></a>Krok 1: Stažení nejnovější verze služby Azure AD Connect
Zpětný zápis hesla je k dispozici ve vydáních služby Azure AD Connect nebo nástroje Azure AD Sync s číslem verze **1.0.0419.0911** nebo vyšším.  Zpětný zápis hesla s automatickým odemknutím účtu je k dispozici ve vydáních služby Azure AD Connect a nástroje Azure AD Sync s číslem verze **1.0.0485.0222** nebo vyšším. Pokud používáte starší verzi, než budete pokračovat, upgradujte prosím alespoň na tuto verzi. [Nejnovější verzi služby Azure AD Connect si můžete stáhnout po kliknutí sem](connect/active-directory-aadconnect.md#install-azure-ad-connect).

#### <a name="to-check-the-version-of-azure-ad-sync"></a>Kontrola verze služby Azure AD Sync
1. Přejděte do složky **%ProgramFiles%\Azure Active Directory Sync\**.
2. Vyhledejte spustitelný soubor **ConfigWizard.exe**.
3. Klikněte na spustitelný soubor pravým tlačítkem a v místní nabídce vyberte položku **Vlastnosti**.
4. Klikněte na kartu **Podrobnosti**.
5. Vyhledejte pole **Verze souboru**.

   ![][021]

Pokud je toto číslo verze větší než nebo rovno číslu **1.0.0419.0911** nebo pokud instalujete službu Azure AD Connect, můžete přeskočit k oddílu [Krok 2: Povolení zpětného zápisu hesla ve službě Azure AD Connect prostřednictvím uživatelského rozhraní nebo prostředí PowerShell a ověření povolení](#step-2-enable-password-writeback-in-azure-ad-connect).

> [!NOTE]
> Pokud instalujete nástroj Azure AD Connect poprvé, doporučuje se, abyste provedli několik osvědčených postupů k přípravě svého prostředí na synchronizaci adresáře.  Před instalací nástroje Azure AD Connect je nutné aktivovat synchronizaci adresáře na [Portálu pro správu Office 365](https://portal.microsoftonline.com) nebo na [portálu Azure Classic](https://manage.windowsazure.com).  Další informace najdete v článku [Správa koncových bodů streamování](active-directory-aadconnect-whats-next.md).
>
>

### <a name="step-2-enable-password-writeback-in-azure-ad-connect"></a>Krok 2: Povolení zpětného zápisu hesla ve službě Azure AD Connect
Po stažení nástroje Azure AD Connect můžete povolit zpětný zápis hesla.  Můžete to provést dvěma způsoby.  Zpětný zápis hesla můžete povolit na obrazovce volitelných funkcí průvodce nastavením služby Azure AD Connect, nebo pomocí Windows PowerShellu.

#### <a name="to-enable-password-writeback-in-the-configuration-wizard"></a>Povolení zpětného zápisu hesla v průvodci konfigurací
1. Na **počítači se službou Directory Sync** spusťte průvodce konfigurací služby **Azure AD Connect**.
2. Projděte jednotlivými kroky konfigurace až k obrazovce **volitelných funkcí**.
3. Zaškrtněte možnost **Zpětný zápis hesla**.

   ![][022]
4. Dokončete průvodce – na poslední stránce se zobrazí souhrn provedených změn včetně změny konfigurace zpětného zápisu hesla.

> [!NOTE]
> Zpětný zápis hesla můžete kdykoliv vypnout opětovným spuštěním průvodce a zrušením výběru funkce nebo nastavením možnosti **Zapisovat hesla zpět do místního adresáře** na hodnotu **Ne** v části **Zásady resetování hesel uživateli** karty **Konfigurovat** svého adresáře na [portálu Azure Classic](https://manage.windowsazure.com).  Další informace o přizpůsobení funkcí resetování hesel nejdete v článku [Přizpůsobení: Správa hesel služby Azure AD](active-directory-passwords-customize.md).
>
>

#### <a name="to-enable-password-writeback-using-windows-powershell"></a>Povolení zpětného zápisu hesla pomocí Windows PowerShellu
1. Na **počítači se službou Directory Sync** otevřete nové  ** okno Windows PowerShellu se zvýšenými oprávněními**.
2. Pokud modul dosud není načtený, načtěte zadáním příkazu `import-module ADSync` do své aktuální relace rutiny služby Azure AD Connect.
3. Spuštěním rutiny `Get-ADSyncConnector` získejte seznam konektorů služby Azure AD a uložte výsledky do atributu `$aadConnectorName`, například `$connectors = Get-ADSyncConnector|where-object {$\_.name -like "\*AAD"}`
4. Spuštěním následující rutiny získejte aktuální stav zpětného zápisu pro aktuální konektor: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5. Spuštěním rutiny povolte zpětný zápis hesla: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`.

> [!NOTE]
> Pokud budete vyzváni k zadání přihlašovacích údajů, ujistěte se, že účet správce, který jste zadali jako parametr AzureADCredential, je **cloudový účet správce (vytvořený ve službě Azure AD)**, nikoliv federovaný účet (vytvořený v místní službě AD a synchronizovaný se službou Azure AD).
>
> [!NOTE]
> Zpětný zápis hesla můžete kdykoliv opět vypnout pomocí prostředí PowerShell zopakováním výše uvedených kroků, ale předáním hodnoty `$false` v kroku&5;, nebo nastavením možnosti **Zapisovat hesla zpět do místního adresáře** na hodnotu **Ne** v části **Zásady resetování hesel uživateli** karty **Konfigurovat** svého adresáře na [portálu Azure Classic](https://manage.windowsazure.com).
>
>

#### <a name="verify-that-the-configuration-was-successful"></a>Ověření úspěšnosti konfigurace
Pokud bude konfigurace úspěšná, zobrazí se v okně Windows PowerShellu zpráva „Zpětný zápis resetování hesla je povolen.“ nebo zpráva o úspěšném provedení konfigurace v uživatelském rozhraní konfigurace.

Správnost instalace služby můžete ověřit také otevřením Prohlížeče událostí, přechodem k protokolu událostí aplikace a vyhledáním události **31005 - OnboardingEventSuccess** ze zdroje **PasswordResetService**.

  ![][023]

### <a name="step-3-configure-your-firewall"></a>Krok 3: Konfigurace brány firewall
Po povolení zpětného zápisu hesla je třeba zajistit, aby měl počítač, na kterém běží služba Azure AD Connect, přístup ke cloudovým službám Microsoftu pro příjem žádostí o zpětný zápis hesla. Tento krok zahrnuje aktualizaci pravidel připojení v síťových zařízeních (proxy servery, brány firewall atd.) pro povolení odchozích připojení k určitým [adresám URL a IP adresám vlastněným Microsoftem](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US) přes konkrétní síťové porty. Tyto změny se mohou lišit v závislosti na verzi nástroje Azure AD Connect. Pro širší kontext si můžete přečíst o tom, [jak funguje zpětný zápis hesla](active-directory-passwords-learn-more.md#how-password-writeback-works), a o [modelu zabezpečení zpětného zápisu hesla](active-directory-passwords-learn-more.md#password-writeback-security-model).

#### <a name="why-do-i-need-to-do-this"></a>Proč je nutné to udělat?

Aby zpětný zápis hesla fungoval správně, musí být počítač, na kterém běží služba Azure AD Connect, schopný navázat odchozí připojení HTTPS k webu **.servicebus.windows.net* a ke konkrétním IP adresám, které Azure používá a které jsou definovány v [seznamu rozsahů IP adres datacentra Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

Pro nástroj Azure AD Connect verze **1.1.439.0** (nejnovější) a vyšší:

- Nejnovější verze nástroje Azure AD Connect bude přes **odchozí připojení HTTPS** potřebovat přístup k:
    - *passwordreset.microsoftonline.com*
    - *servicbus.windows.net*

Pro nástroj Azure AD Connect verze **1.0.8667.0** až **1.1.380.0**:

- **Možnost 1:** Povolení všech odchozích připojení HTTPS přes port 443 (pomocí adresy URL nebo IP adresy).
    - Kdy to použít:
        - Tuto možnost použijte, pokud chcete co nejjednodušší konfiguraci, kterou nebude nutné aktualizovat v případě budoucích změn rozsahů IP adres datacentra Azure.
    - Požadované kroky:
        - Povolení všech odchozích připojení HTTPS přes port 443 pomocí adresy URL nebo IP adresy.
<br><br>
- **Možnost 2:** Povolení odchozích připojení HTTPS ke konkrétním adresám URL a rozsahům IP adres.
    - Kdy to použít:
        - Tuto možnost použijte, pokud se nacházíte v prostředí omezené sítě nebo pokud z jakéhokoli důvodu nechcete povolit odchozí připojení.
        - V této konfiguraci bude nutné pro zajištění fungování zpětného zápisu hesla zajistit týdenní aktualizace vašich síťových zařízení pomocí nejnovějších IP adres ze seznamu rozsahů IP adres datacentra Microsoft Azure. Tyto rozsahy IP adres jsou k dispozici v podobě souboru XML, který se aktualizuje každou středu (Tichomoří) a začíná platit následující pondělí (Tichomoří).
    - Požadované kroky:
        - Povolení všech odchozích připojení HTTPS k webu *.servicebus.windows.net
        - Povolení všech odchozích připojení HTTPS ke všem IP adresám v seznamu rozsahů IP adres datacentra Microsoft Azure a týdenní udržování aktualizované konfigurace

> [!NOTE]
> Pokud jste nakonfigurovali zpětný zápis hesla podle výše uvedených pokynů a v protokolu událostí služby Azure AD Connect se nezobrazí žádné chyby, ale během testování dochází k chybám připojení, příčinou může být některé síťové zařízení ve vašem prostředí, které blokuje připojení HTTPS k požadovaným IP adresám. Například zatímco připojení k webu *https://*.servicebus.windows.net* je povolené, připojení ke konkrétní IP adrese v tomto rozsahu může být blokované. Pokud chcete tento problém vyřešit, budete muset buď nakonfigurovat své síťové prostředí tak, aby povolovalo odchozí připojení HTTPS přes port 443 ke všem adresám URL nebo IP adresám (viz Možnost 1 výše), nebo ve spolupráci se síťovým týmem explicitně povolit připojení HTTPS ke konkrétním IP adresám (viz Možnost 2 výše).

**Pro starší verze:**

- Povolte odchozí připojení TCP přes porty 443, 9350-9354 a 5671.
- Povolte odchozí připojení k webu *https://ssprsbprodncu-sb.accesscontrol.windows.net/*.

> [!NOTE]
> Pokud používáte verzi služby Azure AD Connect starší než 1.0.8667.0, Microsoft důrazně doporučuje upgrade na [nejnovější verzi služby Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594), která zahrnuje řadu vylepšení zpětného zápisu přes síť, která zjednodušují konfiguraci.

Jakmile budou síťová zařízení nakonfigurována, restartujte počítač, na kterém běží nástroj Azure AD Connect.

#### <a name="idle-connections-on-azure-ad-connect-114390-and-up"></a>Nečinná připojení v nástroji Azure AD Connect (verze&1;.1.439.0 a vyšší)
Nástroj Azure AD Connect bude pravidelně odesílat příkazy Ping nebo Keepalive do koncových bodů ServiceBus pro zajištění aktivních připojení. Pokud nástroj zjistí, že se ukončuje příliš mnoho připojení, automaticky zvýší frekvenci odesílání příkazu Ping do koncového bodu. Frekvence odesílání příkazu Ping klesne maximálně na 1 příkaz Ping každých 60 sekund, přesto **důrazně doporučujeme, aby proxy servery a brány firewall umožňovaly zachování nečinných připojení po dobu alespoň 2 až 3 minut.** \*Pro starší verze doporučujeme 4 minuty nebo déle.

### <a name="step-4-set-up-the-appropriate-active-directory-permissions"></a>Krok 4: Nastavení odpovídajících oprávnění služby Active Directory
Pro každou doménovou strukturu obsahující uživatele, jejichž hesla budou resetována, platí, že pokud X je účet určený pro danou doménovou strukturu v průvodci konfigurací (během počáteční konfigurace), pak je účtu X nutné přidělit rozšířená oprávnění **resetovat heslo**, **změnit heslo**, **oprávnění k zápisu** do atributu `lockoutTime` a **oprávnění k zápisu** do atributu `pwdLastSet` v kořenu každé domény v této struktuře. Tato oprávnění je nutné označit jako děděná všemi uživatelskými objekty.  

Pokud si nejste jistí, kterého účtu se výše uvedené informace týkají, otevřete uživatelské rozhraní konfigurace služby Azure Active Directory Connect a klikněte na možnost **Kontrola řešení**.  Účet, ke kterému je nutné přidat oprávnění, je na snímku obrazovky níže červeně podtržen.

**<font color="red">Ujistěte se, že jste toto oprávnění ve svém systému nastavili pro všechny domény ve všech strukturách, jinak zpětný zápis hesla nebude správně fungovat.</font>**

  ![][032]

  Nastavení těchto oprávnění umožní účtu služby MA pro každou doménovou strukturu spravovat hesla v zastoupení uživatelských účtů v dané doménové struktuře. Pokud toto přiřazení oprávnění neprovedete, uživatelé se budu při pokusech o správu svých místních hesel v cloudu setkávat s chybami, i když to bude vypadat, že je zpětný zápis hesel správně konfigurován. Zde je podrobný popis postupu provedení této konfigurace pomocí modulu snap-in **Uživatelé a počítače služby Active Directory**:

> [!NOTE]
> Replikace těchto oprávnění u všech objektů ve vašem adresáři může trvat až hodinu.
>
>

#### <a name="to-set-up-the-right-permissions-for-writeback-to-occur"></a>Nastavení potřebných oprávnění pro zpětný zápis hesla
1. Pomocí účtu, který má odpovídající oprávnění pro správu domény, spusťte modul snap-in **Uživatelé a počítače služby Active Directory**.
2. Ujistěte se, že v části **Nabídka zobrazit** je zapnutá možnost  **Upřesňující funkce**.
3. Na levém panelu klikněte na objekt, který reprezentuje kořen domény.
4. Klikněte na kartu **Zabezpečení**.
5. Poté klikněte na kartu **Upřesnit**.

   ![][024]
6. Na kartě **Oprávnění** klikněte na **Přidat**.

   ![][025]
7. Vyberte účet, kterému chcete udělit oprávnění (jedná se o stejný účet, který byl určen při nastavování synchronizace dané doménové struktury).
8. V rozevírací nabídce nahoře vyberte **Podřízené uživatelské objekty**.
9. V zobrazeném dialogu **Položka oprávnění** zaškrtněte políčka u oprávnění **Resetovat heslo**, **Změnit heslo**, **Zápis** `lockoutTime` a **Zápis** `pwdLastSet`.

   ![][026]
   ![][027]
   ![][028]
10. Poté ve všech otevřených dialozích klikněte na **Použít/OK**.

### <a name="step-5-reset-your-ad-password-as-a-user"></a>Krok 5: Resetování hesla z pohledu uživatele
Zpětný zápis hesla je povolený a resetováním hesla uživatele, jehož účet byl synchronizován do vašeho cloudového klienta, můžete ověřit, zda funguje.

#### <a name="to-verify-password-writeback-is-working-properly"></a>Ověření správné funkce zpětného zápisu hesla
1. Přejděte na adresu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) nebo jakoukoliv jinou přihlašovací obrazovku své organizace a klikněte na odkaz **Nezdařil se přístup k účtu?**.

   ![][029]
2. Nyní by se měla zobrazit nová stránka vyžadující zadání ID uživatele, jehož heslo chcete obnovit. Zadejte ID testovacího uživatele a pokračujte v procesu resetování hesla.
3. Po dokončení resetování hesla se zobrazí obrazovka podobná této. Znamená to, že jste úspěšně resetovali heslo ve svých místních i cloudových adresářích.

   ![][030]
4. Abyste ověřili, že operace byla úspěšná či diagnostikovali případné chyby, přejděte do **počítače se službou Directory Sync**, otevřete  **Prohlížeč událostí**, přejděte k **protokolu událostí aplikace** a vyhledejte událost **31002 - PasswordResetSuccess** ze zdroje **PasswordResetService** pro svého testovacího uživatele.

   ![][031]



## <a name="next-steps"></a>Další kroky
Níže naleznete odkazy na všechny stránky dokumentace k resetování hesel služby Azure AD:

* **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Přečtěte si o šesti různých komponentách služby a o tom, jaké mají funkce.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přečtěte si, jak můžete přizpůsobit vzhled a funkce služby potřebám své organizace.
* [**Osvědčené postupy**](active-directory-passwords-best-practices.md) – Přečtěte si, jak můžete ve své organizaci rychle nasazovat a efektivně spravovat hesla.
* [**Analýzy**](active-directory-passwords-get-insights.md) – Přečtěte si o integrovaných funkcích generování sestav.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md)  – Získejte odpovědi na časté otázky.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Přečtěte si, jak můžete rychle řešit potíže se službou.
* [**Další informace**](active-directory-passwords-learn-more.md) – Prostudujte si podrobné technické informace o tom, jak služba funguje.

[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"

