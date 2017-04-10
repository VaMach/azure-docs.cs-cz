---
title: "Začínáme s Azure Multi-Factor Authentication Serverem | Dokumentace Microsoftu"
description: "Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s Azure MFA Serverem."
services: multi-factor-authentication
keywords: "authentication server,stránka pro aktivaci aplikace azure multi factor authentication,stažení authentication serveru"
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: b769f785f67c24b99156dcfc21a42f661cc9da27
ms.lasthandoff: 03/31/2017

---

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Začínáme s Azure Multi-Factor Authentication Serverem

<center>![Místní MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Teď, když jsme zjistili, jestli se má použít místní Multi-Factor Authentication Server, se můžeme dát do toho. Tato stránka popisuje novou instalaci serveru a jeho nastavení pro spolupráci s místní službou Active Directory. Pokud už máte nainstalovaný server MFA a chcete provést upgrade, přečtěte si téma [Upgrade na nejnovější verzi Azure Multi-Factor Authentication Serveru](multi-factor-authentication-server-upgrade.md). Pokud hledáte informace pouze o instalaci webové služby, přečtěte si téma [Nasazení webové služby mobilní aplikace Azure Multi-Factor Authentication Serveru](multi-factor-authentication-get-started-server-webservice.md).
 

## <a name="download-the-azure-multi-factor-authentication-server"></a>Stažení Azure Multi-Factor Authentication Serveru
Azure Multi-Factor Authentication Server můžete stáhnout dvěma způsoby. Oba se provádí přes Azure Portal. Prvním je přímá správa poskytovatele služby Multi-Factor Auth. Druhý probíhá přes nastavení služby. Druhá možnost vyžaduje poskytovatele služby Multi-Factor Auth nebo licenci Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite.

> [!Important]
> Tyto dvě možnosti mohou vypadat podobně, ale je důležité vědět, kterou z nich použít. Pokud mají vaši uživatelé licence, které jsou součástí MFA (Azure MFA, Azure AD Premium nebo Enterprise Mobility + Security), nevytvářejte kvůli stažení serveru poskytovatele služby Multi-Factor Auth. Místo toho využijte možnost 2 a stáhněte server ze stránky nastavení služby. 

### <a name="option-1-download-azure-multi-factor-authentication-server-from-the-azure-classic-portal"></a>Možnost 1: Stažení Azure Multi-Factor Authentication Serveru z portálu Azure Classic

Tuto možnost použijte, pokud již máte poskytovatele služby Multi-Factor Auth, protože za MFA platíte podle počtu povolených uživatelů nebo ověřování. 

1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Na stránce služby Active Directory klikněte na **Multi-Factor Auth Providers** (Poskytovatelé služby Multi-Factor Authentication).
    ![Multi-Factor Auth Providers (Poskytovatelé služby Multi-Factor Authentication)](./media/multi-factor-authentication-get-started-server/authproviders.png)
4. V dolní části klikněte na **Spravovat**. Otevře se nová stránka.
5. Klikněte na **Soubory ke stažení**.
6. Klikněte na odkaz **Stáhnout** nad částí **Vytvoření přihlašovacích údajů pro aktivaci**.
   ![Stáhnout](./media/multi-factor-authentication-get-started-server/download4.png)
7. Uložte stažený soubor.

### <a name="option-2-download-azure-multi-factor-authentication-server-from-the-service-settings"></a>Možnost 2: Stažení Azure Multi-Factor Authentication Serveru z nastavení služby

Tuto možnost použijte, pokud máte licence Enterprise Mobility Suite, Azure AD Premium nebo Enterprise Cloud Suite. 

1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Dvakrát klikněte na svoji instanci služby Azure AD.
4. Nahoře klikněte na **Konfigurovat**.
5. Přejděte dolů do části **vícefaktorové ověřování** a vyberte **Spravovat nastavení služby**.
6. Dole na stránce s nastavením služby klikněte na **Přejít na portál**. Otevře se nová stránka.
   ![Stáhnout](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Klikněte na **Soubory ke stažení**.
8. Klikněte na odkaz **Stáhnout** nad částí **Vytvoření přihlašovacích údajů pro aktivaci**.
    ![Stáhnout](./media/multi-factor-authentication-get-started-server/download4.png)
9. Uložte stažený soubor.

## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Instalace a konfigurace Azure Multi-Factor Authentication Serveru
Teď, když jste server stáhli, ho můžete nainstalovat a nastavit.  Ujistěte se, že server, na kterém ho chcete nainstalovat, splňuje tyto podmínky:

| Požadavky pro Azure Multi-Factor Authentication Server | Popis |
|:--- |:--- |
| Hardware |<li>200 MB volného místa na pevném disku</li><li>Procesor kompatibilní s x32 nebo x64</li><li>1 GB RAM nebo víc</li> |
| Software |<li>Windows Server 2008 nebo novější, pokud je hostitelem OS serveru</li><li>Windows 7 nebo novější, pokud je hostitelem OS klienta</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 nebo novější, pokud instalujete uživatelský portál nebo sadu SDK webové služby</li> |

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Požadavky na firewall pro Azure Multi-Factor Authentication Server
- - -
Každý server MFA musí být schopný komunikovat na odchozím portu 443 s těmito adresami:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Pokud brána firewall omezuje odchozí port 443, je nutné otevřít tyto rozsahy IP adres:

| Podsíť IP | Síťová maska | Rozsah IP adres |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Pokud nepoužíváte funkci Potvrzení události a vaši uživatelé nepoužívají k ověřování ze zařízení v podnikové síti mobilní aplikace, potřebujete pouze následující rozsahy:

| Podsíť IP | Síťová maska | Rozsah IP adres |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Instalace a konfigurace serveru Azure Multi-Factor Authentication

Tyto kroky ukazovaly expresní instalaci pomocí průvodce konfigurací. Pokud se průvodce nezobrazí nebo jej chcete spustit znovu, můžete jej vybrat z nabídky **Nástroje** na serveru.

1. Dvakrát klikněte na spustitelný soubor. 
2. Na obrazovce Výběr instalační složky zkontrolujte, že je složka zadaná správně, a klikněte na **Další**.
3. Až instalace skončí, klikněte na **Dokončit**.  Spustí se průvodce konfigurací.
4. Na úvodní obrazovce průvodce konfigurací zaškrtněte políčko **Vynechat použití průvodce konfigurací ověřování** a klikněte na **Další**.  Průvodce se zavře a spustí se server.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Zpátky na stránce, odkud jste server stáhli, klikněte na tlačítko **Vytvoření přihlašovacích údajů pro aktivaci**. Tyto údaje zkopírujte do příslušných polí v Azure MFA Serveru a klikněte na **Aktivovat**.

## <a name="import-users-from-active-directory"></a>Import uživatelů ze služby Active Directory
Server nainstalovaný a nakonfigurovaný a teď můžete do Azure MFA Serveru snadno importovat uživatele.

1. V Azure MFA Serveru klikněte vlevo na **Uživatelé**.
2. Dole vyberte **Importovat ze služby Active Directory**.
3. Teď můžete hledat jednotlivé uživatele nebo v adresáři AD vyhledat organizační jednotky, ve kterých jsou uživatelé.  V tomto případě vyhledáme OJ uživatele.
4. Vpravo označte všechny uživatele a klikněte na **Importovat**.  Mělo by se zobrazit vyskakovací okno s informací, že akce proběhla úspěšně.  Zavřete okno importu.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Zaslání e-mailu uživatelům
Teď, když jste naimportovali uživatele do MFA Serveru, odešlete jim e-mail s informací, že byli zaregistrování k dvoustupňovému ověřování.

Jaký e-mail odešlete byste měli určit podle toho, jak jste pro uživatele nakonfigurovali dvoustupňové ověřování. Například pokud se vám podařilo naimportovat telefonní čísla z adresáře společnosti, e-mail by měl obsahovat výchozí telefonní čísla, aby uživatelé věděli, co mají očekávat. Pokud jste telefonní čísla nenaimportovali nebo pokud budou uživatelé používat mobilní aplikace, odešlete jim e-mail, který je nasměruje k dokončení registrace účtu. Do e-mailu přidejte hypertextový odkaz na portál Azure Multi-Factor Authentication User Portal.

Obsah e-mailu se liší také podle metody ověřování nastavené pro konkrétního uživatele (telefonní hovor, zpráva SMS nebo mobilní aplikace).  Pokud například uživatel musí při ověřování zadat PIN, v e-mailu se dozví, jaký počáteční PIN je pro něj nastavený.  Uživatelé musí při prvním ověření svůj PIN změnit.


### <a name="configure-email-and-email-templates"></a>Konfigurace e-mailu a šablon e-mailu
Kliknutím vlevo na ikonu e-mailu můžete změnit nastavení odesílání těchto e-mailů. Na této stránce můžete zadat informace o vašem poštovním serveru SMTP a odesílat e-maily zaškrtnutím políčka **Odesílat uživatelům e-maily**.

![Nastavení e-mailu](./media/multi-factor-authentication-get-started-server/email1.png)

Na kartě Obsah e-mailu uvidíte šablony e-mailů, ze kterých si můžete vybrat. V závislosti na tom, jak jste uživatelům nakonfigurovali dvoustupňové ověřování, vyberte nejvhodnější šablonu.

![Šablony e-mailů](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Jak Azure Multi-Factor Authentication Server nakládá s uživatelskými daty
Když Multi-Factor Authentication (MFA) Server používáte lokálně, uživatelská data se ukládají na lokálních serverech. V cloudu se neukládají žádná trvalá data. Když uživatel provádí dvoustupňové ověření, MFA Server odešle data do cloudové služby Azure MFA, a tam se provede ověření. Když se tyto požadavky na ověření pošlou do cloudové služby, odešlou se v žádosti a následující pole a protokoly, aby byly dostupné pro sestavy o používání/ověřování zákazníka. Některá tato pole jsou volitelná a můžou se v Multi-Factor Authentication Serveru zapnout nebo vypnout. Komunikace z MFA Serveru do cloudové služby MFA probíhá přes SSL/TLS na odchozím portu 443. Tato pole jsou:

* Jedinečné ID - uživatelské jméno nebo interní ID serveru MFA
* Jméno a příjmení (volitelné)
* E-mailová adresa (volitelné)
* Telefonní číslo – při ověření přes telefonní hovor nebo zprávu SMS
* Token zařízení - při ověření přes mobilní aplikaci
* Režim ověřování
* Výsledek ověřování
* Název MFA Serveru
* IP adresa serveru MFA
* IP adresa klienta – pokud je dostupná

Vedle těchto polí se s ověřovacími údaji uloží taky výsledek ověření (úspěch/zamítnuto) a případně důvod zamítnutí, které jsou dostupné v sestavách o ověřování/používání.

## <a name="next-steps"></a>Další kroky

- Nastavení a konfigurace portálu [User Portal](multi-factor-authentication-get-started-portal.md) pro uživatelskou samoobsluhu.

- Nastavení a konfigurace Azure MFA Serveru pomocí [služby Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md), [ověřování pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-radius.md) nebo [ověřování pomocí protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md).

- Nastavení a konfigurace [Brány vzdálené plochy a Azure Multi-Factor Authentication Serveru používajícího protokol RADIUS](multi-factor-authentication-get-started-server-rdg.md). 

- [Nasazení webové služby mobilní aplikace Azure Multi-Factor Authentication Serveru](multi-factor-authentication-get-started-server-webservice.md).

- [Pokročilé scénáře se službou Azure Multi-Factor Authentication a sítěmi VPN třetích stran](multi-factor-authentication-advanced-vpn-configurations.md).

