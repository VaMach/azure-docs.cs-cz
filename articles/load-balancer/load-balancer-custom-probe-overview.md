---
title: "Vlastní testy paměti nástroje pro vyrovnávání zatížení a stav monitorování | Microsoft Docs"
description: "Naučte se používat vlastní testy paměti pro vyrovnávání zatížení Azure k monitorování instancí za službou Vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 102c07ff0994b3b411f2a13d7a43c5398d5dfd42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understand-load-balancer-probes"></a>Porozumění testům nástroje pro vyrovnávání zatížení

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure nástroj pro vyrovnávání zatížení poskytuje schopnost sledovat stav instance serveru s použitím sondy. Když sondu přestane reagovat, nástroj pro vyrovnávání zatížení zastaví odesílání nových připojení k instanci není v pořádku. Neovlivní existující připojení a nová připojení se odesílají do pořádku instancí.

Cloudové služby rolí (role pracovního procesu a webové role) pomocí agenta hosta pro test monitorování. Při použití virtuálních počítačů za službou Vyrovnávání zatížení musí být nakonfigurované TCP nebo HTTP vlastní testy paměti.

## <a name="understand-probe-count-and-timeout"></a>Pochopení počet test a vypršení časového limitu

Postup kontroly závisí na:

* Počet úspěšně sondy, které umožňují instance popisky jako zapnutý.
* Počet neúspěšných sondy, které způsobí instance popisky jako dolů.

Časový limit a četnost hodnotu nastavenou v SuccessFailCount určit, zda je instance potvrzen spuštěna nebo není spuštěna. Na portálu Azure časový limit nastavena na dvojnásobek hodnota frekvence.

Test konfigurace všech instancí Vyrovnávání zatížení sítě pro koncový bod (to znamená, Vyrovnávání zatížení sítě sady) musí být stejné. To znamená, že nemůžete jiný test konfigurace pro každou instanci role nebo virtuální počítač v rámci stejné hostované služby pro konkrétní koncový bod kombinaci. Například každá instance musí mít identické místní porty a vypršení časových limitů.

> [!IMPORTANT]
> Sondu nástroje pro vyrovnávání zatížení používá IP adresu 168.63.129.16. Tato veřejná IP adresa usnadňuje komunikaci k prostředkům interní platformu pro bring vaše – vlastní-IP adresu Azure virtuální síť. Virtuální veřejnou IP adresu 168.63.129.16 se používá ve všech oblastech a nedojde ke změně. Doporučujeme povolit tuto IP adresu na všechny zásady místní brány firewall. Nemělo by se používat bezpečnostní riziko protože zdrojem může být pouze vnitřní platformy Azure zprávy z této adresy. Pokud není to uděláte, budou existovat neočekávanému chování v různých scénářích, jako jsou konfigurace stejného rozsahu IP adres 168.63.129.16 a s duplicitní IP adresy.

## <a name="learn-about-the-types-of-probes"></a>Další informace o typech sondy

### <a name="guest-agent-probe"></a>Test agenta hosta

Tento test je jenom pro cloudové služby Azure k dispozici. Nástroj pro vyrovnávání zatížení, využívá agent hosta ve virtuálním počítači a pak naslouchá a odpoví odpověď HTTP 200 OK jenom v případě, že instance je ve stavu připraveno (to znamená, není v jiném stavu například zaneprázdněn recyklace nebo ukončení).

Další informace najdete v tématu [konfigurace soubor definice služby (csdef) pro sondy stavu](https://msdn.microsoft.com/library/azure/ee758710.aspx) nebo [začínáte s vytvářením Vyrovnávání zatížení straně Internetu pro cloudové služby](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Díky sondu agenta hosta označit instance jako není v pořádku?

Pokud agenta hosta přestane reagovat s HTTP 200 OK, nástroje pro vyrovnávání zatížení označí instance jako reagovat a zastaví odesílání provozu do této instance. Nástroje pro vyrovnávání zatížení bude nadále příkazem ping otestovat instance. Pokud agenta hosta odpoví HTTP 200, nástroje pro vyrovnávání zatížení odešle provoz do této instance znovu.

Pokud používáte webovou roli, kód webu obvykle běží v w3wp.exe, který není monitorován pomocí Azure prostředků infrastruktury nebo hostovaného agenta. To znamená, že selhání v w3wp.exe (například odpovědi HTTP 500), nebudou ohlášena v agentovi hosta a nástroje pro vyrovnávání zatížení nebude tato instance mimo otočení.

### <a name="http-custom-probe"></a>Vlastní test paměti HTTP

Vlastní test paměti pro vyrovnávání zatížení HTTP přepíše výchozí kontroly agenta hosta, což znamená, že můžete vytvořit vlastní logiky k určení stavu role instance. Nástroje pro vyrovnávání zatížení sondy váš koncový bod každých 15 sekund, ve výchozím nastavení. Instance považován za v otočení nástroje pro vyrovnávání zatížení, pokud ji odpoví HTTP 200 během časového limitu (ve výchozím nastavení je 31 sekund).

To může být užitečné, pokud chcete implementovat vlastní logiky k odebrání instance otočení nástroje pro vyrovnávání zatížení. Například může rozhodnout odebrat instanci, pokud je vyšší než 90 % využití procesoru a vrátí stav bez 200. Pokud máte webové role, které používají w3wp.exe, to také znamená získáte automatické monitorování vašeho webu, protože selhání ve vašem kódu webu vrátí stav bez 200 sondu nástroje pro vyrovnávání zatížení.

> [!NOTE]
> Vlastní test paměti HTTP podporuje pouze protokoly HTTP a relativní cesty. Není podporován protokol HTTPS.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Díky HTTP vlastní test paměti označit instance jako není v pořádku?

* HTTP aplikace vrátí kód odpovědi HTTP než 200 (například 403, 404 nebo 500). Toto je kladné potvrzení, která instance aplikace by se dostala mimo službu hned.
* HTTP server neodpovídá vůbec po uplynutí časového limitu. V závislosti na hodnotu časového limitu, který je nastavený, to může znamenat, že více testu požadavků přejděte nezodpovězená před sondy získá označeno není spuštěna (který je před SuccessFailCount sondy odesílají).
* Server uzavře připojení přes TCP resetování.

### <a name="tcp-custom-probe"></a>Vlastní test paměti TCP

Sondy TCP iniciovat připojení k provádění třícestné s definovaný port.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Díky vlastní test paměti TCP označit instance jako není v pořádku?

* Protokolu TCP serveru neodpovídá vůbec po uplynutí časového limitu. Tato kontrola, když je označena jako neběží závisí na počtu selhání kontroly požadavků, které jste nakonfigurovali přejděte nezodpovězená před označením testu jako neběží.
* Tato kontrola obdrží TCP, obnovit z role instance.

Další informace o konfiguraci test stavu protokolu HTTP nebo TCP testu najdete v tématu [začít vytvářet ve Správci prostředků pomocí prostředí PowerShell k pro vyrovnávání zatížení internetového](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Přidání pořádku instancí zpět do otočení nástroje pro vyrovnávání zatížení

Sondy TCP a protokolu HTTP jsou považovány za v pořádku a označit instanci role jako v pořádku, pokud:

* Nástroje pro vyrovnávání zatížení získá sondu kladné poprvé spustí virtuální počítač.
* Číslo SuccessFailCount (popsanou výš) definuje hodnotu úspěšné sondy, které jsou nutné k označení instance role jako v pořádku. Pokud role instance byla odebrána, počet úspěšný, následných sondy musí rovnat nebo překročit hodnotu SuccessFailCount označit jako spuštěná instance role.

> [!NOTE]
> Pokud je kolísal stav instanci role, čeká, nástroje pro vyrovnávání zatížení déle než ji předáte role instance zpět do stavu v pořádku. To se provádí prostřednictvím zásad k ochraně uživatele a infrastruktury.

## <a name="use-log-analytics-for-load-balancer"></a>Pomocí analýzy protokolů pro vyrovnávání zatížení

Můžete použít [protokolu analýzy nástroje pro vyrovnávání zatížení](load-balancer-monitor-log.md) zkontrolujte na kontrolu stavu a počet testu. Protokolování můžete použít s Power BI nebo statistice provozu Azure poskytnout statistiky o stav nástroj pro vyrovnávání zatížení.
