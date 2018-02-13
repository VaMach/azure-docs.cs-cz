---
title: "Principy odchozí připojení v Azure | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure umožňuje virtuálním počítačům komunikovat s služby veřejného Internetu."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Principy odchozích připojení v Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Azure poskytuje odchozí připojení pro zákaznických nasazení pomocí několika různých mechanismů.  Tento článek popisuje, co jsou scénáře, kdy se vztahují, jak pracují a jak spravovat.

Nasazení v Azure může komunikovat s koncovými body mimo Azure ve veřejném adresním prostoru IP adres. Pokud instance zahájí odchozího toku do cílového umístění v prostor veřejných IP adres, Azure dynamicky mapuje privátní IP adresu na veřejnou IP adresu.  Po vytvoření tohoto mapování, návratový provoz pro tuto odchozí původu toku dosáhnout taky privátní IP adresu kde toku vytvořena.

Azure používá zdroj překládání adres (překládat pomocí SNAT) a provést tuto funkci.  Pokud více privátních IP adres se vydávají za jednu veřejnou IP adresu, použije Azure [portu překlad adres (Jan)](#pat) k maskovat privátních IP adres.  Dočasné porty se používají pro Jan a jsou [souhrnů ještě neumístěných](#preallocatedports) na základě velikosti fondu.

Existuje více [odchozí scénáře](#scenarios). Tyto scénáře mohou být kombinovány, podle potřeby. Přečtěte si je pečlivě a pochopit schopnosti, omezení a vzory, protože se vztahují na váš model nasazení a aplikační scénář.  Přečtěte si pokyny pro [Správa těchto scénářů](#snatexhaust).

## <a name="scenarios"></a>Přehled scénáře

Azure má dva modely nasazení hlavní: Azure Resource Manager a Klasický model. Nástroj pro vyrovnávání zatížení a související prostředky jsou explicitně definován při použití [prostředky Azure Resource Manager](#arm).  Nasazení Classic abstraktní koncept služby Vyrovnávání zatížení a express podobné funkce prostřednictvím definice koncové body [Cloudová služba](#classic). Příslušné [scénáře](#scenarios) pro vaše nasazení závislé, na které nasazení model se používá.

### <a name="arm"></a>Azure Resource Manager

Azure aktuálně poskytuje tři různých způsobů dosažení odchozí připojení pro prostředky Azure Resource Manager.  [Classic](#classic) má podmnožinu těchto scénářích nasazení.

| Scénář | Metoda | Popis |
| --- | --- | --- |
| [1. Virtuální počítač s Instance úroveň veřejnou IP adresu (s nebo bez nástroj pro vyrovnávání zatížení)](#ilpip) | Překládat pomocí SNAT, port vydávají nepoužívá |Azure používá veřejnou IP adresu, které jsou přiřazené ke konfiguraci IP adresy z instance síťový adaptér.  Instance má všechny dočasné porty, které jsou k dispozici. |
| [2. Veřejné nástroj pro vyrovnávání zatížení, které jsou spojené s virtuálním Počítačem (žádná Instance úroveň veřejná IP adresa v instanci)](#lb) | Překládat pomocí SNAT s port vydávají (PAT) pomocí frontend(s) nástroj pro vyrovnávání zatížení |Azure sdílí veřejné služby Vyrovnávání zatížení frontend(s) s více privátních IP adres a používá dočasné porty frontend(s) pracovníkovi veřejnou IP adresu. |
| [3. Samostatný virtuální počítač (žádná služba Vyrovnávání zatížení, žádná Instance úroveň veřejná IP adresa)](#defaultsnat) | Překládat pomocí SNAT s port vydávají (PAT) | Azure automaticky označí veřejnou IP adresu pro překládat pomocí SNAT, sdílí tato veřejná IP adresa s více privátních IP adres skupina dostupnosti a používá dočasné porty tato veřejná IP adresa.  Tento scénář je záložní scénář, pro který předchází scénáře 1 a 2 a nedoporučuje se, pokud je třeba viditelnost a kontrolu. |

Pokud nechcete, aby virtuální počítač ke komunikaci s koncovými body mimo Azure ve veřejném adresním prostoru IP adres, můžete použít skupiny zabezpečení sítě (NSG) k blokování přístupu podle potřeby.  Pomocí skupin Nsg je podrobněji popsána v [nelze navázat odchozí připojení](#preventoutbound).  Podrobné pokyny a implementace návrhu pro návrh a spravovat virtuální sítě bez jakékoli odchozí přístup je mimo rámec tohoto článku.

### <a name="classic"></a>Classic (cloudové služby)

Scénáře, které jsou k dispozici pro nasazení Classic jsou podmnožinou scénáře, které jsou k dispozici pro [Azure Resource Manager](#arm) nasazení a základní nástroje pro vyrovnávání zatížení.

Klasické virtuální počítač má stejné tři základní scénáře, jak je popsáno pro prostředky Azure Resource Manager ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Classic webové Role pracovního procesu má jenom dva scénáře ([2](#lb), [3](#defaultsnat)).  [Zmírnění dopadů strategie](#snatexhaust) mít také stejnou rozdíly.

Algoritmus používaný pro [předběžné přidělování dočasné porty](#ephemeralprots) Jan pro nasazení Classic je stejné jako pro nasazení prostředků Azure Resource Manager.  

### <a name="ilpip"></a>Scénář 1: Počítač s adresou veřejná IP adresa na úrovni Instance

V tomto scénáři má virtuální počítač instanci úroveň veřejné IP splnění přiřazen. Pokud jde o odchozí připojení, není důležité, zda je virtuální počítač nebo ne vyrovnáváním zatížení.  Tento scénář má přednost před ostatní. Pokud se používá splnění, virtuální počítač používá splnění pro všechny odchozí toky.  

Port podvržený (Jan) se nepoužívá a virtuální počítač má všechny dočasné porty, které jsou k dispozici pro použití.

Pokud vaše aplikace iniciuje mnoho odchozí toky a dochází k vyčerpání překládat pomocí SNAT port, můžete zvážit přiřazení [splnění zmírnit překládat pomocí SNAT omezení](#assignilpip). Zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust) ji jako celek.

### <a name="lb"></a>Scénář 2: Vyrovnávání zatížení sítě VM bez Instance úroveň veřejnou IP adresu

V tomto scénáři je virtuální počítač součástí veřejné back-endový fond Vyrovnávání zatížení.  Virtuální počítač nemá veřejnou IP adresu přiřazen. Vyrovnávání zatížení prostředků musí být nakonfigurované pravidlo Vyrovnávání zatížení pro vytvoření odkazu mezi veřejnou IP front-endovou s fondem back-end. Pokud neprovedete tohoto pravidla konfigurace, toto chování je, jak je popsáno v tento scénář pro [samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu](#defaultsnat).  Není nutné pro pravidlo tak, aby měl pracovní naslouchací proces ve fondu back-end nebo test stavu úspěšné.

Při vyrovnávání zatížení sítě virtuálního počítače vytvoří odchozí tok, znamená, že Azure privátní zdrojovou IP adresu odchozího toku veřejnou IP adresu veřejnou front-endovou Vyrovnávání zatížení. Azure pomocí této funkce zdroj síťové adresy překlad (SNAT) a také [portu překlad adres (Jan)](#pat) k maskovat více privátních IP adres za veřejnou IP adresu. Dočasné porty nástroje pro vyrovnávání zatížení veřejnou IP adresu front-endovou slouží k rozlišení jednotlivých toky pochází ve virtuálním počítači. Dynamicky používá překládat pomocí SNAT [souhrnů ještě neumístěných dočasné porty](#preallocatedports) vytvoření odchozí toky. V tomto kontextu jsou dočasné porty používané pro překládat pomocí SNAT označovány jako porty překládat pomocí SNAT.

Překládat pomocí SNAT porty jsou souhrnů ještě neumístěných jak je popsáno v [překládat pomocí SNAT principy a Jana](#snat) části a jsou konečný prostředek, který může dojít k vyčerpání. Je důležité pochopit, jak jsou [spotřebované](#pat). Zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust) pochopit, jak pro návrh a zmírnit podle potřeby.

Když [více (veřejné) IP adresy jsou přidruženy základní nástroje pro vyrovnávání zatížení](load-balancer-multivip-overview.md), všechny tyto veřejné IP adresy jsou [kandidátem na odchozí toky a jedna je vybrána](#multivipsnat).  

Můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [výstrahy protokoly událostí pro monitorování překládat pomocí SNAT portu vyčerpání zprávy](load-balancer-monitor-log.md#alert-event-log) pro sledování stavu odchozí připojení s základní nástroje pro vyrovnávání zatížení.

### <a name="defaultsnat"></a>Scénář 3: Samostatný virtuální počítač bez Instance úroveň veřejnou IP adresu

Virtuální počítač není součástí fondu vyrovnávání zatížení Azure a nemá adresu Instance úroveň veřejné IP (splnění), který je přiřazen. Když virtuální počítač se vytváří odchozího toku, překládá Azure privátní zdrojovou IP adresu odchozího toku veřejné Zdrojová IP adresa. Veřejnou IP adresu použitou pro tuto odchozího toku není Konfigurovatelný a nepočítá proti odběru limitu prostředků veřejné IP. Azure používá zdroje síťové adresy překlad (SNAT) s vydávají port ([PAT](#pat)) a provést tuto funkci. Tento scénář je podobná [scénář 2](#lb), s výjimkou je žádnou kontrolu nad adresa IP použitá.  Toto je záložní scénář pro při scénáře 1 a 2 scénáře nejsou k dispozici.  Tento scénář se nedoporučuje, pokud se požaduje kontrolu nad adresu odchozí.

Překládat pomocí SNAT porty jsou souhrnů ještě neumístěných jak je popsáno v [překládat pomocí SNAT principy a Jana](#snat) části a jsou konečný prostředek, který může dojít k vyčerpání. Je důležité pochopit, jak jsou [spotřebované](#pat). Zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust) pochopit, jak pro návrh a zmírnit podle potřeby.

### <a name="combinations"></a>Více, kombinované scénářů

K dosažení konkrétní výsledek lze spojovat scénáře popsané v předchozí části.  Pokud existuje více scénářů, se vztahují pořadí podle priority: [scénář 1](#ilpip) má přednost před [scénář 2](#lb) a [3](#defaultsnat) (Azure Resource Manager pouze) a [scénář 2](#lb) přepsání [scénář 3](#defaultsnat) (Azure Resource Manager a Classic).

Příkladem je nasazení služby Správce prostředků Azure, kde založena na odchozí připojení k omezenému počtu cílů aplikace, ale také přijme příchozí toky přes front-end pro vyrovnávání zatížení. V takovém případě může kombinovat scénáře 1 a 2 pro pomoc.  Zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust) pro další vzory.

### <a name="multivipsnat"></a>Více frontends pro odchozí toky

Základní nástroje pro vyrovnávání zatížení bude zvolte jeden front-endu být použité odchozí toky při [více (veřejné) frontends IP](load-balancer-multivip-overview.md) jsou kandidáty pro odchozí toky.  Tento výběr není Konfigurovatelný a algoritmus výběr by měl být považuje za náhodné.  Můžete určit konkrétní IP adresu pro odchozí, jak je popsáno v [kombinaci scénáře](#combinations).

## <a name="snat"></a>Princip překládat pomocí SNAT a Jan

### <a name="pat"></a>Port podvržený překládat pomocí SNAT (Jan)

Pokud prostředek veřejné nástroje pro vyrovnávání zatížení je spojen s instancí virtuálních počítačů, je přepsaná každý zdroj odchozí připojení. Zdroj je přepsaná z prostor virtuální sítě privátní IP adresy front-endu veřejné IP adresy služby Vyrovnávání zatížení. V veřejný adresní prostor IP adres 5-n-tice toku (zdrojové IP adresy, zdrojového portu, přenosového protokolu IP, cílové IP adresy, cílový port) musí být jedinečný.  Dočasné porty se používají k dosažení tohoto cíle po přepisování privátní zdrojovou IP adresu, protože více toků pocházejí z jedné veřejné IP adresy.  Tyto dočasné porty jsou označovány jako porty překládat pomocí SNAT. 

Jeden port překládat pomocí SNAT obsazením za toku do jedné cílové IP adresy, portu a protokolu. Pro více toků na stejnou cílovou IP adresu, port a protokol využívá každý tok jediný port překládat pomocí SNAT. Tím se zajistí na toky jedinečná při pocházely ze stejné veřejnou IP adresu do stejné cílové IP adresy, portu a protokolu. 

Více toků, každý na jinou cílovou IP adresu, port a protokol, budou sdílet jeden port překládat pomocí SNAT. Cílovou IP adresu, port a protokol umožňuje toky jedinečný bez nutnosti dalších zdrojových portů, který se má použít k rozlišení toků v prostor veřejných IP adres.

Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud překládat pomocí SNAT porty jsou vydal existující toky. Nástroj pro vyrovnávání zatížení získá překládat pomocí SNAT porty, když toku zavře a používá [časový limit nečinnosti 4minutové](#idletimeout) pro opětovného získání překládat pomocí SNAT porty z nečinnosti toků.

Zkontrolujte [Správa překládat pomocí SNAT](#snatexhaust) části vzorů zmírnit podmínek, které běžně vést k vyčerpání port překládat pomocí SNAT.

### <a name="preallocatedports"></a>Předběžné přidělení dočasných portů pro port vydávají překládat pomocí SNAT (Jan)

Azure používá algoritmus určit počet souhrnů ještě neumístěných překládat pomocí SNAT porty, které jsou k dispozici na základě velikosti fondu back-end při použití portu podvržený překládat pomocí SNAT ([PAT](#pat)).  Překládat pomocí SNAT porty jsou dočasné porty, které jsou k dispozici pro danou veřejné zdrojové adresy IP.

Azure preallocates překládat pomocí SNAT porty ke konfiguraci IP adresy síťového adaptéru každý virtuální počítač. Při konfiguraci IP adres je přidán do fondu, jsou porty překládat pomocí SNAT souhrnů ještě neumístěných pro tuto konfiguraci IP adresy na základě velikosti fondu back-end. Pro Classic webové role pracovního procesu je přidělení za role instance.  Když se vytváří odchozí toky, [PAT](#pat) dynamicky využívá (až předběžně přidělené limit) a uvolní tyto porty toku zavře nebo [časové limity nečinnosti](#ideltimeout).

V následující tabulce jsou uvedeny preallocations port překládat pomocí SNAT pro úrovně velikosti fondu back-end:

| Velikost fondu (instance virtuálních počítačů) | Předběžně přidělené překládat pomocí SNAT porty na konfiguraci protokolu IP|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Je důležité si pamatovat, že počet dostupných portů překládat pomocí SNAT nepřekládá přímo k počtu připojení. Jediný port překládat pomocí SNAT lze znovu použít pro více cílů jedinečný. Porty se používají, pouze pokud je nutné, aby toky jedinečný. Odkazovat na [postup správy tento prostředek vyčerpatelným](#snatexhaust) pro návrh a zmírnění pokyny jako část popisující [PAT](#pat).

Změna velikosti fondu back-end může mít vliv na některé z vaší zavedených toky. Pokud velikost fondu back-end zvyšuje a přejde do s další vrstvou, polovinu vaší předběžně přidělené překládat pomocí SNAT porty jsou uvolnit při přechodu do další vrstvou větší fond back-end. Toky, které jsou spojeny s regenerovaný překládat pomocí SNAT portem bude časový limit a musí je znovu vytvořit. Pokusy o připojení úspěšné okamžitě, dokud předběžně přidělené porty jsou k dispozici.

Pokud velikost fondu back-end snižuje a přejde do nižší úrovně, zvyšuje počet dostupných portů překládat pomocí SNAT. V takovém případě existující přidělené překládat pomocí SNAT porty a jejich odpovídajících toky neovlivní.

## <a name="snatexhaust"></a>Správa vyčerpání port překládat pomocí SNAT (Jan)

[Dočasné porty](#preallocatedports) používá pro [PAT](#pat) jsou vyčerpatelným prostředků, jak je popsáno v [samostatný virtuální počítač bez Instance úroveň veřejnou IP adresu](#defaultsnat) a [Vyrovnávání zatížení sítě virtuálních počítačů bez Instance úroveň veřejnou IP adresu](#lb).

Pokud znáte jsou inicializaci mnoho odchozí připojení TCP nebo UDP na stejnou cílovou IP adresu a port a sledovat selhání odchozí připojení nebo doporučujeme podporu jste vyčerpává překládat pomocí SNAT porty (souhrnů ještě neumístěných [dočasné porty](#preallocatedports)používané [PAT](#pat)), máte několik možností obecné zmírnění dopadů.  Přečtěte si tyto možnosti a rozhodnout, jaký je k dispozici a nejvhodnější pro váš scénář.  Je možné jeden nebo více může pomoct spravovat tento scénář.

Pokud máte potíže seznámení s chováním odchozí připojení, můžete použít IP zásobník statistiky (netstat) nebo může být užitečné, abyste mohli pozorovat chování připojení pomocí paketu zachycení.  Můžete provádět tyto záznamy o paketů v hostovaný operační systém instanci služby nebo použít [sledovací proces sítě pro zachytávání paketů](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Upravit aplikaci znovu použít připojení 
Můžete snížit vyžádání pro dočasné porty používané pro překládat pomocí SNAT opakovaného použití připojení v aplikaci.  To platí hlavně pro protokoly, jako je výchozím nastavením, kde znovu použít připojení HTTP/1.1.  A pak využívat jiné protokoly, pomocí protokolu HTTP jako způsob přepravy (tj. REST).  Opakované použití je vždy lepší, než jednotlivé, atomic připojení TCP pro každou žádost a výsledkem další původce, velmi efektivní TCP transakce.

### <a name="connection pooling"></a>Upravit aplikaci, aby používala sdružování připojení
Můžete použít připojení sdružování schéma ve vaší aplikaci, kde jsou požadavky interně distribuovány na pevnou sadu připojení (každý opakovaného použití kde je to možné).  Toto omezení počtu dočasné porty používá a vytvoří předvídatelnější prostředí.  To také můžete zvýšit propustnost žádostí tím, že více souběžných operací, když blokuje jednoho připojení na odpověď operace.  Sdružování připojení možná již existuje v rámci, které používáte k vývoji vaší aplikace nebo nastavení konfigurace pro vaši aplikaci.  Toto můžete kombinovat s opakované použití připojení a více požadavků využívat pevné, předvídatelný řadu porty, které se stejnou cílovou IP adresu a port při také využívají velmi efektivní využití TCP transakce snižuje latenci a prostředků využití.  Transakce UDP mohou také těžit jako správa číslo UDP toky můžete zase vyhnout výfukového podmínky a spravovat využití portu překládat pomocí SNAT.

### <a name="retry logic"></a>Upravit aplikaci, aby používala méně agresivní logika opakovaných pokusů
Když [souhrnů ještě neumístěných dočasné porty](#preallocatedports) používá pro [PAT](#pat) jsou vyčerpání nebo aplikace dojde k selhání, agresivní nebo hrubou silou opakuje bez decay a omezení rychlosti logiku způsobit vyčerpání dojít, nebo zachovat. Vyžádání pro dočasné porty můžete omezit použitím méně agresivní logika opakovaných pokusů.   Dočasné porty mít 4 minut nečinnosti, po vypršení časového limitu (není možné nastavit) a pokud jsou moc agresivní opakované pokusy, má k vyčerpání nebyla příležitost vymazány svoje vlastní.  Vzhledem k tomu, jak a jak často aplikace opakování transakce je proto důležité aspekt návrhu.

### <a name="assignilpip"></a>Přiřadit veřejná IP adresa úrovni Instance pro každý virtuální počítač
Tato operace změní váš scénář [úrovni Instance veřejnou IP adresu pro virtuální počítač](#ilpip).  Všechny dočasné porty veřejné IP adresy použít pro jednotlivé virtuální počítače jsou k dispozici pro virtuální počítač (na rozdíl od scénáře, kde dočasné porty veřejnou IP adresu jsou sdíleny s všechny Virtuálního počítače přidružené fondu příslušných back-end).  Existují kompromisy vzít v úvahu, například další náklady na veřejné IP adresy a potenciální dopad vytvoření seznamu povolených velký počet jednotlivé IP adresy.

>[!NOTE] 
>Tato možnost není k dispozici pro webové role pracovního procesu.

### <a name="idletimeout"></a>Použít udržování otevřených připojení k resetování odchozí časový limit nečinnosti

Odchozí připojení mít časový limit nečinnosti 4 minuty. Toto není upravit. Ale můžete použít přenosu (tj. TCP otevřených) nebo udržování otevřených připojení vrstvy aplikace aktualizujte tok nečinnosti a obnovit tento časový limit nečinnosti, pokud vyžaduje.

## <a name="discoveroutbound"></a>Zjišťování veřejné IP adresy používané daného virtuálního počítače
Existuje mnoho způsobů, jak určit IP adresu zdrojového veřejné odchozí připojení. OpenDNS poskytuje službu, která můžete zobrazit veřejnou IP adresu vašeho virtuálního počítače. Použití příkazu nslookup, můžete odeslat dotaz DNS pro název myip.opendns.com do překladače OpenDNS. Služba vrátí zdrojové IP adresy, který se používá k odeslání dotazu. Při spuštění následující dotaz z virtuálního počítače, je odpověď na veřejné IP adresy použít pro tento virtuální počítač.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Nelze navázat odchozí připojení
Někdy je žádoucí pro virtuální počítač povolit vytváření odchozího toku nebo může být požadavek na správu, které cíle je dostupný odchozí toky nebo které cíle může začít příchozí toky. V takovém případě použijete [skupiny zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md) ke správě cílů, které mohou být také využity virtuálního počítače jako které veřejné cíl, můžete zahájit příchozí toky. Když použijete skupinu NSG k virtuálnímu počítači s vyrovnáváním zatížení, je potřeba věnovat pozornost [výchozí značky](../virtual-network/virtual-networks-nsg.md#default-tags) a [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).

Je nutné zajistit, že virtuální počítač může přijímat žádosti o kontrolu stavu z nástroje pro vyrovnávání zatížení Azure. Pokud skupina NSG blokuje žádostí o stav testu z výchozí značka AZURE_LOADBALANCER, selže test stavu vašeho virtuálního počítače a virtuálního počítače je označena. Nástroj pro vyrovnávání zatížení zastaví odesílání nové toky do tohoto virtuálního počítače.

## <a name="next-steps"></a>Další postup

- Další informace o [základní nástroje pro vyrovnávání zatížení](load-balancer-overview.md).
- Další informace o [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).
- Přečtěte si o některých dalších klíče [sítě možnosti](../networking/networking-overview.md) v Azure.
