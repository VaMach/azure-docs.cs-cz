---
title: "Osvědčené postupy a Průvodci odstraňováním potíží aplikace uzlu ve webových aplikacích Azure"
description: "Zjistěte, osvědčené postupy a řešení potíží pro uzlu aplikace v Azure Web Apps."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 8f39b5e6faf5f9121ec2abe347f50653c5c3e4f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Osvědčené postupy a Průvodci odstraňováním potíží aplikace uzlu ve webových aplikacích Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto článku se dozvíte, osvědčené postupy a řešení potíží pro [uzel aplikace](app-service-web-get-started-nodejs.md) systémem Azure Webapps (s [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Buďte opatrní při používání při řešení potíží na vašem provozním serveru. Doporučuje se řešení problémů aplikace na instalační program mimo produkční například přípravný slot a pokud je problém vyřešen, Prohodit vaší přípravný slot s produkční slot.
> 
> 

## <a name="iisnode-configuration"></a>Konfigurace modulu IISNODE
To [soubor schématu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ukazuje všechna nastavení, které mohou být konfigurovány pro modulu iisnode. Některá nastavení, která bude užitečná pro vaše aplikace jsou:

* nodeProcessCountPerApplication
  
    Toto nastavení určuje počet uzlu procesy, které jsou spouštěny na aplikaci služby IIS. Výchozí hodnota je 1. Pomocí tohoto nastavení můžete spustit jako v mnoha node.exe jako vaše počet jader virtuálního počítače na hodnotu 0. Doporučená hodnota je 0 pro většinu aplikací, takže můžete využít všechny jádra na váš počítač. Node.exe je zřetězený, jeden node.exe spotřebuje maximálně 1 jádra a získat maximální výkon mimo aplikaci uzlu byste chtěli využívat všechna jádra.
* nodeProcessCommandLine
  
    Toto nastavení určuje cestu ke node.exe. Můžete nastavit tuto hodnotu tak, aby odkazoval na vaší verzi node.exe.
* maxConcurrentRequestsPerProcess
  
    Toto nastavení určuje maximální počet souběžných požadavků, které posílá každý node.exe modulu iisnode. Na azure webapps je výchozí hodnota pro tento nekonečné. Nebudete mít na starosti toto nastavení. Mimo azure webapps výchozí hodnota je 1024. Můžete nakonfigurovat tuto funkci v závislosti na tom, kolik požadavky, že aplikace získá a jak rychle vaše aplikace zpracovává každý požadavek.
* maxNamedPipeConnectionRetry
  
    Toto nastavení určuje maximální počet časy modulu iisnode bude opakovat vytváření připojení v pojmenovaném kanálu pro odeslání požadavku na node.exe. Toto nastavení v kombinaci s namedPipeConnectionRetryDelay Určuje celkový časový limit každého požadavku v rámci modulu iisnode. Výchozí hodnota je 200 na Azure Webapps. Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Určuje toto nastavení množství času (v ms) iisnode bude čekat mezi každou opakovat, pokud chcete odeslat požadavek na node.exe přes pojmenovaný kanál. Výchozí hodnota je 250ms.
    Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Ve výchozím nastavení je celkový časový limit v modulu iisnode na azure webapps 200 \* 250ms = 50 sekund.
* logDirectory
  
    Toto nastavení určuje adresář, kde bude modulu iisnode protokolu stdout/stderr. Výchozí hodnota je modulu iisnode, která je relativní vzhledem k adresáři hlavního skriptu (adresáře, kde je hlavní server.js existuje)
* debuggerExtensionDll
  
    Toto nastavení určuje, jaká verze modulu iisnode nástroj node-inspector se bude používat při ladění aplikace uzlu. Aktuálně modulu iisnode. inspector 0.7.3.dll a iisnode inspector.dll jsou platné pouze 2 hodnoty pro toto nastavení. Výchozí hodnota je modulu iisnode. inspector 0.7.3.dll. verze modulu iisnode. inspector 0.7.3.dll používá uzlu inspector 0.7.3 a používá, takže budete muset povolit objekty websockets na vaše azure webapp používat tuto verzi. V tématu <http://www.ranjithr.com/?p=98> další podrobnosti o tom, jak nakonfigurovat modulu iisnode používat nový nástroj node-inspector.
* flushResponse
  
    Výchozí chování služby IIS je, že ukládány data odpovědi až 4 MB před vyčištění, nebo až do konce odpověď, nastane dříve. iisnode nabízí konfigurační nastavení pro toto chování potlačit: Chcete-li vyprázdnit fragment textu entity odpovědi na co nejdříve po přijetí z node.exe modulu iisnode, je potřeba nastavit iisnode/@flushResponse atribut v souboru web.config na hodnotu true.:
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Povolení vyprázdnění každý fragment textu entity odpovědi zvýší nároky na výkon, sníží propustnost systému % ~ 5 (od v0.1.13), takže je vhodné k určení rozsahu toto nastavení pouze pro koncové body, které vyžadují vysílání datového proudu odpovědi (např. použití <location> element v souboru web.config)
  
    Kromě toho pro streamování aplikací, musíte také responseBufferLimit vaší obslužné rutiny modulu iisnode nastaven na hodnotu 0.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Toto je seznam souborů, které budou sledovány změny oddělených středníkem. Ke změně souboru způsobí, že aplikace recyklace. Každý záznam se skládá z název volitelné adresáře plus název požadované souboru, které jsou relativní vzhledem k adresáři, kde se nachází vstupní bod hlavní aplikace. Zástupné znaky nejsou povoleny v pouze část názvu souboru. Výchozí hodnota je "\*. js;web.config"
* recycleSignalEnabled
  
    Výchozí hodnota je false. Pokud je povoleno, aplikace uzlu můžete připojit k pojmenovanému kanálu (proměnnou prostředí modulu IISNODE\_řízení\_kanálu) a odeslat zprávu "recyklaci". To způsobí, že w3wp řádně recyklace.
* idlePageOutTimePeriod
  
    Výchozí hodnota je 0, což znamená, že tato funkce je vypnutá. Pokud nastavíte na hodnotu větší než 0, modulu iisnode bude stránka se všechny jeho podřízené procesy každých milisekund 'idlePageOutTimePeriod'. Chcete-li pochopit, co stránky se znamená, získáte informace k tomuto [dokumentaci](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Toto nastavení bude užitečná pro aplikace, které využívat velké množství paměti a chcete pageout paměti na disk příležitostně uvolněte některé paměti RAM.

> [!WARNING]
> Buďte opatrní při povolování následující nastavení konfigurace na výrobní aplikace. Doporučuje se není je povolit v za provozu výrobní aplikace.
> 
> 

* debugHeaderEnabled
  
    Výchozí hodnota je false. Pokud je nastaven na hodnotu true, modulu iisnode přidá HTTP odpovědi hlavičku modulu iisnode-debug do každé odpovědi HTTP odešle, že hodnota hlavičky ladění modulu iisnode je adresa URL. Jednotlivé diagnostické informace může shromažďovat informace prohlížením fragment adresy URL, ale mnohem lepší vizualizace se dosahuje otevření této adresy URL v prohlížeči.
* loggingEnabled
  
    Toto nastavení určuje protokolování stdout a stderr pomocí modulu iisnode. Iisnode zachytíte stdout/stderr z uzlu procesy, které ho spouští a zapisovat do adresáře zadali v nastavení 'logDirectory'. Po povolení, vaše aplikace bude psát protokoly do systému souborů a v závislosti na množství protokolování, provádí aplikace, může být ovlivnit výkon.
* devErrorsEnabled
  
    Výchozí hodnota je false. Když nastaven na hodnotu true, modulu iisnode zobrazí stavový kód HTTP a kód chyby Win32 prohlížeč. Kód win32 bude užitečné při ladění určité typy potíží.
* debuggingEnabled (nepovolujte na provozním serveru)
  
    Toto nastavení řídí funkce ladění. Pomocí nástroje node-inspector je integrovaná modulu Iisnode. Povolením tohoto nastavení můžete povolit ladění aplikace uzlu. Jakmile je toto nastavení povoleno, bude modulu iisnode rozložení nástroj nezbytné node-inspector soubory v adresáři 'debuggerVirtualDir' na první požadavek ladění na aplikaci uzlu. Odesílání požadavku do http://yoursite/server.js/debug, můžete načíst nástroj node-inspector. Segment adresy URL ladění můžete ovládat nastavení 'debuggerPathSegment'. Ve výchozím nastavení debuggerPathSegment = "debug". Je tento parametr můžete nastavit na identifikátor GUID, například tak, aby je obtížnější být zjištěny jinými uživateli.
  
    Zaškrtněte toto políčko [odkaz](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) Další informace o ladění.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scénáře a doporučení nebo řešení potíží
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moje aplikace uzlu je volání příliš mnoho odchozí.
Mnoho aplikací chtít provést v rámci jejich regulární operace odchozí připojení. Například když přijde žádost, aplikace uzlu vhodnější kontaktovat rozhraní REST API jinde a určité informace pro zpracování požadavku. Chcete by se použít zachování připojení agenta zachovat při volání protokolu http nebo https. Například můžete použít modul agentkeepalive jako aktivní agenta zachovat při provádění těchto odchozí volání. Tím je zajištěno, že jsou sockets opakovaně na vaší webové aplikace azure virtuálních počítačů a snižuje zátěž vytváření nových soketů pro každý požadavek odchozí. Navíc díky tomu se, že používáte menší počet sockets provádět mnoho odchozí požadavky, a proto nepřekračují maxSockets, které jsou přiděleny na virtuální počítač. Doporučení na Azure Webapps by k nastavení hodnoty maxSockets agentKeepAlive na celkem 160 sockets na virtuální počítač. To znamená, že pokud máte 4 node.exe spuštěna na virtuálním počítači, by se nastavit agentKeepAlive maxSockets na 40 za node.exe což je 160 celkový na virtuální počítač.

Příklad [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfigurace:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Tento příklad předpokládá, že máte 4 node.exe systémem virtuálního počítače. Pokud máte jiný počet node.exe spuštěna na virtuálním počítači, je nutné upravit maxSockets nastavení odpovídajícím způsobem.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moje uzel aplikace využívala příliš mnoho procesoru.
Doporučení se zobrazí na portálu o vysoké spotřeby procesoru pravděpodobně z Azure Webapps. Můžete také nastavit monitorování si chcete přehrát určité [metriky](web-sites-monitor.md). Při kontrole využití procesoru na [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), Zkontrolujte prosím maximální hodnoty pro procesor, nemusíte vynechalo hodnoty ve špičce.
V případech, kde by vaše aplikace využívala příliš mnoho procesorů a důvod, proč nelze vysvětlují musíte do profilu aplikace uzlu.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profilace uzlu aplikace v azure webapps s v8: profileru
Například vyslovte vám umožní mít aplikace hello world, které chcete profil, jak je uvedeno níže:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Přejděte do vaší lokality https://yoursite.scm.azurewebsites.net/DebugConsole scm

Zobrazí se příkazový řádek a jak je uvedeno níže. Přejděte do adresáře webu/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Spusťte příkaz "npm install profileru v8:"

To by měli nainstalovat v8: profileru pod uzlem\_directory moduly a všechny jeho závislé součásti.
Nyní upravte vaše server.js do profilu aplikace.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Výše uvedené změny bude profil funkce WriteConsoleLog a zapište si profil výstup do souboru 'profile.cpuprofile' v rámci vaší lokality wwwroot. Odeslat požadavek do vaší aplikace. Zobrazí se soubor 'profile.cpuprofile' vytvořil v rámci vaší lokality wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Tento soubor stáhnout a budete muset otevřít tento soubor s nástroji F12 Chrome. Stiskněte tlačítko F12 na chrome, a potom klikněte na kartu"profily". Klikněte na tlačítko "Zatížení". Vyberte souboru profile.cpuprofile, který jste právě stáhli. Klikněte na profil, který jste právě načetli.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zobrazí se, že 95 % času se spotřebovávají WriteConsoleLog funkce jak je uvedeno níže. To také ukazuje linek čísla a zdrojové soubory, které způsobí potíže.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moje uzel aplikace využívala příliš mnoho paměti.
Pravděpodobně obdržíte doporučení z Azure Webapps na portálu o vysoké spotřeby paměti. Můžete také nastavit monitorování si chcete přehrát určité [metriky](web-sites-monitor.md). Při kontrole využití paměti na [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), takže nemáte vynechalo hodnoty ve špičce, zkontrolujte maximální hodnoty paměti.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Zjištění paměti a Diffing haldy pro node.js
Můžete použít [uzlu memwatch](https://github.com/lloyd/node-memwatch) usnadňující identifikaci paměti nevracení.
Můžete nainstalovat memwatch stejně jako v8: profileru a upravovat kód k zachycení a rozdílové haldách k identifikaci paměť nevracení ve vaší aplikaci.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe jsou získávání ukončeny náhodně
Existují z několika důvodů, proč může být děje toto:

1. Aplikace je vyvolání nezachycená výjimek – d: Zkontrolujte prosím\\domácí\\LogFiles\\aplikace\\protokolování errors.txt souboru podrobnosti v došlo k výjimce. Tento soubor má trasování zásobníku, takže můžete řešit vaší aplikace na základě.
2. Vaše aplikace využívala příliš mnoho paměti, která ovlivňuje jiné procesy z Začínáme. Pokud celková velikost paměti virtuálního počítače je téměř 100 %, může vaše node.exe ukončeny správcem proces umožníte jiné procesy ještě nějakou práci. Chcete-li odstranit tento problém, ujistěte se, že vaše aplikace není vracena paměť nebo pokud je aplikace opravdu potřebuje používat velké množství paměti, prosím vertikálně navýšit kapacitu na větší virtuální počítač s mnohem víc paměti RAM.

### <a name="my-node-application-does-not-start"></a>Moje aplikace uzlu nespustí.
Pokud vaše aplikace vrací 500 chyby při spuštění, může být z několika důvodů:

1. Node.exe se nenachází ve správném umístění. Zkontrolujte nastavení nodeProcessCommandLine.
2. Soubor skriptu hlavní se nenachází ve správném umístění. Zkontrolujte soubor web.config a ujistěte se, že název souboru hlavní skriptu v oddílu obslužných rutin odpovídá souboru hlavní skriptu.
3. Konfigurace souboru Web.config není správný – zkontrolujte nastavení názvy nebo hodnoty.
4. Studený Start – aplikace trvá příliš dlouho na spuštění. Pokud vaše aplikace trvá déle, než (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekund modulu iisnode vrátí chybu 500. Zvýšení hodnoty těchto nastavení tak, aby odpovídaly spuštění vaší aplikace aby modulu iisnode z vypršení časového limitu a vrácení za následek chybu 500.

### <a name="my-node-application-crashed"></a>Moje aplikace uzlu došlo k chybě
Aplikace je vyvolání nezachycená výjimek – d: Zkontrolujte prosím\\domácí\\LogFiles\\aplikace\\protokolování errors.txt souboru podrobnosti v došlo k výjimce. Tento soubor má trasování zásobníku, takže můžete řešit vaší aplikace na základě.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Moje aplikace uzlu trvá příliš mnoho času spuštění (studený Start)
Nejčastější příčinou je, že aplikace má mnoho souborů v uzlu\_moduly a aplikace se pokusí načíst většinu těchto souborů při spuštění. Ve výchozím nastavení protože vaše soubory jsou umístěny ve sdílené síťové složce v Azure Webapps, načítání mnoho souborů může chvíli trvat.
Některá řešení se rychlejší jsou:

1. Zkontrolujte, zda že máte struktura ploché závislostí a žádné duplicitní závislosti pomocí npm3 nainstalovat moduly.
2. Pokuste se opožděné načíst vaše uzlu\_moduly a zatížení všechny moduly při spuštění. To znamená, že volání require('module') se má provést Pokud je ve skutečnosti musí v rámci funkce pokusíte použít modul.
3. Azure Webapps nabízí funkci místní mezipaměti. Tato funkce zkopíruje obsah ze sdílené síťové složce na místní disk ve virtuálním počítači. Vzhledem k tomu, že soubory jsou místní, čas načítání uzlu\_moduly je mnohem rychlejší.

## <a name="iisnode-http-status-and-substatus"></a>Stav protokolu http modulu IISNODE a podřízeného stavu
To [zdrojový soubor](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) zobrazí seznam všech iisnode kombinace možné stav nebo substatus může vrátit v případě chyby.

Povolit FREB pro aplikace, abyste viděli kód chyby win32 (zkontrolujte, zda povolit FREB pouze na webech mimo produkční z důvodů výkonu).

| Stav protokolu HTTP | Podřízený stav protokolu HTTP | Možný důvod? |
| --- | --- | --- |
| 500 |1000 |Se některé problém odeslání požadavku na modulu IISNODE – zaškrtněte, pokud byl zahájen node.exe. Node.exe může mít došlo k chybě při spuštění. Zkontrolujte konfiguraci web.config chyby. |
| 500 |1001 |-Win32Error 0x2 - aplikace nereaguje na adresu URL. Zkontrolujte pravidel přepisování adres URL, nebo pokud má vaše aplikace express správné trasy definované. -Win32Error 0x6d – pojmenovaný kanál je zaneprázdněn – Node.exe nepřijímá požadavky, protože je zaneprázdněný. Zkontrolujte vysoké využití procesoru. -Další chyby – zkontrolujte, pokud node.exe došlo k chybě. |
| 500 |1002 |Došlo k chybě Node.exe – zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 500 |1003 |Přesměrováním konfigurace problém – nikdy byste měli vidět to ale v takovém případě pojmenovaný kanál konfigurace je nesprávná. |
| 500 |1004-1018 |Při odesílání požadavku nebo odpovědi do a z node.exe zpracování se nějaká chyba. Zkontrolujte, zda node.exe došlo k chybě. Zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 503 |1000 |Není dostatek paměti k přidělení více pojmenovaného kanálu připojení. Zkontrolujte, proč aplikace využívala mnoho paměti. Zkontrolujte hodnotu nastavení maxConcurrentRequestsPerProcess. Pokud jeho není nekonečné a budete mít mnoho požadavků, zvýšit tuto hodnotu, aby se tato chyba. |
| 503 |1001 |Žádost nebyla odeslána do node.exe, protože je recyklace aplikace. Po aplikaci má recyklované, by měl obvykle zpracovat požadavky. |
| 503 |1002 |Kód chyby win32 zkontrolujte skutečné důvodu – žádost nebyla odeslána na node.exe. |
| 503 |1003 |Pojmenovaný kanál je příliš zaneprázdněn – zaškrtněte, pokud uzel spotřebovává velké množství procesoru |

Je nastavení v rámci NODE.exe názvem uzlu\_čeká na vyřízení\_kanálu\_instance. Ve výchozím nastavení mimo azure webapps tato hodnota je 4. To znamená, že node.exe přijmout jenom 4 požadavků současně v pojmenovaném kanálu. Na Azure Webapps tato hodnota nastavena na 5000 a tato hodnota by měla být dostatečně vhodné pro většinu uzlu aplikací běžících na azure webapps. 503.1003 byste neměli vidět na azure webapps, protože máme vysoké hodnoty pro uzel\_čeká na vyřízení\_kanálu\_instance.  |

## <a name="more-resources"></a>Další zdroje informací
Další informace o aplikací node.js v Azure App Service na následujících odkazech.

* [Začínáme s webovými aplikacemi Node.js ve službě Azure App Service](app-service-web-get-started-nodejs.md)
* [Postup ladění webové aplikace Node.js ve službě Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Středisko pro vývojáře Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Seznámení se supertajnou konzolou pro ladění modulu Kudu](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

