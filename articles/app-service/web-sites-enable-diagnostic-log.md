---
title: "Povolit protokolování diagnostiky pro webové aplikace v Azure App Service"
description: "Zjistěte, jak povolit protokolování diagnostiky a přidání instrumentace do aplikace, jakož i postupy pro přístup k informacím v Azure protokolu."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 42644bd97443e827b523503989b0b25d3d48028c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Povolit protokolování diagnostiky pro webové aplikace v Azure App Service
## <a name="overview"></a>Přehled
Azure poskytuje integrované diagnostiky vám pomůže při ladění [webové aplikace App Service](http://go.microsoft.com/fwlink/?LinkId=529714). V tomto článku se dozvíte, jak povolit protokolování diagnostiky a přidání instrumentace do aplikace, jakož i postupy pro přístup k informacím v Azure protokolu.

Tento článek používá [portálu Azure](https://portal.azure.com), Azure PowerShell a rozhraní příkazového řádku Azure (Azure CLI) pro práci s diagnostické protokoly. Informace o práci s diagnostických protokolů pomocí sady Visual Studio najdete v tématu [řešení potíží s Azure v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Webový server diagnostics a application diagnostics
Webové aplikace služby App Service poskytují diagnostické funkce pro protokolování informací z webového serveru a webové aplikace. Tyto jsou logicky rozdělené do **webový server diagnostiky** a **rozhraní application diagnostics**.

### <a name="web-server-diagnostics"></a>Diagnostika webového serveru
Můžete povolit nebo zakázat následující typy protokolů:

* **Podrobné protokolování chyb** -podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší). To může obsahovat informace, které vám mohou pomoci určit, proč server vrátil kód chyby.
* **Se nezdařilo, trasování požadavku** -podrobné informace o chybných žádostech, včetně trasování pro součásti služby IIS používá ke zpracování žádostí a doba trvání v jednotlivých součástí. To může být užitečné, pokud se pokoušíte zvýšit výkon webu nebo izolovat, co ho způsobuje. konkrétní chyba protokolu HTTP, který se má vrátit.
* **Webový Server protokolování** -informace o transakcích HTTP pomocí [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). To je užitečné, když chcete určit celkový metriky lokality, jako je počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.

### <a name="application-diagnostics"></a>Rozhraní Application diagnostics
Rozhraní Application diagnostics umožňuje zaznamenat informace o vytvořil webovou aplikací. Aplikace ASP.NET můžete použít [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) třída do protokolu informace o protokolu diagnostiky aplikace. Například:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

V době běhu může načíst tyto protokoly, které pomáhají při řešení potíží. Další informace najdete v tématu [řešení potíží s Azure webové aplikace v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

Webové aplikace služby App Service také protokolu informace o nasazení, při publikování obsahu do webové aplikace. K tomu dojde automaticky a nejsou žádné nastavení konfigurace pro nasazení protokolování. Nasazení protokolování umožňuje určit, proč nasazení se nezdařilo. Například pokud používáte vlastní nasazení skriptu, můžete použít nasazení protokolování určit, proč se skript selhává.

## <a name="enablediag"></a>Postup povolení diagnostiky
Povolí se Diagnostika v [portálu Azure](https://portal.azure.com), přejděte do okna pro vaši webovou aplikaci a klikněte na tlačítko **Nastavení > diagnostické protokoly**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Část protokoly](./media/web-sites-enable-diagnostic-log/logspart.png)

Když povolíte **rozhraní application diagnostics** také vybrat **úroveň**. Toto nastavení umožňuje filtrovat informace zachycen **informační**, **upozornění** nebo **chyba** informace. Toto nastavení na **podrobné** bude protokolovat všechny informace o vytvořil aplikací.

> [!NOTE]
> Na rozdíl od změny v souboru web.config, povolení rozhraní Application diagnostics nebo změna úrovně protokolů diagnostiky není recyklujte doménu aplikace, která je aplikace spuštěná v rámci.
>
>

V [portálu classic](https://manage.windowsazure.com) webové aplikace **konfigurace** kartě můžete vybrat **úložiště** nebo **systém souborů** pro **protokolování webového serveru**. Výběr **úložiště** vám umožní vybrat účet úložiště a kontejner objektů blob, který budou zapisovat do protokolů. Všechny protokoly pro **lokality diagnostiky** se zapisují do systému souborů.

[Portálu classic](https://manage.windowsazure.com) webové aplikace **konfigurace** karta má také další nastavení pro rozhraní application diagnostics:

* **Systém souborů** – ukládá informace o diagnostiky aplikace do systému souborů webové aplikace. Tyto soubory můžete získat přístup pomocí protokolu FTP, nebo stáhnout jako archivu Zip pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI).
* **Tabulka úložiště** – ukládá informace o aplikaci diagnostiky v zadaný název účtu úložiště Azure a tabulku.
* **Úložiště objektů blob** – ukládá informace diagnostiky aplikace v zadaném kontejneru účtu úložiště Azure a objektů blob.
* **Doba uchování** – ve výchozím nastavení, nejsou automaticky odstraněny protokoly z **úložiště objektů blob**. Vyberte **nastavení ukládání** a zadejte počet dní, aby protokoly, pokud chcete automaticky odstranit protokoly.

> [!NOTE]
> Pokud jste [opět vytvořit přístupové klíče účtu úložiště](../storage/common/storage-create-storage-account.md), musíte nastavit konfiguraci příslušných protokolování používali aktualizované klíče. Použijte následující postup:
>
> 1. V **konfigurace** kartě, nastavte funkce příslušných protokolování na **vypnout**. Uložte vaše nastavení.
> 2. Povolte protokolování na účtu úložiště objektů blob nebo table znovu. Uložte vaše nastavení.
>
>

Libovolnou kombinaci systému souborů, úložiště table nebo úložiště objektů blob může být povoleno ve stejnou dobu a mají vlastní protokol úrovni konfigurace. Například můžete chtít protokolovat chyby a upozornění do úložiště objektů blob jako dlouhodobé řešení protokolování, při povolování protokolování systému souborů s úrovní verbose.

Zatímco všech tří umístění úložiště poskytují stejnou základní informace o protokolu událostí, **tabulky úložiště** a **úložiště objektů blob** protokolu Další informace, jako je instance ID, ID vlákna a časové razítko podrobnější (formát značek) než protokolování, aby se **systém souborů**.

> [!NOTE]
> Informace uložené v **tabulky úložiště** nebo **úložiště objektů blob** lze přistupovat pouze pomocí klienta úložiště nebo aplikaci, která může pracovat přímo s těchto systémů úložiště. Například Visual Studio 2013 obsahuje Průzkumníka úložiště, který slouží k prozkoumání tabulky nebo objekt blob úložiště a HDInsight můžete přístup k datům uloženým v úložišti objektů blob. Je také možné zapsat aplikaci, která přistupuje k službě Azure Storage pomocí jednoho z [sady Azure SDK](/downloads/#).
>
> [!NOTE]
> Diagnostika se dá taky povolit z prostředí Azure PowerShell pomocí **Set-AzureWebsite** rutiny. Pokud jste nenainstalovali prostředí Azure PowerShell nebo nenakonfigurovali ho na používání vašeho předplatného Azure, najdete v části [jak používat Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Postupy: stažení protokolů
Diagnostické informace uložené na systém souborů webové aplikace lze přistupovat přímo pomocí protokolu FTP. Lze ji také stáhnout jako archivu Zip pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.

Strukturu adresáře, které protokoly jsou uložené v vypadá takto:

* **Protokoly aplikací** -/LogFiles/aplikace /. Tato složka obsahuje jeden nebo více souborů textu obsahujícího informace o vyprodukované protokolování aplikací.
* **Trasování požadavku se nezdařilo** -/ LogFiles/W3SVC ### /. Tato složka obsahuje soubor XSL a jeden nebo více souborů XML. Ujistěte se, stáhněte soubor XSL do stejného adresáře jako soubor XML souborům, protože soubor XSL poskytuje funkce pro formátování a filtrování obsah soubory XML, pokud v prohlížeči Internet Explorer.
* **Podrobné protokoly chyb** -/LogFiles/DetailedErrors /. Tato složka obsahuje jeden nebo více souborů HTM, které poskytují podrobné informace pro chyby protokolu HTTP, ke kterým došlo.
* **Webový Server protokoly** -/LogFiles/http/RawLogs. Tato složka obsahuje jeden nebo více textových souborů formátován pomocí [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Protokoly nasazení** -/ LogFiles/Git. Tato složka obsahuje protokoly interní nasazení procesy používané modulem webové aplikace Azure, jakož i protokoly pro nasazení Git.

### <a name="ftp"></a>FTP
Chcete-li získat přístup k diagnostických informací s použitím FTP, navštivte **řídicí panel** vaší webové aplikace v [portálu classic](https://manage.windowsazure.com). V **rychlého přehledu** pomocí **diagnostické protokoly FTP** odkaz pro přístup k souborům protokolu pomocí protokolu FTP. **Nasazení nebo FTP uživatele** položka uvádí uživatelské jméno, které se má použít pro přístup k serveru FTP.

> [!NOTE]
> Pokud **uživatele nasazení nebo FTP** položka není nastavena, nebo jste zapomněli heslo pro tohoto uživatele, můžete vytvořit nového uživatele a heslo pomocí **resetovat přihlašovací údaje nasazení** odkaz v **rychlého přehledu** části **řídicí panel**.
>
>

### <a name="download-with-azure-powershell"></a>Stáhnout pomocí prostředí Azure PowerShell
Chcete-li stáhnout soubory protokolů, spustit novou instanci třídy Azure PowerShell a použijte následující příkaz:

    Save-AzureWebSiteLog -Name webappname

Tato akce uloží protokoly pro webovou aplikaci určeného **-název** parametr do souboru s názvem **logs.zip** v aktuálním adresáři.

> [!NOTE]
> Pokud jste nenainstalovali prostředí Azure PowerShell nebo nenakonfigurovali ho na používání vašeho předplatného Azure, najdete v části [jak používat Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Stáhnout pomocí rozhraní příkazového řádku Azure
Chcete-li stáhnout soubory protokolů pomocí rozhraní příkazového řádku Azure, otevřete nový příkazový řádek, prostředí PowerShell, Bash nebo relaci Terminálové služby a zadejte následující příkaz:

    azure site log download webappname

Tato akce uloží protokoly pro webovou aplikaci s názvem "webappname" do souboru s názvem **diagnostics.zip** v aktuálním adresáři.

> [!NOTE]
> Pokud jste nenainstalovali rozhraní příkazového řádku Azure (Azure CLI), nebo nenakonfigurovali ho na používání vašeho předplatného Azure, najdete v části [postup pomocí příkazového řádku Azure CLI](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Postupy: zobrazení protokolů ve službě Application Insights
Visual Studio Application Insights poskytuje nástroje pro filtrování a vyhledávání protokolů a korelace protokoly s požadavky a další události.

1. Přidejte Application Insights SDK do projektu v sadě Visual Studio.
   * V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte Přidat službu Application Insights. Budete se zobrazil pokyn provede kroky, které zahrnují vytváření prostředek Application Insights. [Další informace](../application-insights/app-insights-asp-net.md)
2. Do projektu přidejte balíček naslouchací proces trasování.
   * Klikněte pravým tlačítkem na projekt a zvolte spravovat balíčky NuGet. Vyberte `Microsoft.ApplicationInsights.TraceListener` [Další informace](../application-insights/app-insights-asp-net-trace-logs.md)
3. Odeslání projektu a spusťte ji k vygenerování dat protokolu.
4. V [portálu Azure](https://portal.azure.com/), přejděte na váš nový prostředek Application Insights a otevřete **vyhledávání**. Zobrazí se data protokolu, spolu s požadavku, použití a další telemetrií. Nějaké telemetrie může trvat několik minut příchod: klikněte na tlačítko Aktualizovat. [Další informace](../application-insights/app-insights-diagnostic-search.md)

[Další informace o s Application Insights pro sledování výkonu](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Postupy: Stream protokoly
Při vývoji aplikace, je často užitečné informace protokolování v skoro v reálném čase. To můžete udělat streamování informace o protokolování do vývojového prostředí pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.

> [!NOTE]
> Některé typy protokolování vyrovnávací paměť k zápisu do souboru protokolu, což může vést k události mimo pořadí v datovém proudu. Například položku protokolu aplikace, která nastane, když uživatel navštíví stránky nemusí být zobrazeny v datovém proudu před odpovídající záznam protokolu HTTP pro požadavek na stránku.
>
> [!NOTE]
> Vysílání datového proudu protokolu bude také stream informace zapsané na libovolný textový soubor, který je uložen v **D:\\domácí\\LogFiles\\**  složky.
>
>

### <a name="streaming-with-azure-powershell"></a>Streamování pomocí prostředí Azure PowerShell
Informace o protokolování datového proudu, spustit novou instanci třídy Azure PowerShell a použijte následující příkaz:

    Get-AzureWebSiteLog -Name webappname -Tail

To připojí do webové aplikace určeného **-název** parametr a začít streamování informace do okna prostředí PowerShell jako protokolu událostí, ke kterým došlo u webové aplikace. Žádné informace, zapisovat do souborů končící na .txt, .log nebo htm, které jsou uložené v adresáři /LogFiles (d: nebo Domovská nebo soubory protokolů) bude Streamovat do místní konzoly.

Chcete-li filtrovat konkrétní události, jako je například chyby, použijte **– zpráva** parametr. Například:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

Chcete-li filtrovat konkrétní typy, jako je například HTTP, použijte **-cesta** parametr. Například:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

Pokud chcete zobrazit seznam dostupných cesty, použijte parametr - ListPath.

> [!NOTE]
> Pokud jste nenainstalovali prostředí Azure PowerShell nebo nenakonfigurovali ho na používání vašeho předplatného Azure, najdete v části [jak používat Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Streamování pomocí rozhraní příkazového řádku Azure
Stream informace o protokolování, otevřete nový příkazový řádek, prostředí PowerShell, Bash nebo relaci terminálu a zadejte následující příkaz:

    azure site log tail webappname

To bude připojení k webové aplikaci s názvem 'webappname' a začít streamování informace do okna události protokolu jsou prováděny ve webové aplikaci. Žádné informace, zapisovat do souborů končící na .txt, .log nebo htm, které jsou uložené v adresáři /LogFiles (d: nebo Domovská nebo soubory protokolů) bude Streamovat do místní konzoly.

Chcete-li filtrovat konkrétní události, jako je například chyby, použijte **– filtru** parametr. Například:

    azure site log tail webappname --filter Error

Chcete-li filtrovat konkrétní typy, jako je například HTTP, použijte **– cesta** parametr. Například:

    azure site log tail webappname --path http

> [!NOTE]
> Pokud jste nenainstalovali rozhraní příkazového řádku Azure nebo nenakonfigurovali ho na používání vašeho předplatného Azure, najdete v části [postupy pro použití rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Postupy: pochopení protokolů diagnostiky
### <a name="application-diagnostics-logs"></a>Diagnostické protokoly aplikací
Rozhraní Application diagnostics ukládá informace v konkrétním formátu pro aplikace .NET, v závislosti na tom, jestli ukládání protokolů systému souborů, úložiště table nebo úložiště objektů blob. Základní sada dat uložených je stejná napříč všechny tři typy úložiště - datum a čas výskytu události, ID procesu, který vytváří událost, typu události (informace, upozornění, chyby) a zprávy událostí.

**Systém souborů**

Každý řádek přihlášení k systému souborů nebo pomocí vysílání datového proudu bude v následujícím formátu:

    {Date}  PID[{process id}] {event type/level} {message}

Například událost chyby by se zobrazí podobná této:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Protokolování do systému souborů poskytuje nejzákladnější informace tři dostupné metody poskytnutí jenom čas, id procesu, úroveň události a zprávy.

**Table Storage**

Při přihlašování do úložiště table, další vlastnosti se používají pro usnadnění hledání data uložená v tabulce a také podrobnější informace o události. U každé entity (řádek) uložené v tabulce se používají následující vlastnosti (sloupce).

| Název vlastnosti | Hodnota nebo formátu |
| --- | --- |
| Klíč oddílu |Datum a čas ve formátu yyyyMMddHH události |
| RowKey |Hodnota identifikátoru GUID, který jedinečně identifikuje tuto entitu |
| časové razítko |Datum a čas, kdy došlo k události |
| EventTickCount |Datum a čas, kdy došlo k události, ve formátu značky (větší přesnost) |
| ApplicationName |Název webové aplikace |
| Úroveň |Úroveň události (například Chyba, upozornění, informace) |
| ID události |ID události této události<p><p>Výchozí hodnota je 0-li zadán žádný |
| identifikátor instanceId |Instanci webové aplikace, které i došlo |
| PID |ID procesu |
| TID |ID vlákna vlákna, která vytváří událost |
| Zpráva |Podrobná zpráva události |

**Blob Storage**

Při přihlašování do úložiště objektů blob, data se ukládají ve formátu hodnot oddělených čárkami (CSV). Úložiště table, podobně jako další pole jsou protokolovány k poskytnutí podrobnější informace o události. Pro každý řádek ve sdíleném svazku clusteru se používají následující vlastnosti:

| Název vlastnosti | Hodnota nebo formátu |
| --- | --- |
| Datum |Datum a čas, kdy došlo k události |
| Úroveň |Úroveň události (například Chyba, upozornění, informace) |
| ApplicationName |Název webové aplikace |
| identifikátor instanceId |Instanci webové aplikace, které došlo k události |
| EventTickCount |Datum a čas, kdy došlo k události, ve formátu značky (větší přesnost) |
| ID události |ID události této události<p><p>Výchozí hodnota je 0-li zadán žádný |
| PID |ID procesu |
| TID |ID vlákna vlákna, která vytváří událost |
| Zpráva |Podrobná zpráva události |

Data uložená v objektu blob by vypadat podobně jako následující:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> První řádek protokolu, který bude obsahovat záhlaví sloupců, reprezentovaný v tomto příkladu.
>
>

### <a name="failed-request-traces"></a>Trasování požadavku se nezdařilo
Trasování chybných požadavků, které jsou uložené v soubory XML s názvem **fr ### .xml**. Aby bylo snazší zaznamenané informace zobrazit, s názvem šablony stylů XSL **freb.xsl** ve stejném adresáři jako soubory XML. Otevření jedné ze souborů XML v aplikaci Internet Explorer bude používat šablony stylů XSL zajistit formátovaný zobrazení informací o trasování. Ta se zobrazí podobná této:

![Zobrazit v prohlížeči chybných požadavků](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Podrobné protokoly chyb
Podrobné protokoly chyb jsou dokumenty HTML, které poskytují podrobnější informace o chyby protokolu HTTP, které mají došlo k chybě. Vzhledem k tomu, že se jednoduše dokumentů HTML, bylo možné zobrazit pomocí webového prohlížeče.

### <a name="web-server-logs"></a>Protokoly webového serveru
Protokoly webového serveru jsou formátovány pomocí [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Tyto informace lze číst pomocí textového editoru nebo analyzovat pomocí nástrojů, jako třeba [analyzátoru protokolů](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Nepodporuje protokol vytváří ve službě Azure web apps **s-computername**, **s-ip**, nebo **cs-version** pole.
>
>

## <a name="nextsteps"></a> Další kroky
* [Postup monitorování webové aplikace](web-sites-monitor.md)
* [Řešení potíží s aplikací Azure web apps v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analýza protokolů webové aplikace v prostředí HDInsight](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
* Průvodce změnou starý portál na nový portál najdete v tématu: [referenční informace pro navigace na portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715)
