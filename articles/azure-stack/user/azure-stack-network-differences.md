---
title: "Azure zásobník sítě: Rozdíly a důležité informace o"
description: "Další informace o rozdílech a důležité informace při práci se sítěmi v Azure zásobníku."
services: azure-stack
keywords: 
author: ScottNapolitan
ms.author: victorh
ms.date: 9/25/2017
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 7b7bac508a759a1367ac7328840848efe17ea3c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="considerations-for-azure-stack-networking"></a>Důležité informace týkající se sítě Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Sítě v zásobníku Azure poskytuje řadu funkcí, které můžete najít v Azure, s některé rozdíly, na které byste měli porozumět před zahájením nasazování.


Tento článek obsahuje přehled jedinečné aspekty pro sítě a jeho funkce v zásobníku Azure. Další informace o nejvýraznějších rozdílů mezi zásobník Azure a Azure, najdete v článku [klíčové aspekty](azure-stack-considerations.md) tématu.


## <a name="cheat-sheet-networking-differences"></a>Tahák: rozdíly sítě

|Služba | Funkce | Azure (globální) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Více klientů DNS | Podporuje se| Není dosud podporován.|
| |Záznamy DNS AAAA|Podporuje se|Nepodporuje se|
| |Zóny DNS na předplatné|100 (výchozí)<br>Je možné zvýšit na vyžádání.|100|
| |Sady záznamů DNS na zónu|5000 (výchozí)<br>Je možné zvýšit na vyžádání.|5000|
||Názvové servery pro delegování zóny|Azure poskytují čtyři názvové servery pro každou zónu uživatele (klientů), který je vytvořen.|Zásobník Azure poskytuje dva názvové servery pro každou zónu uživatele (klientů), který je vytvořen.|
| Virtuální síť|Partnerské vztahy virtuálních sítí|Připojení dvou virtuálních sítí ve stejné oblasti přes Azure páteřní síti.|Není dosud podporován.|
| |Adresy IPv6|Můžete přiřadit adresu IPv6 v rámci [konfigurace síťového rozhraní](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|Podporovaný je jenom protokol IPv4.|
|VPN Gateway|Brána sítě VPN Point-to-Site|Podporuje se|Není dosud podporován.|
| |Bránu Vnet-to-Vnet|Podporuje se|Není dosud podporován.|
| |SKU služby VPN Gateway|Podpora pro Basic, GW1, GW2, GW3, standardní vysoký výkon, velmi vysoký výkon. |Podpora Basic, Standard a vysoce výkonné SKU.|
|Nástroj pro vyrovnávání zatížení|Veřejné IP adresy IPv6|Podpora pro přiřazení ke službě Vyrovnávání zatížení veřejnou IP adresu IPv6.|Podporovaný je jenom protokol IPv4.|
|Network Watcher|Sledovací proces sítě klienta možnosti monitorování sítě|Podporuje se|Není dosud podporován.|
|CDN|Profily sítě pro doručování obsahu|Podporuje se|Není dosud podporován.|
|Application Gateway|Vyrovnávání zatížení vrstvy 7|Podporuje se|Není dosud podporován.|
|Traffic Manager|Směrujte příchozí provoz pro aplikace pro optimální výkon a spolehlivost.|Podporuje se|Není dosud podporován.|
|ExpressRoute|Nastavení rychlého, privátní připojení ke cloudovým službám Microsoftu z vaší místní infrastruktury nebo společné umístění zařízení.|Podporuje se|Podpora pro připojení zásobník Azure pro okruh Express Route.|

## <a name="next-steps"></a>Další kroky

[DNS v Azure Stacku](azure-stack-dns.md)
