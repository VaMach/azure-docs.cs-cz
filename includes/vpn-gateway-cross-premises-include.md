|  | **Point-to-Site** | **Site-to-Site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Podporované služby Azure** |Cloud Services a Virtual Machines |Cloud Services a Virtual Machines |[Seznam služeb](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typické šířky pásma** |Obvykle celkem < 100 Mb/s |Obvykle < 1 GB/s agregace |50 Mb/s, 100 Mb/s, 200 Mb/s, 500 Mb/s, 1 Gb/s, 2 Gb/s, 5 Gb/s, 10 Gb/s |
| **Podporované protokoly** |SSTP (Secure Sockets Tunneling Protocol) |IPsec |Přímé připojení přes sítě VLAN, technologie VPN od poskytovatelů síťových služeb (MPLS, VPLS…) |
| **Směrování** |RouteBased (dynamické) |Podporujeme PolicyBased (se statickým směrováním) a RouteBased (dynamické směrování VPN) |Protokol BGP |
| **Odolnost připojení** |aktivní-pasivní |aktivní pasivní nebo aktivní aktivní |aktivní-aktivní |
| **Typický případ použití** |Vytváření prototypů, vývojové/testovací/laboratorní scénáře pro cloudové služby a virtuální počítače |Vývojové/testovací/laboratorní scénáře a produkční úlohy v malém měřítku pro cloudové služby a virtuální počítače |Přístup ke všem službám Azure (ověřený seznam), úlohy podnikové třídy a kritické úlohy, zálohování, velké objemy dat, Azure jako web pro zotavení po havárii |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Ceny** |[Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Ceny](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technická dokumentace** |[Dokumentaci k bráně VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentaci k bráně VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Nejčastější dotazy** |[VPN Gateway – nejčastější dotazy](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN Gateway – nejčastější dotazy](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute – nejčastější dotazy](../articles/expressroute/expressroute-faqs.md) |

