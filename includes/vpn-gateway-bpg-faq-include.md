### Je protokol BGP podporován ve všech SKU služby Azure VPN Gateway?
Ne, protokol BGP je podporován ve službách Azure VPN Gateway **Standard** a **HighPerformance**. Pro SKU **Basic** NENÍ podporován.

### Je možné protokol BGP používat se službami Azure VPN Gateway se zásadami?
Ne, protokol BGP je podporován pouze u služeb VPN Gateway se směrováním.

### Je možné používat privátní čísla ASN (čísla autonomního systému)?
Ano, svá vlastní veřejná čísla ASN nebo privátní čísla ASN můžete používat pro své místní sítě i virtuální sítě Azure.

#### Existují ASN vyhrazená sítí Azure?
Ano, následující ASN jsou vyhrazena sítí Azure pro vnitřní a vnější peering:

* Veřejná ASN: 8075, 8076, 12076
* Soukromá ASN: 65515, 65517, 65518, 65519, 65520

Tato ASN nelze zadat pro místní zařízení VPN při připojování k bránám sítě Azure VPN.

### Je možné používat stejné číslo ASN pro místní sítě VPN a sítě Azure VNet?
Ne, pokud pomocí protokolu BGP vzájemně propojujete místní sítě a sítě Azure VNet, je nutné pro ně přiřadit různá čísla ASN. Službám Azure VPN Gateway je přiřazeno výchozí číslo ASN 65515, ať už je protokol BGP povolen pro připojení mezi místními sítěmi, či nikoliv. Toto výchozí nastavení můžete změnit přiřazením jiného čísla ASN při vytváření služby VPN Gateway nebo můžete číslo ASN změnit po vytvoření služby brány. Místní čísla ASN je nutné přiřadit odpovídajícím bránám místních sítí Azure.

### Které předpony adres budou služby Azure VPN gateway prezentovat?
Služby Azure VPN Gateway budou místním zařízením BGP prezentovat následující směrování:

* předpony adres sítí VNet
* předpony Azure pro jednotlivé brány místních sítí připojené ke službě Azure VPN Gateway
* směrování převzatá z jiných relací vytvoření partnerského vztahu protokolu BGP připojených ke službě Azure VPN Gateway, **kromě výchozího směrování nebo směrování překrytých jinými předponami sítě VNet**

#### Mohu inzerovat výchozí cestu (0.0.0.0/0) do bran Azure VPN Gateway?
V tuto chvíli to není možné.

#### Mohu inzerovat přesné předpony jako předpony mé virtuální sítě?
Ne, inzerování stejných předpon jako předpon adres virtuální sítě bude blokováno nebo filtrováno platformou Azure. Můžete ale inzerovat předponu, která je nadmnožinou toho, co máte ve své virtuální síti. Vaše virtuální síť může například používat adresní prostor 10.10.0.0/16 a přitom můžete inzerovat 10.0.0.0/8.

### Je možné použít protokol BGP u připojení mezi sítěmi VNet?
Ano, protokol BGP lze použít pro připojení mezi místními sítěmi i připojení mezi sítěmi VNet.

### Lze u služeb Azure VPN Gateway používat kombinaci připojení pomocí protokolu BGP a bez protokolu BGP?
Ano, u stejné služby Azure VPN Gateway je možné kombinovat připojení pomocí protokolu BGP i bez protokolu BGP.

### Podporuje služba Azure VPN Gateway směrování provozu pomocí protokolu BGP?
Ano, směrování provozu pomocí protokolu BGP je podporováno, pouze s tou výjimkou, že služby Azure VPN Gateway **NEBUDOU** prezentovat výchozí směrování ostatním partnerům BGP. Pokud chcete povolit směrování provozu mezi více různými službami Azure VPN Gateway, je nutné povolit protokol BGP ve všech zprostředkujících připojení mezi sítěmi VNet.

### Je možné mít víc než jeden tunel mezi službou Azure VPN Gateway a místní sítí?
Ano, můžete vytvořit více než jeden tunel VPN S2S mezi službou Azure VPN Gateway a místní sítí. Upozornění: Všechny tyto tunely se započítají do celkového počtu tunelů služeb Azure VPN Gateway. Pokud máte například dva redundantní tunely mezi službou Azure VPN Gateway a jednou z vašich místních sítí, tyto tunely vyčerpají 2 z celkové kvóty tunelů dostupných pro službu Azure VPN Gateway (služba Standard umožňuje vytvořit 10 tunelů a služba HighPerformance 30 tunelů).

### Mohu mít více různých tunelů mezi dvěma sítěmi Azure VNet s protokolem BGP?
Ne, redundantní tunely mezi párem virtuálních sítí nejsou podporovány.

### Je možné použít protokol BGP pro síť VPN S2S provozovanou v koexistenci se sítí VPN ExpressRoute?
V tuto chvíli to není možné.

### Jakou adresu služba Azure VPN Gateway používá pro IP adresu partnera BGP?
Služba Azure VPN Gateway přidělí jednu IP adresu z rozsahu GatewaySubnet definovaného pro virtuální síť. Ve výchozím nastavení se jedná o předposlední adresu v rozsahu. Pokud například používáte rozsah GatewaySubnet 10.12.255.0/27 v intervalu 10.12.255.0 až 10.12.255.31, IP adresa partnera BGP ve službě Azure VPN Gateway bude 10.12.255.30. Tyto informace můžete získat zobrazením informací služby Azure VPN Gateway.

### Jaké jsou požadavky na IP adresu partnera BGP v zařízení VPN?
Vaše místní adresa partnera BGP **NESMÍ** být stejná, jako veřejná IP adresa vašeho zařízení VPN. Jako místní adresu partnera BGP v zařízení VPN použijte jinou IP adresu. Může se jednat o adresu přiřazenou rozhraní zpětné smyčky v zařízení. Adresu zadejte v odpovídající bráně místní sítě reprezentující umístění.

### Co je třeba zadat jako předpony adres pro bránu místní sítě při použití protokolu BGP?
Azure Local Network Gateway určuje počáteční předpony adres pro místní síť. Při použití protokolu BGP je nutné přidělit předponu hostitele (předponu /32) vaší IP adresy partnera BGP jako adresní prostor pro danou místní síť. Pokud je vaše IP adresa partnera BGP 10.52.255.254, je jako hodnotu localNetworkAddressSpace místní síťové brány reprezentující místní síť nutné zadat 10.52.255.254/32. To zajistí, že služba Azure VPN Gateway vytvoří relaci BGP prostřednictvím tunelu VPN S2S.

### Co je třeba přidat do místního zařízení VPN pro relaci partnerského vztahu protokolu BGP?
Do zařízení VPN je nutné přidat směrování hostitele IP adresy partnera BGP Azure odkazující na tunel VPN IPsec S2S. Pokud je například IP adresa partnera BGP Azure 10.12.255.30, je nutné přidat směrování hostitele pro adresu 10.12.255.30 s rozhraním nexthop odpovídajícího rozhraní tunelu IPsec ve vašem zařízení VPN.

<!--HONumber=Sep16_HO3-->


