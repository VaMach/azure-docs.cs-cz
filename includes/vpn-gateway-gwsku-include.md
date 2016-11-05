Při vytváření brány virtuální sítě musíte určit SKU brány, které chcete použít. Pokud vyberete vyšší SKU brány, přidělí se pro bránu víc procesorů a šířky pásma sítě, a v důsledku toho může brána podporovat vyšší propustnost sítě k virtuální síti.

VPN Gateway může používat následující SKU:

* Basic
* Standard
* Vysokovýkonné

Při výběru SKU zvažte následující omezení:

* Chcete-li použít síť VPN typu PolicyBased, musíte použít základní SKU brány. Sítě VPN typu PolicyBased (dříve nazývané Statické směrování) nejsou podporovány jinými SKU.
* Protokol BGP není podporován v základní SKU.
* Konfigurace současného fungování ExpressRoute a služby VPN Gateway nejsou podporované v základní SKU.

<!--HONumber=Oct16_HO3-->


