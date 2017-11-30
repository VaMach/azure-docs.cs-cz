---
title: "Konfigurace vícefaktorového ověřování Azure | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication, která popisuje, jak postupovat pomocí vícefaktorového ověřování.  To zahrnuje sestavy, upozornění na podvod, jednorázové přihlášení, vlastní hlasové zprávy, ukládání do mezipaměti, důvěryhodné IP adresy a aplikaci hesla."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 11f3a3fdc5caf96ce672976067e47680822315d4
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="configure-azure-multi-factor-authentication-settings---public-preview"></a>Konfigurovat nastavení ověřování Azure Multi-Factor Authentication – ve verzi Public preview

Tento článek usnadňuje správu ověřování Azure Multi-Factor Authentication teď, když jsou spuštěná.  Vysvětluje různé témata, které umožňují využívat naplno Azure Multi-Factor Authentication.  Ne všechny tyto funkce jsou dostupné v každé [verzi Azure Multi-Factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need).

>[!NOTE]
>Tato nastavení jsou ve verzi public preview na portálu Azure. Dokumentaci o tom, jak spravovat nastavení ověřování Azure Multi-Factor Authentication na portálu pfweb najdete v tématu [nastavení konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next-pfweb.md).

| Funkce | Popis | 
|:--- |:--- |
| [Blokovat nebo odblokovat uživatele](#block-and-unblock) |Zablokovat nebo odblokovat uživatele můžete zabránit uživatelům v přijetí žádosti o ověření. |
| [Upozornění na podvod](#fraud-alert) |Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy podvodné pokouší získat přístup k jejich prostředky. |
| [Jednorázové přihlášení](#one-time-bypass) |Jednorázové přihlášení umožňuje uživateli jednorázově ověřit pomocí "obcházení" služby Multi-Factor authentication. |
| [Vlastní hlasové zprávy](#custom-voice-messages) |Vlastní hlasové zprávy umožňují použít vlastní záznamy nebo pozdrav pomocí služby Multi-Factor authentication. |
| [Ukládání do mezipaměti](#caching-in-azure-multi-factor-authentication) |Ukládání do mezipaměti umožňuje nastavit určité časové období, aby automaticky být úspěšné následné pokusy o ověření. |
| [Důvěryhodné IP adresy](#trusted-ips) |Správci spravované nebo federované klienta můžete použít důvěryhodné IP adresy obejít dvoustupňové ověření pro uživatele, kteří se přihlašují z místního intranetu společnosti. |
| [Hesla aplikací](#app-passwords) |Heslo aplikace umožňuje aplikaci, která MFA neví obejít ověřování Multi-Factor authentication a pokračovat v práci. |
| [Zapamatovat Vícefaktorové ověřování na zapamatovaných zařízeních a prohlížeče](#remember-multi-factor-authentication-for-devices-that-users-trust) |Umožňuje mějte na paměti, zařízení pro sadu počet dnů po uživatel se úspěšně přihlásil pomocí vícefaktorového ověřování. |
| [Volitelný ověření metody](#selectable-verification-methods) |Umožňuje výběr metod ověřování, které jsou k dispozici pro uživatelům používat. |

## <a name="block-and-unblock"></a>Blokovat nebo odblokovat
Zablokovat nebo odblokovat uživatele umožňuje uživatelům zabránit v přijetí žádosti o ověření. Jakékoli pokusy o ověření pro blokované uživatele jsou automaticky odepírány. Blokovaným uživatelům zůstat blokované pro 90 dnů od doby, jsou zablokované.

### <a name="block-a-user"></a>Blokování uživatele
1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **zablokovat nebo odblokovat uživatele**.
3. Klikněte na tlačítko **přidat** zablokujete určitému uživateli.
4. Vyberte **replikační skupiny**, vstup blokované uživatelské jméno jako  **username@domain.com** a zadejte komentář do **důvod** pole.
5. Klikněte na tlačítko **přidat** ukončíte blokování uživatele.

### <a name="unblock-a-user"></a>Odblokovat uživatele
1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **zablokovat nebo odblokovat uživatele**.
3. Klikněte na tlačítko **Odblokovat** v **akce** sloupce vedle uživatele, kterou chcete odblokovat.
4. Zadejte komentář do **důvod pro odblokování** pole.
5. Klikněte na tlačítko **Odblokovat** ukončíte odblokování uživatele.

## <a name="fraud-alert"></a>Upozornění na podvod
Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy podvodné pokouší získat přístup k jejich prostředky.  Uživatelé mohou zasílat zprávy podvod pomocí mobilní aplikace nebo přes svůj telefon.

### <a name="turn-on-fraud-alert"></a>Zapnout upozornění na podvod
1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **upozornění na podvod**.

   ![Výstraha podvodů](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Zapnout **povolit uživatelům odesílat upozornění na podvod** k **na**.
4. Vyberte **Uložit**.

### <a name="configuration-options"></a>Možnosti konfigurace

- **Blokování uživatele při nahlášení podvodu** – Pokud uživatel sestavy podvod, svůj účet zablokován 90 dní, nebo dokud správce odblokuje svého účtu. Správce může zkontrolovat přihlášení pomocí sestavy přihlášení a proveďte příslušné akce, aby se zabránilo budoucí podvod. Správce pak může [Odblokovat](#unblock-a-user) uživatelského účtu.
- **Kód pro nahlášení podvodu při úvodním pozdravu** – když uživatelé obdrží telefonní hovor provést dvoustupňové ověření, že normálně stisknutím klávesy # potvrzení jejich přihlášení. Pokud chcete ohlásit podvod, jejich zadat kód ještě před stisknutím klávesy #. Tento kód je **0** ve výchozím nastavení, ale můžete jej upravit.

> [!NOTE]
> Společnosti Microsoft výchozí hlasový pozdrav uživatele vyzvat, aby stisknutím klávesy 0# odeslat upozornění na možný podvod. Pokud chcete použít kód než 0, měli zaznamenávat a nahrát vlastní vlastní hlasový pozdrav příslušné pokyny.

### <a name="view-fraud-reports"></a>Zobrazit sestavy podvod
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Vyberte adresář, který chcete spravovat. 
4. Vyberte **konfigurace**
5. V části ověřování Multi-Factor Authentication, vyberte **spravovat nastavení služby**.
6. V dolní části stránky nastavení služby, vyberte **přejděte na portál**.
7. V Azure Multi-Factor Authentication portálu pro správu, v části sestavy A zobrazení, klikněte na **upozornění na podvod**.
8. Zadejte rozsah dat, který chcete zobrazit v sestavě. Můžete také zadat uživatelských jmen, telefonních čísel a stavu uživatele.
9. Klikněte na tlačítko **spustit** se zprovoznit sestavu upozornění na podvod. Klikněte na tlačítko **exportovat do souboru CSV** Pokud chcete exportovat sestavy.

## <a name="one-time-bypass"></a>Jednorázové přihlášení
Jednorázové přihlášení umožňuje uživateli jednorázově ověřit bez provedení dvoustupňové ověřování. Toto jednorázové přihlášení je dočasné a vyprší po zadaném počtu sekund. V situacích, kde mobilní aplikaci nebo phone nepřijímá oznámení nebo telefonní hovor můžete povolit jednorázové přihlášení, má uživatel přístup požadovaný prostředek.

### <a name="create-a-one-time-bypass"></a>Vytvoření jednorázové přihlášení

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **jednorázové přihlášení**.

   ![Jednorázové přihlášení](./media/multi-factor-authentication-whats-next/onetimebypass.png)
3. Vyberte **Přidat**.
4. V případě potřeby vyberte replikační skupiny pro toto jednorázové přihlášení.
5. Zadejte uživatelské jméno (ve formě username@domain.com), počet sekund, po které by měl pro poslední toto jednorázové přihlášení a případně důvod pro toto jednorázové přihlášení. 
6. Vyberte **Přidat**. Časový limit přejde platit okamžitě, takže uživatel musí k přihlášení, než vyprší platnost jednorázového přihlášení. 

### <a name="view-the-one-time-bypass-report"></a>Zobrazit sestavu jednorázové přihlášení
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Vyberte adresář, který chcete spravovat. 
4. Vyberte **konfigurace**
5. V části ověřování Multi-Factor Authentication, vyberte **spravovat nastavení služby**.
6. V dolní části stránky nastavení služby, vyberte **přejděte na portál**.
7. V Azure Multi-Factor Authentication portálu pro správu, v části sestavy A zobrazení, klikněte na **jednorázové přihlášení**.
8. Zadejte rozsah dat, který chcete zobrazit v sestavě. Můžete také zadat uživatelských jmen, telefonních čísel a stavu uživatele.
9. Klikněte na tlačítko **spustit** se zprovoznit sestavu přeskočení. Klikněte na tlačítko **exportovat do souboru CSV** Pokud chcete exportovat sestavy.

## <a name="custom-voice-messages"></a>Vlastní hlasové zprávy
Vlastní hlasové zprávy umožňují použít vlastní záznamy nebo přivítání pro dvoustupňové ověření. Ty lze kromě nebo nahradit Microsoft záznamy.

Než začnete mějte na paměti následující omezení:

* Podporované formáty souborů jsou ve formátu WAV nebo MP3.
* Maximální velikost souboru je 5 MB.
* Ověřování zprávy musí být kratší než 20 sekund. Nic delší než 20 sekund by mohlo způsobit ověření se nezdařila, protože uživatel nemusí odpovídat před zpráva dokončí, způsobuje ověření vypršení časového limitu.

### <a name="set-up-a-custom-message"></a>Nastavit vlastní zprávu

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **telefonní hovor nastavení**.

   ![Nastavení telefonní hovor](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Vyberte **přidat pozdravu**.
4. Vyberte typ pozdravu a jazyka.
5. Vyberte MP3 nebo WAV zvukový soubor k odeslání.
6. Vyberte **Přidat**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Ukládání do mezipaměti v Azure Multi-Factor Authentication
Ukládání do mezipaměti umožňuje nastavit určité časové období, aby automaticky být úspěšné následné pokusy o ověření v tomto časovém období. Používá se především pokud místních systémů, jako je například VPN odesílat více požadavků na ověření při prvním požadavku stále probíhá. Ukládání do mezipaměti umožňuje následných žádostí úspěšné automaticky po úspěšné uživatele v průběhu prvního ověřování. 

Ukládání do mezipaměti není určena pro použití pro přihlášení ke službě Azure AD.

### <a name="set-up-caching"></a>Nastavení ukládání do mezipaměti 
1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **MFA Server** > **ukládání do mezipaměti pravidla**.

   ![Ukládání do mezipaměti pravidla](./media/multi-factor-authentication-whats-next/cachingrules.png)

4. Vyberte **Přidat**.
5. Z rozevíracího seznamu vyberte typ mezipaměti a zadat počet sekund, po maximální mezipaměti. 
6. V případě potřeby vyberte typ ověřování a určete aplikace. 
7. Vyberte **Přidat**.


## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné že IP adresy je funkce služby Azure MFA, správci spravované nebo federované klienta můžete vynechat dvoustupňové ověřování pro uživatele, kteří se přihlašují ze společnosti místní intranet. Tato funkce je k dispozici plnou verzi Azure Multi-Factor Authentication není bezplatnou verzi pro správce. Podrobnosti o tom, jak získat plnou verzi Azure Multi-Factor Authentication najdete v tématu [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ klientovi Azure AD | Dostupné možnosti důvěryhodných adres IP |
|:--- |:--- |
| Spravované |<li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují z intranetu společnosti.</li> |
| Federované |<li>Všichni uživatelé federovaný – všechny federovaní uživatelé, kteří se přihlašují z uvnitř organizace bude obejít dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS.</li><br><li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují z intranetu společnosti. |

To nepoužívat funguje pouze z uvnitř intranetu společnosti. Například pokud jste vybrali všechny federovaní uživatelé, a uživatel přihlásí z mimo firemní intranet, tento uživatel má k ověření pomocí dvoustupňového ověření i v případě, že uživatel uvede deklaraci identity služby AD FS. 

**Činnost koncového uživatele v podnikové síti:**

Pokud je zakázáno důvěryhodné IP adresy, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

Pokud je povoleno důvěryhodné IP adresy, dvoustupňové ověření je *není* je požadována pro toky prohlížeče, a hesla aplikací jsou *není* požadované pro starší pokročilé klientské aplikace za předpokladu, že uživatel už nevytvořil heslo aplikace. Jakmile heslo aplikace se používá, zůstane vyžaduje. 

**Činnost koncového uživatele mimo corpnet:**

Zda důvěryhodné IP adresy je povolena, nebo Ne, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

### <a name="enable-named-locations-using-conditional-access"></a>Povolit s názvem umístění pomocí podmíněného přístupu

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **podmíněného přístupu** > **s názvem umístění**
3. Vyberte **nové umístění**
4. Zadejte název pro umístění
5. Vyberte **označit jako důvěryhodné umístění**
6. Zadejte rozsah IP adres v notaci CIDR (například 192.168.1.1/24)
7. Vyberte **vytvořit**

### <a name="enable-trusted-ips-using-conditional-access"></a>Povolit důvěryhodné IP adresy pomocí podmíněného přístupu

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **podmíněného přístupu** > **s názvem umístění**
3. Vyberte **MFA konfigurovat důvěryhodné IP adresy**
4. Na stránce nastavení služby v rámci důvěryhodných adres IP máte dvě možnosti:
   
   * **Pro žádosti od federovaných uživatelů pocházející z mém intranetu** – zaškrtněte políčko. Všechny federovaní uživatelé, kteří se přihlašují z podnikové sítě bude nepoužívat dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS. Zajistěte, aby služba AD FS na pravidlo můžete přidat intranetu deklarace identity odpovídající provoz. Pokud pravidlo neexistuje, vytvořte následující pravidlo ve službě AD FS: "c: [typ =="http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] = > issue(claim = c);"

   * **Pro žádosti od určitého rozsahu veřejné IP adresy** – v textovém poli zadat pomocí notace CIDR zadejte IP adresy. Příklad: xxx.xxx.xxx.0/24 pro IP adresy v rozsahu xxx.xxx.xxx.1 – xxx.xxx.xxx.254 nebo xxx.xxx.xxx.xxx/32 pro jednu IP adresu. Můžete zadat až 50 rozsahy IP adres. Uživatelé, kteří se přihlašují z těchto IP adres nepoužívat dvoustupňové ověřování.
5. Vyberte **Uložit**.

### <a name="enable-trusted-ips-using-service-settings"></a>Povolit důvěryhodné IP adresy pomocí nastavení služby

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**
3. Vyberte **Multi-Factor Authentication**.
4. V části ověřování Multi-Factor Authentication, vyberte **nastavení služby**.
5. Na stránce nastavení služby v rámci důvěryhodných adres IP máte dvě možnosti:
   
   * **Pro žádosti od federovaných uživatelů pocházející z mém intranetu** – zaškrtněte políčko. Všechny federovaní uživatelé, kteří se přihlašují z podnikové sítě bude nepoužívat dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS. Zajistěte, aby služba AD FS na pravidlo můžete přidat intranetu deklarace identity odpovídající provoz. Pokud pravidlo neexistuje, vytvořte následující pravidlo ve službě AD FS: "c: [typ =="http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] = > issue(claim = c);"

   * **Pro žádosti od určitého rozsahu veřejné IP adresy** – v textovém poli zadat pomocí notace CIDR zadejte IP adresy. Příklad: xxx.xxx.xxx.0/24 pro IP adresy v rozsahu xxx.xxx.xxx.1 – xxx.xxx.xxx.254 nebo xxx.xxx.xxx.xxx/32 pro jednu IP adresu. Můžete zadat až 50 rozsahy IP adres. Uživatelé, kteří se přihlašují z těchto IP adres nepoužívat dvoustupňové ověřování.
6. Vyberte **Uložit**.

![Důvěryhodné IP adresy](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Hesla aplikací
Některé aplikace, jako je Office 2010 nebo starší a Apple Mail, nepodporují dvoustupňové ověřování. Nejsou nakonfigurované tak, aby přijímal druhé ověření. Pokud chcete tyto aplikace používat, musíte používat "hesla aplikací" místo tradiční heslo. Heslo aplikace umožňuje aplikaci obejít dvoustupňové ověření a pokračovat v práci.

> [!NOTE]
> Moderní ověřování pro klienty Office 2013
> 
> Klienti Office 2013 (včetně Outlook) a novější podporu moderních ověřovacích protokolů a lze je povolit pro práci s dvoustupňové ověřování. Po povolení nejsou hesla aplikací vyžaduje pro tyto klienty.  Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Důležité věci vědět o heslech aplikací
Zde je důležité seznam skutečností, které byste měli vědět o heslech aplikací.

* Hesla aplikací potřeba jenom jednou zadat jednotlivé aplikace. Uživatelé nemají sledování a pokaždé, když je zadat.
* Vlastní heslo se vygeneruje automaticky a není zadané uživatelem. To je proto, že automaticky vygenerované heslo je pro útočníka tak snadno uhodnout obtížnější a je bezpečnější.
* Existuje omezení 40 hesel na uživatele. 
* Aplikace, které hesla do mezipaměti a použít ho v místní scénářích mohou začít selhávat, protože heslo aplikace není známo mimo id organizace. Příkladem je e-mailů Exchange, které jsou na místě, ale Archivovaná pošta se nachází v cloudu. Stejné heslo nebude fungovat.
* Jakmile povolíte službu Multi-Factor authentication na uživatelský účet, lze hesla aplikací s většina neprohlížečové klienty, jako je například Outlook a Lync. Akce správy nelze provést, používání hesel aplikací prostřednictvím neprohlížečové aplikace, jako je Windows PowerShell, i v případě, že uživatel má účet správce.  Vytvoření účtu služby silným heslem ke spouštění skriptů prostředí PowerShell a nepovolujte tohoto účtu pro dvoustupňové ověření.

> [!WARNING]
> Hesla aplikací nepodporují v hybridních prostředích, kde klienti komunikaci s místní a cloudové Automatická konfigurace koncových bodů. Je to proto, že hesla domény jsou potřebné k ověření na místě a hesla aplikací jsou potřebné k ověření s cloudem.

### <a name="naming-guidance-for-app-passwords"></a>Pokyny pro hesla aplikací pro pojmenování
Názvů hesel aplikací vytvořena podle zařízení, na které se používají. Pokud máte přenosný počítač, který má neprohlížečové aplikace, jako je například Outlook, Word a Excel, například vytvořit jedno heslo aplikace s názvem přenosný počítač a používat takové heslo aplikace v těchto aplikacích. Pak vytvořte jiné heslo aplikace s názvem Desktop pro stejné aplikace ve stolním počítači. 

Společnost Microsoft doporučuje vytvoření jednoho hesla aplikace na zařízení, není jedno heslo aplikace na aplikaci.

### <a name="federated-sso-app-passwords"></a>Hesla aplikací federované (SSO)
Azure AD podporuje federation (jednotné přihlášení) s místními systému Windows Server Active Directory Domain Services (AD DS). Pokud je vaše organizace Federovaná pomocí Azure AD a chcete používat Azure Multi-Factor Authentication, je důležité, abyste tyto informace o heslech aplikací. Tato část platí jenom pro federované zákazníků (SSO).

* Hesla aplikací jsou ověřit pomocí služby Azure AD a proto obcházet federace. Federace se používá aktivně pouze při nastavování hesla aplikací.
* Pro federované uživatele (SSO) není kontaktovat zprostředkovatele Identity (IdP) na rozdíl od pasivního toku. Hesla jsou uložena v id organizace. Pokud uživatel odejde ze společnosti, že informace o musí přejít do id organizace pomocí služby DirSync v reálném čase. Zakázání/odstranění účtu může trvat až tři hodiny k synchronizaci, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace ve službě Azure AD.
* Místní nastavení služby Access Control klienta není dodrženo heslem aplikace.
* Bez ověřování místní protokolování nebo auditování funkce je k dispozici u hesla aplikace.
* Některé pokročilé architektury návrhů může vyžadovat kombinaci organizační uživatelského jména a hesla aplikace při použití s klienty, v závislosti na tom, kde ověřují dvoustupňové ověřování. Pro klienty, kteří se ověřují se na místní infrastrukturu byste použili organizační uživatelské jméno a heslo. Pro klienty, kteří se ověřují se Azure AD použijete heslo aplikace.

  Předpokládejme například, že máte architekturu, která se skládá z následujících akcí:

  * Federujete místní instanci služby Active Directory s Azure AD
  * Používáte Exchange online
  * Používáte-li aplikace Lync, který je konkrétně místní
  * Používáte Azure Multi-Factor Authentication

  ![Ověření](./media/multi-factor-authentication-whats-next/federated.png)

  V takové situaci postupujte takto:

  * Při přihlášení do aplikace Lync, použijte uživatelské jméno a heslo vaší organizace.
  * Při pokusu o přístup k adresáři prostřednictvím klienta aplikace Outlook, která se připojuje k Exchange online, používejte heslo aplikace.

### <a name="allow-app-password-creation"></a>Povolit vytváření heslo aplikace
Ve výchozím nastavení uživatelé nemůžou vytvářet hesla aplikací. Tato funkce musí být povolená. Povolit uživatelům možnost vytvářet hesla aplikací, použijte následující postup:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**
3. Vyberte **Multi-Factor Authentication**.
4. V části ověřování Multi-Factor Authentication, vyberte **nastavení služby**.
6. Klepněte na přepínač vedle **povolit uživatelům vytvářet hesla aplikací pro přihlášení do neprohlížečových aplikací**.

![Vytvoření hesel aplikací](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Vytvoření hesel aplikací
Uživatelé mohou vytvářet hesla aplikací během jejich počáteční registrace. Získá možnost na konci procesu registrace, který vám umožní vytvořit hesla aplikací.

Uživatelé mohou také vytvářet hesla aplikací po registraci můžete změnit jejich nastavení v portálu Azure nebo na portálu Office 365. Další informace a podrobné pokyny pro uživatele najdete v tématu [co jsou hesla aplikací v Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Pro zařízení, které důvěřují uživatelům zapamatovat Vícefaktorové ověřování
Zapamatování Multi-Factor Authentication na zařízení a prohlížeče, že uživatelé důvěryhodnosti je volné funkce pro všechny uživatele vícefaktorového ověřování. Toto nastavení poskytuje uživatelům možnost obejít vícefaktorového ověřování pro sadu počet dnů po provedení úspěšně přihlašování pomocí MFA. To může zlepšují použitelnost minimalizací počet, kolikrát může uživatel provést dvoustupňové ověřování na stejné zařízení.

Ale pokud účtu nebo zařízení ohrožení, pak zapamatování MFA pro důvěryhodná zařízení může ovlivnit zabezpečení. Pokud podnikové účty nebo ztráty nebo odcizení důvěryhodné zařízení, měli byste [obnovit Multi-Factor Authentication na všech zařízeních](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Tato akce odvolá důvěryhodný stav ze všech zařízení a uživatele je potřeba provést dvoustupňové ověření znovu. Můžete také požádejte své uživatele k obnovení MFA na jejich vlastní zařízení s pokyny uvedenými v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Jak to funguje

Zapamatování Multi-Factor Authentication funguje tak, že když uživatel kontroluje nastavení trvalého souboru cookie v prohlížeči "nezobrazovat dotaz dalších **X** dní" pole při přihlášení. Uživatel nebude výzva k MFA znovu z tohoto prohlížeče do vypršení platnosti souboru cookie. Pokud uživatel otevře jiný prohlížeč na stejné zařízení nebo vymaže jejich souborů cookie, budou vyzváni k ověření znovu. 

"Nezobrazovat dotaz dalších **X** dní" zaškrtávací políčko se nezobrazí na neprohlížečové aplikace, jestli podporují moderní ověřování. Tyto aplikace použít tokeny obnovení, které poskytují nové přístupové tokeny každou hodinu. Pokud token obnovení je ověřen, Azure AD kontroly, aby byla provedena poslední čas dvoustupňové ověření byla v rámci zadaného počtu dnů. 

Proto nezapomeňte vícefaktorového ověřování na důvěryhodných zařízeních snižuje počet ověření na webové aplikace (které obvykle výzvu pokaždé, když) ale zvyšuje počet ověření pro klienty ověřování, (které obvykle výzvu každých 90 dní).

> [!NOTE]
>Tato funkce není kompatibilní s "Zůstat přihlášeni" funkce služby AD FS, když uživatelé provádět dvoustupňové ověřování pro službu AD FS pomocí Azure MFA serveru nebo řešení třetí strany vícefaktorového ověřování. Pokud uživatelé ve službě AD FS vyberte možnost "Zůstat přihlášeni" a také označit jako důvěryhodné zařízení pro MFA, by nebylo možné ověřit po vypršení platnosti "Zapamatovat MFA" počet dnů. Azure AD požadavky čerstvé dvoustupňové ověření, ale služby AD FS vrátí token s původní vícefaktorového ověřování deklarací identity a datum namísto provádění dvoustupňové ověření znovu. Toto nastaví mimo smyčku ověření mezi Azure AD a služby AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Povolit zapamatovat vícefaktorové ověřování
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**
3. Vyberte **Multi-Factor Authentication**.
4. V části ověřování Multi-Factor Authentication, vyberte **nastavení služby**.
5. Na stránce nastavení služby v rámci **spravovat zapamatovat vícefaktorové ověřování**, zkontrolujte **povolit uživatelům zapamatovat vícefaktorové ověřování u zařízení, které důvěřují** pole.

   ![Mějte na paměti, zařízení](./media/multi-factor-authentication-whats-next/remember.png)

6. Nastavte počet dní, které chcete povolit důvěryhodná zařízení obejít dvoustupňové ověření. Výchozí hodnota je 14 dnů.
7. Vyberte **Uložit**.

### <a name="mark-a-device-as-trusted"></a>Označit jako důvěryhodné zařízení

Po povolení této funkce, uživatelé mohou označit zařízení jako důvěryhodné při přihlášení kontrolou **dotaz již nezobrazovat**.

![Dotaz již nezobrazovat – snímek obrazovky](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Volitelný ověření metody
Můžete zvolit, které metody ověřování jsou k dispozici pro vaše uživatele. Následující tabulka obsahuje stručný přehled jednotlivých metod.

Když uživatelé zaregistrují svoje účty pro MFA, vybírá jejich metoda upřednostňované ověření mimo možnosti, které jste povolili. Pokyny k jejich registraci, najdete v článku [nastavit účtu pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md)

| Metoda | Popis |
|:--- |:--- |
| Volání na telefon |Umístí automatický hlasový hovor. Uživatel přijme hovor a stiskem tlačítka # na klávesnici telefonu provede ověření. Toto telefonní číslo není synchronizován do místní služby Active Directory. |
| Textové zprávy na telefon |Odešle textovou zprávu s ověřovacím kódem. Bude uživatel vyzván k zadání ověřovacího kódu do rozhraní přihlášení. Tento proces se nazývá jednosměrné služby SMS. Obousměrné služby SMS znamená, že uživatel musí text zpět konkrétní kód. Obousměrné služby SMS se už nepoužívá a bude již nebude podporována od 14 listopadu 2018. Uživatelé nakonfigurovaní pro obousměrné služby SMS se automaticky přepne na "volání na telefon" ověřování v daném čase.|
| Oznámení pomocí mobilních aplikací |Odešle nabízených oznámení do telefonu nebo zaregistrovaného zařízení. Zobrazení oznámení a vybere uživatele **ověřte** k dokončení ověření. <br>Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Ověřovací kód z mobilní aplikace |Aplikace Microsoft Authenticator generuje každých 30 sekund nový ověřovací kód OATH. Tento ověřovací kód, uživatel zadá do rozhraní přihlášení.<br>Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Povolení nebo zakázání metody ověřování
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**
3. Vyberte **Multi-Factor Authentication**.
4. V části ověřování Multi-Factor Authentication, vyberte **nastavení služby**.
5. Na stránce nastavení služby v rámci **možnosti ověření**, výběrem nebo zrušením výběru možnosti, které chcete použít.

   ![Požadované možnosti ověření](./media/multi-factor-authentication-whats-next/authmethods.png)

6. Klikněte na **Uložit**.
