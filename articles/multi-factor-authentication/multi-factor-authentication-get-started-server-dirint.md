<properties 
    pageTitle="Integrace adresáře mezi službami Azure Multi-Factor Authentication a Active Directory" 
    description="Toto je stránka vícefaktorového ověření Azure, která popisuje postup integrace Azure Multi-Factor Authentication Serveru se službou Active Directory pro synchronizaci adresářů." 
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

# Integrace adresáře mezi Azure MFA Serverem a službou Active Directory

V části Integrace adresáře můžete nakonfigurovat server pro integraci se službou Active Directory nebo jiným adresářem LDAP.  Můžete tu taky nastavit atributy podle schématu adresáře a zapnout automatickou synchronizaci uživatelů. 

## Nastavení
Ve výchozím nastavení je Azure Multi-Factor Authentication Server nakonfigurovaný pro import nebo synchronizaci uživatelů ze služby Active Directory.  Tato karta vám umožní potlačit výchozí chování a vytvořit vazbu na jiný adresář LDAP, adresář ADAM, nebo konkrétní ovladač domény Active Directory.  Taky umožňuje použití Ověřování pomocí protokolu LDAP na proxy serveru LDAP nebo pro Vázání protokolu LDAP jako cíl pro RADIUS, předběžné ověření pro Ověřování IIS nebo pro primární ověření pro portál User Portal.  V následující tabulce jsou popsaná jednotlivá nastavení.

![Nastavení](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funkce | Popis |
| ------- | ----------- |
| Použít Active Directory | Vyberte možnost Použít Active Directory, pokud chcete používat Active Directory pro importování a synchronizaci.  Toto je výchozí nastavení. <br>Poznámka: Aby integrace služby Active Directory fungovala správně, musí být počítač připojený k doméně a vy musíte být přihlášení pomocí doménového účtu. |
| Zahrnout důvěryhodné domény | Označte zatržítko Zahrnout důvěryhodné domény, pokud chcete, aby se agent pokusil připojit k doménám, kterým důvěřuje aktuální doména, k jiné doméně v doménové struktuře nebo k doménám ve vztahu důvěryhodnosti doménové struktury.  Pokud neimportujete ani nesynchronizujete uživatele z jakékoli důvěryhodné domény, zrušte označení tohoto zatržítka – zvýší se tím výkon.  Ve výchozím nastavení je zatržítko označené. |
| Použít specifickou konfiguraci LDAP | Vyberte možnost Použít LDAP, pokud chcete pro importování a synchronizaci použít specifické nastavení LDAP. Poznámka: Pokud je vybraná možnost Použít LDAP, v uživatelském prostředí se odkazy na Active Directory změní na odkazy na LDAP. |
| Tlačítko Upravit | Tlačítko Upravit umožňuje udělat změny nastavení konfigurace. |
| Použít dotazy na obor atributů | Určuje, jestli se mají použít dotazy na obor atributů.  Dotazy na obor atributů umožňují efektivní prohledávání adresáře a kvalifikaci záznamů podle položek v atributech jiného záznamu.  Azure Multi-Factor Authentication Server používá dotazy na obor atributů pro efektivní dotazování uživatelů, kteří jsou členy skupiny zabezpečení.   <br>Poznámka:  Existují případy, kdy se sice dotazy na obor atributů podporují, ale použít by se neměly.  Active Directory třeba může mít s dotazy na obor atributů problémy, pokud skupina zabezpečení obsahuje členy z více než jedné domény.  V takovém případě by zatržítko nemělo být označené. |

V následující tabulce jsou popsaná nastavení konfigurace LDAP.

| Funkce | Popis |
| ------- | ----------- |
| Server | Zadejte název hostitele nebo IP adresu serveru, na kterém běží adresář LDAP.  Můžete taky zadat záložní server oddělený středníkem. <br>Poznámka: Když je Typ vazby nastavený na SSL, obvykle je potřeba zadat plně kvalifikovaný název hostitele. |
| Základní rozlišující název | Zadejte rozlišující název objektu základního adresáře, ze kterého budou začínat všechny dotazy na adresář.  Příklad: dc=abc,dc=com. |
| Typ vazby - Dotazy | Vyberte požadovaný typ vazby pro použití při vytváření vazby pro vyhledávání v adresáři LDAP.  Používá se pro importy, synchronizaci a překlad uživatelského jména. <br><br>  Anonymní - Vytvoří se anonymní vazba.  Hodnoty Rozlišující název vázání a Heslo vázání se nepoužijí.  To bude fungovat, jen pokud adresář LDAP umožňuje anonymní vytváření vazby a oprávnění umožňují dotazování příslušných záznamů a atributů.  <br><br> Jednoduchá - Hodnoty Rozlišující název vázání a Heslo vázání se předají jako prostý text pro vazby k adresáři LDAP.  Mělo by se použít jen pro testovací účely k ověření, jestli se dá kontaktovat server a jestli má účet vazby správný přístup.  Po nainstalování příslušného certifikátu se místo toho doporučuje použít SSL.  <br><br> SSL - Hodnoty Rozlišující název vázání a Heslo vázání se zašifrují pomocí SSL pro vazbu k adresáři LDAP.  K tomu je potřeba mít lokálně nainstalovaný certifikát, kterému adresář LDAP důvěřuje.  <br><br> Windows - Uživatelské jméno vázání a Heslo vázání se použijí pro zabezpečené připojení k ovladači domény Active Directory nebo adresáře ADAM.  Pokud je pole Uživatelské jméno vázání prázdné, pro vazbu se použije účet přihlášeného uživatele. |
| Typ vazby - Ověřování | Vyberte požadovaný typ vazby pro použití při ověřování vazby LDAP.  Podívejte se na popis typů vazby v části Typ vazby – Dotazy.  Umožňuje například použít Anonymní vázání pro dotazy a SSL vázání pro zabezpečení ověření vazby LDAP. |
| Rozlišující název vázání nebo Uživatelské jméno vázání | Zadejte rozlišující název uživatelského záznamu pro účet, který se má použít při vytváření vazby k adresáři LDAP.<br><br>Rozlišující název se použije jen, když je Typ vazby nastavený na Jednoduchá nebo SSL.  <br><br>Zadejte uživatelské jméno účtu systému Windows, který se má použít při vytváření vazby k adresáři LDAP, když je Typ vazby nastavený na Windows.  Pokud je pole prázdné, pro vazbu se použije účet přihlášeného uživatele. |
| Heslo vázání | Zadejte heslo vázání pro Rozlišující název vázání nebo uživatelské jméno, které se použije pro vazbu k adresáři LDAP.  Aby se mohlo nastavit heslo pro službu Multi-Factor Auth Server AdSync Service, musí být povolená synchronizace a služba musí běžet na místním počítači.  Heslo se uloží do uložených uživatelských jmen a hesel Windows pod účtem, na kterém běží služba Multi-Factor Auth Server AdSync Service.  Heslo se taky uloží pod účtem, na kterém běží uživatelské prostředí Multi-Factor Auth Serveru a pod účtem, na kterém běží služba Multi-Factor Auth Server Service.  <br><br> Poznámka: Protože se heslo uloží jen do uložených uživatelských jmen a hesel Windows na místním serveru, bude potřeba tento krok udělat na každém Multi-Factor Auth Serveru, který potřebuje přístup k tomuto heslu. |
| Omezení velikosti dotazu | Zadejte omezení velikosti pro maximální počet uživatelů, které vrátí vyhledávání v adresáři.  Toto omezení by mělo být stejné jako omezení nastavené v adresáři LDAP.  Pro velká hledání v případech kdy se nepodporuje stránkování, se import a synchronizace pokusí uživatele načíst v dávkách.  Pokud je tady nastavené omezení větší než omezení nastavené v adresáři LDAP, může se stát, že se někteří uživatelé nenačtou. |
| Tlačítko Test | Kliknutím na tlačítko Test můžete otestovat vazbu na server LDAP.  <br><br> Poznámka: Pro otestování vazby není potřeba, aby byla vybraná možnost Použít LDAP.  Umožní vám otestovat vazbu před použitím konfigurace LDAP. |

## Filtry
Filtry vám umožní nastavit kritéria pro kvalifikování záznamů při prohledávání adresáře.  Nastavením filtru můžete upřesnit rozsah objektů, které chcete synchronizovat.  

![Filtry](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Multi-Factor Authentication má tyto 3 možnosti.

- **Filtr kontejneru** - Zadejte kritéria filtru, která se použijí pro kvalifikaci záznamů kontejneru při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (|(objectClass=organizationalUnit)(objectClass=container)).  Pro jiné adresáře LDAP by se měla použít kritéria, která kvalifikují každý typ objektu kontejneru v závislosti na schématu adresáře.  <br>Poznámka:  Pokud je toto pole prázdné, použije se výchozí hodnota ((objectClass=organizationalUnit)(objectClass=container)).

- **Filtr skupiny zabezpečení** - Zadejte kritéria filtru, která se použijí pro kvalifikaci záznamů skupiny zabezpečení při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648).  Pro jiné adresáře LDAP by se měla použít kritéria, která kvalifikují každý typ objektu skupiny zabezpečení v závislosti na schématu adresáře.  <br>Poznámka: Pokud je toto pole prázdné, použije se výchozí hodnota (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).

- **Filtr uživatele** - Zadejte kritéria filtru, která se použijí pro kvalifikaci záznamů uživatele při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (&(objectClass=user)(objectCategory=person)).  Pro jiné adresáře LDAP by se mělo použít (objectClass=inetOrgPerson) nebo něco podobného v závislosti na schématu adresáře. <br>Poznámka: Pokud je toto pole prázdné, použije se výchozí hodnota (&(objectCategory=person)(objectClass=user)).

## Atributy
Atributy se dají podle potřeby upravit pro konkrétní adresář.  To vám umožní přidat vlastní atributy a nastavit synchronizaci na jen ty atributy, které chcete.  Hodnota každého pole atributu by měla mít název atributu, jak je definovaný ve schématu adresáře.  Další informace jsou v tabulce dole.

![Atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funkce | Popis |
| ------- | ----------- |
| Jedinečný identifikátor | Zadejte název atributu, který slouží jako jedinečný identifikátor kontejneru, skupiny zabezpečení a záznamů uživatele.  V Active Directory je to obvykle objectGUID.  V jiných implementacích LDAP to může být entryUUID nebo něco podobného.  Výchozí hodnota je objectGUID. |
| Tlačítka ... (Vybrat atribut) | Každé pole atributu má vedle sebe tlačítko „...“, které zobrazí dialog Vybrat atribut a umožní vám vybrat atribut ze seznamu. <br><br>Dialog Vybrat atribut.<br><br>Poznámka: Atributy můžete zadat ručně a nemusí se shodovat s atributem v seznamu atributů. |
| Typ jedinečného identifikátoru | Vyberte typ atributu jedinečného identifikátoru.  V Active Directory má atribut objectGUID typ GUID.  V jiných implementacích LDAP to může být typ Pole bajtů ASCII nebo Řetězec.  Výchozí hodnota je GUID. <br><br>Poznámka: Je důležité, aby byla tato položka nastavená správně, protože na Položky synchronizace se odkazuje pomocí jejich Jedinečného identifikátoru a Typ jedinečného identifikátoru se používá pro přímé vyhledání objektu v adresáři.  Pokud se typ nastaví na Řetězec, když adresář ve skutečnosti ukládá hodnotu jako pole znaků ASCII, nebude synchronizace fungovat správně. |
| Rozlišující název | Zadejte název atributu, který obsahuje rozlišující název pro každý záznam.  V Active Directory je to obvykle distinguishedName.  V jiných implementacích LDAP to může být entryDN nebo něco podobného.  Výchozí hodnota je distinguishedName. <br><br>Poznámka: Pokud neexistuje atribut obsahující jen rozlišující název, může se použít atribut adspath.  Část cesty „LDAP://<server>/“ se automaticky odstraní a zůstane jen rozlišující název objektu. |
| Název kontejneru | Zadejte název atributu, který v záznamu kontejneru obsahuje název.  Hodnota tohoto atributu se zobrazí v Hierarchii kontejneru při importu z Active Directory nebo při přidávání položek synchronizace.  Výchozí hodnota je name. <br><br>Poznámka: Pokud různé kontejnery používají pro své názvy různé atributy, můžete zadat několik atributů názvu, oddělených středníkem.  Pro zobrazení názvu objektu kontejneru se použije první název kontejneru, který se v něm najde. |
| Název skupiny zabezpečení | Zadejte název atributu, který v záznamu skupiny zabezpečení obsahuje název.  Hodnota tohoto atributu se zobrazí v seznamu Skupiny zabezpečení při importu z Active Directory nebo při přidávání položek synchronizace.  Výchozí hodnota je name. |
| Uživatelé | Následující atributy se používají pro vyhledávání, zobrazování, importování a synchronizování uživatelských informací z adresáře. |
| Uživatelské jméno | Zadejte název atributu, který v záznamu uživatele obsahuje uživatelské jméno.  Hodnota tohoto atributu se použije jako uživatelské jméno pro Multi-Factor Auth Server.  Můžete zadat i druhý, záložní atribut.  Druhý atribut se použije jen tehdy, pokud první atribut nebude obsahovat hodnotu pro uživatele.  Výchozí hodnoty jsou userPrincipalName a sAMAccountName. |
| Jméno | Zadejte název atributu, který v záznamu uživatele obsahuje jméno.  Výchozí hodnota je givenName. |
| Příjmení | Zadejte název atributu, který v záznamu uživatele obsahuje příjmení.  Výchozí hodnota je sn. |
| E-mailová adresa | Zadejte název atributu, který v záznamu uživatele obsahuje e-mailovou adresu.  Na e-mailovou adresu se uživateli pošle uvítací e-mail a budou se posílat e-maily s aktualitami.  Výchozí hodnota je mail. |
| Uživatelská skupina | Zadejte název atributu, který v záznamu uživatele obsahuje uživatelskou skupinu.  Uživatelská skupina se může použít pro filtrování uživatelů v agentovi a v sestavách v Portálu pro správu Multi-Factor Auth Serveru. |
| Popis | Zadejte název atributu, který v záznamu uživatele obsahuje popis.  Popis se používá jen pro vyhledávání.  Výchozí hodnota je description. |
| Jazyk hlasového hovoru | Zadejte název atributu, který obsahuje krátký název jazyka pro hlasové hovory s uživatelem. |
| Jazyk SMS zpráv | Zadejte název atributu, který obsahuje krátký název jazyka pro SMS zprávy posílané uživateli. |
| Jazyk mobilní aplikace | Zadejte název atributu, který obsahuje krátký název jazyka pro textové zprávy v mobilní aplikaci uživatele. |
| Jazyk tokenu OATH | Zadejte název atributu, který obsahuje krátký název jazyka pro textové zprávy tokenu OATH pro uživatele. |
| Telefony | Následující atributy se používají pro import nebo synchronizaci telefonních čísel.  Pokud pro typ telefonu není specifikovaný název atributu, typ telefonu nebude při přidávání položek synchronizace nebo importování do Active Directory dostupný. |
| Do zaměstnání | Zadejte název atributu, který v záznamu uživatele obsahuje telefonní číslo do zaměstnání.  Výchozí hodnota je telephoneNumber. |
| Domů | Zadejte název atributu, který v záznamu uživatele obsahuje telefonní číslo domů.  Výchozí hodnota je homePhone. |
| Pager | Zadejte název atributu, který v záznamu uživatele obsahuje číslo pageru.  Výchozí hodnota je pager. |
| Mobilní | Zadejte název atributu, který v záznamu uživatele obsahuje číslo na mobilní telefon.  Výchozí hodnota je mobile. |
| Fax | Zadejte název atributu, který v záznamu uživatele obsahuje číslo faxu.  Výchozí hodnota je facsimileTelephoneNumber. |
| IP telefon | Zadejte název atributu, který v záznamu uživatele obsahuje číslo IP telefonu.  Výchozí hodnota je ipPhone. |
| Vlastní | Zadejte název atributu, který v záznamu uživatele obsahuje vlastní typ telefonního čísla. |
|  | a user record.  Výchozí hodnota je prázdná. |
| Linka | Zadejte název atributu, který v záznamu uživatele obsahuje linku telefonního čísla.  Hodnota pole Linka se použije jako linka jen pro primární telefonní číslo.  Výchozí hodnota je prázdná. <br><br>Poznámka: Pokud atribut Linka není zadaný, můžete linky zadat v rámci atributu telefon.  Před linku by se měl zadat znak „x“, aby se mohla správně parsovat.  Například hodnota 555-123-4567 x890 by znamenala telefonní číslo 555-123-4567 a linku 890. |
| Tlačítko Obnovit výchozí | Pokud kliknete na tlačítko Obnovit výchozí, všechny atributy se obnoví na výchozí hodnoty.  Výchozí hodnoty by měly fungovat s normální službou Active Directory nebo schématem ADAM. |

Pokud chcete atributy upravit, jednoduše klikněte na tlačítko Upravit na kartě Atributy.  Otevře se okno, kde můžete atributy upravit.

![Upravit atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## Synchronizace
Synchronizace udržuje databázi uživatelů Azure Multi-Factor synchronizovanou s uživateli v Active Directory nebo jiný adresář protokolu LDAP (Lightweight Directory Access Protocol).  Tento proces se podobá ručnímu importování uživatelů z Active Directory, ale se pravidelně dotazuje, jestli nedošlo ke změnám uživatelů a skupin zabezpečení v Active Directory, které by měl zpracovat.  Taky umožňuje vypnutí nebo odstranění uživatelů z kontejneru nebo skupiny zabezpečení a odstranění uživatelů odstraněných z Active Directory.

Služba Multi-Factor Auth ADSync je služba systému Windows, která pravidelně dotazuje službu Active Directory.  Nesmíte si ji plést se službou Azure AD Sync nebo Azure AD Connect  Přestože je služba Multi-Factor Auth ADSync postavená na podobném základu kódu, je specifická pro Azure Multi-Factor Authentication Server.  instaluje se v zastaveném stavu a pokud je nakonfigurovaná k tomu, aby se spustila, spustí ji Multi-Factor Auth Server.  Pokud máte více serverů s Multi-Factor Auth Serverem, může Multi-Factor Auth ADSync běžet jen na jednom serveru.

Služba Multi-Factor Auth ADSync používá rozšíření serveru DirSync LDAP od Microsoftu pro efektivní dotazování na změny.  Tento řídicí volající DirSync musí mít oprávnění „directory get changes“ a oprávnění rozšířeného řídicího přístupu DS-Replication-Get-Changes.  Ve výchozím nastavení se tato oprávnění udělují účtům Správce a LocalSystem na řadičích domény.  Služba Multi-Factor Auth AdSync ve výchozím nastavení běží pod účtem LocalSystem.  Je proto nejjednodušší službu spustit na řadiči domény.  Tato služba může běžet pod účtem s oprávněními nižší úrovně, pokud ji nastavíte tak, aby vždy prováděla úplnou synchronizaci.  Není to tak efektivní, ale nejsou k tomu potřeba úplná práva.

Pokud je nastavená tak, aby používala LDAP, a adresář LDAP podporuje ovládání DirSync, pak bude dotazování na uživatele a skupinu zabezpečení fungovat stejně, jako to funguje se službou Active Directory.  Pokud adresář LDAP nepodporuje ovládání DirSync, pak se při každém cyklu provede úplná synchronizace.

![Synchronizace](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

V této tabulce jsou další informace o každém nastavení na kartě Synchronizace.

| Funkce | Popis |
| ------- | ----------- |
| Povolit synchronizaci se službou Active Directory | Pokud je tato možnost označená, služba Multi-Factor Auth Server se bude pravidelně spouštět a dotazovat službu Active Directory na změny. <br><br>Poznámka: Než služba Multi-Factor Auth Server začne zpracovávat změny, musí se přidat aspoň jedna Synchronizační položka a aspoň jednou provést akce Synchronizovat. |
| Synchronizovat každých | Zadejte dobu, po kterou bude služba Multi-Factor Auth Server čekat mezi jednotlivými dotazy zpracováními změn. <br><br> Poznámka: Zadaná doba je interval mezi začátky jednotlivých cyklů.  Pokud je doba zpracování změn delší než tento interval, služba ihned provede další dotazování. |
| Odebrat uživatele, kteří již nejsou ve službě Active Directory | Pokud je tato možnost označená, služba Multi-Factor Auth Server zpracuje náhrobky uživatelů odstraněných z Active Directory a odstraní odpovídající uživatele ze služby Multi-Factor Auth Server. |
| Vždy provádět úplnou synchronizaci | Pokud je tato možnost označená, služba Multi-Factor Auth Server vždy provede úplnou synchronizaci.  Pokud označená není, služba Multi-Factor Auth Server provede přírůstkovou synchronizaci tak, že se bude dotazovat jen na uživatele, kteří se změnili.  Ve výchozím nastavení zatržítko není označené. <br><br> Poznámka: Pokud tato možnost není označená, může se přírůstková synchronizace provést jen tehdy, když adresář podporuje ovládání DirSync a když účet použitý pro vazbu na adresář má odpovídající oprávnění pro provedení přírůstkových dotazů DirSync.  Pokud účet nemá odpovídající oprávnění nebo pokud se synchronizace týká víc než jedné domény, doporučuje se provádět úplnou synchronizaci. |
| Vyžadovat schválení správce, pokud bude zablokováno nebo odstraněno více než X uživatelů | Synchronizační položky se můžou nastavit tak, aby zablokovaly nebo odstranili uživatele, kteří už nejdou členy skupiny zabezpečení nebo kontejneru položky.  Jako pojistku můžete nastavit, že když počet uživatelů, kteří se mají zablokovat nebo odstranit, přesáhne určitou hranici, bude to muset schválit správce.  Pokud je tato možnost označená, bude při překročení určité hranice potřeba schválení správce.  Výchozí hodnota je 5 a zadat se můžou hodnoty v rozsahu 1 až 999. <br><br> Schválení se získá tak, že se nejdřív správcům pošle e-mail s oznámením. V něm jsou pokyny, že mají správci zkontrolovat a schválit zablokování nebo odstranění uživatelů.  Při spuštění uživatelského prostředí Multi-Factor Auth Serveru se objeví výzva ke schválení. |

Tlačítko **Synchronizovat** vám umožňuje provést úplnou synchronizaci pro označené synchronizační položky.  Při každém přidání, úpravě, odstranění nebo změně pořadí synchronizačních položek je potřeba provést úplnou synchronizaci.  Ta se musí provést i v rámci uvedení služby Multi-Factor Auth AdSync do provozu, protože se tím vytvoří výchozí bod, od kterého bude služba dotazovat přírůstkové změny.  Pokud se změnily synchronizační položky a neprovedla úplná synchronizace, pak se při přechodu do jiné části nebo při zavření uživatelského prostředí zobrazí výzva k provedení úplné synchronizace.

Tlačítko **Odstranit** umožní správci odstranit jednu nebo víc synchronizačních položek ze seznamu synchronizačních položek Multi-Factor Auth Serveru.

>[AZURE.WARNING]Jakmile se záznam synchronizační položky odstraní, už se nedá obnovit. Pokud omylem odstraníte záznam synchronizační položky, budete ho muset znovu přidat.

Synchronizační položka nebo položky se odstranily z Multi-Factor Auth Serveru.  Multi-Factor Auth Server nebude tyto synchronizační položky dál zpracovávat. 

Tlačítka Přesunout nahoru a Přesunout dolů umožňují správci změnit pořadí synchronizačních položek.  Pořadí je důležité, protože jeden uživatel může být členem víc než jedné synchronizační položky (např. kontejneru a skupiny zabezpečení).  Při synchronizaci se na uživatele použije nastavení z první synchronizační položky v seznamu, ke které je uživatel přidružený.  Synchronizační položky by se proto měly seřadit podle priority.

>[AZURE.TIP]Po odebrání synchronizačních položek by se měla provést úplná synchronizace.  Po změně pořadí synchronizačních položek by se měla provést úplná synchronizace.  Úplnou synchronizaci můžete provést kliknutím na tlačítko Synchronizovat.

## Multi-Factor Auth Servery
Můžete zřídit další Multi-Factor Auth Servery, které budou sloužit jako záložní servery proxy pro RADIUS, servery proxy pro LDAP nebo jako servery pro Ověřování IIS. Nastavení na kartě Synchronizace se může sdílet mezi všemi agenty. Služba Multi-Factor Auth Server ale může běžet jen na jednom z těchto agentů. Tato karta vám umožní vybrat Multi-Factor Auth Server, který se měl povolit pro synchronizaci.

![Multi-Factor-Auth Servery](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)



<!---HONumber=Aug16_HO4-->


