<properties
   pageTitle="Umístění ExpressRoute | Microsoft Azure"
   description="Tento článek obsahuje podrobný přehled o umístěních, kde jsou nabízené služby, a jak se připojit k oblastem Azure."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="cherylmc" />

# Partneři ExpressRoute a umístění partnerského vztahu

Tabulky v tomto článku poskytují informace o poskytovatelích připojení ExpressRoute, zeměpisném pokrytí ExpressRoute, cloudových službách Microsoftu podporovaných přes ExpressRoute a systémových integrátorech (SI) ExpressRoute.

## <a name="partners"></a>Poskytovatelé připojení ExpressRoute

Služba ExpressRoute je podporovaná ve všech umístěních a oblastech Azure. Následující mapa obsahuje seznam oblastí Azure a umístění ExpressRoute. Umístění ExpressRoute odkazují na ty, kde má Microsoft partnerský vztah s několika poskytovateli služeb.

![Mapa umístění][0]

Pokud jste připojení k aspoň jednomu umístění ExpressRoute v rámci geopolitické oblasti, budete mít přístup ke službám Azure napříč všemi oblastmi v rámci geopolitické oblasti. Následující tabulka obsahuje mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.

|**Geopolitická oblast**|**Oblast Azure**|**Umístění ExpressRoute**|
|---|---|---|
|**Severní Amerika**|Východní USA, Západní USA, Východní USA 2, Střed USA, Střed USA – jih, Střed USA – sever, Střední Kanada, Východní Kanada|Atlanta, Chicago, Dallas, Las Vegas+, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal+, Quebec City+, Toronto|
|**Jižní Amerika**|Brazílie – jih|Sao Paulo|
|**Evropa**|Severní Evropa, Západní Evropa|Amsterdam, Dublin, Londýn, Newport(Wales)+, Paříž|
|**Asie**|Východní Asie, Jihovýchodní Asie|Hongkong, Singapur|
|**Japonsko**|Japonsko – západ, Japonsko – východ|Ósaka, Tokio|
|**Austrálie**|Austrálie – jihovýchod, Austrálie – východ|Melbourne, Sydney|
|**Indie**|Indie – západ, Indie – střed, Indie – jih|Čennaj, Bombaj|



Následující tabulka obsahuje informace o oblastech a geopolitických hranicích pro národní cloudy.

|**Geopolitická oblast**|**Oblast Azure**|**Umístění ExpressRoute**|
|---|---|---|---|
|**Cloud vlády USA**|USA (Gov) – Iowa, USA (Gov) – Virginia|Chicago, Dallas+, New York, Washington DC|
|**Čína**|Severní Čína, Východní Čína|Peking, Šanghaj|
|**Německo**|Střední Německo, Východní Německo|Berlín, Frankfurt|


Připojení přes geopolitické oblasti není pomocí standardní SKU pro ExpressRoute podporované. Aby bylo podporované globální připojení, budete muset povolit doplněk ExpressRoute Premium. Připojení k prostředím národních cloudů není podporované. Podle potřeby můžete spolupracovat se svým poskytovatelem připojení.


## Umístění poskytovatele připojení

> [AZURE.SELECTOR]
[Umístění podle poskytovatelů](expressroute-locations.md#connectivity-provider-locations)
[Poskytovatelé podle umístění](expressroute-locations-providers.md#connectivity-provider-locations)

### Produkční prostředí Azure
| **Umístění**  | **Poskytovatelé služeb** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT+, InterCloud, Internet Solutions – Cloud Connect, Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** | Equinix |
| **Čennaj** | Tata Communications |
| **Chicago** | AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** | AT&T NetBond, Equinix, Level 3 Communications, Megaport |
| **Dublin** | Colt |
| **Hongkong** | British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Londýn** | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions – Cloud Connect, Interxion, Jisc+, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** | Level 3 Communications+, Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** | Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo Group |
| **Montreal** | Cologix+ |
| **Bombaj** | Tata Communications |
| **Ósaka** | Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, Softbank |
| **Paříž** | Interxion |
| **Sao Paulo** | Equinix, Telefonica+ |
| **Seattle** | Equinix, Level 3 Communications, Megaport |
| **Silicon Valley** | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** | Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Megaport, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** | AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Telstra Corporation, Verizon |
| **Tokio** | Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo Group |
| **Washington DC** | Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |

 **+** označuje brzké uvedení.

### Prostředí národních cloudů

#### Cloud vlády USA

| **Umístění**  |**Poskytovatelé služeb** |
|---------------|--------------------|
| **Chicago** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |  Equinix+, Verizon+ |
| **New York** | Equinix, Level 3 Communications+, Verizon |
| **Washington DC** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |

#### Čína

| **Umístění**  | **Poskytovatelé služeb** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Šanghaj** |  China Telecom |
Další informace najdete v tématu [ExpressRoute v Číně](http://www.windowsazure.cn/home/features/expressroute/).

#### Německo

| **Umístění**  | **Poskytovatelé služeb** |
|---------------|-----------------------|
| **Berlín** | Colt+, e-shelter+ |
| **Frankfurt** | Colt, Equinix+, Interxion |

## <a name="nonpartners"></a>Připojení prostřednictvím neuvedených poskytovatelů služeb

Pokud poskytovatel připojení není v předchozích částech uvedený, můžete přesto vytvořit připojení.

- Zkontrolujte u svého poskytovatele připojení, jestli je připojený k některé z výměn v předchozí tabulce. Můžete zkontrolovat následující odkazy, abyste získali další informace o službách nabízených poskytovateli výměny. Několik poskytovatelů připojení je už připojeno k ethernetovým výměnám.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Rozšířil váš poskytovatel připojení vaši síť ke zvolenému umístění partnerského vztahu?
    - Ujistěte se, že poskytovatel připojení rozšiřuje vaše připojení vysoce dostupným způsobem tak, aby neexistovaly žádné jediné body selhání.
- Abyste se připojili k Microsoftu, objednejte si okruh ExpressRoute s výměnou jako poskytovatel připojení.
    - Při nastavení připojení postupujte podle kroků v tématu [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md).

|**Umístění**|**Exchange**|**Poskytovatelé připojení**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaska Communications |
| **Silicon Valley** | Equinix | XO Communications |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Washington DC** | Equinix | Lightower |

## Systémoví integrátoři ExpressRoute

Povolení soukromého připojení podle vlastních potřeb může být náročné, v závislosti na rozsahu vaší sítě. Můžete spolupracovat s kterýmkoli systémovým integrátorem uvedeným v následující tabulce, aby vám pomohl s připojením k ExpressRoute.

|**Kontinent**|**Systémoví integrátoři**|
|-------------|---------------------|
| **Asie** | Avanade Inc., OneAs1a|
| **Evropa** | Avanade Inc., Dotnet Solutions|
| **USA** | Avanade Inc., Equinix Professional Services, Perficient, Project Leadership|

## Další kroky

- Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
- Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"



<!---HONumber=Aug16_HO4-->


