---
title: "Profilace aktivních webových aplikací v Azure s Application Insights | Microsoft Docs"
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
ms.openlocfilehash: f6669d90878398dcd4592df97180dcd59b146350
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilace za provozu Azure web apps s Application Insights

*Tato funkce Application Insights je GA pro aplikační služby a ve verzi preview pro výpočet.*

Zjistěte, jak dlouho je strávil v jednotlivých metod v za provozu webové aplikace pomocí nástroje pro profilaci z [Azure Application Insights](app-insights-overview.md). Ukazuje podrobné profily živé požadavky, které se spouští vaše aplikace a klade důraz 'aktivní cesta' používající nejvíce času. Automaticky vybere příklady, které mají různé doby odezvy. Profileru využívá různé postupy pro minimalizaci režie.

Profileru aktuálně funguje pro ASP.NET webové aplikace běžící v Azure App Services, nejméně Basic cenová úroveň.

<a id="installation"></a>
## <a name="enable-the-profiler"></a>Povolit profileru

[Nainstalujte službu Application Insights](app-insights-asp-net.md) ve vašem kódu. Pokud je již nainstalována, ujistěte se, že máte nejnovější verzi. (K tomu, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte spravovat balíčky NuGet balíčky. Vyberte aktualizaci a aktualizovat všechny balíčky.) Znovu nasazení vaší aplikace.

*Pomocí ASP.NET Core? [Zkontrolujte, zde](#aspnetcore).*

V [https://portal.azure.com](https://portal.azure.com), otevřete prostředek Application Insights pro webové aplikace. Otevřete **výkonu** a klikněte na tlačítko **povolit Application Insights profileru...** .

![Klikněte na Banner informující o povolení profileru][enable-profiler-banner]

Alternativně můžete vždy kliknout na **konfigurace** Pokud chcete zobrazit stav, povolit nebo zakázat profileru.

![V okně výkonu kliknutím na tlačítko Konfigurovat][performance-blade]

V okně konfigurace jsou uvedeny webových aplikací, které jsou nakonfigurovány s Application Insights. Postupujte podle pokynů pro instalaci agenta profileru v případě potřeby. Pokud žádná webová aplikace je nakonfigurovaný s Application Insights ještě, klikněte na tlačítko *přidat propojené aplikace*.

Použití *povolit profileru* nebo *zakázat profileru* tlačítka v okně Konfigurovat a řídit profileru na všechny propojené webové aplikace.

![Konfigurace okna][linked app services]

Na rozdíl od webové aplikace hostované prostřednictvím plány služby App Service, aplikace hostované v *Azure Compute* prostředky (například: virtuálního počítače, sadu škálování virtuálního počítače, Service Fabric, cloudové služby) nejsou přímo spravuje Azure. V takovém případě neexistuje žádná webová aplikace propojit s sem a stačí kliknutím povolíte profileru na obrazovce.

## <a name="disable-the-profiler"></a>Zakázat profileru
Pro zastavení nebo restartování profileru pro jednotlivé instance služby App Service, naleznete je **v prostředek služby App Service**v **webové úlohy**. Odstranit, podívejte se do části **rozšíření**.

![Zakázat profilovací nástroj pro webové úlohy][disable-profiler-webjob]

Doporučujeme, abyste měli profileru na vaší webové aplikace povolena a co nejdříve zjistit jakékoli problémy s výkonem.

Pokud používáte k nasazení změny k vaší webové aplikaci WebDeploy, ujistěte se, abyste vyloučili **App_Data** složku před odstraněním během nasazení. Jinak, rozšíření profileru soubory se odstraní při další nasazení webové aplikace do Azure.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Pomocí profileru s virtuálními počítači Azure a výpočetní prostředky (preview)

Pokud jste [povolit Application Insights pro aplikace Azure services v době běhu](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), profileru je automaticky dostupný. (Pokud jste povolili pro prostředek Application Insights, může je potřeba aktualizovat na nejnovější verzi pomocí **konfigurace** průvodce.)

Je [verze preview profileru pro Azure výpočetní prostředky](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limitations"></a>Omezení

Uchovávání dat výchozí je 5 dní. Maximální 10 GB požity za den.

Není nijak zpoplatněn pro službu profileru. Webové aplikace musí být hostované v aspoň základní úroveň aplikační služby.

## <a name="overhead-and-sampling-algorithm"></a>Režijní náklady a algoritmus vzorkování

2 minuty každou hodinu profileru náhodně spustí na každém virtuálním počítači, který je hostitelem aplikace s profileru zaznamenat trasování povoleno. Po spuštění profileru přidá mezi 5-15 % zatížení procesoru na server.
Další servery pro hostování aplikace menší dopad, který profileru má na výkon celkové aplikace k dispozici. To je vzhledem k tomu, že algoritmus vzorkování výsledkem profileru systémem spustit pouze na 5 % serverů v každém okamžiku a další servery bude k dispozici pro webové žádosti k posunutí servery s režii z profileru.


## <a name="viewing-profiler-data"></a>Zobrazení dat profileru

Otevřete okno výkon a posuňte se dolů seznam operací.




![Okna Statistika výkonu aplikací příklady sloupce][performance-blade-examples]

Sloupce v tabulce jsou:

* **Počet** -počet tyto požadavky v časové rozmezí okna.
* **Medián** – Typická doba aplikace trvá odpovědět na požadavek. Polovinu všechny odpovědi byly rychlejší než to.
* **95. percentil** 95 % odpovědí byly rychlejší než to. Pokud na tomto obrázku je příliš neliší od medián, může být občasný problém s vaší aplikací. (Nebo může být vysvětlené návrh funkce jako je například ukládání do mezipaměti.)
* **Trasování profileru** – ikona označuje, že má profileru zachytit trasování zásobníku pro tuto operaci.

Klikněte na tlačítko zobrazení otevřete Průzkumníka trasování. Průzkumníku ukazuje několik ukázky, že má zachycení profileru klasifikované podle doby odezvy.

Pokud používáte okno výkonu Preview, přejděte na **provést akce** části v pravém dolním rohu zobrazení profileru trasování. Klikněte na tlačítko profileru trasování.

![Aplikace Statistika výkonu okno náhledu trasování profileru][performance-blade-v2-examples]

Vyberte ukázku zobrazit rozpis úrovni kódu čas provádění požadavku.

![Application Insights trasování Explorer][trace-explorer]

**Zobrazit aktivní trase** otevře biggest list uzlu, nebo aspoň něco zavřete. Ve většině případů bude u kritických bodů výkonu tento uzel.



* **Popisek**: název funkce nebo událostí. Strom zobrazuje směs kód a události, které došlo k chybě (například události SQL a http). Horní událost představuje celkovou dobu trvání požadavku.
* **Uplynulý čas**: časový interval mezi začátkem operace a end.
* **Když**: zobrazí, když byl spuštěn funkce nebo události v vztah do dalších funkcí.

## <a name="how-to-read-performance-data"></a>Informace o načtení dat výkonu

Profileru služby Microsoft používá kombinaci metody vzorkování a instrumentace analyzovat výkon vaší aplikace.
Když probíhá podrobné kolekce, ukázky služby profileru ukazatel instrukce každého z procesoru počítače v každé milisekundu.
Každá ukázka zaznamená zásobníku dokončení volání vlákna aktuálně provádění, udělíte podrobnější a užitečné informace o tom, co vlákno se to, že na úrovni i maximální a minimální úrovně abstrakce. Služba profileru shromažďuje také další události, jako je například přepínání události kontextu, TPL události a události fondu vláken a sledovat aktivity korelace a příčiny.

V zobrazení časové osy zásobníku volání je výsledkem výše uvedeného vzorkování a instrumentace. Protože každá ukázka zaznamená zásobníku dokončení volání vlákna, obsahuje kód z rozhraní .NET framework, stejně jako ostatní platformy, na něž odkazuje.

### <a id="jitnewobj"></a>Objekt přidělení (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`jsou pomocné funkce v rozhraní .NET framework, která se přidělí paměť z spravovaná halda. `clr!JIT\_New`je voláno, když je přidělen objektu. `clr!JIT\_Newarr1`je voláno, když je přidělen pole objektu. Tyto dvě funkce jsou obvykle velmi rychlé a provést relativně malé množství času. Pokud se zobrazí `clr!JIT\_New` nebo `clr!JIT\_Newarr1` trvat významné množství času na časové ose, je jako ukazatel toho, že kód může přidělování mnoho objektů a spotřebovávat značné množství paměti.

### <a id="theprestub"></a>Načítání kódu (`clr!ThePreStub`)
`clr!ThePreStub`je pomocné funkce v rozhraní .NET framework, který připraví kód při prvním spuštění. To obvykle zahrnuje, ale bez omezení (pouze v době) JIT – kompilace. Pro každou C# metodu `clr!ThePreStub` by měla být volána alespoň jednou po dobu životnosti procesu.

Pokud se zobrazí `clr!ThePreStub` trvá značné množství času pro žádost o indikuje, je žádost o první, kterou provádí dané metody a čas pro rozhraní .NET framework runtime k načtení této metody je důležité. Můžete zvážit warm-up proces, který provede část kódu, než uživatelé k němu přístup nebo zvažte spuštění NGen vaše sestavení.

### <a id="lockcontention"></a>Zamknout kolizí (`clr!JITutil\_MonContention` nebo `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`nebo `clr!JITutil\_MonEnterWorker` označuje aktuální vlákno čeká na zámek k uvolnění. To obvykle objeví při provádění jazyka C# zámku výpis, volání metody Monitor.Enter nebo volání metody s atributem MethodImplOptions.Synchronized. Spory uzamčení obvykle dojde, když vláken A získá zámek a vlákno B se pokusí získat stejné zámek před uvolněním přístup z více vláken A.

### <a id="ngencold"></a>Načítání kódu (`[COLD]`)
Pokud obsahuje název metody `[COLD]`, jako například `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, znamená to, že modul runtime rozhraní .NET framework je prováděna kód, který není optimalizována podle <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">optimalizace na základě profilu</a> poprvé. Pro každou metodu měl by se zobrazit maximálně jednou po dobu životnosti procesu.

Načítání kódu trvá dlouhou dobu pro požadavek, označuje tento požadavek je první z nich provést neoptimalizované část metody. Můžete zvážit záložním až proces, který provede část kódu, potom vaši uživatelé přístup.

### <a id="httpclientsend"></a>Odeslání požadavku HTTP
Metody, jako `HttpClient.Send` znamenat kód čeká na dokončení žádosti HTTP.

### <a id="sqlcommand"></a>Operace databáze
Metoda například SqlCommand.Execute označuje, že kód čeká na dokončení operace databáze.

### <a id="await"></a>Čeká se na (`AWAIT\_TIME`)
`AWAIT\_TIME`Označuje, že kód čeká na dokončení jiné úlohy. K tomu obvykle dochází, pomocí C#, await, příkaz. Pokud nemá kód C#, await, vlákno unwinds a vrátí prvek do fondu vláken a neexistuje žádné vlákno, které se zablokuje čekáním na 'await, k dokončení. Však logicky vlákno, které se await, blokováno, čekání na dokončení operace. `AWAIT\_TIME` Označuje blokované dobu čekání na dokončení úlohy.

### <a id="block"></a>Blokované čas
`BLOCKED_TIME`Označuje, že kód čeká jiný prostředek k dispozici, jako je například čekání na synchronizační objekt čekání na vlákno být k dispozici, nebo při čekání dokončení požadavku.

### <a id="cpu"></a>Čas procesoru
Procesor je zaneprázdněný prováděna podle pokynů.

### <a id="disk"></a>Doba disku
Aplikace provádí operace disku.

### <a id="network"></a>Čas sítě
Aplikace je provádění síťových operací.

### <a id="when"></a>Pokud sloupec
Toto je vizualizaci jak včetně ukázky shromážděné pro uzel lišit v průběhu času. Celkový rozsah požadavku je rozdělené do 32 kbelíků čas a včetně ukázky pro tento uzel počítají do těchto 32 intervalů. Každý blok je pak reprezentován jako panel, jehož výška reprezentuje hodnotu škálovat. Pro uzly označena `CPU_TIME` nebo `BLOCKED_TIME`, nebo tam, kde je zřejmé vztah o využívání prostředků (procesor, disk, vlákno), na panelu představuje jeden z těchto prostředků využívání dobu z této sady. Pro tyto metriky můžete získat větší než 100 % podle spotřebovávat více prostředků. Například pokud se v průměru v intervalu, použijte dva procesory, můžete získat 200 %.


## <a id="troubleshooting"></a>Řešení potíží

### <a name="too-many-active-profiling-sessions"></a>Příliš mnoho aktivních relací profilace

Nyní můžete povolit profileru na maximálně 4 Azure Web Apps a nasazovací sloty na stejný plán, který služba spuštěna. Pokud se zobrazí profileru webové úlohy vytváření sestav příliš mnoho aktivních relací profilování, potřebujete přesunout některé webové aplikace do plánu jinou službu.

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Jak poznám, jestli je spuštěný profileru Application Insights?

Profileru spouští jako nepřetržité webové úlohy ve webové aplikaci. Můžete otevřít prostředek webové aplikace v https://portal.azure.com a zkontrolujte stav "ApplicationInsightsProfiler" v okně webové úlohy. Pokud není spuštěna, otevřete **protokoly** Další.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Proč nelze najít žádné příklady zásobníku, i když je spuštěna profileru?

Tady jsou můžete zkontrolovat pár věcí.

1. Zkontrolujte, že váš Web plán služby App Service je úroveň Basic a vyšší.
2. Zajistěte, aby vaše webová aplikace má Application Insights SDK 2.2 Beta a vyšší než povolené.
3. Ověřte, že vaše webová aplikace má nastavení APPINSIGHTS_INSTRUMENTATIONKEY se stejným klíčem instrumentace používané Application Insights SDK.
4. Zajistěte, aby vaše webová aplikace běží na rozhraní .net Framework 4.6.
5. Pokud je aplikace ASP.NET Core, zkontrolujte také, zda [požadované závislosti](#aspnetcore).

Po spuštění profileru není krátké warm-up období, kdy profileru aktivně shromažďuje několik trasování výkonu. Potom profileru shromažďuje trasování výkonu pro dvou minut za každou hodinu.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Použití profileru služby Azure. Co se stalo na ni?  

Když povolíte profileru Application Insights, profileru služby Azure agenta je zakázána.

### <a id="double-counting"></a>Dvojitý počítání v paralelních vláken

V některých případech metrika celkový čas v prohlížeči. zásobník je větší než skutečná délka požadavku.

Tomu může dojít, pokud jsou dostupné dvě nebo více vláken, které jsou přidružené k požadavku operační paralelně. Celkový počet vláken čas je pak větší než uplynulý čas. V mnoha případech může být jedno vlákno čekání na další k dokončení. V prohlížeči se pokusí to zjistit a vynechejte nezajímavá čekání, ale chybuje stranu zobrazující příliš mnoho místo vynechání co může být důležitých informací.  

Až uvidíte paralelních vláken ve vašem trasování, budete muset určit, které vláken čekají, můžete určit kritické cesty pro požadavek. Ve většině případů je jednoduše čekání vlákno, které rychle přejde do stavu čekání na jiná vlákna. Soustředit se jenom na jiné a ignorovat čas v vláken čekání.

### <a id="issue-loading-trace-in-viewer"></a>Žádná data profilování

1. Pokud data, která se pokoušíte zobrazení je starší než několik týdnů, pokuste se omezit váš filtr času a akci opakujte.

2. Zkontrolujte, zda nebyly proxy nebo brána firewall zablokovala přístup k https://gateway.azureserviceprofiler.net.

3. Zkontrolujte, jestli klíč instrumentace Application Insights, kterou používáte ve vaší aplikaci je stejný jako prostředek Application Insights, které jste povolili profilace s. Klíč je obvykle v souboru ApplicationInsights.config, ale můžete také najít v souboru web.config nebo app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Zprávy o chybách v profilaci prohlížeči.

Soubor lístek podpory z portálu. Uveďte ID korelace z chybovou zprávu.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Chyba nasazení adresář není prázdný ' D:\\domácí\\lokality\\wwwroot\\App_Data\\úloh

Pokud vaší webové aplikace na prostředek aplikace služby jsou opětovného nasazení profileru povolené, může dojít podobná následující chyba: adresář není prázdný ' D:\\domácí\\lokality\\wwwroot\\App_Data\\úloh této chybě dojde, pokud spusťte nasazení webu ze skriptů nebo na služby VSTS nasazení kanálu.
Řešení tohoto problému je následující parametry další nasazení přidat úloha nasazení webu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Tato akce odstraní složku používá profileru Statistika aplikace a odblokování znovu ho zaveďte proces. Aktuálně spuštěné instance profileru neovlivní.


## <a name="manual-installation"></a>Ruční instalace

Když konfigurujete profileru, jsou provedeny následující aktualizace nastavení webové aplikace. Můžete je provést sami ručně, pokud vaše prostředí vyžadovat, například pokud vaše aplikace běží v Azure App Service prostředí (App Service Environment):

1. V okně webové aplikace řízení otevřete nastavení.
2. Nastavit "rozhraní .net Framework, verze" k 4.6.
3. Nastavte "Always On" na.
4. Přidání nastavení aplikace "__APPINSIGHTS_INSTRUMENTATIONKEY__" a nastavte hodnotu na stejný klíč instrumentace používá SDK.
5. Otevřete pokročilé nástroje.
6. Klepněte na tlačítko "Vyhledat" otevřete web Kudu.
7. V modulu Kudu web vyberte "Rozšíření lokality".
8. Nainstalujte "__Application Insights__" z galerie.
9. Restartování webové aplikace.

## <a id="aspnetcore"></a>Podpora jádra ASP.NET

Aplikace ASP.NET Core potřebuje k instalaci 2.1.0-beta6 balíček Microsoft.ApplicationInsights.AspNetCore Nuget nebo vyšší pro práci s profileru. Po 27/6/2017 už podporujeme nižší verze.

## <a name="next-steps"></a>Další kroky

* [Práce s Application Insights v sadě Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

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
