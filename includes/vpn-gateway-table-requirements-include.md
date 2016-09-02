Následující tabulka uvádí požadavky na brány sítě VPN založené na zásadách a na trasách. Tato tabulka platí pro model nasazení Resource Manager i pro klasický model. V případě klasického modelu jsou brány sítě VPN založené na zásadách stejné jako statické brány a brány založené na trasách odpovídají dynamickým branám.


|   | **Základní brána sítě VPN založená na zásadách** | **Základní brána sítě VPN založená na trasách** | **Standardní brána sítě VPN založená na trasách**   | **Vysoce výkonná brána sítě VPN založená na trasách** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Připojení typu site-to-site… (S2S)**  | Konfigurace sítě VPN založené na zásadách        | Konfigurace sítě VPN založené na trasách  | Konfigurace sítě VPN založené na trasách     | Konfigurace sítě VPN založené na trasách    |
| **Připojení typu point-to-site (P2S**)      | Nepodporuje se   | Podporuje se (může existovat vedle připojení S2S)  | Podporuje se (může existovat vedle připojení S2S)  | Podporuje se (může existovat vedle připojení S2S) |
| **Metoda ověřování**                 |    Předsdílený klíč  | Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S | Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S | Předsdílený klíč pro připojení S2S, certifikáty pro připojení P2S |
| **Maximální počet připojení S2S**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximální počet připojení P2S**       | Nepodporuje se                  | 128                                                                   | 128                               | 128                              |
|**Podpora aktivního směrování (BGP)**           | Nepodporuje se                  | Nepodporuje se                                                         | Podporuje se                     | Podporuje se                   |
 



<!--HONumber=Aug16_HO4-->


