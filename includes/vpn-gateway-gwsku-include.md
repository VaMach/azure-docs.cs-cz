Při vytváření brány virtuální sítě musíte určit SKU brány, které chcete použít. Pokud vyberete vyšší SKU brány, přidělí se pro bránu víc procesorů a šířky pásma sítě, a v důsledku toho může brána podporovat vyšší propustnost sítě k virtuální síti.

VPN Gateway může používat následující SKU:

* Basic
* Standard
* HighPerformance

VPN Gateway nepoužívá SKU brány UltraPerformance. Informace o SKU UltraPerformance najdete v dokumentaci k [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Při výběru SKU zvažte následující informace:

* Pokud chcete použít síť VPN typu PolicyBased, musíte použít SKU Basic. Sítě VPN typu PolicyBased (dříve nazývané Statické směrování) nejsou podporovány jinými SKU.
* Protokol BGP není podporován v základní SKU.
* Konfigurace současného fungování ExpressRoute a služby VPN Gateway nejsou podporované v základní SKU.
* Propojení VPN Gateway S2S aktivní-aktivní jde konfigurovat jenom v SKU HighPerformance.

