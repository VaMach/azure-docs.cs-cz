<properties 
   pageTitle="Úvod do ExpressRoute | Microsoft Azure"
   description="Tato stránka obsahuje přehled služby ExpressRoute, včetně informací o způsobu práce připojení ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="06/13/2016"
   ms.author="cherylmc"/>

# Technický přehled ExpressRoute

Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do cloudu Microsoftu přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení. Pomocí ExpressRoute může vytvořit připojení ke cloudovým službám Microsoftu, jako je například Microsoft Azure, Office 365 a CRM Online. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím veřejného internetu. To dovoluje připojením ExpressRoute poskytovat větší spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než typická připojení přes internet.  

![](./media/expressroute-introduction/expressroute-basic.png)

**Mezi klíčové výhody patří:**

- Připojení vrstvy 3 mezi místní sítí a v cloudem Microsoftu prostřednictvím poskytovatele připojení. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), připojení Ethernet typu point-to-point nebo virtuální křížové připojení přes ethernetovou výměnu.
- Připojení ke cloudovým službám Microsoftu přes všechny oblasti v geopolitické oblasti.
- Globální připojení ke službám Microsoftu přes všechny oblasti s doplňkem ExpressRoute Premium.
- Dynamické směrování mezi vaší sítí a Microsoftem prostřednictvím standardních protokolů (BGP).
- Vestavěná redundance v každém umístění partnerského vztahu z důvodu vyšší spolehlivosti.
- Smlouva [SLA](https://azure.microsoft.com/support/legal/sla/) pro provoz připojení.
- Technologie QoS a podpora víc tříd služby pro speciální aplikace, jako je Skype pro firmy.

Další podrobnosti najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

## <a name="howtoconnect"></a>Jak můžu pomocí ExpressRoute připojit vlastní síť k Microsoftu?

Připojení mezi místní sítí a cloudem Microsoftu můžete vytvořit třemi různými způsoby:

### Společně umístěné na výměně cloudu

Pokud jste ve společném umístění s výměnou cloudu, můžete si objednat virtuální křížové připojení ke cloudu Microsoftu prostřednictvím ethernetové výměny poskytovatele ve společném umístění. Poskytovatelé ve společném umístění můžou nabízet křížová připojení vrstvy 2 nebo spravovaná křížová připojení vrstvy 3 mezi vaší infrastrukturou ve společném umístění a cloudem Microsoftu.

### Ethernetová připojení typu point-to-point 

Místní datová centra nebo pobočky můžete připojit ke cloudu Microsoftu prostřednictvím ethernetových propojení typu point-to-point. Poskytovatelé ethernetových pripojení typu point-to-point můžou nabízet připojení vrstvy 2 nebo spravovaná připojení vrstvy 3 mezi vaší lokalitou a cloudem Microsoftu.

### Sítě typu any-to-any (IPVPN)

S cloudem Microsoftu můžete integrovat vaši síť WAN. Poskytovatelé IPVPN (obvykle MPLS VPN) nabízejí připojení typu any-to-any mezi pobočkami a datovými centry. Cloud Microsoftu může být připojen do vaší sítě WAN, aby vypadal stejně jako jiné pobočky. Poskytovatelé sítě WAN obvykle nabízejí spravované připojení vrstvy 3. Funkce a možnosti ExpressRoute jsou u všech výše uvedených modelů připojení identické. 

Poskytovatelé připojení můžou nabízet jeden nebo víc modelů připojení. Můžete ve spolupráci s poskytovatelem připojení vybrat model, který vám bude nejlépe vyhovovat.

![](./media/expressroute-introduction/expressroute-connectivitymodels.png)



## Funkce ExpressRoute

ExpressRoute podporuje následující funkce a možnosti: 

### Připojení vrstvy 3

Microsoft používá standardní protokol dynamického směrování (BGP) k výměně tras mezi místní sítí, vašimi instancemi v Azure a veřejnými adresami Microsoftu.  Navážeme několik relací protokolu BGP s vaší sítí pro různé profily přenosu. Další informace jsou uvedené v tématu [Okruh ExpressRoute a domény směrování](expressroute-circuit-peerings.md).

### Redundance

Každý okruh ExpressRoute sestává ze dvou připojení ke dvěma hraničním směrovačům Microsoft Enterprise (MSEE) od poskytovatele připojení nebo z hranice vaší sítě. Microsoft bude vyžadovat od poskytovatele připojení nebo z vaší strany ke každému směrovači MSEE duální připojení BGP. Můžete se rozhodnout nenasazovat redundantní zařízení nebo ethernetové okruhy na vaší straně. Poskytovatelé připojení však používají redundantní zařízení k zajištění, že vaše připojení jsou předávána Microsoftu redundantním způsobem. Konfigurace redundantního připojení vrstvy 3 je požadavkem k tomu, aby byla naše smlouva [SLA](https://azure.microsoft.com/support/legal/sla/) platná. 

### Připojení ke cloudovým službám Microsoftu

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Připojení ExpressRoute umožňují přístup k následujícím službám:

- Služby Microsoft Azure
- Služby Microsoft Office 365
- Služby Microsoft CRM Online 
 
Podrobný seznam služeb podporovaných přes ExpressRoute najdete na stránce [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

### Připojení ke všem oblastem v geopolitické oblasti

Když se připojíte k Microsoftu v jednom z našich [umístění partnerského vztahu](expressroute-locations.md), budete mít přístup ke všem oblastem v geopolitické oblasti. 

Pokud jste například připojení k Microsoftu prostřednictvím ExpressRoute v Amsterdamu, budete mít přístup ke všem cloudovým službám Microsoftu hostovaným v oblastech Severní Evropa a Západní Evropa. Přehled geopolitických oblastí, přidružených oblastí cloudu Microsoftu a odpovídajících umístění partnerského vztahu ExpressRoute najdete v tématu [Partneři ExpressRoute a umístění partnerského vztahu](expressroute-locations.md).

### Globální připojení s doplňkem ExpressRoute Premium

Můžete povolit funkci doplňku ExpressRoute Premium, abyste rozšířili připojení přes geopolitické hranice. Pokud jste například připojení k Microsoftu prostřednictvím ExpressRoute v Amsterdamu, budete mít přístup ke všem cloudovým službám Microsoftu hostovaným ve všech oblastech po celém světě (národní cloudy jsou vyloučeny). Můžete přistupovat ke službám nasazeným v oblastech Jižní Amerika nebo Austrálie stejným způsobem, jakým přistupujete k oblastem Severní Evropa a Západní Evropa.

### Bohatý ekosystém partnerů připojení

ExpressRoute má stále rostoucí ekosystém poskytovatelů připojení a partnerů SI. Nejnovější informace najdete v tématu [Poskytovatelé a umístění služby ExpressRoute](expressroute-locations.md).

### Připojení k národním cloudům

Microsoft provozuje izolovaná cloudová prostředí pro speciální geopolitické oblasti a segmenty zákazníků. Seznam národních cloudů a poskytovatelů najdete na stránce [Poskytovatelé a umístění služby ExpressRoute](expressroute-locations.md).

### Možnosti podporované šířky pásma

Okruhy ExpressRoute můžete zakoupit pro širokou škálu šířek pásma. Seznam podporovaných šířek pásma je uveden níže. U svého poskytovatele připojení zkontrolujte, které z podporovaných šířek pásma poskytuje.

- 50 Mb/s
- 100 Mb/s
- 200 Mb/s
- 500 Mb/s
- 1 Gb/s
- 2 Gb/s
- 5 Gb/s
- 10 Gb/s

### Dynamické škálování šířky pásma

Máte možnost zvětšit šířku pásma okruhu ExpressRoute (jak kapacita systému dovolí) bez nutnosti přerušit připojení. 

### Flexibilní modely fakturace

Můžete si vybrat fakturační model, který vám nejlépe vyhovuje. Zvolte si některý z fakturačních modelů uvedených dál. Další podrobnosti najdete na stránce [ExpressRoute – nejčastější dotazy](expressroute-faqs.md). 

- **Neomezená data** Okruh ExpressRoute je účtován na základě měsíčních poplatků a všechny příchozí a odchozí přenosy dat jsou zahrnuté zdarma. 
- **Měření podle objemu dat**. Okruh ExpressRoute je účtován na základě měsíčních poplatků. Všechny příchozí přenosy dat jsou zadarmo. Odchozí přenosy dat se účtují podle přenesených gigabajtů. Sazby za přenos dat se liší podle oblasti.
- **Doplněk ExpressRoute Premium**. ExpressRoute Premium je doplněk nad rámec okruhu ExpressRoute. Doplněk ExpressRoute Premium poskytuje následující možnosti: 
    - Zvýšené limity tras pro veřejný partnerský vztah Azure a soukromý partnerský vztah Azure ze 4 000 tras na 10 000 tras.
    - Globální připojení pro služby. Okruh ExpressRoute vytvořený v libovolné oblasti (s výjimkou národních cloudů) bude mít přístup k prostředkům v libovolné jiné oblasti na světě. Například virtuální sítě vytvořené v oblasti Západní Evropa budou přístupné prostřednictvím okruhu ExpressRoute zřízeného ze Silicon Valley.
    - Zvýšení počtu propojení virtuálních sítí na jeden okruh ExpressRoute z 10 na vyšší limit, v závislosti na šířce pásma okruhu.

## Další kroky

- Přečtěte si další informace o připojeních ExpressRoute a doménách směrování. Viz [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
- Vyhledejte poskytovatele služeb. Viz [Partneři ExpressRoute a umístění partnerského vztahu](expressroute-locations.md).
- Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).
- Přečtěte si požadavky pro [směrování](expressroute-routing.md), [překlad adres (NAT)](expressroute-nat.md) a [technologii QoS](expressroute-qos.md).
- Nakonfigurujte připojení ExpressRoute.
    - [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfigurace směrování](expressroute-howto-routing-classic.md)
    - [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-classic.md)



<!--HONumber=sep16_HO1-->


