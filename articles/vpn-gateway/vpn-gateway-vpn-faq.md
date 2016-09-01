<properties 
   pageTitle="Brána BPN virtuální sítě – nejčastější dotazy | Microsoft Azure"
   description="Nejčastější dotazy k branám VPN. Nejčastější dotazy týkající se propojení Microsoft Azure Virtual Network mezi různými místy, připojení s hybridní konfigurací a bran VPN"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# Nejčastější dotazy k branám VPN

## Připojování k virtuálním sítím

### Je možné propojit virtuální sítě v různých oblastech Azure?
Ano. Žádné omezení oblastí se ve skutečnosti neuplatňuje. Jedna virtuální síť může být propojena s jinou virtuální sítí ve stejném oblasti nebo v jiné oblasti Azure.

### Je možné propojovat virtuální sítě v rámci různých předplatných?
Ano.

### Je možné připojovat se k více serverům z jedné virtuální sítě?

K více serverům se lze připojovat prostřednictvím prostředí Windows PowerShell a rozhraní API REST Azure. Přečtěte si oddíl [Možnosti připojování k více serverům a připojení VNet-to-VNet](#multi-site-and-vnet-to-vnet-connectivity) v nejčastějších dotazech.
## Jaké jsou možnosti připojení mezi různými místy?

Podporovány jsou následující možnosti připojení mezi různými místy:

- [Site-to-Site](vpn-gateway-site-to-site-create.md) – připojení VPN prostřednictvím protokolu IPsec (IKE v1 a IKE v2). Tento typ připojení vyžaduje zařízení VPN nebo službu RRAS.

- [Point-to-Site](vpn-gateway-point-to-site-create.md) – připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Toto připojení nevyžaduje zařízení VPN.

- [VNet-to-VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – tento typ připojení je stejný jako konfigurace Site-to-Site. VNet-to-VNet je připojení k síti VPN prostřednictvím protokolu IPsec (IKE v1 a IKE v2). Nevyžaduje zařízení VPN.

- [Více míst](vpn-gateway-multi-site.md) – varianta konfigurace Site-to-Site, která umožňuje propojit několik místních serverů do virtuální sítě.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute je přímé připojení k Azure z vaší sítě WAN, nikoli prostřednictvím veřejného internetu. Další informace najdete v tématech [Technický přehled ExpressRoute](../expressroute/expressroute-introduction.md) a [ExpressRoute – nejčastější dotazy](../expressroute/expressroute-faqs.md).

Další informace o připojeních mezi různými místy najdete v tématu [Informace o bezpečném připojení mezi různými místy](vpn-gateway-cross-premises-options.md).

### Jaký je rozdíl mezi připojením Site-to-Site a Point-to-Site?

Připojení typu **Site-to-Site** umožňují propojení kteréhokoli počítače vaší lokality se kterýmkoli virtuálním počítačem nebo instancí role ve vaší virtuální síti v závislosti na zvoleném způsobu konfigurace směrování. Představuje skvělou možnost připojení mezi různými místy, která je vždy k dispozici a je velmi vhodná pro hybridní konfigurace. Tento typ připojení využívá zařízení sítě VPN s protokolem IPsec (hardwarové nebo softwarové zařízení), které musí být nasazeno v hraniční části sítě. Chcete-li vytvořit tento typ připojení, musíte mít požadovaný hardware sítě VPN a adresu IPv4 přístupnou zvenčí.

Připojení typu **Point-to-Site** umožňují připojit se z jednoho počítače odkudkoli kamkoli do vaší virtuální sítě. Využívá integrovaného klienta VPN ve Windows. Jako součást konfigurace Point-to-Site nainstalujete certifikát a balíček konfigurace klienta VPN, který obsahuje nastavení umožňující vašemu počítači připojit se ke kterémukoli virtuálnímu počítači nebo instanci role v rámci virtuální sítě. Toto řešení je skvělé, pokud se chcete připojit k virtuální síti, ale nenacházíte se na příslušném místě. Je také vhodné, pokud nemáte přístup k hardwaru sítě VPN nebo adrese IPv4 přístupné zvenčí; obě tyto položky jsou pro připojení Site-to-Site vyžadovány. 

Virtuální sítě můžete konfigurovat tak, aby souběžně používaly připojení Site-to-Site i Point-to-Site, pokud připojení Site-to-Site vytvoříte s použitím typu sítě VPN založeného na směrování pro bránu. Typy sítí VPN založené na směrování se v modelu nasazení Classic nazývají dynamické brány.

Další informace najdete v tématu [Informace o bezpečném připojení mezi různými místy pro virtuální sítě](vpn-gateway-cross-premises-options.md).

### Co je ExpressRoute?

ExpressRoute vám umožňuje vytvářet privátní připojení mezi datovými centry společnosti Microsoft a infrastrukturou, která se nachází ve vašem umístění nebo v prostředí ve společném umístění. Prostřednictvím ExpressRoute můžete vytvářet připojení ke cloudovým službám společnosti Microsoft jako je například Microsoft Azure nebo Office 365, v partnerském zařízení ExpressRoute ve společném umístění, nebo se připojovat přímo z existující sítě WAN (například ze sítě VPN MPLS poskytované poskytovatelem síťové služby).

Připojení ExpressRoute nabízejí vyšší úroveň zabezpečení, větší spolehlivost, větší šířku pásma a nižší latenci než typická připojení přes internet. V některých případech může používání připojení ExpressRoute pro přenos dat mezi vaší místní sítí a Azure přispět také k výraznému snížení nákladů. Pokud jste již vytvořili připojení mezi různými místy z místní sítě do Azure, můžete provést migraci na připojení ExpressRoute, a přitom zachovat virtuální síť beze změn.

Další podrobnosti najdete v tématu [ExpressRoute – nejčastější dotazy](../expressroute/expressroute-faqs.md).

## Připojení typu Site-to-Site a zařízení VPN

### Co je třeba zvážit při výběru zařízení VPN?

Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN pro připojení Site-to-Site. Seznam známých kompatibilních zařízení VPN, příslušné pokyny ke konfiguraci nebo ukázky a specifikace zařízení najdete [zde](vpn-gateway-about-vpn-devices.md). Všechna zařízení v řadách zařízení uvedená jako známá kompatibilní by měla spolupracovat s virtuální sítí. Konfiguraci zařízení VPN vám usnadní ukázka konfigurace zařízení nebo odkaz, který odpovídá příslušné řadě zařízení.

### Jak postupovat, pokud mám zařízení VPN, které není v seznamu známých kompatibilních zařízení?

Pokud vaše zařízení není uvedeno v seznamu jako známé kompatibilní zařízení VPN a chcete je použít pro připojení k síti VPN, budete muset ověřit, zda splňuje podporované možnosti konfigurace protokolu IPsec/IKE a parametry uvedené [zde](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Zařízení splňující minimální požadavky by měla spolupracovat s bránami VPN bez problémů. Kvůli další podpoře a pokynům ke konfiguraci se prosím obraťte na výrobce zařízení.

### Proč se tunelové připojení sítě VPN založené na zásadách při nečinnosti deaktivuje?

Toto je očekávané chování u bran VPN pracujících na základě zásad (označují se také výrazem statické směrování). Když je přenos přes tunelové propojení nečinný déle než 5 minut, tunelové propojení se deaktivuje. Jakmile se ale provoz v některém směru obnoví, tunelové propojení se ihned aktivuje znovu. Pokud používáte brány VPN založené na směrování (také označované jako dynamické), toto chování se neprojeví.

### Je možné používat pro připojení k Azure softwarové sítě VPN?

Konfigurace připojení Site-to-Site pro více míst je podporována u serverů RRAS (Routing and Remote Access) Windows Server 2012.

Jiná softwarová řešení sítě VPN by měla s naší bránou spolupracovat, pokud odpovídají implementacím protokolu IPsec podle průmyslových standardů. Ohledně pokynů ke konfiguraci a podporu se obraťte na výrobce příslušného softwaru.

## Připojení typu Point-to-Site

### Jaké operační systémy je možné používat s připojeními Point-to-Site?

Podporovány jsou následující operační systémy:

- Windows 7 (32bitové a 64bitové verze)

- Windows Server 2008 R2 (pouze 64bitové verze)

- Windows 8 (32bitové a 64bitové verze)

- Windows 8.1 (32bitové a 64bitové verze)

- Windows Server 2012 (pouze 64bitové verze)

- Windows Server 2012 R2 (pouze 64bitové verze)

- Windows 10

### Je možné použít libovolného softwarového klienta sítě VPN pro připojení Point-to-Site, pokud podporuje protokol SSTP?

Ne. Podpora je omezena pouze na verze operačního systému Windows uvedené výše.

### Kolik koncových bodů klienta VPN je možné mít v konfiguraci připojení Point-to-Site?

Podporujeme až 128 klientů VPN, kteří se mohou připojit k virtuální síti současně.

### Je možné používat vlastní interní kořenové certifikační autority infrastruktury veřejných klíčů pro připojení Point-to-Site?

Ano. Dříve bylo možné používat pouze kořenové certifikáty podepsané svým držitelem. I nyní je možné nahrát 20 kořenových certifikátů.

### Je možné procházet proxy servery a brány firewall s využitím schopnosti Point-to-Site?

Ano. Pro tunelové propojení prostřednictvím brány firewall používáme protokol SSTP (Secure Socket Tunneling Protocol). Toto tunelové propojení se jeví jako připojení s protokolem HTTPs.

### Pokud restartuji klientský počítač nakonfigurovaný pro připojení Point-to-Site, připojí se síť VPN automaticky znovu?

Ve výchozím nastavení nebude klientský počítač vytvářet připojení k síti VPN automaticky znovu.

### Podporuje připojení Point-to-Site automatické připojení a DDNS u klientů sítě VPN?

Automatické opětné připojení a DDNS se u sítí VPN s připojením Point-to-Site aktuálně nepodporují.

### Je možné současně používat konfigurace Site-to-Site a Point-to-Site pro stejnou virtuální síť?

Ano. Obě tato řešení budou funkční, pokud pro bránu používáte typ sítě VPN založené na směrování. Pro model nasazení Classic budete potřebovat dynamickou bránu. Připojení Point-to-Site pro brány VPN se statickým směrováním ani brány používající typ sítě VPN PolicyBased se nepodporuje.

### Je možné nakonfigurovat klienta Point-to-Site tak, aby se připojoval k několik virtuálním sítím současně?

Ano, je to možné. Nesmějí se však překrývat předpony IP adres virtuálních sítí ani adresní prostory připojení Point-to-Site různých virtuálních sítí.

### Jakou propustnost je možné očekávat u připojení typu Site-to-Site nebo Point-to-Site?

Určit přesnou propustnost tunelových propojení sítí VPN je obtížné. IPsec a SSTP jsou kryptograficky náročné protokoly sítě VPN. Propustnost je také omezena latencí a šířkou pásma mezi vaší lokalitou a internetem.

## Brány

### Co je to brána založená na zásadách (se statickým směrováním)?

Brány založené na zásadách implementují sítě VPN založené na zásadách. Sítě VPN založené na zásadách šifrují pakety a směrují je do tunelových propojení IPsec na základě kombinací předpon adres mezi vaší místní sítí a virtuální sítí VNet Azure. Zásada (nebo selektor provozu) se většinou v konfiguraci sítě VPN definuje jako přístupový seznam.

### Co je to brána založená na směrování (s dynamickým směrováním)?

Brány založené na směrování implementují sítě VPN založené na směrování. Sítě VPN založené na směrování používají ke směrování paketů do příslušných rozhraní tunelových propojení „trasy“ v tabulce předávání IP nebo směrovací tabulce IP. Rozhraní tunelového propojení potom šifrují nebo dešifrují pakety směřující do tunelových propojení nebo z nich. Zásady nebo selektor provozu pro sítě VPN založené na směrování používají konfiguraci typu any-to-any (se zástupnými znaky).

### Je možné získat IP adresu brány VPN předtím, než se vytvoří?

Ne. Nejprve je třeba vytvořit bránu, aby se získala IP adresa. Pokud bránu VPN odstraníte a znovu vytvoříte, IP adresa se změní.

### Jak se tunelové připojení sítě VPN ověřuje?

Síť VPN Azure používá ověřování PSK (předsdílený klíč). Při vytváření tunelového propojení sítě VPN se vygeneruje předsdílený klíč (PSK). Automaticky generovaný předsdílený klíč můžete nahradit vlastním prostřednictvím rutiny prostředí PowerShell nebo rozhraní API REST pro nastavení předsdíleného klíče.

### Je možné použít rozhraní API pro nastavení předsdíleného klíče ke konfiguraci sítě VPN s bránou založenou na zásadách (se statickým směrováním)?

Ano, rozhraní API i rutinu prostředí PowerShell pro nastavení předsdíleného klíče je možné použít ke konfiguraci sítí VPN Azure založených na zásadách (se statickým směrováním) i sítí VPN Azure založených na směrování (s dynamickým směrováním).

### Je možné použít jiné možnosti ověřování?

Ověřování je omezeno na použití předsdílených klíčů (PSK).

### Co je to „podsíť brány“ a proč je potřebná?

Aby bylo možné povolit propojení mezi různými místy, spouští se služba brány. 

Pro virtuální síť je třeba vytvořit podsíť brány, aby bylo možné konfigurovat bránu VPN. Všechny podsítě brány musí mít název GatewaySubnet, aby fungovaly správně. Nenastavujte pro podsíť brány jiný název. A v podsíti brány nenasazujte virtuální počítače ani žádná jiná zařízení.

Minimální velikost podsítě brány zcela závisí na konfiguraci, kterou chcete vytvořit. Přestože je pro některé konfigurace možné vytvořit tak malou podsíť brány, jako je /29, doporučujeme vytvořit podsíť brány o velikosti /28 nebo větší (/28, /27, /26 atd.). 

## Je možné nasazovat do podsítě brány virtuální počítače nebo instance role?

Ne.

### Jak určit provoz, který má procházet bránou VPN?

Pokud používáte portál Azure Classic, přidejte všechny rozsahy, které chcete odesílat prostřednictvím brány pro virtuální síť, na stránce Sítě v části Místní sítě.

### Je možné konfigurovat vynucené tunelování?

Ano. Informace najdete v části [Konfigurace vynuceného tunelování](vpn-gateway-about-forced-tunneling.md).

### Je možné nastavit v Azure vlastní server VPN a používat ho pro připojování k místní síti?

Ano, je možné nasazovat v Azure vlastní servery nebo brány VPN buď z webu Azure Marketplace, nebo vytvořené jako vlastní směrovače sítě VPN. Je třeba nakonfigurovat ve virtuální síti trasy definované uživatelem, aby se zajistilo správné směrování provozu mezi místními sítěmi a podsítěmi virtuálních sítí.

### Proč jsou některé porty brány VPN otevřené?

Jsou vyžadovány pro komunikaci infrastruktury Azure. Jsou chráněny (uzamknuty) s použitím certifikátů Azure. Bez správných certifikátů nemohou externí entity (včetně zákazníků těchto bran) žádným způsobem ovlivnit činnost těchto koncových bodů.

Brána sítě VPN je v podstatě zařízení s více adresami, přičemž jedna síťová karta je připojena k privátní síti zákazníka a jedna je přístupná z veřejné sítě. Entity infrastruktury Azure nemohou kvůli shodě procházet do privátních sítí zákazníků, takže je pro komunikaci infrastruktury třeba využívat veřejné koncové body. Veřejné koncové body jsou pravidelně kontrolovány auditem zabezpečení Azure.


### Další informace o typech brány, požadavcích a propustnosti

Další informace najdete v tématu [Informace o bránách VPN](vpn-gateway-about-vpngateways.md).

## Možnosti připojování k více serverům a připojení VNet-to-VNet

### Které typy brány podporují možnosti připojování k více serverům a připojení VNet-to-VNet?

Pouze sítě VPN založené na směrování (s dynamickým směrováním).

### Je možné připojit virtuální síť typu RouteBased k jiné virtuální síti typu PolicyBased?

Ne, obě virtuální sítě MUSÍ používat sítě VPN založené na směrování (s dynamickým směrováním).

### Je provoz prostřednictvím připojení VNet-to-VNet bezpečný?

Ano, je chráněn šifrováním s použitím protokolu IPsec/IKE.

### Prochází provoz připojení VNet-to-VNet přes páteřní strukturu Azure?

Ano.

### Ke kolika místních serverům a virtuálním sítím se může připojit jedna virtuální síť?

Nejvýše k 10 dohromady u bran s dynamickým směrováním Basic a Standard; ke 30 u bran VPN s vysokým výkonem.

### Je možné používat sítě VPN Point-to-Site v případě virtuální sítě s několika tunelovými propojeními sítě VPN?

Ano, sítě VPN Point-to-Site (P2S) je možné používat spolu s bránami VPN, které se připojují k několika místním serverům a dalším virtuálním sítím.

### Je možné nakonfigurovat více tunelových propojení mezi virtuální sítí a místního serverem prostřednictvím sítě VPN pro více serverů?

Ne, redundantní tunelová propojení mezi virtuální sítí Azure a místním serverem nejsou podporována.

### Mohou se adresní prostory mezi propojenými virtuálními sítěmi a místními servery překrývat?

Ne. Překrývající se adresní prostory by způsobily selhání při odesílání souboru netcfg nebo při vytváření virtuální sítě.

### Je možné dosáhnout větší šířky pásma použitím několika sítí VPN Site-to-Site než v případě jedné virtuální sítě?

Ne, všechna tunelové propojení sítí VPN Point-to-Site sdílejí tutéž bránu VPN Azure a dostupnou šířku pásma.

### Je možné používat bránu VPN Azure pro provoz mezi místními servery a jinou virtuální sítí?

Provoz prostřednictvím brány VPN Azure je možné, je však závislý na staticky definovaných adresních prostorech v konfiguračním souboru netcfg. Protokol BGP není u virtuálních sítí a bran VPN Azure dosud podporován. Bez protokolu BGP je ruční definování adresních prostorů pro přenos velmi náchylné k chybám a nedoporučuje se.

### Generuje Azure stejný předsdílený klíč protokolu IPsec/IKE pro všechna připojení k síti VPN pro stejnou virtuální síť?

Ne, Azure ve výchozím nastavení pro různá připojení k síti VPN generuje různé předsdílené klíče. Prostřednictvím rozhraní API REST nebo rutiny prostředí PowerShell pro nastavení předsdíleného klíče však můžete nastavit požadovanou hodnotu klíče. Klíčem MUSÍ být alfanumerický řetězec o délce 1 až 128 znaků.

### Účtuje se v Azure provoz mezi virtuálními sítěmi?

Provoz mezi různými virtuálními sítěmi Azure se v Azure účtuje pouze jako provoz z jedné oblasti Azure do jiné. Poplatky jsou uvedeny v tématu [Ceny služby VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/) pro Azure.


### Je možné připojit virtuální síť se sítěmi VPN s protokolem IPsec k okruhu ExpressRoute?

Ano, tato možnost je podporována. Další informace najdete v tématu [konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe](../expressroute/expressroute-howto-coexist-classic.md).

## Protokol BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## Možnosti propojení různých míst a virtuální počítače

### Pokud se virtuální počítač nachází ve virtuální síti s propojením mezi různými místy, jak se k virtuálnímu počítači připojovat?

Možností je několik. Pokud je povolen protokol RDP a je vytvořen koncový bod, je možné připojit se k virtuálnímu počítači s použitím virtuální IP adresy. V takovém případě zadejte virtuální IP adresu a port, ke kterému se chcete připojit. Je třeba nakonfigurovat příslušný port ve virtuálním počítači pro provoz. Obvykle je třeba přejít na portál Azure Classic a uložit si do počítače nastavení pro připojení RDP. Nastavení budou obsahovat potřebné informace o připojení.

V případě virtuální sítě s nakonfigurovanou možností propojení mezi různými místy je možné připojit se k virtuálnímu počítači s použitím vnitřní vyhrazené IP adresy nebo privátní IP adresy. K virtuálnímu počítači se lze připojit pomocí vnitřní vyhrazené IP adresy i z jiného virtuálního počítače umístěného ve stejné virtuální síti. K virtuálnímu počítači se nelze připojit pomocí protokolu RDP s použitím vyhrazené IP adresy z místa mimo virtuální síť. Pokud je například nakonfigurována virtuální síť Point-to-Site, ale z počítače není vytvořeno připojení, nelze se k počítači připojit pomocí vyhrazené IP adresy.

### Pokud se virtuální počítač nachází ve virtuální síti s možností připojení mezi různými místy, prochází tímto připojením veškerý provoz z virtuálního počítače?

Ne. Bránou virtuální sítě prochází pouze s cílovou IP adresou uvedenou v nastavených rozsazích IP adres místní sítě pro příslušnou virtuální síť. Provoz s cílovou adresou IP v příslušné virtuální síti probíhá v rámci této virtuální sítě. Ostatní provoz je odesílán prostřednictvím služby Load Balancer do veřejných sítí nebo prostřednictvím brány VPN Azure, pokud je použito vynucené tunelování. Při řešení problémů je důležité ujistit se, že jsou pro místní síť nastaveny všechny rozsahy, jejichž prostřednictvím chcete provádět odesílání. Ověřte, že rozsahy adres místní sítě se nepřekrývají se žádnými rozsahy adres ve virtuální síti. Je také vhodné ověřit, že server DNS, který používáte, překládá název na správnou adresu IP.


## Nejčastější dotazy týkající se virtuálních sítí

Další informace o virtuálních sítích najdete v tématu [Nejčastější dotazy týkající se virtuálních sítí](../virtual-network/virtual-networks-faq.md).

## Další kroky

Další informace o branách VPN najdete na [stránce dokumentace k branám VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/).

 



<!---HONumber=Aug16_HO4-->


