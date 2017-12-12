---
title: "Konfigurace vícefaktorového ověřování Azure | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication, která popisuje, jak postupovat pomocí vícefaktorového ověřování.  To zahrnuje sestavy, upozornění na podvod, jednorázové přihlášení, vlastní hlasové zprávy, ukládání do mezipaměti, důvěryhodné IP adresy a aplikaci hesla."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: joflore
ms.openlocfilehash: a6064e33dca09a304074efa4029c0f96fc19b2a2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurovat nastavení ověřování Azure Multi-Factor Authentication
Tento článek usnadňuje správu ověřování Azure Multi-Factor Authentication teď, když jsou spuštěná.  Vysvětluje různé témata, které umožňují využívat naplno Azure Multi-Factor Authentication.  Ne všechny tyto funkce jsou dostupné v každé verzi Azure Multi-Factor Authentication.

| Funkce | Popis | 
|:--- |:--- |
| [Upozornění na podvod](#fraud-alert) |Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy podvodné pokouší získat přístup k jejich prostředky. |
| [Jednorázové přihlášení](#one-time-bypass) |Jednorázové přihlášení umožňuje uživateli jednorázově ověřit pomocí "obcházení" služby Multi-Factor authentication. |
| [Vlastní hlasové zprávy](#custom-voice-messages) |Vlastní hlasové zprávy umožňují použít vlastní záznamy nebo pozdrav pomocí služby Multi-Factor authentication. |
| [Ukládání do mezipaměti](#caching-in-azure-multi-factor-authentication) |Ukládání do mezipaměti umožňuje nastavit určité časové období, aby automaticky být úspěšné následné pokusy o ověření. |
| [Důvěryhodné IP adresy](#trusted-ips) |Správci spravované nebo federované klienta můžete použít důvěryhodné IP adresy obejít dvoustupňové ověření pro uživatele, kteří se přihlašují z místního intranetu společnosti. |
| [Hesla aplikací](#app-passwords) |Heslo aplikace umožňuje aplikaci, která MFA neví obejít ověřování Multi-Factor authentication a pokračovat v práci. |
| [Zapamatovat Vícefaktorové ověřování na zapamatovaných zařízeních a prohlížeče](#remember-multi-factor-authentication-for-devices-that-users-trust) |Umožňuje mějte na paměti, zařízení pro sadu počet dnů po uživatel se úspěšně přihlásil pomocí vícefaktorového ověřování. |
| [Volitelný ověření metody](#selectable-verification-methods) |Umožňuje výběr metod ověřování, které jsou k dispozici pro uživatelům používat. |

## <a name="access-the-azure-mfa-management-portal"></a>Přístup k portálu pro správu Azure MFA

Funkce popsaná v tomto článku jsou nakonfigurované v portálu Azure Multi-Factor Authentication Management Portal. Existují dva způsoby přístup k portálu pro správu MFA prostřednictvím portálu Azure classic. Prvním je přímá správa zprostředkovatel vícefaktorového ověřování. Druhý probíhá přes nastavení služby vícefaktorového ověřování. 

### <a name="use-an-authentication-provider"></a>Použití zprostředkovatele ověřování

Pokud používáte zprostředkovatel vícefaktorového ověřování na základě spotřeby mfa, můžete tuto metodu použijte pro přístup k portálu pro správu.

Pro přístup k portálu pro správu MFA prostřednictvím poskytovatele Azure Multi-Factor Auth, přihlaste se k portálu Azure classic jako správce a vyberte možnost Active Directory. Klikněte na tlačítko **zprostředkovatelé vícefaktorového ověřování** , pak vyberte adresář a klikněte **spravovat** tlačítko dole.

### <a name="use-the-mfa-service-settings-page"></a>Na stránce nastavení služby vícefaktorového ověřování 

Pokud máte jeden z následujících licencí, můžete použít stránku nastavení vícefaktorového ověřování služby:
- Azure MFA
- Azure AD Premium 
- Enterprise Mobility + Security

Pro přístup k portálu pro správu MFA prostřednictvím stránky nastavení vícefaktorového ověřování služby, přihlaste se k portálu Azure classic jako správce a vyberte možnost Active Directory. Klikněte na adresáře a pak klikněte na kartu **Konfigurovat**. V části ověřování vícefaktorového ověřování vyberte **Spravovat nastavení služby**. V dolní části stránky nastavení služby MFA klikněte na odkaz **Přejít na portál**.


## <a name="fraud-alert"></a>Upozornění na podvod
Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy podvodné pokouší získat přístup k jejich prostředky.  Uživatelé mohou zasílat zprávy podvod pomocí mobilní aplikace nebo přes svůj telefon.

### <a name="set-up-fraud-alert"></a>Nastavení upozornění na podvod
1. Přejděte do portálu pro správu MFA podle pokynů v horní části této stránky.
2. V portálu Azure Multi-Factor Authentication Management Portal, klikněte na **nastavení** v části konfigurace.
3. Upozornění na podvod části na stránku nastavení, zkontrolujte **povolit uživatelům odesílat upozornění na podvod** zaškrtávací políčko.
4. Vyberte **Uložit** proveďte změny. 

### <a name="configuration-options"></a>Možnosti konfigurace

- **Blokování uživatele při nahlášení podvodu** – Pokud uživatel sestavy podvod, svůj účet je blokován.
- **Kód pro sestavu podvod během počáteční s pozdravem** -uživatelé obvykle stisknutím klávesy # potvrzení dvoustupňové ověřování. Pokud chcete ohlásit podvod, jejich zadat kód ještě před stisknutím klávesy #. Tento kód je **0** ve výchozím nastavení, ale můžete jej upravit.

> [!NOTE]
> Společnosti Microsoft výchozí hlasový pozdrav uživatele vyzvat, aby stisknutím klávesy 0# odeslat upozornění na možný podvod. Pokud chcete použít kód než 0, měli zaznamenávat a nahrát vlastní vlastní hlasový pozdrav příslušné pokyny.

![Možnosti upozornění podvod – snímek obrazovky](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Jak uživatelé nahlášení podvodu 
Upozornění na podvod mohou být oznámeny dvěma způsoby.  Buď prostřednictvím mobilní aplikace nebo prostřednictvím telefonu.  

#### <a name="report-fraud-with-the-mobile-app"></a>Ohlásit podvod v mobilní aplikaci
1. Odeslání ověření na váš telefon, vyberte ji otevřít aplikaci Microsoft Authenticator.
2. Vyberte **Odepřít** na oznámení. 
3. Vyberte **nahlášení podvodu**.
4. Zavřete aplikaci.

#### <a name="report-fraud-with-a-phone"></a>Nahlášení podvodu po telefonu
1. Pokud ověřovací hovor jde telefonu, odpovězte na ni.  
2. Chcete-li nahlášení podvodu, zadejte kód podvodu (výchozí hodnota je 0) a poté znak #. Potom budete upozorněni, aby byla odeslána upozornění na možný podvod.
3. Ukončení hovoru.

### <a name="view-fraud-reports"></a>Zobrazit sestavy podvod
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost Active Directory.
3. V horní části, vyberte **zprostředkovatelé vícefaktorového ověřování** zobrazíte seznam vaši zprostředkovatelé vícefaktorového ověřování.
4. Vyberte poskytovatele služby Multi-Factor Auth a klikněte na tlačítko **spravovat** v dolní části stránky. Otevře se Azure Multi-Factor Authentication Management Portal.
5. Na Azure Multi-Factor Authentication portálu pro správu, v části sestavy A zobrazení, klikněte na tlačítko **upozornění na podvod**.
6. Zadejte rozsah dat, který chcete zobrazit v sestavě. Můžete také zadat uživatelských jmen, telefonních čísel a stavu uživatele.
7. Klikněte na tlačítko **spustit** zobrazíte sestavu upozornění na podvod. Klikněte na tlačítko **exportovat do souboru CSV** Pokud chcete exportovat sestavy.

## <a name="one-time-bypass"></a>Jednorázové přihlášení
Jednorázové přihlášení umožňuje uživateli jednorázově ověřit bez provedení dvoustupňové ověřování. Toto jednorázové přihlášení je dočasné a vyprší po zadaném počtu sekund. V situacích, kde mobilní aplikaci nebo phone nepřijímá oznámení nebo telefonní hovor můžete povolit jednorázové přihlášení, má uživatel přístup požadovaný prostředek.

### <a name="create-a-one-time-bypass"></a>Vytvoření jednorázové přihlášení
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte do portálu pro správu MFA podle pokynů v horní části této stránky.
3. Pokud na levé straně se zobrazí název vaší zprostředkovatele Azure MFA  **+**  vedle sebe, klikněte  **+** . Jsou uvedeny různé skupiny replikace MFA Server a Azure výchozí skupiny. Vyberte příslušné skupiny.
4. V části Správa uživatele, vyberte **jednorázové přihlášení**.
5. Na stránce jednorázové přihlášení, klikněte na **nové jednorázové přihlášení**.

  ![Cloud](./media/multi-factor-authentication-whats-next/create1.png)

6. Zadejte uživatelské jméno, doba trvání Toto jednorázové přihlášení a důvody, proč toto jednorázové přihlášení. Klikněte na tlačítko **nepoužívat**.
7. Časový limit přejde platit okamžitě, takže uživatel musí k přihlášení, než vyprší platnost jednorázového přihlášení. 

### <a name="view-the-one-time-bypass-report"></a>Zobrazit sestavu jednorázové přihlášení
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost Active Directory.
3. V horní části, vyberte **zprostředkovatelé vícefaktorového ověřování** zobrazíte seznam vaši zprostředkovatelé vícefaktorového ověřování.
4. Vyberte poskytovatele služby Multi-Factor Auth a klikněte na tlačítko **spravovat** v dolní části stránky. Otevře se Azure Multi-Factor Authentication Management Portal.
5. Na portálu Azure Multi-Factor Authentication Management Portal, na levé straně, v části sestavy A zobrazení, klikněte na tlačítko **jednorázové přihlášení**.
6. Zadejte rozsah dat, který chcete zobrazit v sestavě. Můžete také zadat uživatelských jmen, telefonních čísel a stavu uživatele.
7. Klikněte na tlačítko **spustit** zobrazíte sestavu přeskočení. Klikněte na tlačítko **exportovat do souboru CSV** Pokud chcete exportovat sestavy.

## <a name="custom-voice-messages"></a>Vlastní hlasové zprávy
Vlastní hlasové zprávy umožňují použít vlastní záznamy nebo přivítání pro dvoustupňové ověření. Můžete použít vlastní hlasové zprávy kromě nebo nahradit Microsoft záznamy.

Než začnete, nezapomeňte tyto věci:

* Podporované formáty souborů jsou ve formátu WAV nebo MP3.
* Maximální velikost souboru je 5 MB.
* Ověřování zprávy musí být kratší než 20 sekund. Zprávy, které jsou delší než 20 sekund neposkytují uživatele dostatek času reagovat před uplynutím ověření časového limitu.

### <a name="set-up-a-custom-message"></a>Nastavit vlastní zprávu

Existují dvě části k vytvoření vlastní zprávu. Nejdřív odešlete zprávu a pak můžete zapnout pro vaše uživatele.

Chcete-li nahrát vlastní zprávu:

1. Vytvořte vlastní hlasové zprávy pomocí jedné z podporované formáty souborů.
2. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
3. Přejděte do portálu pro správu MFA podle pokynů v horní části této stránky.
4. V portálu Azure Multi-Factor Authentication Management Portal, klikněte na **hlasové zprávy** v části konfigurace.
5. Na konfigurace: hlasové zprávy stránky, klikněte na tlačítko **nové hlasové zprávy**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6. Na konfigurace: stránka nové hlasové zprávy, klikněte na tlačítko **Správa souborů zvuk**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7. Na konfigurace: zvukových souborů stránky, klikněte na tlačítko **nahrát soubor zvuk**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8. Na konfigurace: nahrát soubor zvuku, klikněte na tlačítko **Procházet** a přejděte do hlasové zprávy, klikněte na tlačítko **otevřete**.
9. Přidejte popis a klikněte na tlačítko **nahrát**.
10. Po nahrání hlasové zprávy zprávu potvrdí, že jste úspěšně odeslali soubor.

Chcete-li na zprávu pro vaše uživatele:

1. Na levé straně klikněte na tlačítko **hlasové zprávy**.
2. V části hlasové zprávy, klikněte na tlačítko **nové hlasové zprávy**.
3. Jazyk rozevíracího seznamu vyberte jazyk.
4. Pokud se tato zpráva je pro danou aplikaci, zadejte ji do pole aplikace.
5. Typ zprávy rozevíracího seznamu vyberte typ zprávy k přepsání s novou vlastní zprávu.
6. Zvukový soubor rozevíracího seznamu vyberte v první části zvukového souboru, který jste nahráli.
7. Klikněte na možnost **Vytvořit**. Zobrazí se zpráva potvrzující, že jste úspěšně vytvořili hlasové zprávy.
    ![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Ukládání do mezipaměti v Azure Multi-Factor Authentication
Ukládání do mezipaměti umožňuje nastavit určité časové období, aby automaticky být úspěšné následné pokusy o ověření v tomto časovém období. Ukládání do mezipaměti se používá, pokud místních systémů, jako je například VPN odesílat mnoho žádostí o ověření při prvním požadavku stále probíhá. Povolení dalších požadavků úspěšná automaticky po úspěšné uživatele v průběhu prvního ověřování. 

Ukládání do mezipaměti není určena pro použití pro přihlášení ke službě Azure AD.

### <a name="set-up-caching"></a>Nastavení ukládání do mezipaměti 
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte do portálu pro správu MFA podle pokynů v horní části této stránky.
3. V portálu Azure Multi-Factor Authentication Management Portal, klikněte na **ukládání do mezipaměti** v části konfigurace.
4. Klikněte na tlačítko **nová mezipaměť** na konfigurovat ukládání do mezipaměti stránky.
5. Vyberte typ mezipaměti a sekund mezipaměti. Klikněte na možnost **Vytvořit**.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné IP adresy je funkce Azure mfa, kterého spravované nebo federované klienta mohou správci obejít dvoustupňové ověření. Která se používá pro uživatele, kteří se přihlašují ze společnosti místní intranet. Tato funkce je k dispozici plnou verzi Azure Multi-Factor Authentication není bezplatnou verzi pro správce. Podrobnosti o tom, jak získat plnou verzi Azure Multi-Factor Authentication najdete v tématu [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ klientovi Azure AD | Dostupné možnosti důvěryhodných adres IP |
|:--- |:--- |
| Spravované |<li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují z intranetu společnosti.</li> |
| Federované |<li>Všichni uživatelé federovaný – všechny federovaní uživatelé, kteří se přihlašují z uvnitř organizace bude obejít dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS.</li><br><li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují z intranetu společnosti. |

To nepoužívat funguje pouze z uvnitř intranetu společnosti. 

**Činnost koncového uživatele v podnikové síti:**

Pokud je zakázáno důvěryhodné IP adresy, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

Pokud je povoleno důvěryhodné IP adresy, dvoustupňové ověření je *není* požadované pro toky prohlížeče. Hesla aplikací jsou *není* požadované pro starší pokročilé klientské aplikace za předpokladu, že uživatel už nevytvořil heslo aplikace. Jakmile heslo aplikace se používá, zůstane vyžaduje. 

**Činnost koncového uživatele mimo corpnet:**

Zda důvěryhodné IP adresy je povolena, nebo Ne, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

### <a name="to-enable-trusted-ips"></a>Chcete-li povolit důvěryhodné IP adresy
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte na stránku nastavení vícefaktorového ověřování služby podle pokynů na začátku tohoto článku.
3. Na stránce nastavení služby v rámci důvěryhodných adres IP máte dvě možnosti:
   
   * **Pro žádosti od federovaných uživatelů pocházející z mém intranetu** – zaškrtněte políčko. Všechny federovaní uživatelé, kteří se přihlašují z podnikové sítě nepoužívat dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS.
   * **Pro žádosti od určitého rozsahu veřejné IP adresy** – v textovém poli zadat pomocí notace CIDR zadejte IP adresy. Příklad: xxx.xxx.xxx.0/24 pro IP adresy v rozsahu xxx.xxx.xxx.1 – xxx.xxx.xxx.254 nebo xxx.xxx.xxx.xxx/32 pro jednu IP adresu. Můžete zadat až 50 rozsahy IP adres. Uživatelé, kteří se přihlašují z těchto IP adres nepoužívat dvoustupňové ověřování.
4. Klikněte na **Uložit**.
5. Jakmile se aktualizace se aplikovaly, klikněte na **Zavřít**.

![Důvěryhodné IP adresy](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Hesla aplikací
Některé aplikace, jako je Office 2010 nebo starší a Apple Mail, nepodporují dvoustupňové ověřování. Nejsou nakonfigurované tak, aby přijímal druhé ověření. Pokud chcete tyto aplikace používat, musíte používat "hesla aplikací" místo tradiční heslo. Heslo aplikace umožňuje aplikaci obejít dvoustupňové ověření a pokračovat v práci.

> [!NOTE]
> Moderní ověřování pro klienty Office 2013
> 
> Klienti Office 2013 (včetně Outlook) a novější podporu moderních ověřovacích protokolů a lze je povolit pro práci s dvoustupňové ověřování. Po povolení nejsou hesla aplikací vyžaduje pro tyto klienty.  Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Důležité věci vědět o heslech aplikací
Tady je seznam důležitých věcí, které byste měli vědět o heslech aplikací.

* Hesla aplikací by měly být zadány do vstupního pole jednou za aplikace. Uživatelé nemají sledování a pokaždé, když je zadat.
* Vlastní heslo se vygeneruje automaticky a není zadané uživatelem. Automaticky vygenerované heslo je pro útočníka tak snadno uhodnout obtížnější a je bezpečnější.
* Existuje omezení 40 hesel na uživatele. 
* Aplikace, které hesla do mezipaměti a použít ho v místní scénářích mohou začít selhávat, protože heslo aplikace není známo mimo id organizace. Příkladem je e-mailů Exchange, které jsou na místě, ale Archivovaná pošta se nachází v cloudu. Stejné heslo nebude fungovat.
* Při spuštění služby Multi-Factor authentication, můžete použít heslo s některé neprohlížečových klientů. Pro úlohy správy, nelze používat hesla aplikací. Ujistěte se, vytvořte účet služby s silné heslo ke spouštění skriptů prostředí PowerShell a nepovolujte tohoto účtu pro dvoustupňové ověření.

> [!WARNING]
> Hesla aplikací nepodporují v hybridních prostředích, kde klienti komunikaci s místní a cloudové Automatická konfigurace koncových bodů. Hesla domény jsou potřebné k ověření na místě a hesla aplikací jsou nezbytné k ověření s cloudem.

### <a name="naming-guidance-for-app-passwords"></a>Pokyny pro hesla aplikací pro pojmenování
Názvů hesel aplikací vytvořena podle zařízení, na které se používají. Pokud máte přenosný počítač, který má neprohlížečové aplikace, například vytvořit jedno heslo aplikace s názvem přenosný počítač a používat takové heslo aplikace. Pak vytvořte jiné heslo aplikace s názvem Desktop pro stejné aplikace ve stolním počítači. 

Společnost Microsoft doporučuje vytvoření jednoho hesla aplikace na zařízení, není jedno heslo aplikace na aplikaci.

### <a name="federated-sso-app-passwords"></a>Hesla aplikací federované (SSO)
Azure AD podporuje federation (jednotné přihlášení) s místními systému Windows Server Active Directory Domain Services (AD DS). Pokud je vaše organizace Federovaná pomocí Azure AD a chcete používat Azure Multi-Factor Authentication, informace o heslech aplikací jsou pro vás důležité. Tato část platí jenom pro federované zákazníků (SSO).

* Hesla aplikací jsou ověřit pomocí služby Azure AD a proto obcházet federace. Federace se používá aktivně pouze při nastavování hesla aplikací.
* Pro federované uživatele (SSO) jsme nikdy přejděte k poskytovateli Identity (IdP) na rozdíl od pasivního toku. Hesla jsou uložena v id organizace. Pokud uživatel odejde ze společnosti, že informace o musí přejít do id organizace pomocí služby DirSync v skutečný čas. Zakázání/odstranění účtu může trvat až tři hodiny k synchronizaci, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace ve službě Azure AD.
* Místní nastavení služby Access Control klienta není dodrženo heslem aplikace.
* Bez ověřování místní protokolování nebo auditování funkce je k dispozici u hesla aplikace.
* Některé pokročilé architektury návrhů může vyžadovat kombinaci organizační uživatelského jména, hesla a hesla aplikací, při použití dvoustupňové ověřování s klienty. I když to závisí na kde ověřují. Pro klienty, kteří se ověřují se místní infrastruktury by použijte organizační uživatelské jméno a heslo. Pro klienty, kteří se ověřují se Azure AD použijete heslo aplikace.

  Předpokládejme například, že máte architekturu, která se skládá z těchto instancí:

  * Federujete místní instanci služby Active Directory s Azure AD
  * Používáte Exchange online
  * Používáte-li aplikace Lync, který je konkrétně místní
  * Používáte Azure Multi-Factor Authentication

  ![Ověření](./media/multi-factor-authentication-whats-next/federated.png)

  V těchto případech musí postupujte takto:

  * Při přihlášení do aplikace Lync, použijte uživatelské jméno a heslo vaší organizace.
  * Při pokusu o přístup k adresáři prostřednictvím klienta aplikace Outlook, která se připojuje k Exchange online, používejte heslo aplikace.

### <a name="allow-app-password-creation"></a>Povolit vytváření heslo aplikace
Ve výchozím nastavení uživatelé nemůžou vytvářet hesla aplikací, ale můžete povolit funkci sami. Povolit uživatelům možnost vytvářet hesla aplikací, použijte následující postup:

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte na stránku nastavení vícefaktorového ověřování služby podle pokynů na začátku tohoto článku.
3. Klepněte na přepínač vedle **povolit uživatelům vytvářet hesla aplikací pro přihlášení do neprohlížečových aplikací**.

![Vytvoření hesel aplikací](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Vytvoření hesel aplikací
Uživatelé mohou vytvářet hesla aplikací během jejich počáteční registrace. Získá možnost na konci procesu registrace, který vám umožní vytvořit hesla aplikací.

Uživatelé mohou také vytvářet hesla aplikací po registraci. Můžete změnit jejich nastavení v portálu Azure nebo na portálu Office 365 mohou vytvářet hesla aplikací. Další informace a podrobné pokyny pro uživatele najdete v tématu [co jsou hesla aplikací v Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Pro zařízení, které důvěřují uživatelům zapamatovat Vícefaktorové ověřování
Zapamatování Multi-Factor Authentication na zařízení a prohlížeče, že uživatelé důvěryhodnosti je volné funkce pro všechny uživatele vícefaktorového ověřování. Služba Multi-Factor authentication umožňuje uživatelům obejít MFA po určený počet dní, po přihlášení. Tato funkce vylepšuje použitelnost a minimalizovat počet, kolikrát může uživatel provést dvoustupňové ověřování na stejné zařízení.

Ale pokud účtu nebo zařízení ohrožení, pak zapamatování MFA pro důvěryhodná zařízení může ovlivnit zabezpečení. Pokud podnikové účty nebo ztráty nebo odcizení důvěryhodné zařízení, budete muset [obnovit Multi-Factor Authentication na všech zařízeních](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Tato akce odvolá důvěryhodný stav ze všech zařízení a uživatele je potřeba provést dvoustupňové ověření znovu. Můžete také požádejte své uživatele k obnovení MFA na jejich vlastní zařízení s pokyny uvedenými v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Jak to funguje

Zapamatování Multi-Factor Authentication funguje tak, že když uživatel kontroluje nastavení trvalého souboru cookie v prohlížeči "nezobrazovat dotaz dalších **X** dní" pole při přihlášení. Uživatel nebude výzva k MFA znovu z tohoto prohlížeče do vypršení platnosti souboru cookie. Pokud uživatel otevře jiný prohlížeč na stejné zařízení nebo vymaže jejich souborů cookie, budou vyzváni k ověření znovu. 

"Nezobrazovat dotaz dalších **X** dní" zaškrtávací políčko se nezobrazí na neprohlížečové aplikace, jestli podporují moderní ověřování. Tyto aplikace použít tokeny obnovení, které poskytují nové přístupové tokeny každou hodinu. Po ověření se token obnovení, Azure AD ověří, že byla provedena poslední čas dvoustupňové ověřování v rámci zadaného počtu dnů. 

Na závěr zapamatování vícefaktorového ověřování na důvěryhodných zařízeních snižuje počet ověření na webové aplikace (které obvykle poskytne pokaždé, když). Můžete ale taky zvyšuje počet ověření pro klienty moderní ověřování, (což obvykle poskytne každých 90 dní).

> [!NOTE]
>Tato funkce není kompatibilní s "Zůstat přihlášeni" funkce služby AD FS, když uživatelé provádět dvoustupňové ověřování pro službu AD FS pomocí Azure MFA serveru nebo řešení třetí strany vícefaktorového ověřování. Pokud uživatelé ve službě AD FS vyberte možnost "Zůstat přihlášeni" a také označit jako důvěryhodné zařízení pro MFA, by nebylo možné ověřit po vypršení platnosti "Zapamatovat MFA" počet dnů. Azure AD požadavky čerstvé dvoustupňové ověření, ale služby AD FS vrátí token s původní vícefaktorového ověřování deklarací identity a datum namísto provádění dvoustupňové ověření znovu. Tento proces nastaví mimo smyčku ověření mezi Azure AD a služby AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Povolit zapamatovat vícefaktorové ověřování
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte na stránku nastavení vícefaktorového ověřování služby podle pokynů na začátku tohoto článku.
3. Na stránce nastavení služby ve správě uživatelských nastavení zařízení, zkontrolujte **povolit uživatelům zapamatovat vícefaktorové ověřování u zařízení, které důvěřují** pole.
   ![Mějte na paměti, zařízení](./media/multi-factor-authentication-whats-next/remember.png)
4. Nastavte počet dní, které chcete povolit důvěryhodná zařízení obejít dvoustupňové ověření. Výchozí hodnota je 14 dnů.
5. Klikněte na **Uložit**.
6. Klikněte na **Zavřít**.

### <a name="mark-a-device-as-trusted"></a>Označit jako důvěryhodné zařízení

Po povolení této funkce, uživatelé mohou označit zařízení jako důvěryhodné při přihlášení kontrolou **dotaz již nezobrazovat**.

![Dotaz již nezobrazovat – snímek obrazovky](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Volitelný ověření metody
Můžete zvolit, které metody ověřování jsou k dispozici pro vaše uživatele. Následující tabulka obsahuje stručný přehled jednotlivých metod.

Když uživatelé zaregistrují svoje účty pro MFA, vybírá jejich metoda upřednostňované ověření mimo možnosti, které jste povolili. Pokyny k jejich registraci, najdete v článku [nastavit účtu pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md)

| Metoda | Popis |
|:--- |:--- |
| Volání na telefon |Umístí automatický hlasový hovor. Uživatel přijme hovor a stiskem tlačítka # na klávesnici telefonu provede ověření. Toto telefonní číslo není synchronizován do místní služby Active Directory. |
| Textové zprávy na telefon |Odešle textovou zprávu s ověřovacím kódem. Uživatel je vyzván k odpovědi na text, zprávu s ověřovacím kódem a zadejte ověřovací kód do rozhraní přihlášení. |
| Oznámení pomocí mobilních aplikací |Odešle nabízených oznámení do telefonu nebo zaregistrovaného zařízení. Zobrazení oznámení a vybere uživatele **ověřte** k dokončení ověření. <br>Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Ověřovací kód z mobilní aplikace |Aplikace Microsoft Authenticator generuje každých 30 sekund nový ověřovací kód OATH. Tento ověřovací kód, uživatel zadá do rozhraní přihlášení.<br>Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Povolení nebo zakázání metody ověřování
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte na stránku nastavení vícefaktorového ověřování služby podle pokynů na začátku tohoto článku.
3. Na stránce nastavení služby v rámci možnosti ověření výběrem nebo zrušením výběru možnosti, které chcete použít.
   ![Požadované možnosti ověření](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Klikněte na **Uložit**.
5. Klikněte na **Zavřít**.
