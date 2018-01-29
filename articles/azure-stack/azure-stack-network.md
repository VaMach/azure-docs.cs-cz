---
title: "Sítě aspekty integrace pro Azure zásobníku integrované systémy | Microsoft Docs"
description: "Zjistěte, co můžete dělat plánování integrace sítě datového centra s několika uzly Azure zásobníku."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 43d79a8c14751ee25eaca7a3b50649702d159d76
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
## <a name="network-connectivity"></a>Připojení k síti
Tento článek obsahuje informace o infrastruktuře síťový zásobník Azure vám pomohou rozhodnout, jak nejlépe zásobník Azure integrovat do stávajícího síťového prostředí. 

> [!NOTE]
> Chcete-li vyřešit externí názvy DNS z Azure zásobníku (například www.bing.com), potřebujete poskytovat servery DNS pro předávání DNS požadavků. Další informace o požadavcích na Azure DNS zásobníku najdete v tématu [zásobník Azure datacenter integrace - DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Návrh fyzické sítě
Řešení Azure zásobníku vyžaduje odolné a vysoce dostupné fyzické infrastruktury pro podporu jeho operace a služeb. Následující diagram představuje naše doporučený návrh:

![Doporučený návrh sítě Azure zásobníku](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Logické sítě
Logické sítě představují abstrakci podkladové fyzické síťové infrastruktury. Používají se k organizaci a zjednodušení přiřazení v síti pro hostitele, virtuální počítače a služby. V rámci vytváření logické sítě síťové lokality nejsou vytvářeny zadat virtuální místní sítě (VLAN), podsítě IP a páry IP podsíť či síť VLAN, které jsou přidružené logické sítě v každém fyzické umístění.

V následující tabulce jsou uvedeny logické sítě a přidružené podsítě rozsahy IPv4, které je nutné naplánovat:

| Logické sítě | Popis | Velikost | 
| -------- | ------------- | ------------ | 
| Veřejná virtuální IP adresy | Veřejné IP adresy pro malého služby Azure zásobníku se zbytkem používat klientské virtuální počítače. Infrastruktura Azure zásobníku používá 32 adres z této sítě. Pokud máte v úmyslu používat službu App Service a poskytovateli prostředků SQL, se používají 7 víc adres. | / 26 (62 hostitelé) – /22 (1022 hostitele)<br><br>Doporučená = /24 (254 hostitele) | 
| Přepínač infrastruktury | Point-to-Point IP adresy pro účely směrování, vyhrazené přepínače rozhraní pro správu a zpětné smyčky adresy přiřazené k přepínači. | /26 | 
| Infrastruktura | Používá se pro interní součásti zásobníku Azure ke komunikaci. | /24 |
| Privátní | Použít pro sítě úložiště a privátní virtuální IP adresy. | /24 | 
| BMC | Používá ke komunikaci s bmc na fyzických hostitelích. | /27 | 
| | | |

## <a name="network-infrastructure"></a>Síťová infrastruktura
Síťové infrastruktury pro Azure zásobníku se skládá z několika logické sítě, které jsou nakonfigurované v přepínačích. Následující diagram znázorňuje tyto logické sítě a způsob jejich integraci s top-of-rack (TOR), řadiče pro správu základní desky (BMC) a ohraničení přepínače (síť zákazníka).

![Diagram a přepínače připojení logické sítě](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC sítě
Tato síť je vyhrazen až po připojení všech řadiči pro správu (také označované jako služby procesory, například iDRAC, iLO, iBMC, atd.) pro síť pro správu. Pokud je k dispozici, hostitele životního cyklu hardwaru (HLH) se nachází v této síti a může poskytnout určitým softwarem OEM pro údržbu hardwaru nebo monitorování. 

### <a name="private-network"></a>Privátní síť
Tato /24 254 hostitele sítě (IP na) je privátní oblast Azure zásobníku (nerozšiřuje nad rámec zařízení přepínač ohraničení oblasti Azure zásobníku) a je rozdělená do dvou podsítí:

- **Síť úložiště**. Migrace za provozu /25 126 hostitele sítě (IP na) použitá k podpoře použití prostorů Direct a Server Message Block (SMB) přenosy dat úložiště a virtuální počítač. 
- **Interní virtuální síť IP**. A/25 síti vyhrazený pro interní jen VIP pro vyrovnávání zatížení softwaru.

### <a name="azure-stack-infrastructure-network"></a>Síť infrastruktury Azure zásobníku
To/24 síť je vyhrazená pro interní součásti zásobníku Azure, aby se mohli komunikovat a výměnu dat mezi sebou. Tato podsíť vyžaduje směrovatelné IP adresy, ale se ukládají privátní do řešení pomocí seznamů řízení přístupu (ACL), není očekáván směrování nad rámec přepínače ohraničení s výjimkou velmi malé rozsah ekvivalentní velikost/27 sítě využívaných některé z těchto služby, při které vyžadují přístup k externím prostředkům nebo k Internetu. 

### <a name="public-infrastructure-network"></a>Infrastruktura veřejných sítě
To/27 síť je velmi malé rozsahu podsítě infrastruktury Azure zásobníku, již bylo zmíněno dříve, nevyžaduje veřejné IP adresy, ale vyžaduje přístup k Internetu prostřednictvím NAT nebo transparentní proxy server. Tato síť se přidělí pro nouzové obnovení konzoly System (ERCS), virtuální počítač ERCS během registrace do Azure vyžaduje přístup k Internetu a musí být směrovatelné vaší síti pro správu pro účely odstraňování potíží.

### <a name="public-vip-network"></a>Veřejné síti VIP
Veřejné síti VIP je přiřazen do síťového adaptéru v zásobníku Azure. Není logická síť na přepínači. SLB pomocí fondu adres a přiřadí/32 sítě pro úlohy klientů. V tabulce směrování přepínače jsou inzerovaný těchto 32 IP adres jako trasu k dispozici prostřednictvím protokolu BGP. Tato síť obsahuje externí dostupný nebo veřejné IP adresy. Infrastruktura Azure zásobníku používá aspoň 8 adres z této veřejné síti VIP, zatímco zbytek používá klientské virtuální počítače. Velikost sítě v této podsíti může být v rozsahu od minimálně /26 (64 hostitele) do maximálně /22 (1022 hostitele), doporučujeme, abyste naplánovali pro/24 sítě.

### <a name="switch-infrastructure-network"></a>Přepínač síti infrastruktury.
To, 26 sítě je podsítě, která obsahuje podsítě/30 IP směrovatelné point-to-point (2 hostitele IP na) a vytváření, které jsou vyhrazené zpětných smyček nebo přepínač 32 podsítě pro integrované správy a ID směrovače protokolu BGP Tento rozsah IP adres musí být směrovatelné externě z řešení Azure zásobníku vašeho datového centra, mohou být privátní nebo veřejné IP adresy.

### <a name="switch-management-network"></a>Síť pro správu přepínače
Tato /29 (6 hostitele IP adresy) sítě je vyhrazen pro správu porty přepínačů připojení. To umožňuje přístup out-of-band pro nasazení, správu a řešení potíží. Vypočítá ze sítě infrastruktury přepínač uvedených výše.

## <a name="publish-azure-stack-services"></a>Publikování služby Azure zásobníku
Budete muset zpřístupnění služby Azure zásobníku uživatelům z mimo Azure zásobníku. Azure zásobníku nastaví různé koncové body pro její role infrastruktury. Tyto koncové body jsou přiřazeny virtuální IP adresy z fondu veřejných IP adres. Položka DNS se vytvoří pro každý koncový bod v externí zónu DNS, který byl zadán v době nasazení. Například portál user portal je přiřazen záznam hostitele DNS portálu.  *&lt;oblast >.&lt; plně kvalifikovaný název domény >*.

### <a name="ports-and-urls"></a>Porty a adresy URL
Aby služby Azure zásobníku (například portálů, Azure Resource Manager, DNS, atd.) k dispozici pro externí sítě, musíte povolit příchozí přenosy s těmito koncovými body pro konkrétní adresy URL, porty a protokoly.
 
V nasazení, kde transparentní proxy odesílání tradiční proxy serveru, musíte povolit určité porty a adresy URL pro odchozí komunikaci. Mezi ně patří porty a adresy URL pro identitu, syndikace marketplace, opravy a aktualizace, registrace a data o využití.

Další informace naleznete v tématu:
- [Příchozí porty a protokoly](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Odchozí porty a adresy URL](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)


## <a name="next-steps"></a>Další postup
[Azure připojení ohraničení zásobníku](azure-stack-border-connectivity.md)