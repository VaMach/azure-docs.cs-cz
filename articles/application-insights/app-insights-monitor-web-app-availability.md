---
title: "Sledování dostupnosti a odezvy libovolných webů | Dokumentace Microsoftu"
description: "Nastavení testů webu ve službě Application Insights. Zasílání upozornění, pokud web přestane být k dispozici nebo reaguje pomalu."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: cfe70aa09b21aa914e3705bf7969583c7a1bbd52
ms.lasthandoff: 04/07/2017


---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Sledování dostupnosti a odezvy libovolných webů
Po nasazení webové aplikace nebo webu na libovolném serveru můžete nastavit webové testy ke sledování dostupnosti a odezvy. [Azure Application Insights](app-insights-overview.md) odesílá do vaší aplikace webové požadavky v pravidelných intervalech z bodů po celém světě. Upozorní vás v případě, že vaše aplikace reaguje pomalu nebo nereaguje vůbec.

![Příklad webového testu](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Webové testy můžete nastavit pro libovolný koncový bod HTTP nebo HTTPS, který je přístupný z veřejného internetu. Na testovaný web není třeba nic přidávat. Dokonce se ani nemusí jednat o váš web – můžete například testovat službu REST API, na které jste závislí.

Existují dva typy webového testu:

* [Testování ping adresy URL](#create): jednoduchý test, který můžete vytvořit na portálu Azure.
* [Vícekrokový webový test](#multi-step-web-tests): který můžete vytvořit v sadě Visual Studio Enterprise a odeslat na portál.

Můžete vytvořit až 10 webových testů na prostředek aplikace.

## <a name="create"></a>1. Vytvoření prostředku pro testovacích sestavy
Tento krok přeskočte, pokud jste již [nastavili prostředek Application Insights][start] pro tuto aplikaci a chcete zobrazit sestavy dostupnosti na stejném místě.

Zaregistrujte se na portálu [Microsoft Azure](http://azure.com), přejděte na [portál Azure](https://portal.azure.com) a vytvořte prostředek Application Insights.

![Nový > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Kliknutím na možnost **Všechny prostředky** otevřete okno Přehled pro nový prostředek.

## <a name="setup"></a>2. Vytvoření testu adresy URL pomocí příkazu Ping
V prostředku Application Insights vyhledejte dlaždici dostupnosti. Klikněte na něj pro otevření okna webové testy pro vaši aplikaci a přidejte webový test.

![Vyplňte alespoň adresu URL webu](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **Adresa URL** musí být viditelná z veřejného internetu. Může obsahovat řetězec dotazu&#151; takže také například můžete odněkud získat databáze. Pokud se adresa URL přeloží na přesměrování, budeme ji sledovat až po 10 přesměrování.
* **Analyzovat závislé požadavky**: Obrázky, skripty, soubory stylu a další materiály na stránce jsou požadovány v rámci testu a zaznamenaná doba odezvy zahrnuje tyto časy. Pokud tyto prostředky nelze úspěšně stáhnout v časovém limitu pro celý test, test se nezdaří.
* **Povolit opakování**: Pokud se test nezdaří, zopakuje se za krátkou dobu. Selhání je nahlášeno pouze v případě tří po sobě jdoucích neúspěšných pokusů. Následné testy jsou pak provedeny s obvyklou frekvencí testu. Opakování je dočasně pozastaveno do dalšího úspěchu. Toto pravidlo platí nezávisle na každém umístění testu. (Doporučujeme toto nastavení. V průměru přibližně 80 % selhání zmizí při opakování.)
* **Frekvence testů**: Nastaví, jak často se test spustí z umístění každého testu. S pětiminutovou četností a pěti testovanými místy bude váš web testován v průměru každou minutu.
* **Testovací umístění** jsou místa, ze kterých naše servery odesílají webové požadavky na adresu URL. Zvolte více než jeden, aby bylo možné rozlišit problémy ve vašem webu od problémů se sítí. Můžete vybrat až 16 umístění.
* **Kritéria úspěchu**:

    **Časový limit testu**: Pokud chcete dostávat upozornění na pomalé odezvy, zmenšete tuto hodnotu. Test se počítá jako selhání, pokud během tohoto období nebyly přijaty odpovědí z webu. Pokud jste vybrali možnost **Analyzovat závislé požadavky**, potom všechny image, soubory stylů, skripty a další závislé prostředky musejí být přijaty během tohoto období.

    **Odpověď HTTP**: vrácený kód stavu, který se počítá jako úspěšný. 200 je kód, který označuje, že byla vrácena normální webová stránka.

    **Shoda obsahu**: řetězec, například „Vítejte!“ U každé odpovědi testujeme výskyt přesné shody (s rozlišováním velkých a malých písmen). Musí být prostý řetězec bez zástupných znaků. Nezapomeňte, že pokud se obsah vaší stránka změní, bude pravděpodobně nutné jej aktualizovat.
* **Výstrahy** jsou ve výchozím nastavení odesílány, pokud se vyskytnou selhání ve třech umístěních po dobu delší než pět minut. Selhání v jednom umístění bude pravděpodobně problém se sítí a nejedná se o problém s webem. Ale můžete změnit prahovou hodnotu na citlivější nebo méně citlivou a můžete také změnit, komu mají být e-maily zasílány.

    Můžete nastavit [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán, když je vydána výstraha. (Všimněte si, že v současné době parametry dotazu neprocházejí jako vlastnosti.)

### <a name="test-more-urls"></a>Testování více adres URL
Přidat další testy Pro příklad, a také jako testování domovské stránky, můžete zajistit, že vaše databáze se spouští otestováním adresy URL pro hledání.

## <a name="monitor"></a>3. Zobrazení výsledků testu webu
Po 1–2 minutách se v okně Test webu zobrazí výsledky.

![Souhrnné výsledky na domácím okně](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Kliknutím na libovolný panel v grafu souhrnu získáte podrobnější zobrazení tohoto časového období.

Tyto grafy kombinují výsledky pro všechny webové testy aplikace.

## <a name="failures"></a>Pokud se zobrazí chyby
Klikněte na červenou tečku.

![Klikněte na červenou tečku](./media/app-insights-monitor-web-app-availability/open-instance.png)


Z výsledku webového testu můžete provádět následující akce:

* Kontrolovat odpověď přijatou ze serveru.
* Otevřít telemetrii zaslanou aplikací serveru při zpracovávání instance neúspěšné žádosti.
* Zaznamenat problém nebo pracovní položku do Gitu nebo VSTS kvůli sledování problému. Chyba bude obsahovat odkaz na tuto událost.
* Otevřít výsledek webového testu v sadě Visual Studio.


*Zdá se, že všechno je v pořádku, ale přesto je hlášena chyba.* Zkontrolujte všechny image, skripty, šablony stylů a všechny další soubory, které stránka načetla. Pokud některý z nich selže, test se ohlásí jako neúspěšný i v případě, že se hlavní html stránka načte bez problémů.

*Žádné související položky?* To může být způsobené tím, že je v probíhá [Vzorkování](app-insights-sampling.md).

## <a name="multi-step-web-tests"></a>Vícekrokové webové testy
Je možné sledovat scénář, který zahrnuje posloupnost adres URL. Například pokud sledujete prodejní web, můžete otestovat, zda správně funguje přidávání položek do nákupního košíku.

> [!NOTE] 
> Vícekrokové webové testy jsou zpoplatněné. [Cenové schéma](http://azure.microsoft.com/pricing/details/application-insights/).
> 

Pro vytvoření vícekrokového testu uložte scénář pomocí sady Visual Studio Enterprise a pak nahrajte tento záznam do služby Application Insights. Application Insights přehrává scénář v intervalech a ověřuje odezvy.

Všimněte si, že nemůžete použít kódované funkce ve svých testech: kroky scénáře musí být obsaženy jako skript v souboru .webtest.

#### <a name="1-record-a-scenario"></a>1. Záznam scénáře
Slouží k zaznamenání relace webové aplikace Visual Studio Enterprise.

1. Vytvořte projekt testu výkonnosti webu.

    ![Vytvořte v sadě Visual Studio Enterprise nový projekt ze šablony výkonnostního testu webu a zátěžového testu.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Nevidíte šablonu výkonnostního testu webu a zátěžového testu?* – Zavřete sadu Visual Studio Enterprise. Otevřete **Instalační program sady Visual Studio**, pomocí kterého upravíte svou instalaci sady Visual Studio Enterprise. V části **Jednotlivé komponenty** vyberte **Nástroje pro testování výkonnosti webů a zátěžové testování**.

2. Otevřete soubor .webtest a spusťte záznam.

    ![Otevřete soubor .webtest a klikněte na tlačítko Záznam.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Proveďte uživatelské akce, kterou chcete simulovat v testu: otevřete webovou stránku, přidat do košíku produkt a tak dále. Pak test zastavte.

    ![Záznamník testování webu běží v aplikaci Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Nevytvářejte příliš dlouhý scénář. Platí limit 100 kroků a 2 minut.
4. Upravte test na:

   * Přidání ověření ke kontrole přijatého textu a kódů odpovědi.
   * Odeberte všechny nadbytečné interakce. Můžete také odebrat závislé požadavky pro obrázky, reklamy nebo sledování lokalit.

     Mějte na paměti, že můžete upravit pouze zkušební skript – nelze přidat vlastní kód nebo volat jiné webové testy. Nevkládejte do testu smyčky. Můžete použít standardní zásuvné moduly webového testu.
5. Spusťte test v sadě Visual Studio a ujistěte se, že funguje.

    Spouštěč webových testů otevře webový prohlížeč a zopakuje zaznamenané akce. Zkontrolujte, zda vše funguje podle očekávání.

    ![V sadě Visual Studio otevřete soubor .webtest a klikněte na tlačítko Spustit.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. Nahrajte test webu do služby Application Insights
1. Na portálu služby Application Insights vytvořte nový webový test.

    ![V okně webového testu zvolte možnost Přidat.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Vyberte vícekrokový test a nahrajte soubor .webtest.

    ![Vyberte vícekrokový webový test.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Nastavte umístění testu, četnost a parametry výstrah stejným způsobem jako u testů pomocí příkazu ping.

Prohlédněte si výsledky testu a všechny chyby stejným způsobem jako u testů jedné adresy URL.

Běžným důvodem selhání je, že test běží příliš dlouho. Nesmí se spouštět déle než dvě minuty.

Nezapomeňte, že všechny prostředky stránky se musí načíst správně, včetně skriptů, šablon stylů, obrázků a tak dále, jinak test nebude úspěšný.

Všimněte si, že webový test musí být zcela obsažen v souboru .webtest: programové funkce nelze použít v testu.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Doba zapojení a náhodná čísla do vícekrokového testu
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

## <a name="dealing-with-sign-in"></a>Vyřešení přihlášení
Pokud se uživatelé přihlásí do aplikace, máte několik možností pro simulaci přihlášení, takže můžete otestovat stránky následující po přihlášení. Použitý přístup závisí na typu zabezpečení poskytovaném aplikací.

Ve všech případech musíte v aplikaci vytvořit účet jenom pro účely testování. Pokud je to možné, omezte oprávnění tohoto testovacího účtu, aby webové testy nemohly žádným způsobem ovlivnit skutečné uživatele.

### <a name="simple-username-and-password"></a>Jednoduché uživatelské jméno a heslo
Webový test zaznamenejte obvyklým způsobem. Nejprve odstraňte soubory cookie.

### <a name="saml-authentication"></a>Ověřování SAML
Použijte zásuvný modul SAML, který je pro webové testy dostupný.

### <a name="client-secret"></a>Tajný klíč klienta
Pokud vaše aplikace obsahuje trasu přihlášení, která zahrnuje tajný klíč klienta, použijte ji. Azure Active Directory (AAD) je příkladem služby, která poskytuje přihlašování pomocí tajného klíče klienta. Ve službě AAD je tajným klíčem klienta klíč aplikace.

Tady je ukázkový webový test webové aplikace v Azure pomocí klíče aplikace:

![Ukázkový tajný klíč klienta](./media/app-insights-monitor-web-app-availability/110.png)

1. Získejte token ze služby AAD pomocí tajného klíče klienta (AppKey).
2. Extrahujte nosný token z odpovědi.
3. Pomocí nosného tokenu v autorizační hlavičce zavolejte rozhraní API.

Ujistěte se, jestli je webový test skutečným klientem – to znamená, že má vlastní aplikaci ve službě AAD – a použijte jeho clientId + appkey. Testovaná služba má také vlastní aplikaci ve službě AAD: identifikátor URI appID této aplikace se ve webovém testu projevuje v poli „prostředek“.

### <a name="open-authentication"></a>Otevřené ověřování
Příkladem otevřeného ověřování je přihlašování pomocí účtu Microsoft nebo Google. Velký počet aplikací, které používají OAuth, nabízí alternativní tajný klíč klienta, takže prvním cílem bude prozkoumání této možnosti.

Pokud váš test vyžaduje přihlášení pomocí OAuth, bude obecný postup následující:

* K prozkoumání přenosu dat mezi webovým prohlížečem, webem ověřování a aplikací použijte například nástroj Fiddler.
* Proveďte dvě nebo více přihlášení pomocí různých počítačů nebo prohlížečů nebo v dlouhých intervalech (aby mohla vypršet platnost tokenů).
* Porovnáním různých relací identifikujte token předaný zpět z webu ověřování, který byl následně předán do aplikačního serveru po přihlášení.
* Uložte webový test pomocí sady Visual Studio.
* Parametrizujte tokeny, nastavte parametr při vrácení tokenu z ověřovatele a použijte ho v dotazu na web.
  (Sada Visual Studio se pokusí o parametrizaci testu, ale nebude tokeny parametrizovat správně.)

## <a name="edit"></a> Úprava nebo zakázání testu
Otevřete jednotlivé testy a upravte je nebo je zakažte.

![Upravit nebo zakázat webový test](./media/app-insights-monitor-web-app-availability/19-availEdit.png)

Můžete chtít zakázat webové testy během provádění údržby vaší služby.

## <a name="performance-tests"></a>Testy výkonnosti
Na svém webu můžete spustit zátěžový test. Podobně jako v testu dostupnosti můžete z našich bodů po celém světě odeslat buď jednoduché požadavky, nebo vícekrokové požadavky. Na rozdíl od testu dostupnosti se odesílá mnoho požadavků, které simulují několik souběžných uživatelů.

V okně Přehled otevřete **Nastavení**, **Testy výkonnosti**. Při vytváření testu budete vyzváni k připojení k účtu Visual Studio Team Services nebo k jeho vytvoření.

Po dokončení testu se zobrazí časy odezvy a míra úspěšnosti.

## <a name="automation"></a>Automation
* [Automaticky používat skripty prostředí PowerShell k nastavení webového](app-insights-powershell.md#add-an-availability-test) testu.
* Nastavení [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán při vydání výstrahy.

## <a name="questions-problems"></a>Máte dotazy? Problémy?
* *Můžu z webového testu volat kód?*

    Ne. Kroky testu musí být v souboru .webtest. A nemůžete volat jiné webové testy nebo používat smyčky. Existují různé zásuvné moduly, které se vám můžou hodit.
* *Je podporovaný protokol HTTPS?*

    Podporujeme protokoly TLS 1.1 a TLS 1.2.
* *Je nějaký rozdíl mezi „webovými testy“ a „testy dostupnosti“?*

    Používáme tyto dvě podmínky zaměnitelně.
* *Chci používat testy dostupnosti na našem interním serveru, který je spuštěný za bránou firewall.*

    Existují dvě možná řešení:
    
    * Nakonfigurujte bránu firewall, aby povolovala příchozí požadavky z [IP adres našich agentů webového testu](app-insights-ip-addresses.md).
    * Napište vlastní kód, který pravidelně testuje interní server. Spusťte kód na testovacím serveru jako proces na pozadí za vaší bránou firewall. Testovací proces můžete odesílat své výsledky do Application Insights pomocí rozhraní API [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) v balíčku Core SDK. To vyžaduje, aby měl váš testovací server odchozí přístup ke koncovému bodu ingestování Application Insights, ale to je mnohem menší riziko zabezpečení než případné povolení příchozích požadavků. Výsledky se nebudou zobrazovat v oknech webových testů dostupnosti, ale zobrazí se jako výsledky dostupnosti v Průzkumníku metrik, analýzy a vyhledávání.
* *Vícekrokový webový test se nepodařilo nahrát.*

    Maximální velikost je 300 kB.

    Smyčky nejsou podporovány.

    Odkazy na jiné webové testy nejsou podporovány.

    Zdroje dat nejsou podporovány.
* *Můj vícekrokový test se nedokončil.*

    Existuje limit 100 požadavků na test.

    Test bude zastaven, pokud poběží déle než dvě minuty.
* *Jak spustím test s klientskými certifikáty?*

    Tuto možnost nepodporujeme, je nám líto.


## <a name="next"></a>Další kroky
[Prohledávání diagnostických protokolů][diagnostic]

[Řešení potíží][qna]

[IP adresy webových testovacích agentů](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

