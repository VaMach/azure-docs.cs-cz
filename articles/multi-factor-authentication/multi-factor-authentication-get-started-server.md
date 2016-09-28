<properties 
    pageTitle="Začínáme s Azure Multi-Factor Authentication Serverem"
    description="Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s Azure MFA Serverem."
    services="multi-factor-authentication"
    keywords="authentication server,stránka pro aktivaci aplikace azure multi factor authentication,stažení authentication serveru"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>


# Začínáme s Azure Multi-Factor Authentication Serverem




<center>![Cloud](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Teď, když jsme zjistili, jestli se má použít místní vícefaktorové ověřování, se můžeme dát do toho. Tato stránka popisuje novou instalaci serveru a nastavení pro spolupráci s místní službou Active Directory. Pokud už máte nainstalovaný server PhoneFactor a chcete upgradovat, přečtěte si téma [Upgrade na Azure Multi-Factor Server](multi-factor-authentication-get-started-server-upgrade.md). Pokud hledáte informace jen o instalaci webové služby, přečtěte si téma [Nasazení webové služby mobilní aplikace Azure Multi-Factor Authentication Serveru](multi-factor-authentication-get-started-server-webservice.md).


## Stažení Azure Multi-Factor Authentication Serveru



Azure Multi-Factor Authentication Server můžete stáhnout dvěma způsoby. Oba se provádí přes Azure Portal. Prvním je přímá správa poskytovatele služby Multi-Factor Auth. Druhý probíhá přes nastavení služby. Druhá možnost vyžaduje poskytovatele služby Multi-Factor Auth nebo licenci Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite.


### Stažení serveru Azure Multi-Factor Authentication z portálu Azure
--------------------------------------------------------------------------------

1. Přihlaste se do Portálu Azure jako správce.
2. Vlevo vyberte možnost Active Directory.
3. Na stránce služby Active Directory nahoře vyberte možnost **Poskytovatelé služby Multi-Factor Auth**.
4. V dolní části, klikněte na **Spravovat**.
5. Otevře se nová stránka.  Klikněte na **Soubory ke stažení.**
![Stáhnout](./media/multi-factor-authentication-sdk/download.png)
6. Nad částí **Vytvoření přihlašovacích údajů pro aktivaci** klikněte na **Stáhnout.**
![Stáhnout](./media/multi-factor-authentication-get-started-server/download4.png)
7. Uložte stažený soubor.



### Stažení Azure Multi-Factor Authentication Serveru přes nastavení služby


1. Přihlaste se do Portálu Azure jako správce.
2. Vlevo vyberte možnost Active Directory.
3. Poklikejte na svoji instanci služby Azure AD.
4. Nahoře klikněte na **Konfigurovat**
![Stáhnout](./media/multi-factor-authentication-sdk/download2.png)
5. V části týkající se vícefaktorového ověřování klikněte na **Správa nastavení služby**.
6. Dole na stránce s nastavením služby klikněte na **Přejít na portál**.
![Stáhnout](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Otevře se nová stránka. Klikněte na **Soubory ke stažení**.
8. Nad částí **Vytvoření přihlašovacích údajů pro aktivaci** klikněte na **Stáhnout.**
9. Uložte stažený soubor.




## Instalace a konfigurace Azure Multi-Factor Authentication Serveru
Teď, když jste server stáhli, ho můžete nainstalovat a nastavit.  Ujistěte se, že server, na kterém ho chcete nainstalovat, splňuje tyto podmínky:



Požadavky pro Azure Multi-Factor Authentication Server|Popis|
:------------- | :------------- |
Hardware|<li>200 MB volného místa na pevném disku</li><li>Procesor kompatibilní s x32 nebo x64</li><li>1 GB RAM nebo víc</li>
Software|<li>Windows Server 2008 nebo novější, pokud je hostitelem OS serveru</li><li>Windows 7 nebo novější, pokud je hostitelem OS klienta</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 nebo novější, pokud instalujete uživatelský portál nebo sadu SDK webové služby</li>

### Požadavky na firewall pro Azure Multi-Factor Authentication Server
--------------------------------------------------------------------------------
Každý server MFA musí být schopný komunikovat na odchozím portu 443 s těmito cíli:

- https://pfd.phonefactor.net
- https://pfd2.phonefactor.net
- https://css.phonefactor.net

Pokud firewall omezuje odchozí port 443, je nutné otevřít tyto rozsahy IP adres:

Podsíť IP|Síťová maska|Rozsah IP adres
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Pokud nepoužíváte funkce Potvrzení události služby Azure Multi-Factor Authentication a pokud se uživatelé neověřují přes mobilní aplikace Multi-Factor Auth ze zařízení v podnikové síti, může se rozsah IP adres omezit na tyto:


Podsíť IP|Síťová maska|Rozsah IP adres
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### Instalace a konfigurace serveru Azure Multi-Factor Authentication
--------------------------------------------------------------------------------


1. Poklikejte na spustitelný soubor. Spustí se instalace.
2. Na obrazovce Výběr instalační složky zkontrolujte, že je složka zadaná správně, a klikněte na Další.
3. Až instalace skončí, klikněte na Dokončit.  Spustí se průvodce konfigurací.
4. Na úvodní obrazovce průvodce instalací označte zatržítko **Vynechat použití průvodce konfigurací ověřování** a klikněte na **Další**.  Průvodce se zavře a spustí se server.
![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)

5. Zpátky na stránce, odkud jste server stáhli, klikněte na tlačítko **Vytvoření přihlašovacích údajů pro aktivaci**.  Tyto údaje zkopírujte do příslušných polí v Azure MFA Serveru a klikněte na **Aktivovat**.


Předchozí kroky ukazují expresní nastavení pomocí průvodce konfigurací.  Průvodce ověřením můžete spustit znovu z nabídky Nástroje, která je na serveru.



##Import uživatelů ze služby Active Directory

Server nainstalovaný a nakonfigurovaný a teď můžete do Azure MFA Serveru snadno importovat uživatele.

### Import uživatelů ze služby Active Directory
--------------------------------------------------------------------------------


1. V Azure MFA Serveru klikněte vlevo na **Uživatelé**.
2. Dole vyberte **Importovat ze služby Active Directory**.
3. Teď můžete hledat jednotlivé uživatele nebo v adresáři AD vyhledat organizační jednotky, ve kterých jsou uživatelé.  V tomto případě vyhledáme OJ uživatelé.
4. Vpravo označte všechny uživatele a klikněte na **Importovat**.  Mělo by se zobrazit vyskakovací okno s informací, že akce proběhla úspěšně.  Zavřete okno importu.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## Zaslání e-mailu uživatelům
Teď, když jste naimportovali uživatele do serveru Azure Multi-Factor Authentication, se doporučuje poslat uživatelům e-mail s informací, že jsou zapsaní do vícefaktorového ověřování.

Při použití Azure Multi-Factor Authentication Serveru máte různé možnosti, jak nakonfigurovat uživatele pro používání vícefaktorového ověřování.  Pokud například znáte telefonní čísla uživatelů nebo jste do Azure Multi-Factor Authentication Serveru naimportovali telefonní čísla z firemního adresáře, můžete e-mailem uživatelům oznámit, že je pro ně nastavené ověřování Azure Multi-Factor Authentication, poslat jim pokyny pro používání ověřování Azure Multi-Factor Authentication a dát jim vědět, na jaký telefon budou ověření dostávat.  

Obsah e-mailu se bude lišit podle metody ověřování nastavené pro konkrétního uživatele (např. telefonní hovor, SMS, mobilní aplikace).  Pokud například uživatel musí při ověřování zadat PIN, v e-mailu se dozví, jaký PIN je pro něj nastavený.  Uživatelé obvykle musí při prvním ověření svůj PIN změnit.

Pokud telefonní čísla nejsou nakonfigurovaná nebo naimportovaná do Azure Multi-Factor Authentication Serveru nebo pokud je pro uživatele předem nastavené ověřování přes mobilní aplikaci, můžete jim poslat e-mail s informací, že je pro ně nastavené ověřování Azure Multi-Factor Authentication, a odkazem na dokončení zápisu jejich účtu na portálu User Portal pro službu Azure Multi-Factor Authentication.  V e-mailu bude odkaz, kterým se uživatel dostane na portál User Portal. Když uživatel klikne na odkaz, otevře se jeho webový prohlížeč na stránce firemního portálu User Portal pro Azure Multi-Factor Authentication.   


### Konfigurace emailu a šablon e-mailu

Kliknutím vlevo na ikonu e-mailu můžete změnit nastavení odesílání těchto e-mailů.  Můžete tu zadat údaje o vašem SMTP serveru a když označíte zatržítko Odesílat uživatelům e-maily, můžete uživatelům poslat hromadný e-mail.

![Nastavení e-mailu](./media/multi-factor-authentication-get-started-server/email1.png)

Na kartě Obsah e-mailu uvidíte všechny různé šablony, ze kterých si můžete vybrat.  Takže se můžete vybrat nejvhodnější šablonu podle toho, jak jste uživatelům nastavili vícefaktorové ověřování.

![Šablony e-mailů](./media/multi-factor-authentication-get-started-server/email2.png)

## Jak Azure Multi-Factor Authentication Server nakládá s uživatelskými daty

Když Multi-Factor Authentication (MFA) Server používáte lokálně, uživatelská data se ukládají na lokálních serverech. V cloudu se neukládají žádná trvalá data. Když uživatel provádí dvoufaktorové ověření, MFA Server odešle data do cloudové služby Azure MFA, a tam se provede ověření. Když se tyto požadavky na ověření pošlou do cloudové služby, odešlou se v žádosti a následující pole a protokoly, aby byly dostupné pro sestavy o používání/ověřování zákazníka. Některá tato pole jsou volitelná a můžou se v Multi-Factor Authentication Serveru zapnout nebo vypnout. Komunikace z MFA Serveru do cloudové služby MFA probíhá přes SSL/TLS na odchozím portu 443. Tato pole jsou:

- Jedinečné ID - uživatelské jméno nebo interní ID serveru MFA
- Jméno a příjmení - volitelné
- E-mailová adresa - volitelné
- Telefonní číslo - při ověření přes telefonní hovor nebo SMS zprávu
- Token zařízení - při ověření přes mobilní aplikaci
- Režim ověřování
- Výsledek ověřování
- Název serveru MFA
- IP adresa serveru MFA
- IP adresa klienta – pokud je dostupná



Vedle těchto polí se s ověřovacími údaji uloží taky výsledek ověření (úspěch/zamítnuto) a případně důvod zamítnutí, které jsou dostupné v sestavách o ověřování/používání.


## Pokročilá konfigurace Azure Multi-Factor Authentication Serveru
Další informace o pokročilém nastavení a konfiguraci najdete v této tabulce.

Metoda|Popis
:------------- | :------------- |
[Uživatelský portál](multi-factor-authentication-get-started-portal.md)|  Informace o vytvoření a nastavení uživatelského portálu včetně nasazení a samoobslužného provozu pro uživatele.
[Služba AD FS (Active Directory Federation Service)](multi-factor-authentication-get-started-adfs.md)|Informace o nastavení ověřování Azure Multi-Factor Authentication s AD FS.
[Ověřování RADIUS](multi-factor-authentication-get-started-server-radius.md)|  Informace o vytvoření a nastavení Azure MFA Serveru s protokolem RADIUS.
[Ověřování IIS](multi-factor-authentication-get-started-server-iis.md)|Informace o vytvoření a nastavení Azure MFA Serveru se službou IIS.
[Ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md)|  Informace o vytvoření a nastavení Azure MFA Serveru s Ověřováním systému Windows.
[Ověřování pomocí protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informace o vytvoření a nastavení Azure MFA Serveru s ověřováním pomocí protokolu LDAP.
[Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informace o vytvoření a nastavení Azure MFA Serveru s Bránou vzdálení plochy pomocí protokolu RADIUS.
[Synchronizace s Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informace o vytvoření a nastavení synchronizace mezi službou Active Directory a Azure MFA Serverem.
[Nasazení webové služby mobilní aplikace pro Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server-webservice.md)|Informace o vytvoření a nastavení webové služby serveru Azure MFA.



<!--HONumber=Sep16_HO3-->


