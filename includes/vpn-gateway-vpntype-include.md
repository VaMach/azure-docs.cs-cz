- **Typ sítě VPN založený na zásadách:** Sítě VPN založené na zásadách se v klasickém modelu nasazení označovaly jako brány se statickým směrováním. Sítě VPN založené na zásadách šifrují pakety a směrují je do tunelových propojení IPsec na základě zásad IPsec nakonfigurovaných pomocí kombinace předpon adres mezi vaší místní sítí a virtuální sítí VNet Azure. Zásada (nebo selektor provozu) se většinou určuje v konfiguraci zařízení VPN jako přístupový seznam. Typ sítě VPN založený na zásadách má hodnotu *PolicyBased*. Síť VPN založená na zásadách se při použití SKU pro vysokou dostupnost nepodporuje.

- **Typ sítě VPN založený na trasách**: Sítě VPN založené na trasách se v klasickém modelu nasazení označovaly jako brány s dynamickým směrováním. Sítě VPN založené na trasách používají ke směrování paketů do příslušných rozhraní tunelových propojení „trasy“ v tabulce předávání IP nebo směrovací tabulce IP. Rozhraní tunelového propojení potom zašifruje nebo dešifruje pakety směřující do tunelových propojení nebo z nich. Zásady (nebo selektor provozu) pro sítě VPN založené na trasách mají konfiguraci typu any-to-any (takže se jedná o zástupné znaky). Typ sítě VPN založený na trasách má hodnotu *RouteBased*.


<!---HONumber=Aug16_HO4-->


