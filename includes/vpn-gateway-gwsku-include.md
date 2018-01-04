Při vytváření brány virtuální sítě musíte určit SKU brány, které chcete použít. Vyberte SKU, které splňují vaše požadavky na základě typů úloh, propustnosti, funkcí a SLA.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Produkční *vs.* vývojářské a testovací úlohy

Vzhledem k rozdílům ve SLA a sadách funkcí doporučujeme pro produkci *vs.* vývoj a testování následující SKU:

| **Úloha**                       | **SKU**               |
| ---                                | ---                    |
| **Produkce, kritické úlohy** | VpnGw1, VpnGw2, VpnGw3 |
| **Vývoj a testování nebo testování konceptu**   | Basic                  |
|                                    |                        |

Pokud používáte staré SKU, pro produkci se doporučují Standard a HighPerformance SKU. Informace o starých skladových položkách najdete v tématu [Skladové položky brány (starší skladové položky)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Sady funkcí SKU brány

Nové SKU brány zjednodušují sady funkcí, které se na branách nabízejí:

| **SKU**| **Funkce**|
| ---    | ---         |
|**Basic**   | **Sítě VPN založené na trasách**: 10 tunelů s P2S; bez ověřování RADIUS pro P2S; žádné IKEv2 pro P2S<br>**Síť VPN založená na zásadách (IKEv1):** 1 tunel, bez P2S|
| **VpnGw1, VpnGw2 a VpnGw3** | **Síť VPN založená na směrování:** až 30 tunelů ( * ), P2S, BGP, aktivní-aktivní, vlastní zásady IPsec/IKE, koexistence ExpressRoute/VPN |
|        |             |

( * ) Můžete nakonfigurovat PolicyBasedTrafficSelectors pro připojení brány sítě VPN založené na směrování (VpnGw1, VpnGw2, VpnGw3) k několika místním zařízením brány firewall založeným na zásadách. Podrobnosti najdete v tématu věnovaném [připojení bran VPN k několika místním zařízením VPN založeným na zásadách s využitím PowerShellu](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

###  <a name="resize"></a>Změna velikosti SKU brány

1. Můžete měnit velikost mezi VpnGw1, VpnGw2 a VpnGw3 SKU.
2. Pokud používáte staré SKU brány, můžete měnit velikost mezi Basic, Standard a HighPerformance SKU.
2. **Není možné** změnit velikost z Basic/Standard/HighPerformance SKU na nové VpnGw1/VpnGw2/VpnGw3 SKU. Místo toho je nutné na nové SKU [migrovat](#migrate).

###  <a name="migrate"></a>Migrace ze starých SKU na nové SKU

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
