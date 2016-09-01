
<properties
    pageTitle="Používání služby Azure AD Connect Health se službou AD FS | Microsoft Azure"
    description="Toto je stránka o službě Azure AD Connect Health, která popisuje postup monitorování místní infrastruktury služby AD FS."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="stevenpo"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="vakarand"/>

# Používání služby Azure AD Connect Health se službou AD FS
Následující dokumentace se věnuje sledování infrastruktury služby AD FS ve službě Azure AD Connect Health. Informace o sledování služby Azure AD Connect (Sync) pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md). Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md). 

## Upozornění služby AD FS
Část pojednávající o upozorněních služby Azure AD Connect Health uvádí seznam aktivních upozornění. Každé upozornění obsahuje důležité informace, postup řešení a odkazy na související dokumentaci. Výběrem aktivního nebo vyřešeného upozornění zobrazíte nové okno s doplňujícími informacemi, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci. Můžete si zobrazit i historické údaje o dříve vyřešených upozorněních.

Výběrem některého upozornění zobrazíte doplňující informace, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/alert2.png)



## Funkce analýzy využití služby AD FS
Funkce analýzy využití služby Azure AD Connect Health analyzuje ověřovací provoz na federačních serverech. Zaškrtnutím políčka funkce analýzy využití otevřete okno analýzy využití, ve kterém se zobrazí metriky a seskupení.

>[AZURE.NOTE] Pokud chcete použít funkci analýzy využití ve službě AD FS, povolte auditování AD FS. Další informace najdete v článku o [povolení auditování služby AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/report1.png)

Pokud chcete vybrat další metriky, určit časový rozsah nebo změnit seskupení, stačí kliknout pravým tlačítkem na graf analýzy využití a vybrat Upravit graf. Potom můžete zadat časový rozsah, změnit nebo vybrat metriky a změnit seskupení. Distribuci ověřovacího provozu můžete zobrazit podle různých „metrik“ a jednotlivé metriky můžete seskupit pomocí příslušných parametrů možnosti „Seskupit podle“, které jsou uvedené níže.

| Metrika | Seskupit podle | Co seskupení znamená a proč je užitečné? |
| ------ | -------- | -------------------------------------------- |
| Celkový počet požadavků: Celkový počet požadavků zpracovaných službou FS | Všechny | Zobrazí celkový počet požadavků bez seskupení. |
|  | Aplikace | Tato možnost seskupí celkový počet požadavků podle cílové přijímající strany. Toto seskupení vás seznámí s procentem celkového provozu, které jednotlivé aplikace přijímají. |
|  | Server | Tato možnost seskupí celkový počet požadavků podle serveru, který požadavek zpracoval. Toto seskupení vás seznámí s distribucí zatížení celkového provozu. |
|  | Připojení k pracovišti | Tato možnost seskupí celkový počet požadavků podle toho, jestli požadavky přicházejí ze zařízení, která jsou připojená k pracovišti (známá). Toto seskupení vás seznámí s tím, jestli se k vašim prostředkům přistupuje pomocí zařízení, které infrastruktura identity nezná. |
|  | Metoda ověřování | Tato možnost seskupí celkový počet požadavků podle metody ověřování, která se k ověřování používá. Toto seskupení vás seznámí s běžnou metodu ověřování, která se k ověřování používá. Níže jsou uvedené možné metody ověřování. <ol> <li>integrované ověřování systému Windows (Windows)</li> <li>ověřování pomocí formulářů (formuláře)</li> <li>jednotné přihlašování (SSO)</li> <li>ověření certifikátem X509 (certifikát)</li> <br>Všimněte si, že požadavek se počítá jako jednotné přihlašování (SSO), když federační servery požadavek přijmou pomocí souboru cookie jednotného přihlašování. V takových případech (pokud je soubor cookie platný) se od uživatele nevyžadují přihlašovací údaje a uživatel získá bezproblémový přístup k aplikaci. Toto je běžné v případě, kdy máte několik přijímajících stran, které jsou chráněné federačními servery. |
|  | Umístění v síti | Tato možnost seskupí celkový počet požadavků podle umístění uživatele v síti. Může to být intranet nebo extranet. Toto seskupení vás seznámí s procentuálním podílem provozu z intranetu a z extranetu. |
| Celkový počet neúspěšných žádostí: Celkový počet neúspěšných požadavků zpracovaných službou FS <br> (Tato metrika je dostupná pouze ve službě AD FS pro Windows Server 2012 R2.)| Typ chyby | Zobrazí počet chyb podle předdefinovaných typů chyb. Toto seskupení vás seznámí s běžnými typy chyb. <ul><li>Nesprávné uživatelské jméno nebo heslo: Chyby způsobené zadáním nesprávného uživatelského jména nebo hesla.</li> <li>„Uzamčení extranetu“: Selhání způsobené požadavky přijatými od uživatele, který má uzamčený přístup do extranetu. </li><li> „Prošlé heslo“: Chyby způsobené uživateli, kteří se přihlašují pomocí hesla, kterému vypršela platnost.</li><li>„Deaktivovaný účet“: Chyby způsobené uživateli, kteří se přihlašují pomocí deaktivovaného účtu.</li><li>„Ověřování zařízení“: Chyby způsobené uživateli, kteří neprovádějí ověřování pomocí ověření zařízení.</li><li>„Ověřování certifikátu uživatele“: Chyby způsobené uživateli, kterým nefunguje ověřování kvůli neplatnému certifikátu.</li><li>„MFA“: Chyby způsobené uživateli, kteří k ověřování nepoužívají vícefaktorové ověřování.</li><li>„Jiné přihlašovací údaje“: „Autorizace vystavení“: Chyby způsobené selháním autorizace.</li><li>„Delegování vystavení“: Chyby způsobené chybami delegace vystavení.</li><li>„Přijetí tokenu“: Chyby způsobené tím, že služba ADFS odmítla token od zprostředkovatele identity z řad třetích stran.</li><li>„Protokol“: Chyba způsobená chybami protokolu.</li><li>„Neznámá“: Zachytit vše. Jakékoli jiné chyby, které se nehodí do definovaných kategorií.</li> |
|  | Server | Seskupí chyby podle serveru. Seznámí vás s distribucí chyb mezi servery. Nerovnoměrná distribuce může naznačovat vadný stav serveru. |
|  | Umístění v síti | Seskupí chyby podle umístění požadavků v síti (intranet vs. extranet). Seznámí vás s typy neúspěšných požadavků. |
|  | Aplikace | Seskupí chyby podle cílové aplikace (přijímající strany). Seznámí vás s tím, která cílová aplikace zaznamenává největší počet chyb. |
| Počet uživatelů: Průměrný počet jedinečných uživatelů, kteří jsou v systému aktivní. | Vše | Poskytuje průměrný počet uživatelů, kteří používají službu FS ve vybraném časovém intervalu. Uživatelé nejsou seskupení. <br>Průměr bude záviset na vybraném časovém intervalu. |
|  | Aplikace | Seskupí průměrný počet uživatelů podle cílové aplikace (přijímající strany). Seznámí vás s počtem uživatelů a s aplikacemi, které používají. |


## Sledování výkonu služby AD FS
Sledování výkonu služby Azure AD Connect Health poskytuje sledovací informace o metrikách. Zaškrtnutím políčka sledování otevřete okno s podrobnými informacemi o metrikách.


![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/perf1.png)


Výběrem možnosti Filtrovat (v horní části okna) můžete filtrovat podle serveru a prohlédnout si metriky na jednotlivých serverech. Pokud chcete změnit metriky, jednoduše klikněte pravým tlačítkem na graf sledování pod oknem sledování a vyberte Upravit graf. Potom můžete v nově otevřeném okně vybrat další metriky pomocí rozevíracího seznamu a také zadat časový rozsah pro zobrazení dat výkonu.

## Sestavy služby AD FS
Azure AD Connect Health poskytuje sestavy s informacemi o činnosti a výkonu služby AD FS. Tyto sestavy pomáhají správcům získat přehled o aktivitách na jejich serverech AD FS.

### Nejčastějších 50 uživatelů s neúspěšným přihlášením kvůli uživatelskému jména nebo heslu

Jednou z běžných příčin neúspěšného požadavku na ověření na serveru AD FS je požadavek provedený s neplatnými přihlašovacími údaji, tedy s nesprávným uživatelským jménem nebo heslem. Obvykle k tomu dochází kvůli složitým heslům, zapomenutým heslům nebo překlepům.

Existují i jiné důvody, které můžou způsobit neočekávané množství takových požadavků, které musí servery AD FS zpracovat. Patří sem aplikace, která uloží uživatelské údaje do mezipaměti a těmto údajům mezitím vyprší platnost, nebo uživatel se zlými úmysly, který se pokouší o přihlášení k účtu pomocí řady známých hesel.

Azure AD Connect Health pro ADFS poskytuje sestavy s nejčastějšími 50 uživateli, kteří se neúspěšně přihlašovali pomocí neplatného uživatelského jména nebo hesla. Toho můžete dosáhnout zpracováním událostí auditu, které jsou vytvořené všemi servery AD FS ve farmách.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report1a.png)

V rámci této sestavy máte snadný přístup k následujícím informacím:

- Celkový počet neúspěšných požadavků s nesprávným uživatelským jménem nebo heslem za posledních 30 dní.
- Průměrný počet uživatelů, kteří mají každý den problém s přihlašováním kvůli chybnému uživatelskému jménu nebo heslu.

Kliknutím na tuto část přejdete do hlavního okna sestavy, které vám nabídne další podrobnosti. Mezi ně patří graf, který nabízí informace o trendech, abyste mohli stanovit základní hodnotu požadavků s nesprávným uživatelským jménem nebo heslem, a seznam nejčastějších 50 uživatelů s počtem neúspěšných pokusů.

Graf obsahuje následující informace:

- Celkový počet neúspěšných přihlášení z důvodu chybného uživatelského jména nebo hesla na denní bázi.
- Celkový počet jedinečných uživatelů s neúspěšným přihlášení na denní bázi.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report2a.png)

Tato sestava poskytuje následující:

| Položky sestavy | Popis
| ------ | -------- |
|ID uživatele| Zobrazuje použité ID uživatele. Všimněte si, že hodnotou je to, co uživatel zadal. V některých případech uvidíte, že bylo použito chybné ID uživatele.|
|Neúspěšné pokusy|Zobrazuje celkový počet neúspěšných pokusů s konkrétním ID uživatele. Tabulka je řazená podle největšího počtu neúspěšných pokusů v sestupném pořadí.|
|Poslední chyba|Zobrazuje časové razítko poslední chyby.



>[AZURE.NOTE] Sestava se každé dvě hodiny automaticky aktualizuje novými informacemi, které byly za tu dobu shromážděné. V důsledku tohoto postupu nemusí být v sestavě zahrnuté přihlašovací pokusy za poslední dvě hodiny.



## Související odkazy

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!---HONumber=Aug16_HO4-->


