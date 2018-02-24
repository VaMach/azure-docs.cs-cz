---
title: "Síťová řešení pro sledování výkonu v Azure Log Analytics | Microsoft Docs"
description: "Funkce ExpressRoute správce v nástroji Sledování výkonu sítě umožňuje sledovat výkon mezi pobočkami a Azure a začátku do konce připojení přes Azure ExpressRoute."
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
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>Správce ExpressRoute

Funkce správce ExpressRoute v [sledování výkonu sítě](log-analytics-network-performance-monitor.md) umožňuje sledovat výkon mezi pobočkami a Azure a začátku do konce připojení přes Azure ExpressRoute. Jsou klíčové výhody: 

- Automatickou detekci. hodnoty okruhy ExpressRoute, které jsou spojené s vaším předplatným 
- Sledování využití šířky pásma, ztráta a latence v okruhu, vztahy a úroveň virtuální sítě pro vaše ExpressRoute 
- Zjišťování topologie sítě vaší okruhy ExpressRoute 

![Monitorování ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Konfigurace 
Chcete-li spustit nástroj konfigurace pro sledování výkonu sítě, otevřete [řešení pro sledování výkonu sítě](log-analytics-network-performance-monitor.md) a klikněte na tlačítko **konfigurace** tlačítko.

### <a name="configure-nsg-rules"></a>Konfigurace pravidla NSG 
Pro servery v Azure, které jsou používány pro monitorování přes NPM je nutné nakonfigurovat pravidla skupiny (NSG) zabezpečení sítě tak, aby umožnila přenosy TCP na portu používá NPM pro syntetické transakce. Výchozí port je 8084. To umožňuje agent OMS nainstalovaný na virtuálním počítači Azure ke komunikaci s místní monitorování agenta. 

Další informace o NSG najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Ujistěte se, že jste nainstalovali agenty (místní server agent a Azure server agenta) a spuštění skriptu EnableRules.ps1 PowerShell před pokračováním se tento krok. 

 
### <a name="discover-expressroute-peering-connections"></a>Zjistit partnerský vztah ExpressRoute připojení 
 
1. Klikněte na **partnerských vztahů ExpressRoute** zobrazení.  
2. Klikněte na **zjistit teď** tlačítko vyhledat všechny ExpressRoute soukromé partnerské vztahy, které jsou připojené k virtuálním sítím v rámci předplatného Azure spojena se tento pracovní prostor analýzy protokolů.  

>[!NOTE]  
> Řešení aktuálně zjišťuje pouze soukromé partnerské vztahy ExpressRoute. 

>[!NOTE]  
> Pouze ty soukromé partnerské vztahy jsou zjišťovány, které jsou připojené k virtuální sítě přidružený k odběru spojena se tento pracovní prostor analýzy protokolů. Pokud vaše ExpressRoute se připojit k virtuálním sítím mimo předplatné propojené do tohoto pracovního prostoru, budete muset vytvořit pracovní prostor analýzy protokolů v těchto předplatných a monitorování těchto partnerských vztahů pomocí NPM. 

 ![Konfigurace ExpressRoute monitorování](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Po dokončení zjišťování zjištěných soukromého partnerského vztahu připojení jsou uvedeny v tabulce. Monitorování pro tyto partnerské vztahy budou zpočátku v zakázaném stavu. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Povolit monitorování připojení partnerského vztahu ExpressRoute 

1. Klikněte na privátní partnerský vztah můžete připojení jsou zajímá monitorování.  
2. V podokně RHS klikněte na zaškrtávací políčko pro **sledovat tento partnerský vztah**. 
3. Pokud máte v úmyslu vytvořit události stavu pro toto připojení, zkontrolujte **povolit sledování stavu pro tento partnerský vztah**. 
4. Vyberte sledování podmínek. Můžete nastavit vlastní prahové hodnoty pro generování událostí stavu zadáním prahové hodnoty. Vždy, když je hodnota stavu překročí jeho vybraná prahová hodnota pro připojení k partnerského vztahu, je generována událost stavu. 
5. Klikněte na **přidat agenty** zvolit sledování agentů máte v úmyslu použít pro sledování tohoto partnerského vztahu připojení. Je potřeba zajistit, abyste přidali agentů na obou koncích připojení, alespoň jeden agent ve virtuální síti Azure připojené k tohoto partnerského vztahu i alespoň jeden místní agent připojené k tohoto partnerského vztahu. 
6. Klikněte na tlačítko **Uložit** konfiguraci uložíte. 

![Konfigurace ExpressRoute monitorování](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Po povolení pravidla a výběr hodnoty a agentů, které chcete monitorovat, je Počkejte přibližně 30 – 60 minut pro hodnoty k zahájení naplňování a **ExpressRoute monitorování** dlaždice k dispozici. Jakmile najdete v části monitorování dlaždice okruhů ExpressRoute a připojení prostředky jsou monitorovány pomocí NPM. 

>[!NOTE]
> Tato funkce funguje spolehlivé na pracovní prostory, které jste upgradovali na nový dotazovací jazyk.  

## <a name="walkthrough"></a>Názorný postup 

Monitorování výkonu sítě řídicího panelu ukazuje přehled stavu okruhy ExpressRoute a připojení partnerského vztahu. 

![ExpressRoute NPM řídicí panel](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Okruhy seznamu 

Chcete-li zobrazit seznam všech monitorovaných okruhy ExpressRoute, klikněte na dlaždici okruhů ExpressRoute. Můžete vybrat okruhu a zobrazit její stav, trend grafy pro ztráta paketů, využití šířky pásma a latence. Grafy, jsou interaktivní. Můžete vybrat vlastní časový interval pro vykreslení grafy. Přes oblast můžete přetáhnout myší na graf přiblížení a zobrazit podrobné datových bodů. 

![Seznam okruhy ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Trend ztrátě, latence a propustnosti 

Šířku pásma, latence a ztráta grafy, jsou interaktivní. Můžete zvětšení všechny části tyto grafy, použití ovládacích prvků myši. Zobrazí se také, že šířku pásma, latence a ztráta dat pro další intervaly kliknutím na datum a čas, nacházel pod tlačítko akce v levé horní části. 

![Latence ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Seznam partnerských vztahů 

Kliknutím na **soukromé partnerské vztahy** dlaždice na řídicím panelu zobrazí seznam všech připojení k virtuálním sítím přes soukromého partnerského vztahu. Tady můžete vybrat virtuální síťové připojení a zobrazit její stav, trend grafy pro ztráta paketů, využití šířky pásma a latence. 

![Partnerské vztahy ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Okruh topologie 

Chcete-li zobrazit okruh topologie, klikněte na **topologie** dlaždici. Tím přejdete na zobrazení topologie vybrané okruhu nebo partnerský vztah. Diagram topologie poskytuje latence pro každý segment v síti a každé směrování vrstvy 3 je reprezentována uzlu diagramu. Kliknutím na směrování, zobrazí se další podrobnosti o směrování. Můžete zvýšit úroveň viditelnosti zahrnout směrování místní přesunutím posuvníku níže **filtry**. Přesunutí jezdce doleva nebo doprava, zvýšení nebo snížení počtu směrování v grafu topologie. Latence v každém segmentu je viditelná, což umožňuje rychlejší izolace vysokou latencí segmentů ve vaší síti. 

![Topologie ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Podrobné zobrazení topologie okruhu 

Toto zobrazení uvádí připojení mezi sítěmi VNet. 

![Virtuální síť ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnostika 

Sledování výkonu sítě umožňuje diagnostikovat problémy s připojením k několika okruh. Níže jsou uvedeny některé problémy 

**Okruh je mimo provoz.** NPM vás upozorní, jakmile dojde ke ztrátě připojení mezi místními prostředky a virtuální sítě Azure. To vám pomůže proaktivní akce před příjmem uživatele eskalací a zkrátit čas dolů 

![Okruh ExpressRoute dolů](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Provoz v není určený okruh.** NPM vás může upozornit, vždy, když není provoz neočekávaně předávaných mezi určený okruh ExpressRoute. Tomu může dojít, pokud okruh je mimo provoz a přenosy dat je předávaných mezi zálohování trasy, nebo pokud nastane problém s směrování. Tyto informace pomáhají aktivně spravovat problémy s konfigurací v vaše zásady směrování a ujistěte se, že trasy, která nejvíce optimální a zabezpečení se používá. 

 

**Přenosy dat není předávaných mezi primární okruh.** Možnosti vás upozorní, když je provoz předávaných mezi sekundární okruh ExpressRoute. I když nebude setkáte s problémy s připojením v takovém případě však proaktivně odstraňování problémů s primární okruh požadovat, abyste lépe připravené. 

 
![Tok přenosů ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Snížení výkonu z důvodu využití ve špičce.** Mohou korelovat trend využití šířky pásma s trendu latence a určete, jestli úloha Azure snížení je z důvodu ve špičce ve využití šířky pásma, nebo Ne a odpovídajícím způsobem proveďte akce.  

![Monitorování ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Další postup
* [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné sítě záznamů dat výkonu.
