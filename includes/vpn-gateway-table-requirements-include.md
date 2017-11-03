Následující tabulka uvádí požadavky na brány PolicyBased a sítě VPN RouteBased. Tato tabulka platí pro model nasazení Resource Manager i pro klasický model. Pro klasického modelu brány sítě VPN PolicyBased jsou stejné jako statické brány a trasové brány jsou stejné jako dynamické brány.

|  | **Brána sítě VPN PolicyBased Basic** | **Brána sítě VPN RouteBased Basic** | **Brána sítě VPN RouteBased Standard** | **RouteBased vysoce výkonná brána sítě VPN** |
| --- | --- | --- | --- | --- |
| **Připojení Site-to-Site (S2S)** |Konfigurace sítě VPN PolicyBased |Konfigurace sítě VPN RouteBased |Konfigurace sítě VPN RouteBased |Konfigurace sítě VPN RouteBased |
| **Připojení typu point-to-site (P2S**) |Nepodporuje se |Podporuje se (může existovat vedle připojení S2S) |Podporuje se (může existovat vedle připojení S2S) |Podporuje se (může existovat vedle připojení S2S) |
| **Metoda ověřování** |Předsdílený klíč |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |
| **Maximální počet připojení S2S** |1 |10 |10 |30 |
| **Maximální počet připojení P2S** |Nepodporuje se |128 |128 |128 |
| **Podpora aktivního směrování (BGP)** |Nepodporuje se |Nepodporuje se |Podporuje se |Podporuje se |

