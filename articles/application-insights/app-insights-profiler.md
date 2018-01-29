---
title: "Profil za provozu webové aplikace v Azure s Application Insights profileru | Microsoft Docs"
description: "Identifikujte aktivní trase v kódu webového serveru s nízkými nároky profileru."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: 5f691fb88c6764309bf012dfc65b561ec87afede
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profil za provozu Azure web apps s Application Insights

*Tato funkce Application Insights je obecně dostupné pro Azure App Service a je ve verzi preview pro výpočetní prostředky Azure.*

Zjistěte, jak dlouho je věnovaný jednotlivých metod v za provozu webové aplikace při používání [Application Insights](app-insights-overview.md). Application Insights profilace nástroj zobrazuje podrobné profily živé požadavky, které se spouští vaše aplikace a klade důraz *aktivní trase* používající nejvíce času. Požadavky s jinou odezvy jsou profilovaným na základě vzorkování. Režijní náklady na aplikaci je minimalizován, pomocí různých metod.

Profileru aktuálně funguje pro ASP.NET a ASP.NET základní webové aplikace běžící v Azure App Service v alespoň **základní** vrstvy služeb.

## <a id="installation"></a>Povolit profileru pro aplikaci služby webovou aplikaci
Pokud už je aplikace publikována aplikační služby, ale ještě nepracovali ve zdrojovém kódu pro použití služby Application Insights, přejděte do podokna aplikační služby na portálu Azure, přejděte na **monitorování | Application Insights**, postupujte podle pokynů v podokně pro vytvoření nových nebo vyberte existující prostředek Application Insights k monitorování vaší webové aplikace. Všimněte si, že profileru pracuje pouze s **základní** plán aplikační služby nebo vyšší.

![Povolit aplikaci Insights na portálu aplikační služby][appinsights-in-appservices]

Pokud máte přístup k projektu zdrojový kód, [nainstalujte službu Application Insights](app-insights-asp-net.md). Pokud je již nainstalována, ujistěte se, že máte nejnovější verzi. Kontroluje nejnovější verzi, v Průzkumníku řešení klikněte pravým tlačítkem na projekt a potom vyberte **balíčky spravovat balíčky NuGet** > **aktualizace** > **aktualizovat všechny balíčky**. Potom nasaďte aplikaci.

Chcete-li nainstalovat 2.1.0-beta6 balíček Microsoft.ApplicationInsights.AspNetCore NuGet nebo novější pro práci s profileru potřebuje aplikace ASP.NET Core. Od verze 27. června 2017 nepodporujeme starší verze.

V [portálu Azure](https://portal.azure.com), otevřete prostředek Application Insights pro webové aplikace. Vyberte **výkonu** > **povolit Application Insights profileru**.

![Vyberte Banner informující o povolení profileru][enable-profiler-banner]

Alternativně můžete vybrat **profileru** konfigurace k zobrazení stavu a povolení nebo zakázání profileru.

![V části výkon vyberte konfiguraci profileru][performance-blade]

Webové aplikace, které jsou nakonfigurovány s Application Insights, jsou uvedeny v **profileru** podokně Konfigurace. Pokud jste postupovali podle kroků výše, musí být profileru agent již nainstalován. Vyberte **povolit profileru** v **profileru** podokně Konfigurace.

V případě potřeby, postupujte podle pokynů pro instalaci agenta profileru. Pokud byly nakonfigurovány žádné webové aplikace pomocí Application Insights, vyberte **přidat propojené aplikace**.

![Konfigurovat možnosti podokna][linked app services]

Na rozdíl od webových aplikací, které jsou hostované prostřednictvím plány služby App Service nejsou přímo spravovat aplikace, které jsou hostované v Azure výpočetní prostředky (například virtuální počítače Azure, sady škálování virtuálního počítače, Azure Service Fabric nebo Azure Cloud Services) v Azure. V takovém případě není žádná webová aplikace propojení. Místo propojení na aplikaci, vyberte **povolit profileru** tlačítko.

### <a name="enable-the-profiler-for-azure-compute-resources-preview"></a>Povolit profileru pro výpočetní prostředky Azure (preview)

Získání informací [verze preview profileru pro výpočetní prostředky Azure](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Zobrazení dat profileru

**Ujistěte se, že vaše aplikace přijímá traffics.** Při provádění experiment, můžete vygenerovat požadavky na vaší webové aplikace pomocí [testování výkonu Application Insights](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Pokud jste povolili nově profileru, můžete spustit krátké zátěžový test asi 15 minut a měli byste obdržet profileru trasování. Pokud máte profileru povolené už nějakou dobu, prosím mějte na paměti, profileru běží náhodně dvakrát každou hodinu a dvě minuty pokaždé, když ji spustí. Navrhněte ke spuštění zátěžového testu pro jednu hodinu a ujistěte se, že získají ukázková profiler trasování.

Po aplikaci přijme některé provozy, přejděte do **výkonu** okno, přejděte na **provést akce** části stránky zobrazíte profileru trasování. Vyberte **profileru trasování** tlačítko.

![Application Insights výkonu podokně preview profileru trasování][performance-blade-v2-examples]

Vyberte ukázku zobrazit rozpis úrovni kódu čas provádění požadavku.

![Application Insights trasování explorer][trace-explorer]

Trasování explorer zobrazí následující informace:

* **Zobrazit aktivní trase**: Otevře biggest list uzlu, nebo aspoň něco zavřete. Ve většině případů je tento uzel u kritických bodů výkonu.
* **Popisek**: název funkce nebo událostí. Strom zobrazuje směs kód a události, které došlo k chybě (např. SQL a HTTP událostí). Horní událost představuje celkovou dobu trvání požadavku.
* **Uplynulý čas**: časový interval mezi začátkem operace a konci operace.
* **Když**: když pro funkci nebo událostí byl spuštěn v vztah do dalších funkcí.

## <a name="how-to-read-performance-data"></a>Informace o načtení dat výkonu

Profileru služby Microsoft používá kombinaci metod vzorkování a instrumentace analyzovat výkon vaší aplikace. Když probíhá podrobné kolekce, ukázky služby profileru ukazatel instrukce každého z procesoru počítače v každé milisekundu. Každá ukázka zaznamená zásobníku dokončení volání vlákna, která je aktuálně spuštěné. Nabízí podrobnější a užitečné informace o daném vláknu byl činnosti, na vysoké úrovni i na nízké úrovni abstrakce. Služba profileru taky shromažďuje dalších událostí, sledovat aktivity korelace a příčiny, včetně kontext přepínání události, Task Parallel Library (TPL) události a události fondu vláken.

Zásobník volání, které se zobrazí v zobrazení časové osy je výsledkem vzorkování a instrumentace. Protože každá ukázka zaznamená zásobníku dokončení volání vlákna, obsahuje kód z rozhraní Microsoft .NET Framework a z dalších rozhraní, které odkazovat.

### <a id="jitnewobj"></a>Objekt přidělení (clr! JIT\_nový nebo clr! JIT\_Newarr1)
**CLR! JIT\_nový** a **clr! JIT\_Newarr1** jsou pomocných funkcí v rozhraní .NET Framework, které přidělit paměť z spravovaná halda. **CLR! JIT\_nový** je volána, když je přidělen objektu. **CLR! JIT\_Newarr1** je volána, když je přidělen pole objektu. Tyto dvě funkce obvykle jsou rychlé a proveďte relativně malé množství času. Pokud se zobrazí **clr! JIT\_nový** nebo **clr! JIT\_Newarr1** trvat významné množství času na časové ose, je jako ukazatel toho, že kód mohou být přidělování mnoho objektů a využívají poměrně velké množství paměti.

### <a id="theprestub"></a>Načítání kódu (clr! ThePreStub)
**CLR! ThePreStub** je pomocné funkce v rozhraní .NET Framework, který připraví kód při prvním spuštění. To obvykle zahrnuje, ale není omezeno na kompilace v běhu (JIT). Pro každou C# metodu **clr! ThePreStub** by měla být volána alespoň jednou po dobu životnosti procesu.

Pokud **clr! ThePreStub** trvá značně času pro žádost o to označuje, že požadavek je první, kterou provádí dané metody. Čas načtení dané metody modulu runtime rozhraní .NET Framework je důležité. Můžete zvážit použití zahřívání proces, který spouští před vaši uživatelé k němu přístup nebo zvažte spuštění generátor (ngen.exe) část kód ve vaší sestavení.

### <a id="lockcontention"></a>Spory uzamčení (clr! JITutil\_MonContention nebo clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** nebo **clr! JITutil\_MonEnterWorker** označuje, že je aktuální vlákno čekání na zámek k uvolnění. To obvykle objeví při provádění jazyka C# **ZÁMKU** při vyvolání příkazu **Monitor.Enter** metoda, nebo při vyvolání metody s **MethodImplOptions.Synchronized**atribut. Spory uzamčení obvykle dochází, když vláken A získá zámek a vlákno B se pokusí získat stejné zámek před uvolněním přístup z více vláken A.

### <a id="ngencold"></a>Načítání kódu ([STUDENÝ])
Pokud název metody obsahuje **[COLD]**, jako například **mscorlib.ni! [COLD]System.Reflection.CustomAttribute.IsDefined**, modul runtime rozhraní .NET Framework je provádění kódu poprvé není optimalizována podle <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">optimalizace na základě profilu</a>. Pro každou metodu měl by se zobrazit maximálně jednou po dobu životnosti procesu.

Pokud podstatnou část času pro žádost o načtení kód přijímá, to znamená, že požadavek je první z nich provést neoptimalizované část metody. Zvažte použití zahřívání proces, který provede část kódu, potom vaši uživatelé přístup.

### <a id="httpclientsend"></a>Odeslání požadavku HTTP
Metody jako **HttpClient.Send** znamenat, že kód čeká na dokončení požadavku HTTP.

### <a id="sqlcommand"></a>Operace databáze
Metody jako **SqlCommand.Execute** znamenat, že kód čeká na dokončení operace databáze.

### <a id="await"></a>Čeká se na (AWAIT\_čas)
**AWAIT\_čas** označuje, že kód čeká na dokončení jiné úlohy. To se obvykle stává pomocí jazyka C# **AWAIT** příkaz. Pokud nemá kód C# **AWAIT**, vlákno unwinds a vrátí prvek fond vláken a neexistuje žádný vlákno, které se zablokuje čekáním na **AWAIT** ukončíte. Však logicky, vlákno, nebyla **AWAIT** je "Uzavřeno" a čeká na dokončení operace. **AWAIT\_čas** příkaz označuje blokované dobu čekání na dokončení úlohy.

### <a id="block"></a>Blokované čas
**BLOCKED_TIME** označuje, že kód čeká na jiný prostředek k dispozici. Například může čekání na synchronizační objekt, vlákno být k dispozici nebo dokončení požadavku.

### <a id="cpu"></a>Čas procesoru
Procesor je zaneprázdněný prováděna podle pokynů.

### <a id="disk"></a>Doba disku
Aplikace provádí operace disku.

### <a id="network"></a>Čas sítě
Aplikace je provádění síťových operací.

### <a id="when"></a>Pokud sloupec
**Při** sloupec je vizualizaci jak včetně ukázky shromážděné pro uzel lišit v průběhu času. Celkový rozsah požadavku je rozdělené do 32 kbelíků čas. V těchto 32 kbelíků počítají včetně ukázky pro tento uzel. Každý blok je reprezentována jako pruh. Výška panelu reprezentuje hodnotu škálovat. Pro uzly označena **CPU_TIME** nebo **BLOCKED_TIME**, nebo tam, kde je zřejmé vztah o využívání prostředků (procesor, disk, vlákno), na panelu představuje jeden z těchto prostředků využívání po dobu čas této sady. Pro tyto metriky je možné získat hodnotu větší než 100 % spotřebovávat více prostředků. Například pokud použijete v průměru dva procesory během intervalu, získat 200 %.

## <a name="limitations"></a>Omezení

Uchovávání dat výchozí je pět dní. Maximální data požity za den je 10 GB.

Neexistují žádné poplatky za používání služby profileru. Používat službu profileru, musí být webové aplikace hostované v aspoň základní úroveň služby App Service.

## <a name="overhead-and-sampling-algorithm"></a>Režijní náklady a algoritmus vzorkování

Profileru náhodně spustí dvě minuty každou hodinu na každém virtuálním počítači, který je hostitelem aplikace, která profileru pro zaznamenání trasování povoleno. Pokud profileru běží, přidá 5 až 15 % zatížení procesoru na serveru.
Další servery, které jsou k dispozici pro hostování aplikace, menší dopad profileru má na výkon aplikací. Je to proto, že algoritmus vzorkování výsledkem profileru systémem jenom 5 % servery kdykoli. Další servery jsou k dispozici pro webové žádosti k posunutí nároky na serveru kvůli spuštěním profileru.

## <a name="disable-the-profiler"></a>Zakázat profileru
Pro zastavení nebo restartování profileru pro jednotlivé instance služby App Service v části **webové úlohy**, přejděte k prostředku služby App Service. Chcete-li odstranit profileru, přejděte na **rozšíření**.

![Zakázat profilovací nástroj pro webové úlohy][disable-profiler-webjob]

Doporučujeme, abyste měli profileru na vaší webové aplikace povolena a zjistit všechny problémy výkonu v míře.

Pokud používáte k nasazení změny k vaší webové aplikaci WebDeploy, ujistěte se, abyste vyloučili složce App_Data před odstraněním během nasazení. Jinak hodnota rozšíření profileru soubory se odstraní při příštím nasazení webové aplikace do Azure.


## <a id="troubleshooting"></a>Řešení potíží

### <a name="too-many-active-profiling-sessions"></a>Příliš mnoho aktivních relací profilace

V současné době můžete povolit profileru na maximálně čtyři webové aplikace Azure a nasazovací sloty, které jsou spuštěny v stejný plán, který služba. Pokud webovou úlohu profileru hlásí příliš mnoho aktivních relací profilování, přesunete některé webové aplikace do plánu jinou službu.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Jak zjistím, zda je spuštěn profileru Application Insights?

Profileru spouští jako nepřetržité webové úlohy ve webové aplikaci. Můžete otevřít prostředek webové aplikace v [portálu Azure](https://portal.azure.com). V **WebJobs** podokně, zkontrolujte stav **ApplicationInsightsProfiler**. Pokud není spuštěna, otevřete **protokoly** získat další informace.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Proč nelze najít žádné příklady zásobníku, i když je spuštěna profileru?

Zde je několik věcí, které můžete zkontrolovat:

* Ujistěte se, že váš plán služby app service web je úroveň Basic nebo vyšší.
* Ujistěte se, že vaše webová aplikace je Application Insights SDK 2.2 Beta nebo později povoleno.
* Ujistěte se, že má vaše webová aplikace **APPINSIGHTS_INSTRUMENTATIONKEY** nastavení nakonfigurované stejný klíč instrumentace, který je používán Application Insights SDK.
* Ujistěte se, že vaše webová aplikace běží v rozhraní .NET Framework 4.6.
* Pokud vaše webová aplikace je aplikace ASP.NET Core, zkontrolujte [požadované závislosti](#aspnetcore).

Po spuštění profileru není krátké zahřívání období, během které profileru aktivně shromažďuje několik trasování výkonu. Potom profileru shromažďuje trasování výkonu pro dvou minut za každou hodinu.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Použití profileru služby Azure. Co se stalo na ni?  

Když povolíte profileru Application Insights, agent profileru služba Azure je zakázán.

### <a id="double-counting"></a>Dvojitý počítání v paralelních vláken

V některých případech metrika celkový čas v prohlížeči. zásobník je větší než doba trvání požadavku.

To může dojít, když existují dvě nebo více podprocesů přidružený k požadavku, a jsou provozu paralelně. Celkový počet vláken čas v takovém případě je větší než uplynulý čas. Jedno vlákno může čeká na dalším dokončit. Prohlížeč pokusí to zjistit a vynechá nezajímavá čekání, ale jeho chybuje na straně zobrazující příliš mnoho místo vynechání, které mohou být důležitých informací.  

Až uvidíte paralelních vláken ve vašem trasování, určete, které vláken čekají, můžete určit kritické cesty pro požadavek. Ve většině případů je jednoduše čekání vlákno, které rychle přejde do stavu čekání na jiná vlákna. Soustředit se jenom na jiné vláken a ignorovat čas v vláken čekání.

### <a id="issue-loading-trace-in-viewer"></a>Žádná data profilování

Zde je několik věcí, které můžete zkontrolovat:

* Pokud data, která se pokoušíte zobrazení je starší než několik týdnů, pokuste se omezit váš filtr času a akci opakujte.
* Zkontrolujte, zda nebyly proxy nebo brána firewall zablokovala přístup k https://gateway.azureserviceprofiler.net.
* Zkontrolujte, že klíč instrumentace Application Insights, kterou používáte ve vaší aplikaci je stejný jako prostředek Application Insights, který jste použili k povoleno profilace. Klíč je obvykle v souboru ApplicationInsights.config, ale může být také umístěny v souborech web.config nebo app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Zprávy o chybách v profilaci prohlížeči.

Odešlete lístek podpory na portálu. Nezapomeňte zahrnout ID korelace z chybovou zprávu.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Chyba nasazení: adresář není prázdný ' D:\\domácí\\lokality\\wwwroot\\App_Data\\úloh

Pokud vaší webové aplikace na prostředek aplikace služby jsou opětovného nasazení profileru povoleno, mohou se zobrazit zpráva, která vypadá takto:

Adresář není prázdný ' D:\\domácí\\lokality\\wwwroot\\App_Data\\úloh

K této chybě dojde, je-li spustit nasazení webu z skripty nebo Visual Studio Team Services nasazení kanálu. Řešení je přidat následující parametry další nasazení do úlohy nasazení webu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Tyto parametry odstranit složku, která je používána profileru Application Insights a odblokování znovu ho zaveďte proces. Neovlivňují profileru instanci, která je aktuálně spuštěna.


## <a name="manual-installation"></a>Ruční instalace

Když konfigurujete profileru, jsou provedeny aktualizace nastavení webové aplikace. Aktualizace můžete použít ručně, pokud to vyžaduje prostředí. Pokud například vaše aplikace běží v App Service Environment pro PowerApps.

1. V podokně ovládacího prvku webové aplikace, otevřete **nastavení**.
2. Nastavit **rozhraní .net Framework verze** k **4.6**.
3. Nastavit **Always On** k **na**.
4. Přidat __APPINSIGHTS_INSTRUMENTATIONKEY__ aplikaci nastavení a nastavte hodnotu na stejný klíč instrumentace, který je používán sady SDK.
5. Otevřete **pokročilé nástroje**.
6. Vyberte **přejděte** otevřete web Kudu.
7. Na webu Kudu vyberte **lokality rozšíření**.
8. Nainstalujte __Application Insights__ z Galerie Azure webové aplikace.
9. Restartování webové aplikace.

## <a id="profileondemand"></a>Aktivovat ručně profileru
Když profileru Vyvinuli jsme přidali rozhraní příkazového řádku, abychom může otestovat profileru na aplikační služby. Pomocí těchto stejným uživatelům rozhraní můžete také upravit způsob spuštění profileru. Na vysoké úrovni profileru používá systém Kudu služby App Service spravovat profilace na pozadí. Při instalaci rozšíření Application Insights, vytvoříme nepřetržité webové úlohy, který hostuje profileru. Tato technologie používáme k vytvoření nové webové úlohy, kterou si můžete přizpůsobit svým potřebám.

Tato část vysvětluje, jak:

1.  Vytvoříte webovou úlohu, která můžete spustit profileru pro dvě minuty. stisknutím tlačítka.
2.  Vytvoříte webovou úlohu, která můžete naplánovat profileru ke spuštění.
3.  Nastavit argumenty profileru.


### <a name="set-up"></a>Nastavení
První umožňuje Seznamte se s řídicího panelu webové úlohy. V části nastavení klikněte na kartu webové úlohy.

![okno webové úlohy](./media/app-insights-profiler/webjobs-blade.png)

Jak vidíte, že tento řídicí panel jsou zobrazeny všechny webové úlohy, které jsou aktuálně nainstalovány na váš web. Můžete zobrazit ApplicationInsightsProfiler2 webovou úlohu, která je spuštěná úloha profileru. Toto je, kde jsme skončili vytváření naší nové webové úlohy pro ruční a naplánované profilace.

První Pojďme potřebujeme binární soubory.

1.  Přejděte na web kudu. V části vývoj klikněte na kartu Nástroje na kartě "Rozšířené nástroje" s logem Kudu. Klikněte na "Přejděte". Tím můžete přejít k novému webu a automaticky vás přihlásit.
2.  Další musíme binární soubory stahovány profileru. Přejděte do Průzkumníka souborů prostřednictvím konzoly Ladění -> CMD nachází v horní části stránky.
3.  Kliknutím na lokality -> wwwroot -> App_Data -> úlohy -> průběžné. Měli byste vidět do složky "ApplicationInsightsProfiler2". Klikněte na ikonu stahování nalevo od složce. To se stáhnout soubor "ApplicationInsightsProfiler2.zip".
4.  To stáhne všechny soubory, které budete potřebovat postoupíte. I doporučujeme vytvářet vyčistit adresář přesunout archivu zip do než budete pokračovat.

### <a name="setting-up-the-web-job-archive"></a>Nastavení webové úlohy archivu
Když přidáte novou webovou úlohu do webu azure v podstatě můžete vytvořit archivu zip s run.cmd uvnitř. Run.cmd říká systému webové úlohy, jak postupovat při spuštění webové úlohy. Existují další možnosti, které si můžete přečíst v dokumentaci k webové úlohy, ale pro naše účel jsme není nutné nic jiného.

1.  Spuštění vytvoření nové složky, I s názvem dolování "RunProfiler2Minutes".
2.  Zkopírujte soubory ve složce extrahované ApplicationInsightProfiler2 do této nové složky.
3.  Vytvořte nový soubor run.cmd. (Po otevření této pracovní složky v produktu vs code před zahájením pro usnadnění práce)
4.  Příkaz Přidat `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`a soubor uložte.
a.  `start` Příkaz zjistí profileru ke spuštění.
b.  `--engine-mode immediate`profileru informuje, že chceme okamžitě spustit profilování.
c.  `--single`způsob, jak spustit a pak ukončete automaticky d.  `--immediate-profiling-duration 120`znamená mít profileru spustit 120 sekund nebo 2 minut.
5.  Uložte tento soubor.
6.  Tuto složku archivovat, můžete klikněte pravým tlačítkem složku a zvolte odeslat do složky komprimované ->. Tím se vytvoří soubor .zip pomocí názvu složky.

![spuštění příkazu profileru](./media/app-insights-profiler/start-profiler-command.png)

Nyní je k dispozici ZIP webové úlohy, které používáme můžete nastavit webové úlohy na našem webu.

### <a name="add-a-new-web-job"></a>Přidejte novou webovou úlohu
Další přidáme nové webové úlohy na našem webu. Tento příklad vám ukáže, jak přidat ruční spouštěná webovou úlohu. Až budete moci provést proces je téměř úplně stejné pro naplánovanou. Další informace o naplánovaných úlohách spouštěná sami.

1.  Přejděte na řídicí panel webové úlohy.
2.  Klikněte na příkaz přidat na panelu nástrojů.
3.  Pojmenujte webovou úlohu, aby odpovídal názvu archivu pro přehlednost a otevřete až s různými verzemi run.cmd vybrali jste.
4.  V souboru uložit části formuláře klikněte na ikonu otevřít soubor a vyhledejte soubor .zip, který jste přidali výše.
5.  Pro typ zvolte aktivované.
6.  Zvolte ruční aktivačních událostí.
7.  Stiskněte tlačítko OK uložíte.

![spuštění příkazu profileru](./media/app-insights-profiler/create-webjob.png)

### <a name="run-the-profiler"></a>Spustit profileru

Teď, když máme nové webovou úlohu, která jsme můžete spouštět ručně jsme můžete zkusit spustit.

1.  Návrh může mít pouze jeden ApplicationInsightsProfiler.exe proces, který běží na počítači v daném okamžiku. Proto kvůli začít s Ujistěte se, že chcete zakázat nepřetržité webové úlohy z tohoto řídicího panelu. Klikněte na řádek a stiskněte klávesu "Stop". Obnovení na panelu nástrojů a potvrďte, že stav potvrdí, že je úloha zastavena.
2.  Klikněte na řádek s novou webovou úlohu, která jste přidali a stiskněte klávesu spustit.
3.  Řádek stále vybrané kliknutím na příkaz protokoly na panelu nástrojů tím přejdete na řídicí panel webové úlohy pro tuto webovou úlohu, kterou jste spustili. Se zobrazí seznam nejnovější spustí a jejich výsledek.
4.  Klikněte na spustit, kterou jste právě spuštěna.
5.  Pokud všechny se i byste měli vidět některé diagnostické protokoly pocházejících z profileru potvrzení, že jsme spustili profilace.

### <a name="things-to-consider"></a>Co je třeba zvážit

I když tato metoda je relativně jednoduché existují některé věci vzít v úvahu.

1.  Protože to není spravován nástrojem naši službu máme žádným způsobem aktualizace binárních souborů agenta pro webové úlohy. Jsme nyní není k dispozici na stránce stabilní stahování pro naše binární soubory tak, aby jediný způsob, jak získat všechny nejnovější aktualizace rozšíření a metodou ve složce průběžné jako jsme provedli výše.
2.  Jak je to využitím argumenty příkazového řádku, které byly původně navrženy s použít developer, nikoli pomocí koncového uživatele, tyto argumenty může změnit v budoucnu, takže jen znát, při upgradu. Protože můžete přidat webovou úlohu, spuštění a test, který funguje ho by neměl být mnohem na problém. Nakonec vytvoříme uživatelského rozhraní k tomu bez proces ručního nastavení, ale je něco vzít v úvahu.
3.  Funkce webové úlohy pro aplikační služby je jedinečný, v tom, že při spuštění webové úlohy zajišťuje, že má váš proces stejné proměnné prostředí a nastavení aplikace, které webový server se ukončí tím, že mají. To znamená, že není potřeba předat klíč instrumentace pomocí příkazového řádku profileru, ho měli právě vyzvedávat klíč instrumentace z prostředí. Ale pokud chcete spustit profileru na vaše dev pole nebo na počítač mimo App Services budete muset zadat kód instrumentace. Můžete k tomu předáním v argumentu `--ikey <instrumentation-key>`. Všimněte si, že tato hodnota musí odpovídat klíč instrumentace, který vaše aplikace používá. Ve výstupu protokolu profileru jsou profilace oznámí, které ikey profileru začít s a pokud jsme zjistili aktivitu z tohoto klíč instrumentace při jsme.
4.  Ručně aktivované webové úlohy můžete spustit ve skutečnosti prostřednictvím háku Web. Tuto adresu url můžete získat z pravým tlačítkem myši na webovou úlohu z řídicího panelu a zobrazení vlastností nebo výběr vlastnosti na panelu nástrojů po výběru webovou úlohu z tabulky. Je celá řada články, které můžete najít online o tom, nebude přejít do množství podrobností o něm, ale to otevře možnost spuštění profileru z vašeho kanálu CI/CD (např. služby VSTS) nebo něco podobného jako Microsoft Flow (https://flow.microsoft.com/en-us/). V závislosti na tom, jak zvláštní chcete nastavit vaše run.cmd, kterým tím, jak může být run.ps1, jsou rozsáhlé možnosti.  

## <a name="next-steps"></a>Další postup

* [Práce s Application Insights v sadě Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
