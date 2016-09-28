<properties 
    pageTitle="Ověření služby RADIUS a server Azure Multi-Factor Authentication"
    description="Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření RADIUS a serveru Azure Multi-Factor Authentication."
    services="multi-factor-authentication"
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




# Ověření služby RADIUS a server Azure Multi-Factor Authentication

V části ověřování pomocí protokolu RADIUS umožňuje povolit a konfigurovat ověřování RADIUS pro server Azure Multi-Factor Authentication. RADIUS je standardní protokol pro přijímání požadavků na ověření a zpracování těchto požadavků. Server Azure Multi-Factor Authentication funguje jako server RADIUS a je vložen mezi vašeho klienta protokolu RADIUS (např. zařízení VPN) a cíl ověřování, který může být adresář služby Active Directory (AD), adresář LDAP nebo jiný server RADIUS, aby bylo možné přidat ověřování Azure Multi-Factor Authentication. Aby ověřování Azure Multi-Factor Authentication fungovalo, musíte server Azure Multi-Factor Authentication nakonfigurovat tak, aby komunikoval s klientskými servery a cílem ověřování. Server Azure Multi-Factor Authentication přijímá požadavky od klienta RADIUS, ověřuje pověření proti cíli ověřování, přidává ověřování Azure Multi-Factor Authentication a odesílá odpověď zpět do klienta protokolu RADIUS. Celé ověřování bude úspěšné pouze v případě, že primární ověřování Multi-Factor Authentication uspěje.

>[AZURE.NOTE]
>Server MFA podporuje pouze PAP (protokol ověřování hesla) a protokoly MSCHAPv2 (protokol ověřování Challenge Handshake společnosti Microsoft) RADIUS při fungování jako server RADIUS.  Když server MFA funguje jako server proxy protokolu RADIUS na jiný server RADIUS, který podporuje tento protokol, například Microsoft NPS, lze použít jiné protokoly, například protokol EAP (extensible authentication protocol).
></br>
>Pokud v této konfiguraci používáte jiné protokoly, jednosměrné tokeny SMS a OATH nebudou funkční, protože Server MFA není schopen vytvořit úspěšné odpovědi RADIUS na výzvy k ověření pomocí protokolu.


![Ověřování Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## Konfigurace ověření RADIUS

Pro konfiguraci ověřování pomocí protokolu RADIUS nainstalujte server Azure Multi-Factor Authentication na server Windows. Pokud máte prostředí služby Active Directory, server by měl být připojen k doméně uvnitř sítě. Pomocí následujícího postupu nakonfigurujte server Azure Multi-Factor Authentication:

1. V rámci serveru Azure Multi-Factor Authentication klikněte na ikonu ověření služby RADIUS v levé nabídce.
2. Zaškrtněte políčko Povolit ověřování pomocí protokolu RADIUS.
3. Na kartě Klienti změňte porty ověřování a komunikace se zákazníky, pokud by měla služba RADIUS Azure Multi-Factor Authentication vytvořit vazbu na nestandardní porty pro naslouchání požadavků protokolu RADIUS od klientů, kteří budou nakonfigurováni.
4. Klikněte na tlačítko Přidat... .
5. V dialogovém okně Přidat klienta RADIUS zadejte IP adresu zařízení/serveru, který se bude ověřovat na server Azure Multi-Factor Authentication, název aplikace (volitelné) a sdílený tajný klíč. Sdílený tajný klíč bude muset být stejný jak na serveru Azure Multi-Factor Authentication , tak i v zařízení/serveru. Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
6. Zaškrtněte políčko Vyžadovat uživatele Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud dosud nebyl importován velký počet uživatelů do serveru a/nebo budou vyloučeni z Multi-Factor Authentication, nechejte pole nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
7. Zaškrtněte políčko Povolit záložní token OATH, pokud budou uživatelé používat ověřování mobilní aplikaci Azure Multi-Factor Authentication a chcete použít jako záložní ověřování hesla OATH pro telefonní volání, SMS nebo nabízená oznámení mimo pásmo.
8. Klikněte na tlačítko OK.
9. Kroky 4 až 8 můžete opakovat pro přidání dalších klientů RADIUS.
10. Klikněte na kartu Cíl.
11. Pokud je server Azure Multi-Factor Authentication nainstalován na server připojený k doméně v prostředí služby Active Directory, vyberte doménu systému Windows.
12. Pokud musí být uživatelé ověřování proti adresáři LDAP, vyberte vazbu protokolu LDAP. Při použití vazby protokolu LDAP musíte kliknout na ikonu integrace adresáře a upravit konfiguraci LDAP na kartě nastavení tak, aby server mohl vytvořit vazbu k adresáři. Pokyny ke konfiguraci LDAP naleznete v průvodci konfigurací serveru Proxy protokolu LDAP.
13. Pokud mají být uživatelé ověřování proti jinému serveru RADIUS, vyberte servery RADIUS.
14. Nakonfigurujte server, na který server předává požadavky protokolu RADIUS přes proxy kliknutím na tlačítko Přidat... .
15. V dialogovém okně Přidat server RADIUS zadejte IP adresu serveru RADIUS a sdílený tajný klíč. Sdílený tajný klíč bude muset být stejný jak na serveru Azure Multi-Factor Authentication , tak i na serveru RADIUS. Změňte port ověřování a port monitorování účtů, pokud server RADIUS využívá jiné porty.
16. Klikněte na tlačítko OK.
17. Server Azure Multi-Factor Authentication musíte přidat jako klienta protokolu RADIUS v jiném serveru RADIUS, aby mohl zpracovávat požadavky na přístup odeslané ze serveru Azure Multi-Factor Authentication. Je nutné použít stejný sdílený tajný klíč konfigurovaný na serveru Azure Multi-Factor Authentication.
18. Tento krok můžete opakovat pro přidání dalších serverů RADIUS a konfigurovat příkaz, ve kterém je server musí volat pomocí tlačítek nahoru a dolů. Dokončení konfigurace serveru Azure Multi-Factor Authentication. Server teď naslouchá na nakonfigurovaných portech požadavkům přístupu protokolu RADIUS z konfigurovaných klientů.   


## Konfigurace klienta RADIUS

Chcete-li nakonfigurovat klienta RADIUS, postupujte podle pokynů:

- Nakonfigurujte zařízení/server k ověřování prostřednictvím protokolu RADIUS nIP adrese serveru Azure Multi-Factor Authentication, která bude fungovat jako RADIUS server.
- Použijte stejný sdílený tajný klíč, který byl nakonfigurován výše.
- Časový limit platnosti protokolu RADIUS nakonfigurujte na 30–60 sekund, aby byl dostatek času na ověření pověření uživatele, provedení ověřování multi-factor authentication, obdržení odpovědi a pak odpověď na žádost o přístup protokolu RADIUS.



<!--HONumber=Sep16_HO3-->


