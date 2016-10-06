|    | **Propustnost brány sítě VPN (1)** | **Maximální počet tunelových propojení IPsec brány sítě VPN (2)** | **Propustnost brány ExpressRoute** | **Brána sítě VPN a ExpressRoute existují vedle sebe**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Základní SKU**              |  100 Mb/s | 10                         |  500 Mb/s                           | Ne   |
| **Standardní SKU (3)**           |  100 Mb/s | 10                         | 1000 Mb/s                           | Ano  |
| **SKU pro vysoký výkon (3)**   | 200 Mb/s  | 30                         | 2000 Mb/s                           | Ano  |

- (1) Propustnost sítě VPN představuje přibližný odhad na základě měření mezi sítěmi VNet ve stejné oblasti Azure. Není zaručena propustnost pro připojení mezi místními systémy přes internet. Jedná se o maximální možné měření propustnosti.
- (2) Počet tunelových propojení se týká sítí VPN RouteBased. Sítě VPN PolicyBased můžou podporovat jen jeden tunel VPN typu S2S (Site-to-Site).
- (3) Sítě VPN PolicyBased nejsou touto SKU podporované. Podporuje je jenom základní SKU.

<!--HONumber=Sep16_HO4-->


