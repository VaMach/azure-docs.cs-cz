---
title: "Pomocí Azure DNS pro privátní domény | Microsoft Docs"
description: "Přehled privátní DNS, který je hostitelem služby v Microsoft Azure."
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: 
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: 95cf8ab2bd34e698e12452e062687219bad49eb6
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/21/2017
---
# <a name="using-azure-dns-for-private-domains"></a>Pomocí Azure DNS pro privátní domény
Systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) název služby na jeho IP adresu. Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure.  Kromě internetových domén DNS Azure DNS teď také podporuje privátní DNS domény jako funkce preview.  
 
Azure DNS poskytuje spolehlivé, zabezpečené služby DNS pro správu a překládat názvy domén ve virtuální síti, aniž by bylo nutné přidat vlastní řešení DNS. Zóny DNS privátní umožňují použít vlastní názvy vlastních domén namísto názvů Azure, které jsou dnes k dispozici.  Pomocí vlastních názvů domén umožňuje přizpůsobit Architektura virtuální sítě podle potřeb vaší organizace. Poskytuje překlad názvů pro virtuální počítače v rámci virtuální sítě a mezi virtuálními sítěmi. Kromě toho můžete nakonfigurovat názvy zón s rozdělení horizon zobrazení – povolení, privátní a veřejné zóny DNS sdílet stejný název.

![Přehled systému DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="benefits"></a>Výhody

* **Eliminuje nutnost pro vlastní řešení DNS.** Mnoho zákazníků vytvořili vlastní DNS řešení pro správu zón DNS ve své virtuální síti.  Správa zóny DNS lze nyní provést pomocí nativní infrastrukturu Azure, která odebere zatížení vytváření a správě vlastních řešení DNS.

* **Použijte všechny běžné typy záznamů DNS.**  Azure DNS podporuje záznamy A, AAAA, CNAME, MX, NS, PTR, SOA, SRV a TXT.

* **Správa záznamů automatické název hostitele.** Společně s hostování svoje vlastní záznamy DNS, Azure automaticky udržuje záznamy název hostitele pro virtuální počítače v zadané virtuální sítě.  To umožňuje optimalizovat názvy domén, které můžete použít bez nutnosti vytvářet vlastní řešení DNS nebo upravit aplikaci.

* **Rozlišení názvu hostitele mezi virtuálními sítěmi.** Na rozdíl od názvů hostitelů Azure může být privátní zóny DNS sdílena mezi virtuálními sítěmi.  Tato funkce zjednodušuje scénáře napříč sítí a službu zjišťování například partnerský vztah virtuální sítě.

* **Známých nástrojů a činnost koncového uživatele.** K redukovat křivku, používá tato nová nabídka již zavedené nástroje Azure DNS (PowerShell, šablony Resource Manageru, REST API).

* **Rozdělení horizon DNS podporují.** Azure DNS vám umožní vytvořit zóny se stejným názvem, který odkazující na různých odpovědi z virtuální sítě a z veřejného Internetu.  Typický scénář v nástroji rozdělení horizon DNS je poskytnout vyhrazené verzi služby pro použití ve virtuální síti.


## <a name="pricing"></a>Ceny

Zóny DNS privátní je zdarma spravované verzi Preview. Při obecné dostupnosti si tato funkce bude používat na základě využití cenové model podobná existující Azure DNS nabídky. 


## <a name="next-steps"></a>Další kroky

Zjistěte, jak [vytvořit zónu DNS privátní](./private-dns-getstarted-powershell.md) v Azure DNS.

Další informace o zóny DNS a záznamy, navštivte stránky: [DNS zóny a zaznamenává přehled](dns-zones-records.md).

Informace o některých dalších klíčových [možnostech sítě](../networking/networking-overview.md) v Azure.

