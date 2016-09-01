<properties 
    pageTitle="Ověření služby LDAP a server Azure Multi-Factor Authentication" 
    description="Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření LDAP a serveru Azure Multi-Factor Authentication." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Ověření služby LDAP a server Azure Multi-Factor Authentication 


Ve výchozím nastavení je server Azure Multi-Factor Authentication nakonfigurován pro import nebo synchronizaci uživatelů ze služby Active Directory. Můžete ho však navázat na různé adresáře LDAP, například adresář ADAM nebo konkrétní řadič domény služby Active Directory. Při konfiguraci připojení k adresáři prostřednictvím protokolu LDAP, může být server Azure Multi-Factor Authentication nakonfigurován pro fungování jako server proxy protokolu LDAP k provedení ověření. Umožňuje také pro použití vazby protokolu LDAP jako cíle RADIUS pro předběžné ověření uživatele při použití ověřování pomocí služby IIS, nebo pro primární ověření na portálu Azure Multi-Factor Authentication.

Při použití Azure Multi-Factor Authentication jako proxy protokolu LDAP, je server Azure Multi-Factor Authentication vložen mezi klienta LDAP (např. zařízení VPN, aplikace) a adresářový server LDAP, aby bylo možné přidat ověřování Multi-Factor Authentication. Aby ověřování Azure Multi-Factor Authentication fungovalo, musí být server Azure Multi-Factor Authentication nakonfigurován tak, aby komunikoval s klientskými servery a adresářem LDAP. V této konfiguraci server Azure Multi-Factor Authentication přijímá požadavky LDAP od klientských serverů a aplikací a předává je cílovému adresářovému serveru LDAP pro ověření primárních pověření. Pokud odpověď z adresáře LDAP ukazuje, že primární pověření jsou platná, provede Azure Multi-Factor Authentication dvouúrovňové ověřování a odešle odpověď zpět do klienta LDAP. Celé ověřování bude úspěšné pouze v případě, že ověřování na serveru LDAP a ověřování multi-factor authentication uspějí. 





## Konfigurace ověření LDAP


Pro konfiguraci ověřování pomocí protokolu LDAP nainstalujte server Azure Multi-Factor Authentication na server Windows. Použijte následující postup: 

1. V rámci serveru Azure Multi-Factor Authentication klikněte na ikonu ověření služby LDAP v levé nabídce.
2. Zaškrtněte políčko Povolit ověřování pomocí protokolu LDAP.![Ověřování pomocí protokolu LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png) 
3. Na kartě Klienti změňte port TCP a SSL, pokud by měla služba LDAP Azure Multi-Factor Authentication vytvořit vazbu na nestandardní porty pro naslouchání požadavků protokolu LDAP od klientů, kteří budou nakonfigurováni.
4. Pokud plánujete používat LDAPS z klienta na server Azure Multi-Factor Authentication, musí být na serveru, na kterém server běží, nainstalován certifikát SSL. Klikněte na tlačítko Procházet... vedle pole certifikátu SSL a vyberte nainstalovaný certifikát, který bude použit pro zabezpečené připojení. 
5. Klikněte na tlačítko Přidat... .
6. V dialogovém okně Přidat klienta LDAP zadejte IP adresu zařízení, serveru nebo aplikace, které se budou ověřovat na server a název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
7. Zaškrtněte políčko Vyžadovat uživatele Azure Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud dosud nebyl importován velký počet uživatelů do serveru a/nebo budou vyloučeni z Multi-Factor Authentication, nechejte pole nezaškrtnuté. Další informace o této funkci naleznete v souboru nápovědy. 
8. Kroky 5 až 7 můžete opakovat pro přidání dalších klientů LDAP.
9. Pokud je Azure Multi-Factor Authentication nakonfigurováno pro příjem ověřování LDAP, musí směrovat proxy těchto ověřování do adresáře protokolu LDAP. Proto karta Cíl pouze zobrazí jednu zašedlou možnost použití cíle LDAP. Pro konfiguraci připojení adresáře LDAP klikněte na ikonu integrace adresáře. 
10. Na kartě Nastavení vyberte přepínač Použít konkrétní konfiguraci LDAP.
11. Klikněte na tlačítko Upravit... .
12. V dialogovém okně Upravit konfiguraci LDAP vyplňte pole pomocí informací požadovaných pro připojení k adresáři protokolu LDAP. Popisy polí jsou zahrnuty v následující tabulce. Poznámka: Tyto informace jsou také uvedeny v souboru nápovědy serveru Azure Multi-Factor Authentication.![Integrace adresáře](./media/multi-factor-authentication-get-started-server-ldap/ldap.png) 
13. Otestujte připojení LDAP kliknutím na tlačítko Test.
14. Pokud byl test připojení LDAP úspěšný, klikněte na tlačítko OK. 
15. Klikněte na kartu Filtry. Server je předem nakonfigurovaný k načítání kontejnerů, skupin zabezpečení a uživatelů ze služby Active Directory. Pokud provedete navázání na jiný adresář LDAP, budete pravděpodobně muset upravit zobrazené filtry. Klikněte na odkaz nápovědy pro další informace o filtrech.
16. Klikněte na kartu Atributy. Server je předem nakonfigurovaný k mapování atributů ze služby Active Directory.
17. Pokud provádíte navázání na jiný adresář protokolu LDAP nebo měníte mapování předem nakonfigurovaných atributů, klikněte na tlačítko Upravit... .
18. V dialogovém okně Upravit atributy upravte mapování atributů protokolu LDAP pro váš adresář. Názvy atributů lze zadat nebo vybrat kliknutím... na tlačítko vedle každého pole.
19. Klikněte na odkaz nápovědy pro další informace o atributech.
20. Klikněte na tlačítko OK.
21. Klikněte na ikonu Nastavení společnosti a vyberte kartu Překlad uživatelského jména.
22. Při připojování ke službě Active Directory ze serveru připojeného k doméně byste měli být schopni ponechat identifikátory zabezpečení používání systému Windows (SID) pro odpovídající vybraný přepínač uživatelských jmen. V opačném případě vyberte možnost Použít atribut jedinečného identifikátoru LDAP pro odpovídající přepínač uživatelských jmen. Vyberete-li tuto možnost, server Azure Multi-Factor Authentication se pokusí přeložit každé uživatelské jméno na jedinečný identifikátor v adresáři protokolu LDAP. Vyhledávání protokolem LDAP se provede u atributů uživatelského jména definovaných v adresáři Integrace -> karta Atributy. Při ověření uživatele bude uživatelské jméno přeloženo na jedinečný identifikátor v adresáři protokolu LDAP a jedinečný identifikátor se použije pro spárování uživatele v datovém souboru Azure Multi-Factor Authentication. To umožňuje porovnávání s rozlišováním velkých a malých písmen a také dlouhých a krátkých formátů uživatelských jmen. Tato funkce uzavírá konfiguraci serveru Azure Multi-Factor Authentication. Server nyní naslouchá na nakonfigurovaných portech žádosti o přístup do LDAP z konfigurovaných klientů a je nastaven pro proxy směřování těchto požadavků do adresáře LDAP pro ověřování.


## Konfigurace klienta LDAP

Chcete-li nakonfigurovat klienta LDAP, postupujte podle pokynů:

- Nakonfigurujte zařízení, server nebo aplikaci k ověřování prostřednictvím protokolu LDAP na serveru Azure Multi-Factor Authentication, jako by šlo o váš adresář LDAP. Musíte použít stejné nastavení, které běžně používáte pro připojení přímo k adresáři LDAP, s výjimkou názvu serveru nebo IP adresy, které budou stejné jako u serveru Azure Multi-Factor Authentication. 
- Časový limit platnosti protokolu LDAP nakonfigurujte na 30–60 sekund, aby byl dostatek času na ověření pověření uživatele v adresáři LDAP, provedení dvouúrovňového ověřování, obdržení odpovědi a pak odpověď na žádost o přístup protokolu LDAP. 
- V případě použití LDAPS musí zařízení nebo server provádějící žádosti LDAP důvěřovat certifikátu SSL nainstalovaném na serveru Azure Multi-Factor Authentication.




<!--HONumber=Aug16_HO4-->


