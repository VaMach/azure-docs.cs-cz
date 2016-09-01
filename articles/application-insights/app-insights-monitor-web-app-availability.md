<properties
    pageTitle="Sledování dostupnosti a odezvy libovolných webů | Microsoft Azure"
    description="Nastavení testů webu ve službě Application Insights. Zasílání upozornění, pokud web přestane být k dispozici nebo reaguje pomalu."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="awills"/>

# Sledování dostupnosti a odezvy libovolných webů

Po nasazení webové aplikace na libovolného hostitele můžete nastavit webové testy ke sledování dostupnosti a odezvy. [Application Insights v sadě Visual Studio](app-insights-overview.md) odesílá webové požadavky v pravidelných intervalech z bodů po celém světě a může vás upozornit v případě, že aplikace reaguje pomalu nebo vůbec.

![Příklad webového testu](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Webové testy můžete nastavit pro libovolný koncový bod HTTP nebo HTTPS, který je přístupný z veřejného internetu.

Existují dva typy webového testu:

* [Testování ping adresy URL](#set-up-a-url-ping-test): jednoduchý test, který můžete vytvořit na portálu Azure.
* [Vícekrokový test webu](#multi-step-web-tests): který můžete vytvořit v sadě Visual Studio Ultimate nebo Visual Studio Enterprise a odeslat na portál.

Můžete vytvořit až 10 webových testů na prostředek aplikace.


## Nastavení testu příkazu ping adresy URL

### <a name="create"></a>1. Chcete vytvořit nový prostředek?

Tento krok přeskočte, pokud jste již [nastavili prostředek Application Insights][start] pro tuto aplikaci a chcete zobrazit data dostupnosti na stejném místě.

Zaregistrujte se na portálu [Microsoft Azure](http://azure.com), přejděte na [portál Azure](https://portal.azure.com) a vytvořte prostředek Application Insights.

![Nový > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Pro nový prostředek se otevře okno Přehled. To můžete kdykoli najít na [portálu Azure](https://portal.azure.com) kliknutím na tlačítko **Procházet**.

### <a name="setup"></a>2. Vytvoření webového testu

V prostředku Application Insights vyhledejte dlaždici dostupnosti. Klikněte na něj pro otevření okna webové testy pro vaši aplikaci a přidejte webový test.

![Vyplňte alespoň adresu URL webu](./media/app-insights-monitor-web-app-availability/13-availability.png)

- **Adresa URL** musí být viditelná z veřejného internetu. Může obsahovat řetězec dotazu&#151; takže také například můžete odněkud získat databáze. Pokud se adresa URL přeloží na přesměrování, budeme ji sledovat až po 10 přesměrování.
- **Analyzovat závislé požadavky**: obrázky, skripty, soubory stylu a další materiály na stránce jsou požadovány v rámci testu. Pokud tyto prostředky nelze úspěšně stáhnout v časovém limitu pro celý test, test se nezdaří.
- **Povolit opakování**: Pokud se test nezdaří, zopakuje se za krátkou dobu. Selhání je nahlášeno pouze v případě tří po sobě jdoucích neúspěšných pokusů. Následné testy jsou pak provedeny s obvyklou frekvencí testu. Opakování je dočasně pozastaveno do dalšího úspěchu. Toto pravidlo platí nezávisle na každém umístění testu. (Doporučujeme toto nastavení. V průměru přibližně 80 % selhání zmizí při opakování.)
- **Frekvence testů**: Nastaví, jak často se test spustí z umístění každého testu. S pětiminutovou četností a pěti testovanými místy bude váš web testován v průměru každou minutu.
- **Testovací umístění** jsou místa, ze kterých naše servery odesílají webové požadavky na adresu URL. Zvolte více než jeden, aby bylo možné rozlišit problémy ve vašem webu od problémů se sítí. Můžete vybrat až 16 umístění.

- **Kritéria úspěchu**:

    **Časový limit testu**: Pokud chcete dostávat upozornění na pomalé odezvy, zmenšete tuto hodnotu. Test se počítá jako selhání, pokud během tohoto období nebyly přijaty odpovědí z webu. Pokud jste vybrali možnost **Analyzovat závislé požadavky**, potom všechny image, soubory stylů, skripty a další závislé prostředky musejí být přijaty během tohoto období.

    **Odpověď HTTP**: vrácený kód stavu, který se počítá jako úspěšný. 200 je kód, který označuje, že byla vrácena normální webová stránka.

    **Shoda obsahu**: řetězec, například „Vítejte!“ Testujeme, zda se vyskytuje v každé odpovědi. Musí být prostý řetězec bez zástupných znaků. Nezapomeňte, že pokud se obsah vaší stránka změní, bude pravděpodobně nutné jej aktualizovat.


- **Výstrahy** jsou ve výchozím nastavení odesílány, pokud se vyskytnou selhání ve třech umístěních po dobu delší než pět minut. Selhání v jednom umístění bude pravděpodobně problém se sítí a nejedná se o problém s webem. Ale můžete změnit prahovou hodnotu na citlivější nebo méně citlivou a můžete také změnit, komu mají být e-maily zasílány.

    Můžete nastavit [webhook](../azure-portal/insights-webhooks-alerts.md), který je volán, když je vydána výstraha. (Všimněte si, že v současné době parametry dotazu neprocházejí jako vlastnosti.)

#### Testování více adres URL

Přidat další testy Pro příklad, a také jako testování domovské stránky, můžete zajistit, že vaše databáze se spouští otestováním adresy URL pro hledání.


### <a name="monitor"></a>3. Zobrazit sestavy dostupnosti

Po 1–2 minutách klikněte na tlačítko **Aktualizovat** v okně dostupnost / testy webu. (Automaticky se neobnoví.)

![Souhrnné výsledky na domácím okně](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Kliknutím na libovolný panel v grafu souhrnu získáte podrobnější zobrazení tohoto časového období.

Tyto grafy kombinují výsledky pro všechny webové testy aplikace.

#### Komponenty webové stránky

Obrázky, šablony stylů, skripty a další statické součásti webové stránky, které testujete, jsou požadovány v jako část testu.  

Doba zaznamenané odezvy je čas potřebný k dokončení načítání pro všechny součásti.

Pokud se jakákoliv komponenta nepodaří načíst, test se označí jako nezdařený.

## <a name="failures"></a>Pokud vidíte chyby...

Klikněte na červenou tečku.

![Klikněte na červenou tečku](./media/app-insights-monitor-web-app-availability/14-availRedDot.png)

Případně přejděte dolů a klikněte na tlačítko test, kde uvidíte menší než 100% úspěch.

![Klikněte na konkrétní test webu](./media/app-insights-monitor-web-app-availability/15-webTestList.png)

Otevřou se výsledky tohoto testu.

![Klikněte na konkrétní test webu](./media/app-insights-monitor-web-app-availability/16-1test.png)

Test se spouští z několika umístění&#151; vyberte takový, kde jsou výsledky méně než 100%.

![Klikněte na konkrétní test webu](./media/app-insights-monitor-web-app-availability/17-availViewDetails.png)


Přejděte dolů na **Nezdařené testy** a vyberte výsledek.

Klikněte na výsledek pro vyhodnocení na portálu a podívejte se, proč byl neúspěšný.

![Výsledek spuštění webového testu](./media/app-insights-monitor-web-app-availability/18-availDetails.png)


Alternativně můžete stáhnout soubor s výsledky a zkontrolovat v sadě Visual Studio.


*Vypadá v pořádku, ale je hlášen jako selhání?* Zkontrolujte všechny image, skripty, šablony stylů a všechny další soubory, které stránka načetla. Pokud některý z nich selže, test se ohlásí jako neúspěšný i v případě, že se hlavní html stránka načte bez problémů.



## Vícekrokové webové testy

Je možné sledovat scénář, který zahrnuje posloupnost adres URL. Například pokud sledujete prodejní web, můžete otestovat, zda správně funguje přidávání položek do nákupního košíku.

Pro vytvoření vícekrokového testu uložte scénář pomocí sady Visual Studio a pak nahrajte tento záznam do služby Application Insights. Application Insights přehrává scénář v intervalech a ověřuje odezvy.

Všimněte si, že nemůžete použít kódované funkce ve svých testech: kroky scénáře musí být obsaženy jako skript v souboru .webtest.

#### 1. Záznam scénáře

Slouží k zaznamenání relace webové aplikace Visual Studio Enterprise nebo Ultimate. 

1. Vytvořte projekt testu výkonnosti webu.

    ![Vytvořte v sadě Visual Studio nový projekt ze šablony výkonu webu a zátěžového testu.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

2. Otevřete soubor .webtest a spusťte záznam.

    ![Otevřete soubor .webtest a klikněte na tlačítko Záznam.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)

3. Proveďte uživatelské akce, kterou chcete simulovat v testu: otevřete webovou stránku, přidat do košíku produkt a tak dále. Pak test zastavte.

    ![Záznamník testování webu běží v aplikaci Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Nevytvářejte příliš dlouhý scénář. Platí limit 100 kroků a 2 minut.

4. Upravte test na:
 - Přidání ověření ke kontrole přijatého textu a kódů odpovědi.
 - Odeberte všechny nadbytečné interakce. Můžete také odebrat závislé požadavky pro obrázky, reklamy nebo sledování lokalit.

    Mějte na paměti, že můžete upravit pouze zkušební skript – nelze přidat vlastní kód nebo volat jiné webové testy. Nevkládejte do testu smyčky. Můžete použít standardní zásuvné moduly webového testu.

5. Spusťte test v sadě Visual Studio a ujistěte se, že funguje.

    Spouštěč webových testů otevře webový prohlížeč a zopakuje zaznamenané akce. Zkontrolujte, zda vše funguje podle očekávání.

    ![V sadě Visual Studio otevřete soubor .webtest a klikněte na tlačítko Spustit.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)


#### 2. Nahrajte test webu do služby Application Insights

1. Na portálu služby Application Insights vytvořte nový webový test.

    ![V okně webového testu zvolte možnost Přidat.](./media/app-insights-monitor-web-app-availability/16-another-test.png)

2. Vyberte vícekrokový test a nahrajte soubor .webtest.

    ![Vyberte vícekrokový webový test.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Nastavte umístění testu, četnost a parametry výstrah stejným způsobem jako u testů pomocí příkazu ping.

Prohlédněte si výsledky testu a všechny chyby stejným způsobem jako u testů jedné adresy URL.

Běžným důvodem selhání je, že test běží příliš dlouho. Nesmí se spouštět déle než dvě minuty.

Nezapomeňte, že všechny prostředky stránky se musí načíst správně, včetně skriptů, šablon stylů, obrázků a tak dále, jinak test nebude úspěšný.

Všimněte si, že webový test musí být zcela obsažen v souboru .webtest: programové funkce nelze použít v testu.


### Doba zapojení a náhodná čísla do vícekrokového testu

Předpokládejme, že testujete nástroj, který získá data závislá na čase, například akcie z externího kanálu. Při záznamu webového testu je nutné použít konkrétní časy, ale nastavit je jako parametry testu, čas spuštění a čas ukončení.

![Webový test s parametry.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Při spuštění testu chcete, aby čas ukončení vždy představoval aktuální čas a čas spuštění by měl začínat před 15 minutami.

Zásuvné moduly webového testu nabízejí způsob parametrizace časů.

1. Přidejte zásuvný modul webového testu pro každou hodnotu parametru proměnné, kterou chcete. V panelu nástrojů webového testu zvolte **Přidat zásuvný modul pro testování webu**.

    ![Zvolte možnost přidat zásuvný modul pro testování webu a vyberte typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    V tomto příkladu používáme dvě instance zásuvného modulu Datum čas. Jedna instance je „před 15 minutami“ a druhá „teď“.

2. Otevřete vlastnosti každého zásuvného modulu. Pojmenujte ho a nastavte ho na použití aktuálního času. Pro jeden z nich nastavte Přidat minuty = -15.

    ![Nastavte název, použijte aktuální čas a přidejte minuty.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)

3. V parametrech webového testu použijte {{plug-in name}} pro odkazování na název zásuvného modulu.

    ![V parametru testu použijte {{plug-in name}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Teď nahrajte svůj test na portál. Při každém spuštění testu se budou používat dynamické hodnoty.

## Vyřešení přihlášení

Pokud se uživatelé přihlásí do aplikace, máte několik možností pro simulaci přihlášení, takže můžete otestovat stránky následující po přihlášení. Použitý přístup závisí na typu zabezpečení poskytovaném aplikací.

Ve všech případech musíte v aplikaci vytvořit účet jenom pro účely testování. Pokud je to možné, omezte oprávnění tohoto testovacího účtu, aby webové testy nemohly žádným způsobem ovlivnit skutečné uživatele.

### Jednoduché uživatelské jméno a heslo

Webový test zaznamenejte obvyklým způsobem. Nejprve odstraňte soubory cookie.

### Ověřování SAML

Použijte zásuvný modul SAML, který je pro webové testy dostupný.

### Tajný klíč klienta

Pokud vaše aplikace obsahuje trasu přihlášení, která zahrnuje tajný klíč klienta, použijte ji. Azure Active Directory (AAD) je příkladem služby, která poskytuje přihlašování pomocí tajného klíče klienta. Ve službě AAD je tajným klíčem klienta klíč aplikace. 

Tady je ukázkový webový test webové aplikace v Azure pomocí klíče aplikace:

![Ukázkový tajný klíč klienta](./media/app-insights-monitor-web-app-availability/110.png)

1. Získejte token ze služby AAD pomocí tajného klíče klienta (AppKey).
2. Extrahujte nosný token z odpovědi.
3. Pomocí nosného tokenu v autorizační hlavičce zavolejte rozhraní API.

Ujistěte se, jestli je webový test skutečným klientem – to znamená, že má vlastní aplikaci ve službě AAD – a použijte jeho clientId + appkey. Testovaná služba má také vlastní aplikaci ve službě AAD: identifikátor URI appID této aplikace se ve webovém testu projevuje v poli „prostředek“. 

### Otevřené ověřování

Příkladem otevřeného ověřování je přihlašování pomocí účtu Microsoft nebo Google. Velký počet aplikací, které používají OAuth, nabízí alternativní tajný klíč klienta, takže prvním cílem bude prozkoumání této možnosti. 

Pokud váš test vyžaduje přihlášení pomocí OAuth, bude obecný postup následující:

 * K prozkoumání přenosu dat mezi webovým prohlížečem, webem ověřování a aplikací použijte například nástroj Fiddler. 
 * Proveďte dvě nebo více přihlášení pomocí různých počítačů nebo prohlížečů nebo v dlouhých intervalech (aby mohla vypršet platnost tokenů).
 * Porovnáním různých relací identifikujte token předaný zpět z webu ověřování, který byl následně předán do aplikačního serveru po přihlášení. 
 * Uložte webový test pomocí sady Visual Studio. 
 * Parametrizujte tokeny, nastavte parametr při vrácení tokenu z ověřovatele a použijte ho v dotazu na web.
 (Sada Visual Studio se pokusí o parametrizaci testu, ale nebude tokeny parametrizovat správně.)


## <a name="edit"></a> Upravit nebo zakázat test

Otevřete jednotlivé testy a upravte je nebo je zakažte.

![Upravit nebo zakázat webový test](./media/app-insights-monitor-web-app-availability/19-availEdit.png)

Můžete chtít zakázat webové testy během provádění údržby vaší služby.

## Testy výkonnosti

Na svém webu můžete spustit zátěžový test. Podobně jako v testu dostupnosti můžete z našich bodů po celém světě odeslat buď jednoduché požadavky, nebo vícekrokové požadavky. Na rozdíl od testu dostupnosti se odesílá mnoho požadavků, které simulují několik souběžných uživatelů.

V okně Přehled otevřete **Nastavení**, **Testy výkonnosti**. Při vytváření testu budete vyzváni k připojení k účtu Visual Studio Team Services nebo k jeho vytvoření. 

Po dokončení testu se zobrazí časy odezvy a míra úspěšnosti.


## Automation

* [Automaticky používat skripty prostředí PowerShell k nastavení webového](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/) testu. 
* Nastavení [webhook](../azure-portal/insights-webhooks-alerts.md), který je volán při vydání výstrahy.

## Máte dotazy? Problémy?

* *Mohu volat kód z mého webového testu?*

    Ne. Kroky testu musí být v souboru .webtest. A nemůžete volat jiné webové testy nebo používat smyčky. Existují různé zásuvné moduly, které se vám můžou hodit.

* *Je podporován protokol HTTPS?*

    Podporujeme protokoly TLS 1.1 a TLS 1.2.

* *Existuje rozdíl mezi "webovými testy" a "testy dostupnosti"?*

    Používáme tyto dvě podmínky zaměnitelně.

* *Chci používat testy dostupnosti na našem interním serveru, který se spouští za bránou firewall.*

    Nakonfigurujte bránu firewall, aby povolovala požadavky z [IP adres agentů webového testu](app-insights-ip-addresses.md#availability).

* *Nahrávání vícekrokového webového testu se nezdaří*

    Maximální velikost je 300 kB.

    Smyčky nejsou podporovány.

    Odkazy na jiné webové testy nejsou podporovány.

    Zdroje dat nejsou podporovány.

    
* *Můj vícekrokový test se nedokončil*

    Existuje limit 100 požadavků na test.

    Test bude zastaven, pokud poběží déle než dvě minuty.

* *Jak mohu spustit test pomocí certifikátů klientů?*

    Tuto možnost nepodporujeme, je nám líto.


## <a name="video"></a>Video

> [AZURE.VIDEO monitoring-availability-with-application-insights]

## <a name="next"></a>Další kroky

[Vyhledávání diagnostických protokolů][diagnostika]

[Poradce při potížích][qna].

[IP adresy agentů webových testů](app-insights-ip-addresses.md)


<!--Link references-->

[dostupnost-azure]: ../insights-create-web-tests.md
[diagnostika]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md



<!--HONumber=Aug16_HO4-->


