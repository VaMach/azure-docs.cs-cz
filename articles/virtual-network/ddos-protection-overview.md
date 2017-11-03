---
title: "Standardní Přehled ochrany Azure DDoS | Microsoft Docs"
description: "Další informace o službě Azure ochrana proti útoku DDoS."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 76da0d4e805c732d40a7bd02e5c70973c792e26c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>Přehled služby Azure DDoS ochrany standardní

>[!IMPORTANT]
>Azure DDoS ochrany standardní je aktuálně ve verzi preview. Omezený počet prostředků Azure podpory DDoS ochrany standardní a vyberte počet oblastí. Budete muset [zaregistrovat pro službu](http://aka.ms/ddosprotection) během omezené preview získat DDoS ochrany standardní pro vaše předplatné povolený. Se vás kontaktovat tým Azure DDoS při registraci, který vás provede procesem povolování. DDoS ochrany Standard je k dispozici v oblastech USA – Východ USA – západ a – Západ střední USA. Verzi Preview se vám neúčtují poplatky za používání služby.

Distribuované útoky Denial of Service (DDoS) jsou jedním z největší dostupnost a zabezpečení se týká přístupných zákazníkům přesunutí svých aplikací do cloudu. Útoku DDoS pokusí vyčerpat prostředky aplikace, které se znepřístupní aplikace oprávněných uživatelů. Útoky DDoS můžete cílit na všechny koncový bod, který je veřejně dostupný prostřednictvím Internetu.

Azure ochrana proti útoku DDoS v kombinaci s osvědčenými postupy návrhu aplikace společně ubrání proti těmto útokům. Jsou k dispozici tyto dvě služby úrovně: 

- **Azure DDoS ochrany základní** -je automaticky povolené v rámci platformy Azure bez dalších poplatků. Vždy na provoz monitorování a v reálném čase zmírnění běžné útokům na úrovni síť poskytuje stejné obrany využívaných online službách společnosti Microsoft.  Celého rozsahu globální sítě Azure slouží k distribuci a zmírnit útok provoz v oblastech. 
- **Azure DDoS ochrany standardní** -poskytuje možnosti Další zmírnění přizpůsobená speciálně pro virtuální síťové prostředky. Je snadno povolit a nevyžaduje žádné změny aplikace. Zásady ochrany jsou přizpůsobená prostřednictvím monitorování vyhrazené provozu a machine learning algoritmy a použity na veřejné IP adresy přidružené virtuálním síťovým prostředkům, například nástroj pro vyrovnávání zatížení, aplikační brány a Service Fabric instancí.  Při útoku a historie je k dispozici prostřednictvím Azure monitorování zobrazení telemetrická data v reálném čase. Ochrana vrstvy aplikace mohou být přidány prostřednictvím [brány Firewall webových aplikací aplikace brány](https://azure.microsoft.com/services/application-gateway/). 

![Standardní ochrany Azure DDoS](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

Doporučujeme vám opakujte DDoS ochrany standardní vývoj, testovací nebo produkční prostředí. Použijte v následujících zdrojích informací k poskytnutí zpětné vazby na vaše prostředí:
- [Ochrana Azure proti útoku DDoS ve fóru Microsoft Azure](https://feedback.azure.com/forums/905032-azure-ddos-protection). 
- [Ochrana Azure proti útoku DDoS ve fóru MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Ochrana Azure proti útoku DDos na přetečení zásobníku](https://stackoverflow.com/tags/azure-ddos/info)

Pro problémy podpory můžete [otevřete lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md). Při DDoS ochrany standardní je ve verzi preview, podpora je k dispozici na základě typu best effort.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typy útoků DDoS, které DDoS ochrany standardní snižuje

DDoS ochrany standardní zmírnit tyto typy útoků:

- **Odměrné útoky** -cílem útok je k vyplnění síťovou vrstvou s vyžadovat značné množství zdánlivě legitimní provoz. Obsahuje UDP záplavy, záplavy zesílení a dalších záplavy maskování paketů. DDoS ochrany standardní snižuje tyto možných útoků, které více gigabajt podle přijmout & čištění je automaticky využití Škálováním globální sítě Azure. 
- **Protokol útoky** – tyto útoky vykreslení cíl nepřístupný zneužitím slabé místo v zásobníku protokolu vrstvy 3 a 4 vrstvy. Obsahuje, útokům zahlcení SYN, reflexe útokům a jiným útokům protokolu. DDoS ochrany standardní snižuje tyto útoky rozlišování škodlivý a legitimní provoz interakci s klienta a blokovat škodlivý přenos. 
- **Útoky na aplikace** – tyto útoky cílové webové aplikace pakety narušit přenosu dat mezi hostiteli. Obsahuje porušení protokolu HTTP, SQL vkládání, skriptování mezi servery a jiné útoky vrstvy 7. Pomocí aplikace brány firewall webových aplikací s DDoS ochrany standardní zajistit ochranu proti těmto útokům. 

DDoS ochrany standardní chrání prostředky ve virtuální síti, včetně veřejné IP adresy přidružené virtuální počítače, interní služby load balancer a aplikačních bran. Při kombinaci s aplikací skladová položka brány firewall webových aplikací, může DDoS ochrany standardní poskytnout úplné L3 k L7 zmírnění schopnosti.

## <a name="ddos-protection-standard-features"></a>DDoS ochrany standardní funkce

![Funkce DDoS](./media/ddos-protection-overview/ddos-overview-fig1.png)

DDoS ochrany standardní funkce patří: 

- **Integrace nativní platformy:** DDoS ochrany Standard je nativně integrováno do Azure a zahrnuje konfiguraci prostřednictvím portálu Azure a prostředí PowerShell. DDoS ochrany standardní rozumí vašich prostředků a konfigurace prostředků.
- **Monitorování vždy v provozu:** vzory přenosů dat vaší aplikace jsou monitorované 24 x 7, hledá indikátory útoků DDoS. Zmírnění dopadů se provádí při překročení zásady ochrany.
- **Klíč ochrany:** zjednodušená konfigurace okamžitě chrání všechny prostředky ve virtuální síti, jakmile je povoleno DDoS ochrany standardní. Je vyžadována definice bez zásahu nebo uživatel – standardní ochrany DDoS okamžitě a automaticky snižuje útoku po byla zjištěna.
- **Adaptivní ladění:** inteligentního provoz profilace zjišťuje provoz vaší aplikace v průběhu času a vybere a aktualizuje profil, který je nejvhodnější pro vaši službu. Profil upraví jako provoz v průběhu času mění.
- **L3 L7 ochranu pomocí služby application gateway:** aplikace brány firewall webových aplikací funkce Ochrana proti útoku DDoS poskytnete úplné zásobníku.
- **Zmírnění dopadů rozsáhlé škálování:** přes 60 útoku různé typy lze zmírnit s globální kapacity pro ochranu proti největší známé útoky DDoS. 
- **Útokům metriky:** Summarized metriky z každé útoku jsou přístupné prostřednictvím Azure monitorování.
- **Útok výstrahy:** výstrah lze nakonfigurovat na spuštění a ukončení útoku a přes útoku je doba trvání pomocí předdefinovaných útoku metriky. Výstrahy integrovat do vaší provozní softwaru, třeba OMS, Splunk, Azure Storage, e-mailu a portálu Azure.
- **Cenově záruku:** přenosu dat a aplikací Škálováním na více systémů kompenzace zdokumentovaných útoku DDoS.

## <a name="ddos-protection-standard-mitigation"></a>Zmírnění dopadů DDoS ochrany standardní

Ochrana proti útoku DDoS služby společnosti Microsoft monitoruje provoz skutečné využití a porovná je neustále s definované v zásadách DDoS prahové hodnoty. Při překročení prahové hodnoty provoz DDoS zmírnění iniciován automaticky. Pokud provoz vrátí pod prahovou hodnotou, odebere se zmírnění dopadů.

Během zmírnění dopadů provoz směrem k chráněnému prostředku je přesměrován službou ochrana proti útoku DDoS a několik ověřování. Těmito kontrolami obecně provést následující funkce:

- Zkontrolujte pakety specifikacím Internet a nejsou poškozené.
- Komunikovat s zjistit, jestli je potenciálně zfalšovaný paket klienta (např: SYN Auth nebo soubor Cookie SYN nebo vyřazení paket pro zdrojů a přenést ho znovu).
- Pokud lze provést žádnou jinou metodu vynucení omezení četnosti paketů.

Ochrana proti útoku DDoS bloky útokům provozu a zbývající dál provoz určený cíl. Během několika minut detekce útoku budete upozorněni, používání Azure monitorování metriky. Konfigurace protokolování na DDoS ochrany standardní telemetrie, můžete napsat protokoly dostupné možnosti pro budoucí analýzu. Metriky dat v Azure monitorování pro DDoS ochrany Standard se aktuálně uchovávají po dobu 30 dnů.

Nedoporučujeme zákazníků k simulaci vlastní útoky DDoS. Místo toho zákazníci mohou používat podporu kanál k vyžádání DDoS útokům simulace provedený sítí Azure. Technika vás bude kontaktovat uspořádání podrobnosti útoku DDoS (porty, protokoly, cílové IP adresy) a uspořádat dobu a naplánovat test.

## <a name="next-steps"></a>Další kroky

- Další informace o správě DDoS ochrany standardní pomocí [prostředí Azure PowerShell](ddos-protection-manage-ps.md) nebo [portál Azure](ddos-protection-manage-portal.md).
