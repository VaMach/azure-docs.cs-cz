---
title: "O službě VPN gateway pro Azure zásobníku | Microsoft Docs"
description: "Další informace o a konfiguraci brány VPN, které používáte pro Azure zásobníku."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: ba9642d8c51f57623aded44b84d7127334806bc1
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="about-vpn-gateway-for-azure-stack"></a>O službě VPN gateway pro Azure zásobníku
*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*


Před odesláním síťový provoz mezi virtuální sítě Azure a místními servery, musíte vytvořit bránu virtuální sítě pro virtuální síť.

Brána VPN je typem brány virtuální sítě, která odesílá šifrovaný síťový provoz přes veřejné spojení. Brány sítě VPN můžete odesílat provoz bezpečně mezi virtuální sítě v zásobníku Azure a virtuální sítě v Azure nebo mezi virtuální sítě a jiné síti, která je připojena k zařízení VPN.

Při vytváření brány virtuální sítě musíte určit typ brány, který chcete vytvořit. Azure zásobníku podporuje jeden typ brány virtuální sítě: typ 'Vpn'.

Každá virtuální síť může mít dvě brány virtuální sítě, ale každého typu jenom jednu. V závislosti na nastavení, které zvolíte, můžete k jedné bráně VPN vytvořit několik připojení. Příkladem je konfigurace připojení typu Multi-Site.

> [!NOTE]
> V Azure propustnost šířky pásma pro SKU brány sítě VPN zvolíte musí rozdělit mezi všechna připojení, které jsou připojené k němu.  V zásobníku Azure, je každý prostředek připojení, který je připojený ke svému použita hodnota šířky pásma pro SKU brány VPN.     

> Například v Azure, základní SKU brány VPN zvládne přibližně 100 MB/s agregační propustnosti.  Pokud vytvoříte dvě připojení k brány sítě VPN a jednoho připojení používá 50 MB/s šířky pásma, pak 50 MB/s je k dispozici pro jiné připojení.   

> V zásobníku Azure *každý* připojení k základní SKU brány sítě VPN, získá přidělené 100 MB/s propustností.

## <a name="configuring-a-vpn-gateway"></a>Konfigurace služby VPN Gateway
Připojení brány VPN se spoléhá na několik prostředků nakonfigurovaných se specifickými nastaveními. Většinu prostředků lze nakonfigurovat jednotlivě, nicméně v některých případech je třeba je konfigurovat v určitém pořadí.

### <a name="settings"></a>Nastavení
Nastavení, která jste pro jednotlivé zdroje zvolili, jsou pro vytvoření úspěšného připojení zásadní. Informace o jednotlivých prostředcích a nastavení pro bránu VPN najdete v tématu [nastavení brány sítě VPN o pro Azure zásobníku](azure-stack-vpn-gateway-settings.md). Můžete získat informace, které vám pomohou pochopit, brány typy, typy sítě VPN, typy připojení, podsítě brány, brány místní sítě a různým dalším nastavením prostředků, které můžete vzít v úvahu.

### <a name="deployment-tools"></a>Nástroje pro nasazení
Prostředky můžete začít vytvářet a konfigurovat pomocí konfiguračního nástroje, jako je například Azure Portal. Později se můžete rozhodnout používat ke konfiguraci dalších prostředků nebo úpravám stávajících prostředků jiný nástroj, třeba PowerShell. V současné době nelze konfigurovat všechny prostředky a nastavení prostředků pomocí webu Azure Portal. Pokyny v článcích pro každou topologii připojení určují, kdy je zapotřebí specifický konfigurační nástroj.

## <a name="connection-topology-diagrams"></a>Diagramy topologie připojení
Je důležité vědět, že pro připojení brány VPN jsou dostupné různé konfigurace. Musíte určit, která konfigurace bude nejlépe vyhovovat vašim potřebám. V následujících oddílech si můžete prohlédnout diagramy topologie a informace o následujících připojení brány sítě VPN. Následující oddíly obsahují tabulky, které uvádějí:

- dostupný model nasazení,
- dostupné konfigurační nástroje,
- odkazy na příslušný článek, pokud existuje.

Diagramy a popisy v následujících částech můžete vybrat topologie připojení tak, aby odpovídaly vašim požadavkům. Diagramy popisují základní topologie, ale je možné vytvořit komplexnější konfigurace s použitím diagramů jako vodítek.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Site-to-Site a Multi-Site (tunel VPN IPsec/IKE)
### <a name="site-to-site"></a>Site-to-Site
Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení S2S můžete použít pro konfigurace mezi různými místy a pro hybridní konfigurace.    
![Příklad konfigurace připojení Site-to-site VPN](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multi-Site
Tento typ připojení je variací připojení Site-to-Site. Z brány virtuální sítě vytvoříte několik připojení VPN, obvykle pro připojení k několika místním lokalitám. Při práci s několika připojeními je nutné použít typ sítě VPN RouteBased (při práci s klasickými virtuálními sítěmi se označuje jako dynamická brána). Vzhledem k tomu, že virtuální síť může mít jenom jednu bránu virtuální sítě, všechna připojení prostřednictvím brány sdílejí dostupnou šířku pásma. To se často označuje jako připojení „více lokalit“ (Multi-Site).   
![Příklad propojení Multi-Site pomocí Azure VPN Gateway](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## <a name="gateway-skus"></a>Skladové jednotky (SKU) brány
Když vytvoříte bránu virtuální sítě pro Azure zásobníku, určíte SKU brány, kterou chcete použít. Podporovány jsou následující SKU brány sítě VPN:
- Basic
- Standard
- HighPerformance

Když vyberete bránu vyšší skladová položka jako standardní přes Basic nebo HighPerformance přes standardní nebo Basic, jsou přiděleny více procesorů a šířku pásma sítě k bráně. V důsledku toho brány může podporovat vyšší propustnost sítě do virtuální sítě.

Azure zásobník nepodporuje bránu UltraPerformance SKU, které používá výhradně s Express Route.

Při výběru SKU zvažte následující informace:
- Azure zásobník nepodporuje brány na základě zásad.
- Základní SKU nepodporuje protokol BGP (Border Gateway).
- Brána ExpressRoute VPN existovat vedle sebe konfigurace nejsou podporovány v Azure zásobníku
- Propojení VPN Gateway S2S aktivní-aktivní jde konfigurovat jenom v SKU HighPerformance.

## <a name="estimated-aggregate-throughput-by-sku"></a>Odhadovaná agregovaná propustnost podle typů SKU
Následující tabulka ukazuje typy brány a odhadovanou agregovanou propustnost podle SKU brány.

|   | Propustnost brány sítě VPN *(1)* |Brána sítě VPN maximální počet tunelových propojení IPsec |
|-------|-------|-------|
|**Základní SKU** ***(2)***    | 100 Mb/s  | 10    |
|**Standardní SKU**       | 100 Mb/s  | 10    |
|**SKU pro vysoký výkon** | 200 Mb/s    | 30    |
***(1)***  Propustnost VPN není zaručenou propustnost pro připojení mezi různými místy přes Internet. Jedná se o maximální možné měření propustnosti.  
***(2)***  Protokol BGP není podporován pro základní SKU.

## <a name="next-steps"></a>Další kroky
Další informace o [nastavení pro brány VPN](azure-stack-vpn-gateway-settings.md) pro Azure zásobníku.
