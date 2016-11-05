|  | **Point-to-site** | **Site-to-site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Podporované služby Azure** |Cloud Services a Virtual Machines |Cloud Services a Virtual Machines |[Seznam služeb](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typické šířky pásma** |Obvykle celkem < 100 Mb/s |Obvykle celkem < 100 Mb/s |50 Mb/s, 100 Mb/s, 200 Mb/s, 500 Mb/s, 1 Gb/s, 2 Gb/s, 5 Gb/s, 10 Gb/s |
| **Podporované protokoly** |SSTP (Secure Sockets Tunneling Protocol) |IPsec |Přímé připojení přes sítě VLAN, technologie VPN od poskytovatelů síťových služeb (MPLS, VPLS…) |
| **Směrování** |Na základě trasy (dynamické) |Podporujeme sítě VPN se směrováním na základě zásad (statické) nebo trasy (dynamické) |BGP |
| **Odolnost připojení** |aktivní-pasivní |aktivní-pasivní |aktivní-aktivní |
| **Typický případ použití** |Vytváření prototypů, vývojové/testovací/laboratorní scénáře pro cloudové služby a virtuální počítače |Vývojové/testovací/laboratorní scénáře a produkční úlohy v malém měřítku pro cloudové služby a virtuální počítače |Přístup ke všem službám Azure (ověřený seznam), úlohy podnikové třídy a kritické úlohy, zálohování, velké objemy dat, Azure jako web pro zotavení po havárii |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Ceny** |[Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Ceny](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technická dokumentace** |[Dokumentace brány sítě VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentace brány sítě VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Nejčastější dotazy** |[Brána sítě VPN – nejčastější dotazy](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Brána sítě VPN – nejčastější dotazy](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute – nejčastější dotazy](../articles/expressroute/expressroute-faqs.md) |

<!--HONumber=Jun16_HO2-->


