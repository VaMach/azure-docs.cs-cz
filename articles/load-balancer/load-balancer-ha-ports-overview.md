---
title: "Přehled porty vysoké dostupnosti v Azure | Microsoft Docs"
description: "Další informace o na interní nástroj pro vyrovnávání zatížení porty vysokou dostupnost."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: f72f4a3a81fc3a03c86805787caeeacbe6135c5e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="high-availability-ports-overview"></a>Přehled porty vysokou dostupnost

Azure načíst vyrovnávání standardní umožňuje načíst vyrovnávání na všechny porty TCP a UDP toky současně, pokud používáte interní Vyrovnávání zatížení. 

>[!NOTE]
> Porty funkci vysoké dostupnosti (HA) jsou k dispozici standardní nástroje pro vyrovnávání zatížení a je aktuálně ve verzi preview. Verzi Preview funkci nemusí mít stejnou úroveň dostupnost a spolehlivost jako funkce, které jsou ve verzi obecné dostupnosti. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Zaregistrujte si standardní nástroje pro vyrovnávání zatížení ve verzi Preview pro používání portů HA s prostředky standardní nástroje pro vyrovnávání zatížení. Postupujte podle pokynů pro registraci ke službě Vyrovnávání zatížení [standardní preview](https://aka.ms/lbpreview#preview-sign-up) také.

Pravidlo porty HA je varianta pravidlo, které jsou nakonfigurované na vnitřní načíst vyrovnávání Standard Vyrovnávání zatížení. Použití nástroje pro vyrovnávání zatížení můžete zjednodušit tím, že poskytuje všechny TCP a UDP toky přicházejících na všech portech k interní načíst vyrovnávání Standard jednoho pravidla pro vyrovnávání zatížení. Rozhodnutí Vyrovnávání zatížení sítě se provádí na toku. To je založené na následujících pět řazené kolekce členů připojení: zdrojové IP adresy, zdrojového portu, cílové IP adresy, cílový Port a protokol.

Funkce porty HA vám pomůže se kritických scénářů, jako je vysoká dostupnost a dosah pro sítě virtuálních zařízení (hodnocení chyb zabezpečení) uvnitř virtuální sítě. Může také pomoct při velký počet portů musí být skupinu s vyrovnáváním zatížení. 

Funkce porty HA je nakonfigurovaná, když nastavíte porty front-end a back-end na **0**a protokol, který se **všechny**. Interní nástroj pro vyrovnávání zatížení prostředků pak vyrovnává všechny toky TCP a UDP, bez ohledu na číslo portu.

## <a name="why-use-ha-ports"></a>Proč používat porty HA?

### <a name="nva"></a>Virtuální síťová zařízení

NVAs můžete použít pro zabezpečení vaše úloha Azure z více typů ohrožení zabezpečení. Pokud v těchto scénářích používají NVAs, musí být spolehlivé a vysoce dostupné a musí škálovat pro vyžádání.

Jednoduše tak, že přidáním instance hodnocení chyb zabezpečení sítě k back-end fondu Azure interní služby Vyrovnávání zatížení a nakonfigurování pravidla pro vyrovnávání zatížení porty HA můžete dosáhnout těchto cílů.

HA porty poskytují některé výhody pro scénáře HA hodnocení chyb zabezpečení sítě:
- Rychlé převzetí služeb při selhání v pořádku instancí s sondy stavu jednotlivých instancí
- Vyšší výkon se Škálováním na více systémů pro  *n* -aktivní instance
- *N*-aktivní a aktivní – pasivní scénáře
- Povinnost komplexní řešení, jako je Apache ZooKeeper uzly pro monitorování zařízení

Následující diagram představuje nasazení střed a paprsek virtuální sítě. Vynuceného tunelování koncových jejich provoz na virtuální síť rozbočovače a prostřednictvím hodnocení chyb zabezpečení, před opuštěním důvěryhodné místa. NVAs jsou připojeni k interní zatížení vyrovnávání standardní s konfigurací portů HA. Veškerý provoz můžete zpracovat a předávat odpovídajícím způsobem.

![Diagram střed a paprsek virtuální síť s NVAs nasazené v režimu HA](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Pokud používáte NVAs, zkontrolujte u příslušného poskytovatele, jak nejlépe používat porty HA a jaké postupy se podporují.

### <a name="load-balancing-large-numbers-of-ports"></a>Velký počet portů Vyrovnávání zatížení

Můžete také použít HA porty pro aplikace, které vyžadují Vyrovnávání zatížení velkého počtu portů. Tyto scénáře můžete zjednodušit pomocí interní [standardní nástroje pro vyrovnávání zatížení](https://aka.ms/lbpreview) s vysokou DOSTUPNOSTÍ porty. Pravidlo Vyrovnávání zatížení jednu nahrazuje více jednotlivých pravidel, jeden pro každou port Vyrovnávání zatížení.

## <a name="region-availability"></a>Dostupnost v oblastech

Funkce porty HA je dostupná [stejné oblasti jako standardní nástroje pro vyrovnávání zatížení](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Náhled registrace

K účasti ve verzi preview funkci HA porty ve standardní nástroje pro vyrovnávání zatížení, registrace předplatného pro nástroj pro vyrovnávání zatížení [standardní preview](https://aka.ms/lbpreview#preview-sign-up). Můžete zaregistrovat pomocí Azure CLI 2.0 nebo prostředí PowerShell.

>[!NOTE]
>Registrace může trvat až jednu hodinu.

## <a name="limitations"></a>Omezení

Tady jsou podporované konfigurace nebo výjimky pro funkci HA porty:

- Na jednom front-endové konfiguraci protokolu IP může mít jedno pravidlo Vyrovnávání zatížení DSR s vysokou DOSTUPNOSTÍ porty, nebo můžete mít pravidlo služby load balancer jeden jiný DSR s vysokou DOSTUPNOSTÍ porty. Nemůže mít obě.
- Konfigurace protokolu IP rozhraní jedním síťovým může mít pouze jeden jiný DSR načíst pravidlo vyrovnávání s vysokou DOSTUPNOSTÍ porty. Nelze nakonfigurovat další pravidla pro tento příkaz ipconfig.
- Konfigurace protokolu IP rozhraní jedním síťovým může mít jeden nebo více DSR načíst pravidla vyrovnávání s vysokou DOSTUPNOSTÍ porty, ale všechny jejich odpovídajících front-endové konfigurace protokolu IP musí být jedinečný.
- Pokud jsou všechny pravidla Vyrovnávání zatížení HA porty (pouze DSR), může existovat společně pravidla Vyrovnávání zatížení pro dva (nebo více) odkazující na stejný fond back-end. Stejné je hodnota true, pokud jsou všechna pravidla jiný-HA porty (DSR a bez DSR). Pokud je kombinace HA porty a porty bez HA pravidla, ale dvě takové pravidla vyrovnávání zátěže nemohou existovat společně.
- Funkci porty HA není k dispozici pro protokol IPv6.
- Tok symetrie pro scénáře hodnocení chyb zabezpečení sítě je podporováno pouze jednu síťovou kartu. Zobrazit popis a diagram pro [síťových virtuálních zařízení](#nva). 



## <a name="next-steps"></a>Další kroky

- [Konfigurace portů HA na vnitřní zatížení vyrovnávání Standard](load-balancer-configure-ha-ports.md)
- [Další informace o standardní nástroje pro vyrovnávání zatížení preview](https://aka.ms/lbpreview)

