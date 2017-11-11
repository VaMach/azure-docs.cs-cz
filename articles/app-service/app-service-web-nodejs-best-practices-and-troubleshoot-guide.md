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
ms.date: 11/09/2017
ms.author: ranjithr
ms.openlocfilehash: f7f3186ba93670e566a10f97dde86a977101e8fc
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Osvědčené postupy a Průvodci odstraňováním potíží aplikace uzlu ve webových aplikacích Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto článku se dozvíte osvědčené postupy a řešení potíží pro [uzel aplikace](app-service-web-get-started-nodejs.md) systémem Azure Web Apps (s [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Buďte opatrní při používání při řešení potíží na vašem provozním serveru. Doporučuje se řešení problémů aplikace na instalační program mimo produkční například přípravný slot a pokud je problém vyřešen, Prohodit vaší přípravný slot s produkční slot.
> 
> 

## <a name="iisnode-configuration"></a>Konfigurace modulu IISNODE
To [soubor schématu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ukazuje všechna nastavení, můžete nakonfigurovat pro modulu iisnode. Některá nastavení, které jsou užitečné pro vaši aplikaci:

* nodeProcessCountPerApplication
  
    Toto nastavení určuje počet uzlu procesy, které jsou spouštěny na aplikaci služby IIS. Výchozí hodnota je 1. Pomocí tohoto nastavení můžete spustit jako v mnoha node.exes jako vaše počet virtuálních procesorů virtuálních počítačů na hodnotu 0. Doporučená hodnota je 0 pro většinu aplikací, abyste mohli používat všechny Vcpu na váš počítač. Node.exe je jedním podprocesem, jeden node.exe využívá maximálně 1 virtuální procesor. Chcete-li získat maximální výkon mimo aplikaci uzlu, chcete použít všechny Vcpu.
* nodeProcessCommandLine
  
    Toto nastavení určuje cestu ke node.exe. Můžete nastavit tuto hodnotu tak, aby odkazoval na vaší verzi node.exe.
* maxConcurrentRequestsPerProcess
  
    Toto nastavení určuje maximální počet souběžných požadavků, které posílá každý node.exe modulu iisnode. Ve webových aplikacích Azure je výchozí hodnota pro tento nekonečné. Nemusíte si dělat starosti o toto nastavení. Mimo Azure Web Apps výchozí hodnota je 1024. To můžete nakonfigurovat v závislosti na tom, kolik žádostí že aplikace přijímá a jak rychle vaše aplikace zpracovává každý požadavek.
* maxNamedPipeConnectionRetry
  
    Toto nastavení určuje maximální počet opakování modulu iisnode provedením připojení v pojmenovaném kanálu pro odeslání požadavku na node.exe. Toto nastavení v kombinaci s namedPipeConnectionRetryDelay Určuje celkový časový limit každého požadavku v rámci modulu iisnode. Výchozí hodnota je 200 ve webových aplikacích Azure. Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Toto nastavení určuje dobu (v ms) modulu iisnode čeká mezi každou opakovat, pokud chcete odeslat požadavek node.exe přes pojmenovaný kanál. Výchozí hodnota je 250 ms.
    Celkový časový limit v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Ve výchozím nastavení, celkový časový limit v modulu iisnode ve webových aplikacích Azure je 200 \* 250 ms = 50 sekund.
* logDirectory
  
    Toto nastavení určuje adresář, kam modulu iisnode protokoluje stdout/stderr. Výchozí hodnota je modulu iisnode, která je relativní vzhledem k adresáři hlavního skriptu (adresáře, kde je hlavní server.js existuje)
* debuggerExtensionDll
  
    Toto nastavení určuje, jaká verze modulu iisnode nástroj node-inspector používá při ladění aplikace uzlu. V současné době modulu iisnode. inspector 0.7.3.dll a iisnode inspector.dll jsou pouze dvě platné hodnoty pro toto nastavení. Výchozí hodnota je modulu iisnode. inspector 0.7.3.dll. Verze modulu iisnode. inspector 0.7.3.dll používá uzlu inspector 0.7.3 a používá. Na vaše Azure webapp používat tuto verzi je nutné povolit objekty websockets. V tématu <http://www.ranjithr.com/?p=98> další podrobnosti o tom, jak nakonfigurovat modulu iisnode používat nový nástroj node-inspector.
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
  
    Povolení vyprázdnění každý fragment textu entity odpovědi zvýší nároky na výkon, sníží propustnost systému % ~ 5 (od v0.1.13), takže je vhodné k určení rozsahu toto nastavení pouze pro koncové body, které vyžadují vysílání datového proudu odpovědi (například pomocí <location> element v souboru web.config).
  
    Kromě toho pro streamování aplikací, musíte taky nastavit responseBufferLimit vaší obslužné rutiny modulu iisnode na hodnotu 0.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Toto je seznam souborů, které jsou sledovaná změny oddělených středníkem. Ke změně souboru způsobí, že aplikace recyklace. Každá položka obsahuje název volitelné directory plus požadovaný soubor názvů, které jsou relativní vzhledem k adresáři, kde se nachází vstupní bod hlavní aplikace. Zástupné znaky nejsou povoleny v pouze část názvu souboru. Výchozí hodnota je "\*. js;web.config"
* recycleSignalEnabled
  
    Výchozí hodnota je false. Pokud je povoleno, aplikace uzlu můžete připojit k pojmenovanému kanálu (proměnnou prostředí modulu IISNODE\_řízení\_kanálu) a odeslat zprávu "recyklaci". To způsobí, že w3wp řádně recyklace.
* idlePageOutTimePeriod
  
    Výchozí hodnota je 0, což znamená, že tato funkce je vypnutá. Pokud nastavíte na hodnotu větší než 0, modulu iisnode bude stránka se všechny jeho podřízené procesy každých 'idlePageOutTimePeriod' v milisekundách. V tématu [dokumentace](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) zjistit, co stránky na prostředky. Toto nastavení je užitečné pro aplikace, které spotřebovávají velké množství paměti a chcete na stránku limitu paměti na disk příležitostně pro uvolnění některé paměti RAM.

> [!WARNING]
> Buďte opatrní při povolování následující nastavení konfigurace na výrobní aplikace. Doporučuje se není je povolit v za provozu výrobní aplikace.
> 
> 

* debugHeaderEnabled
  
    Výchozí hodnota je false. Je-li nastaven na hodnotu true, modulu iisnode přidá HTTP odpovědi hlavičku modulu iisnode-debug každé odpovědi HTTP odešle, že hodnota hlavičky ladění modulu iisnode je adresa URL. Jednotlivé diagnostické informace může shromažďovat informace prohlížením fragment adresy URL, ale mnohem lepší vizualizace se dosahuje otevření této adresy URL v prohlížeči.
* loggingEnabled
  
    Toto nastavení určuje protokolování stdout a stderr pomocí modulu iisnode. Iisnode zaznamená stdout/stderr z uzlu procesy se spustí a zapíše ho do adresáře zadali v nastavení 'logDirectory'. Jakmile je tato možnost povolena, vaše aplikace zapisuje protokoly do systému souborů a v závislosti na množství protokolování, provádí aplikace, může být ovlivnit výkon.
* devErrorsEnabled
  
   Výchozí hodnota je false. Když nastaven na hodnotu true, modulu iisnode zobrazí stavový kód HTTP a kód chyby Win32 prohlížeč. Kód win32 je užitečné při ladění určité typy potíží.
* debuggingEnabled (nepovolujte na provozním serveru)
  
    Toto nastavení řídí funkce ladění. Pomocí nástroje node-inspector je integrovaná modulu Iisnode. Povolením tohoto nastavení můžete povolit ladění aplikace uzlu. Jakmile je toto nastavení povoleno, bude modulu iisnode Rozvrhněte nástroj nezbytné node-inspector soubory v adresáři 'debuggerVirtualDir' na první požadavek ladění na aplikaci uzlu. Odesílání požadavku do http://yoursite/server.js/debug, můžete načíst nástroj node-inspector. Segment adresy URL ladění můžete ovládat nastavení 'debuggerPathSegment'. Ve výchozím nastavení debuggerPathSegment = "debug". Je tento parametr můžete nastavit na identifikátor GUID, například tak, aby je obtížnější být zjištěny jinými uživateli.
  
    Zaškrtněte toto políčko [odkaz](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) Další informace o ladění.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scénáře a doporučení nebo řešení potíží
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moje aplikace uzlu je volání příliš mnoho odchozí.
Mnoho aplikací chtít provést v rámci jejich regulární operace odchozí připojení. Například když přijde žádost, aplikace uzlu vhodnější kontaktovat rozhraní REST API jinde a určité informace pro zpracování požadavku. Chcete by se použít zachování připojení agenta zachovat při volání protokolu http nebo https. Například můžete použít modul agentkeepalive jako aktivní agenta zachovat při provádění těchto odchozí volání. Tím je zajištěno, že jsou sockets opakovaně na vaší webové aplikace Azure virtuálních počítačů a snižuje zátěž vytváření nových soketů pro každý požadavek odchozí. Navíc díky tomu se, že používáte menší počet sockets provádět mnoho odchozí požadavky, a proto nepřekračují maxSockets, které jsou přiděleny na virtuální počítač. Doporučení ve webových aplikacích Azure je k nastavení hodnoty maxSockets agentKeepAlive na celkem 160 sockets na virtuální počítač. To znamená, že pokud máte čtyři node.exe spuštěna na virtuálním počítači, nastavte agentKeepAlive maxSockets na 40 na node.exe, což je 160 celkový na virtuální počítač.

Příklad [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfigurace:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Tento příklad předpokládá, že máte 4 node.exe systémem virtuálního počítače. Pokud máte jiný počet node.exe spuštěna na virtuálním počítači, musíte upravit maxSockets nastavení odpovídajícím způsobem.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moje uzel aplikace využívala příliš mnoho procesoru.
Doporučení se zobrazí na portálu o vysoké spotřeby procesoru pravděpodobně z Azure Web Apps. Můžete také nastavit tak, monitorování si chcete přehrát určité [metriky](web-sites-monitor.md). Při kontrole využití procesoru na [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), zkontrolujte maximální hodnoty pro procesor, nemusíte neproběhly hodnoty ve špičce.
V případech, kde by vaše aplikace využívala příliš mnoho procesorů a důvod, proč nelze vysvětlují můžete profil aplikace uzlu zjistit.

### 
#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>Profilace uzlu aplikace v Azure Web Apps s v8: profileru
Řekněme například, že máte aplikaci hello world, které chcete profil následujícím způsobem:

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

Zobrazí příkazový řádek, jak je uvedeno níže. Přejděte do adresáře webu/wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Spusťte příkaz "npm install profileru v8:".

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

Předchozí kód profily WriteConsoleLog funkce a pak zapíše profil výstup do souboru 'profile.cpuprofile' v rámci vaší lokality wwwroot. Odeslat požadavek do vaší aplikace. Zobrazí soubor 'profile.cpuprofile' vytvořil v rámci vaší lokality wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Tento soubor stáhnout a otevřete jej pomocí nástroje F12 Chrome. Stisknutím klávesy F12 na Chrome, a potom vyberte **profily** kartě. Vyberte **zatížení** tlačítko. Vyberte souboru profile.cpuprofile, který jste si stáhli. Klikněte na profil, který jste právě načetli.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Uvidíte, že 95 % času se spotřebovávají WriteConsoleLog funkce. To také ukazuje linek čísla a zdrojové soubory, které způsobila problém.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moje aplikace uzlu je spotřeba paměti převýší
Pokud vaše aplikace využívala příliš mnoho paměti, zobrazí na portálu o vysoké spotřeby paměti oznámení z Azure Web Apps. Můžete nastavit monitorování si chcete přehrát určité [metriky](web-sites-monitor.md). Při kontrole využití paměti na [řídicího panelu portálu Azure](../application-insights/app-insights-web-monitor-performance.md), nezapomeňte zkontrolovat maximální hodnoty paměti, nemáte neproběhly hodnoty ve špičce.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Zjištění paměti a Diffing haldy pro node.js
Můžete použít [uzlu memwatch](https://github.com/lloyd/node-memwatch) usnadňující identifikaci paměti nevracení.
Můžete nainstalovat memwatch stejně jako v8: profileru a upravovat kód k zachycení a rozdílové haldách k identifikaci paměť nevracení ve vaší aplikaci.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe jsou získávání ukončeny náhodně
Existují z několika důvodů, proč může být děje toto:

1. Aplikace je vyvolání nezachycená výjimek – kontrola d:\\domácí\\LogFiles\\aplikace\\protokolování errors.txt souboru podrobnosti v došlo k výjimce. Tento soubor má trasování zásobníku, takže můžete řešit vaší aplikace na základě.
2. Vaše aplikace využívala příliš mnoho paměti, která ovlivňuje jiné procesy z Začínáme. Pokud celková velikost paměti virtuálního počítače je téměř 100 %, může vaše node.exe ukončeny správcem proces umožníte jiné procesy ještě nějakou práci. Chcete-li odstranit tento problém, ujistěte se, že vaše aplikace není vracena paměť nebo pokud aplikace potřebuje používat značnou část paměti, škálování na větší virtuální počítače s mnohem víc paměti RAM.

### <a name="my-node-application-does-not-start"></a>Moje aplikace uzlu nespustí.
Pokud vaše aplikace vrací chyby 500 po jeho spuštění, může být z několika důvodů:

1. Node.exe se nenachází ve správném umístění. Zkontrolujte nastavení nodeProcessCommandLine.
2. Soubor skriptu hlavní se nenachází ve správném umístění. Zkontrolujte soubor web.config a ujistěte se, že název souboru hlavní skriptu v oddílu obslužných rutin odpovídá souboru hlavní skriptu.
3. Konfigurace souboru Web.config není správný – zkontrolujte nastavení názvy nebo hodnoty.
4. Studený Start – aplikace trvá příliš dlouho spustit. Pokud vaše aplikace trvá déle, než (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekund modulu iisnode vrátí chybu 500. Zvýšení hodnoty těchto nastavení tak, aby odpovídaly spuštění vaší aplikace aby modulu iisnode z vypršení časového limitu a vrácení za následek chybu 500.

### <a name="my-node-application-crashed"></a>Moje aplikace uzlu došlo k chybě
Aplikace je vyvolání nezachycená výjimek – d: Zkontrolujte prosím\\domácí\\LogFiles\\aplikace\\protokolování errors.txt souboru podrobnosti v došlo k výjimce. Tento soubor má trasování zásobníku, takže můžete řešit vaší aplikace na základě.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Moje aplikace uzlu trvá příliš mnoho času spuštění (studený Start)
Nejčastější příčinou pro aplikaci trvá příliš dlouho, spusťte je vysoký počet souborů v uzlu\_moduly. Aplikace se pokusí načíst většinu těchto souborů při spuštění. Ve výchozím nastavení protože vaše soubory jsou umístěny ve sdílené síťové složce ve webových aplikacích Azure, načítání mnoho souborů může trvat dobu.
Některá řešení rychleji, aby tento proces se:

1. Ujistěte se, že máte struktura ploché závislostí a žádné duplicitní závislosti pomocí npm3 nainstalovat moduly.
2. Pokuste se opožděné načíst vaše uzlu\_moduly a zatížení všechny moduly při spuštění aplikace. To znamená, že volání require('module') má být provedena, když ve skutečnosti to potřebujete v rámci funkce, které můžete provést při použití modulu.
3. Azure Web Apps nabízí funkci místní mezipaměti. Tato funkce zkopíruje obsah ze sdílené síťové složce na místní disk ve virtuálním počítači. Vzhledem k tomu, že soubory jsou místní, čas načítání uzlu\_moduly je mnohem rychlejší.

## <a name="iisnode-http-status-and-substatus"></a>Stav protokolu http modulu IISNODE a podřízeného stavu
To [zdrojový soubor](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) seznamy všechny iisnode kombinace možné stav nebo substatus může vrátit v případě chyby.

Povolit FREB pro aplikace, abyste viděli kód chyby win32 (ujistěte se, povolíte FREB pouze na webech mimo produkční z důvodů výkonu).

| Stav protokolu HTTP | Podřízený stav protokolu HTTP | Možný důvod? |
| --- | --- | --- |
| 500 |1000 |Se některé problém odeslání požadavku na modulu IISNODE – zkontrolujte, zda bylo zahájeno node.exe. Node.exe může selhání při spouštění. Zkontrolujte konfiguraci web.config chyby. |
| 500 |1001 |-Win32Error 0x2 - aplikace nereaguje na adresu URL. Zkontrolujte pravidel přepisování adres URL nebo zkontrolujte, zda aplikace express má správný trasy definované. -Win32Error 0x6d – pojmenovaný kanál je zaneprázdněn – Node.exe nepřijímá požadavky, protože je zaneprázdněný. Zkontrolujte vysoké využití procesoru. -Další chyby – zkontrolujte, pokud node.exe došlo k chybě. |
| 500 |1002 |Došlo k chybě Node.exe – zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 500 |1003 |Přesměrováním konfigurace problém – nikdy byste měli vidět to ale v takovém případě pojmenovaný kanál konfigurace je nesprávná. |
| 500 |1004-1018 |Při odesílání požadavku nebo odpovědi do a z node.exe zpracování se nějaká chyba. Zkontrolujte, zda node.exe došlo k chybě. Zkontrolujte d:\\domácí\\LogFiles\\errors.txt protokolování pro trasování zásobníku. |
| 503 |1000 |Není dostatek paměti k přidělení více pojmenovaného kanálu připojení. Zkontrolujte, proč aplikace využívala mnoho paměti. Zkontrolujte hodnotu nastavení maxConcurrentRequestsPerProcess. Pokud není nekonečné a budete mít mnoho požadavků, zvýšit tuto hodnotu, aby se tato chyba. |
| 503 |1001 |Žádost nebyla odeslána do node.exe, protože je recyklace aplikace. Po aplikaci má recyklované, by měl obvykle zpracovat požadavky. |
| 503 |1002 |Kód chyby win32 zkontrolujte skutečné důvodu – žádost nebyla odeslána na node.exe. |
| 503 |1003 |Pojmenovaný kanál je příliš zaneprázdněn – zkontrolujte, zda uzel nespotřeboval nadměrnému využití procesoru |

Je nastavení v rámci NODE.exe názvem uzlu\_čeká na vyřízení\_kanálu\_instance. Ve výchozím nastavení mimo Azure Web Apps tato hodnota je 4. To znamená, že node.exe přijmout jenom čtyři požadavků současně v pojmenovaném kanálu. Azure Web Apps tato hodnota nastavena do 5 000. Tato hodnota by měla být dostatečně vhodné pro většinu uzlu aplikací běžících na Azure Web Apps. 503.1003 byste neměli vidět ve webových aplikacích Azure z důvodu vysoké hodnoty pro uzel\_čeká na vyřízení\_kanálu\_instance.  |

## <a name="more-resources"></a>Další zdroje informací
Další informace o aplikací node.js v Azure App Service na následujících odkazech.

* [Začínáme s webovými aplikacemi Node.js ve službě Azure App Service](app-service-web-get-started-nodejs.md)
* [Postup ladění webové aplikace Node.js ve službě Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Středisko pro vývojáře Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Seznámení se supertajnou konzolou pro ladění modulu Kudu](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

