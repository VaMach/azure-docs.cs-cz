---
title: "Sledování dostupnosti a odezvy libovolných webů | Dokumentace Microsoftu"
description: "Nastavení testů webu ve službě Application Insights. Zasílání upozornění, pokud web přestane být k dispozici nebo reaguje pomalu."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: b70c8baab03703bc00b75c2c611f69e3b71d6cd7
ms.openlocfilehash: d3478ef704c0029f69cca141bd3fa0b3ac54de15


---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Sledování dostupnosti a odezvy libovolných webů
Po nasazení webové aplikace nebo webu na libovolném serveru můžete nastavit webové testy ke sledování dostupnosti a odezvy. [Application Insights v sadě Visual Studio](app-insights-overview.md) odesílá do vaší aplikace webové požadavky v pravidelných intervalech z bodů po celém světě. Upozorní vás v případě, že vaše aplikace reaguje pomalu nebo nereaguje vůbec.

![Příklad webového testu](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Webové testy můžete nastavit pro libovolný koncový bod HTTP nebo HTTPS, který je přístupný z veřejného internetu.

Existují dva typy webového testu:

* [Testování ping adresy URL](#create): jednoduchý test, který můžete vytvořit na portálu Azure.
* [Vícekrokový test webu](#multi-step-web-tests): který můžete vytvořit v sadě Visual Studio Ultimate nebo Visual Studio Enterprise a odeslat na portál.

Můžete vytvořit až 10 webových testů na prostředek aplikace.

## <a name="a-namecreatea1-create-a-resource-for-your-test-reports"></a><a name="create"></a>1. Vytvoření prostředku pro testovacích sestavy
Tento krok přeskočte, pokud jste již [nastavili prostředek Application Insights][start] pro tuto aplikaci a chcete zobrazit sestavy dostupnosti na stejném místě.

Zaregistrujte se na portálu [Microsoft Azure](http://azure.com), přejděte na [portál Azure](https://portal.azure.com) a vytvořte prostředek Application Insights.

![Nový > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Kliknutím na možnost **Všechny prostředky** otevřete okno Přehled pro nový prostředek.

## <a name="a-namesetupa2-create-a-url-ping-test"></a><a name="setup"></a>2. Vytvoření testu adresy URL pomocí příkazu Ping
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

    **Shoda obsahu**: řetězec, například „Vítejte!“ Testujeme, zda se vyskytuje v každé odpovědi. Musí být prostý řetězec bez zástupných znaků. Nezapomeňte, že pokud se obsah vaší stránka změní, bude pravděpodobně nutné jej aktualizovat.
* **Výstrahy** jsou ve výchozím nastavení odesílány, pokud se vyskytnou selhání ve třech umístěních po dobu delší než pět minut. Selhání v jednom umístění bude pravděpodobně problém se sítí a nejedná se o problém s webem. Ale můžete změnit prahovou hodnotu na citlivější nebo méně citlivou a můžete také změnit, komu mají být e-maily zasílány.

    Můžete nastavit [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán, když je vydána výstraha. (Všimněte si, že v současné době parametry dotazu neprocházejí jako vlastnosti.)

### <a name="test-more-urls"></a>Testování více adres URL
Přidat další testy Pro příklad, a také jako testování domovské stránky, můžete zajistit, že vaše databáze se spouští otestováním adresy URL pro hledání.

## <a name="a-namemonitora3-see-your-web-test-results"></a><a name="monitor"></a>3. Zobrazení výsledků testu webu
Po 1–2 minutách se v okně Test webu zobrazí výsledky.

![Souhrnné výsledky na domácím okně](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Kliknutím na libovolný panel v grafu souhrnu získáte podrobnější zobrazení tohoto časového období.

Tyto grafy kombinují výsledky pro všechny webové testy aplikace.

## <a name="a-namefailuresaif-you-see-failures"></a><a name="failures"></a>Pokud se zobrazí chyby
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

*Zdá se, že všechno je v pořádku, ale přesto je hlášena chyba.* Zkontrolujte všechny image, skripty, šablony stylů a všechny další soubory, které stránka načetla. Pokud některý z nich selže, test se ohlásí jako neúspěšný i v případě, že se hlavní html stránka načte bez problémů.

## <a name="multistep-web-tests"></a>Vícekrokové webové testy
Je možné sledovat scénář, který zahrnuje posloupnost adres URL. Například pokud sledujete prodejní web, můžete otestovat, zda správně funguje přidávání položek do nákupního košíku.

Pro vytvoření vícekrokového testu uložte scénář pomocí sady Visual Studio a pak nahrajte tento záznam do služby Application Insights. Application Insights přehrává scénář v intervalech a ověřuje odezvy.

Všimněte si, že nemůžete použít kódované funkce ve svých testech: kroky scénáře musí být obsaženy jako skript v souboru .webtest.

#### <a name="1-record-a-scenario"></a>1. Záznam scénáře
Slouží k zaznamenání relace webové aplikace Visual Studio Enterprise nebo Ultimate.

1. Vytvořte projekt testu výkonnosti webu.

    ![Vytvořte v sadě Visual Studio nový projekt ze šablony výkonu webu a zátěžového testu.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)
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

### <a name="plugging-time-and-random-numbers-into-your-multistep-test"></a>Doba zapojení a náhodná čísla do vícekrokového testu
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

## <a name="dealing-with-signin"></a>Vyřešení přihlášení
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

## <a name="a-nameedita-edit-or-disable-a-test"></a><a name="edit"></a> Úprava nebo zakázání testu
Otevřete jednotlivé testy a upravte je nebo je zakažte.

![Upravit nebo zakázat webový test](./media/app-insights-monitor-web-app-availability/19-availEdit.png)

Můžete chtít zakázat webové testy během provádění údržby vaší služby.

## <a name="performance-tests"></a>Testy výkonnosti
Na svém webu můžete spustit zátěžový test. Podobně jako v testu dostupnosti můžete z našich bodů po celém světě odeslat buď jednoduché požadavky, nebo vícekrokové požadavky. Na rozdíl od testu dostupnosti se odesílá mnoho požadavků, které simulují několik souběžných uživatelů.

V okně Přehled otevřete **Nastavení**, **Testy výkonnosti**. Při vytváření testu budete vyzváni k připojení k účtu Visual Studio Team Services nebo k jeho vytvoření.

Po dokončení testu se zobrazí časy odezvy a míra úspěšnosti.

## <a name="automation"></a>Automation
* [Automaticky používat skripty prostředí PowerShell k nastavení webového](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/) testu.
* Nastavení [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán při vydání výstrahy.

## <a name="questions-problems"></a>Máte dotazy? Problémy?
* *Můžu z webového testu volat kód?*

    Ne. Kroky testu musí být v souboru .webtest. A nemůžete volat jiné webové testy nebo používat smyčky. Existují různé zásuvné moduly, které se vám můžou hodit.
* *Je podporovaný protokol HTTPS?*

    Podporujeme protokoly TLS 1.1 a TLS 1.2.
* *Je nějaký rozdíl mezi „webovými testy“ a „testy dostupnosti“?*

    Používáme tyto dvě podmínky zaměnitelně.
* *Chci používat testy dostupnosti na našem interním serveru, který je spuštěný za bránou firewall.*

    Nakonfigurujte bránu firewall, aby povolovala požadavky z [IP adres agentů webového testu](app-insights-ip-addresses.md).
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

## <a name="a-namevideoavideo"></a><a name="video"></a>Video
> [!VIDEO https://channel9.msdn.com/Series/Application-Insights-on-Azure-Preview-Portal/Monitoring-Availability-with-Application-Insights/player]
>
>

## <a name="a-namenextanext-steps"></a><a name="next"></a>Další kroky
[Vyhledávání diagnostických protokolů][diagnostika]

[Poradce při potížích][qna].

[IP adresy webových testovacích agentů](app-insights-ip-addresses.md)

<!--Link references-->

[dostupnost-azure]: ../insights-create-web-tests.md
[diagnostika]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md



<!--HONumber=Nov16_HO2-->


