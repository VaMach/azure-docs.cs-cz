---
title: "Funkce monitorování výkonu v řešení pro sledování výkonu sítě v Azure Log Analytics | Microsoft Docs"
description: "Funkce sledování výkonu v nástroji Sledování výkonu sítě umožňuje sledovat připojení k síti v rámci různých bodů ve vaší síti, jako je například nasazení cloudu a místními umístěními a několika datových centrech a firemní pobočky, zvláště důležité vícevrstvé aplikace a micro služby."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Síťová řešení pro sledování výkonu – sledování výkonu

Funkce sledování výkonu v [sledování výkonu sítě](log-analytics-network-performance-monitor.md) vám pomůže s monitorováním síťové připojení mezi různými body sítě, jako je nasazení cloudu a místními umístěními a několika datových centrech a firemní pobočky, zvláště důležité vícevrstvé aplikace a micro služby. Pomocí tohoto nástroje můžete zjistit problémy se síťovým před stížnost vaši uživatelé. Jsou klíčové výhody: 

- Sledovat ztrátě a latence v rámci různých podsítí a nastavit upozornění 
- Monitorovat všechny cesty (včetně redundantní cesty) v síti 
- Řešení přechodných & bodu v čase síťových problémů, které je obtížné k replikaci 
- Určit konkrétní segmentu v síti, která je zodpovědná za snížení výkonu 
- Monitorování stavu sítě, bez nutnosti SNMP 


![Sledování výkonu sítě](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Konfigurace
Chcete-li spustit nástroj konfigurace pro sledování výkonu sítě, otevřete [řešení pro sledování výkonu sítě](log-analytics-network-performance-monitor.md) a klikněte na tlačítko **konfigurace** tlačítko.

![Konfigurace programu Sledování výkonu sítě](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Vytvořit nové sítě

Sítě v NPM je logický kontejner pro podsítě. Pomáhá uspořádat monitorování infrastruktury sítě podle potřeb monitorování. Můžete vytvořit síť s popisným názvem a do ní přidejte podsítě podle obchodní logiky. Například můžete vytvořit síť s názvem Londýnské a přidat všechny podsítě ve vašem datovém centru Londýn nebo síť s názvem *ContosoFrontEnd* a přidejte všechny podsítě obsluhující front-endu vaší aplikace s názvem Contoso k této síti. Řešení automaticky vytvoří výchozí sítě, který obsahuje všechny podsítě zjištěných ve vašem prostředí. Vždy, když vytvoříte síť, přidáte podsíť a této podsíti se odebere z výchozí sítí. Pokud odstraníte síť, všechny jeho podsítě se automaticky vrátí k výchozí síti. Proto výchozí sítí slouží jako kontejner pro všechny podsítě, které nejsou obsaženy v síti žádné uživatelem definované. Nelze upravit nebo odstranit výchozí sítí. Vždy zůstává v systému. Ale můžete vytvořit tolik vlastní sítě podle potřeby. Ve většině případů podsítí ve vaší organizaci jsou uspořádány do více než jedna síť a je třeba vytvořit jednu nebo více sítí, které chcete seskupit podsítě pro vaši obchodní logiku.

Chcete-li vytvořit nové sítě:


1. Klikněte na **karta sítě**.
1. Klikněte na tlačítko **sítě přidat** a pak zadejte název sítě a popis. 
2. Vyberte jednu nebo více podsítí a potom klikněte na **přidat**. 
3. Klikněte na tlačítko **Uložit** konfiguraci uložíte. 


### <a name="create-monitoring-rules"></a>Vytvoření pravidla monitorování 

Sledování výkonu generuje události stavu, když je prahová hodnota výkonu síťové připojení mezi dvěma podsítěmi nebo mezi dvěma sítěmi porušena. Tyto prahové hodnoty je možné zjistit automaticky systém nebo můžete zadat vlastní prahové hodnoty. Systém automaticky vytvoří výchozí pravidlo, které generuje událost stavu vždy, když ke ztrátě nebo latenci mezi žádném páru síť nebo podsíť odkazy narušení prahovou hodnotu naučili systému. To pomáhá monitorovat infrastrukturu sítě, dokud jste nevytvořili žádné pravidla monitorování explicitně řešení. Pokud **výchozí pravidlo** je povoleno, všechny uzly odeslat na všechny další uzly, které jste povolili pro monitorování syntetické transakce. Výchozí pravidlo je užitečný v případě malé sítě, například ve scénáři, kde máte malý počet serverů se systémem mikroslužbu a chcete zajistit, že všechny servery mají připojení k sobě navzájem. 

>[!NOTE]
> Důrazně doporučujeme zakázat **výchozí pravidlo** a vytvořit vlastní pravidla monitorování, zejména u rozsáhlých sítí, kde používáte velký počet uzlů pro monitorování. Tím se snižuje přenosy dat vytvářené řešení, tak k uspořádání monitorování sítě. 

Vytvoření pravidla monitorování podle obchodní logiky. Například pokud chcete monitorovat výkon síťové připojení dvou lokalit office na sídlo organizace, pak skupiny jsou podsítě v office web1 v síti O1, jsou podsítě v office site2 v síti O2 a všechny podsítě v sídlo organizace v síti H. Vytvoření pravidla: 1 mezi O1 a H a dalších až O2 H. monitorování 2 

Chcete-li vytvořit vlastní pravidla monitorování:

1. Klikněte na tlačítko **přidat pravidlo** v **monitorování** kartě a zadejte název pravidla a popis. 
2. Vyberte dvojice síť nebo podsíť odkazy na monitorování seznamů. 
3. Nejprve vyberte síť, ve kterém se nachází první podsíť/s, které vás zajímají z rozevíracího seznamu síť a potom vyberte z rozevíracího seznamu odpovídající podsítí podsítí/s. Vyberte **všechny podsítě** Pokud chcete monitorovat všechny podsítě v síťové propojení. Vyberte podobně jako jiných podsítí nebo s zájmu. A, můžete kliknout na **přidat výjimky** vyloučit monitorování pro konkrétní podsítí odkazy z výběru, které jste udělali. 
4. Volba mezi ICMP a TCP protokoly pro provádění syntetické transakce. 
5. Pokud nechcete, aby k vytvoření události stavu pro položky, které jste vybrali, poté zrušte **povolit sledování stavu na odkazy předmětem toto pravidlo**. 
6. Vyberte sledování podmínek. Můžete nastavit vlastní prahové hodnoty pro generování událostí stavu zadáním prahové hodnoty. Vždy, když je hodnota stavu překročí jeho vybraná prahová hodnota pro dvojici vybrané sítě nebo podsítí, je generována událost stavu. 
7. Klikněte na tlačítko **Uložit** konfiguraci uložíte. 

Po uložení monitorování pravidlo, můžete integrovat daného pravidla s správu výstrah kliknutím **vytvoření výstrahy**. Pravidlo výstrahy se automaticky vytvoří pomocí vyhledávacího dotazu a další potřebné parametry automaticky vyplněné. Pravidlo výstrahy může přijímat e mailové výstrahy, kromě existující výstrahy v rámci NPM. Výstrahy můžete spustit taky nápravné akce pomocí runbooků nebo můžete integrovat stávajících řešení pro správu služby pomocí webhooků. Můžete kliknout na **Spravovat výstrahy** upravit nastavení výstrah. 

Nyní můžete vytvořit další pravidla pro sledování výkonu nebo přesunout na řídicí panel řešení spustit pomocí funkce 

### <a name="choose-the-protocol"></a>Vyberte protokol

Monitorování výkonu v síti (NPM) používá k výpočtu metriky výkonu sítě jako paketu ztrátě a odkaz latence syntetické transakce. To lépe pochopit, zvažte agenta NPM připojené k jednom konci síťového propojení. Tento agent NPM odesílá pakety testu druhý Agent NPM připojené k jiné konec sítě. Druhý agenta odpoví odpovědí s odpovědí na žádost. Tento proces se opakuje několikrát. Měřením počet odpovědi a čas potřebný k přijetí každé odpovědi prvního agenta NPM vyhodnocuje odkaz latence a zahazování paketů. 

Formát, velikosti a pořadí tyto pakety je určen podle protokol, který zvolíte, při vytváření pravidla monitorování. Na základě protokolu paketů, mezilehlých síťových zařízení (směrovače, přepínače atd.) může zpracovat tyto pakety jinak. V důsledku toho zvoleného protokol ovlivní přesnost výsledků. A, vaše volba protokol také určuje, zda je nutné provést jakékoli ruční kroky, poté, co nasadíte řešení NPM. 

NPM nabízí volba mezi protokoly ICMP a TCP pro provádění syntetické transakce. Pokud si zvolíte ICMP, při vytváření pravidla syntetické transakce, agenti NPM ODEZVU protokolu ICMP zprávy použít k výpočtu latence sítě a ztráta paketů. ODEZVU protokolu ICMP používá stejnou zprávu, kterou posílá konvenční nástroj Ping. Použijete-li jako protokol TCP, NPM agenti odesílají paket TCP SYN přes síť. Následují TCP handshake dokončení a potom odebírání připojení pomocí RVNÍ paketů. 

Před vyberte protokol, který má používat, zvažte následující informace: 

**Zjišťování několik síťových tras.**  TCP je přesnější při zjišťování víc tras a je menší počet agentů v jednotlivých podsítích. Jeden nebo dva agenty pomocí protokolu TCP můžete například zjistit všechny redundantní cesty mezi podsítěmi. Musíte však několik agenty pomocí protokolu ICMP a dosáhnout tak podobné. Pokud máte několik tras mezi dvěma podsítěmi pomocí protokolu ICMP, je potřeba více než 5N agentů ve zdrojové nebo cílové podsíti. 

**Přesné výsledky.** Směrovače a přepínače zpravidla přiřadit ODEZVU protokolu ICMP pakety ve srovnání s TCP pakety s nižší prioritou. V některých situacích při síťová zařízení jsou velkém zatížení, data získat TCP přesněji odráží ztrátě a latence, které aplikace. K tomu dochází, protože většina dat aplikace probíhá přes protokol TCP. V takových případech ICMP poskytuje ve srovnání s TCP méně přesné výsledky. 

**Konfigurace brány firewall.** Protokolu TCP vyžaduje, aby TCP pakety odesílají na cílový port. Výchozí port je používán agenty NPM je 8084, ale toto můžete změnit, když konfigurujete agenty. Ano je potřeba zajistit, že síťové brány firewall nebo pravidla NSG (v Azure) jsou umožňuje přenosy na portu. Budete také muset Ujistěte se, že místní brány firewall na počítačích, kde jsou nainstalováni agenti konfigurace umožňuje provozu na tomto portu. Konfigurace pravidel brány firewall na počítačích s Windows, ale budete muset ručně nakonfigurovat bránu firewall vaší sítě můžete použít skripty prostředí PowerShell. Naproti tomu ICMP nefunguje pomocí portu. Ve většině scénářů organizace ICMP je povolen přenos přes bránu firewall umožňují použijte nástroj Diagnostika sítě jako nástroj Ping. Takže pokud jeden počítač z jiné, může odeslat příkaz Ping, můžete použít protokol ICMP bez nutnosti ruční konfigurace brány firewall. 

>[!NOTE] 
> Některé brány firewall může blokovat ICMP, což může vést k opakování přenosu výsledkem velké množství událostí v systému správy informace a událostí zabezpečení. Zajistěte, aby nebyl blokován protokol, který zvolíte, pomocí brány firewall sítě nebo NSG, jinak hodnota NPM není schopen monitorovat segmentu sítě. Z toho důvodu doporučujeme použít TCP pro sledování. ICMP využít ve scénářích kde nemůžete se použití protokolu TCP, jako např. kdy: 
>
> - Vzhledem k tomu, že nezpracovaná sockets TCP nejsou povoleny v klientovi Windows používáte uzlů na základě klienta Windows
> - Brána firewall vaší sítě nebo NSG blokuje TCP
> - Jak přepínat protokol 

Pokud jste se rozhodli použít ICMP během nasazení, můžete přepnout TCP kdykoli úpravou výchozí pravidlo monitorování:

1. Přejděte na **síťový výkon** > **monitorování** > **konfigurace**   >  **Monitorování** a pak klikněte na **výchozí pravidlo**. 
2. Přejděte k položce **protokol** a vyberte protokol, který chcete použít. 
3. Klikněte na tlačítko **Uložit** použití nastavení. 

I když výchozí pravidlo používá určitý protokol, můžete vytvořit nová pravidla s jiný protokol. Můžete například vytvořit směs pravidla, kde některé z pravidel pomocí protokolu ICMP a jiný používá protokol TCP. 

## <a name="walkthrough"></a>Názorný postup 

Teď, když jste si přečetli o sledování výkonu, podíváme se na jednoduchý vyšetřování příčiny stavu události.  

Na řídicím panelu řešení si všimnete, že existuje událost stavu – síťové propojení není v pořádku. Rozhodnete zkoumat problém a klikněte na **sítě odkazy monitorovaných** dlaždici.

Na stránce přejít k podrobnostem zjistíte, který **DMZ2 DMZ1** síťové propojení není v pořádku. Klikněte na **zobrazení podsítě odkazy** odkaz na tento odkaz sítě. 


Na stránce podrobností se zobrazí všechna propojení podsítí v **DMZ2 DMZ1** síťového propojení. Jste si všimli, že pro obě podsítí odkazy, latence překročila prahovou hodnotu, což není v pořádku síťové propojení. Zobrazí se také latence trendy podsítí odkazy. Můžete použít volbu čas ovládacího prvku grafu a zaměřit se na požadované časové rozmezí. Najdete v průběhu dne, kdy latence bylo dosaženo jeho ve špičce. Je možné později článek vyhledat v protokolech pro toto časové období můžete prozkoumat problém. Klikněte na tlačítko **zobrazení uzlu odkazy** k podrobnostem. 
 
 ![Propojení podsítí](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Podobně jako na předchozí stránku, na stránce podrobností pro konkrétní podsítí propojení uvedeny dolů jeho základní uzlu odkazy. Můžete provádět akce podobně jako zde, stejně jako v předchozím kroku. Klikněte na tlačítko **zobrazení topologie** zobrazíte topologie mezi dvěma uzly. 
 
 ![Propojení uzlů](media/log-analytics-network-performance-monitor/node-links.png) 

Všechny cesty mezi dva vybrané uzly jsou zobrazeny v mapě topologie. Můžete vizualizovat topologii směrování směrování trasy mezi dvěma uzly na mapě topologie. Poskytuje přehledné informace o tom, kolik tras mezi dva uzly a co cesty trvá datových paketů. Kritické body sítě jsou označeny červenou barvu. Chyba síťové připojení nebo chyba síťové zařízení, můžete vyhledat prohlížením red barevnou elementy na mapě topologie. 

 ![Řídicí panel topologie](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Mohou být zjišťovány ztrátu, latenci a počet skoků v každá cesta **akce** podokně. Posuvník slouží k zobrazení podrobností o těchto cestách není v pořádku. Pomocí filtrů můžete vybrat cesty s směrování není v pořádku, aby se vykreslí topologie pro pouze vybrané cesty. Kolečko myši můžete zvětšit nebo zmenšit mapy topologie. 

V pod obrázkem, jasně uvidíte hlavní příčinu problému oblasti, které mají určitou část sítě podle cesty a segmentů směrování v červenou barvu. Kliknutím na uzel v mapě topologie zjistí vlastnosti uzlu, včetně plně kvalifikovaný název domény a IP adresu. Kliknutím na směrování zobrazuje IP adresu směrování. 
 
![Řídicí panel topologie](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Další postup
* [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné sítě záznamů dat výkonu.
