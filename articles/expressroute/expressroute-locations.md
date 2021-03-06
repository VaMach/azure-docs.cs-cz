---
title: "Poskytovatelé připojení a umístění: Azure ExpressRoute | Microsoft Docs"
description: "Tento článek obsahuje podrobný přehled o umístěních, kde jsou nabízené služby, a jak se připojit k oblastem Azure. Řazení je podle poskytovatele připojení."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2018
ms.author: pareshmu
ms.openlocfilehash: 828532dcb583dc46ecb5c9fc63e47a263ddc2a20
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="expressroute-partners-and-peering-locations"></a>Partneři ExpressRoute a umístění partnerského vztahu

> [!div class="op_single_selector"]
> * [Umístění podle poskytovatele](expressroute-locations.md)
> * [Poskytovatelé podle umístění](expressroute-locations-providers.md)


Tabulky v tomto článku poskytují informace o poskytovatelích připojení ExpressRoute, zeměpisném pokrytí ExpressRoute, cloudových službách Microsoftu podporovaných přes ExpressRoute a systémových integrátorech (SI) ExpressRoute.

## <a name="partners"></a>Poskytovatelé připojení ExpressRoute
Služba ExpressRoute je podporovaná ve všech umístěních a oblastech Azure. Následující mapa obsahuje seznam oblastí Azure a umístění ExpressRoute. Umístění ExpressRoute odkazují na ty, kde má Microsoft partnerský vztah s několika poskytovateli služeb.

![Mapa umístění][0]

Pokud jste připojení k aspoň jednomu umístění ExpressRoute v rámci geopolitické oblasti, budete mít přístup ke službám Azure napříč všemi oblastmi v rámci geopolitické oblasti.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.
Následující tabulka obsahuje mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- |
| **Severní Amerika** |Východní USA, Západní USA, Východní USA 2, Západní USA 2, Střed USA, Střed USA – jih, Střed USA – sever, Střed USA – západ, Střední Kanada, Východní Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Jižní Amerika** |Brazílie – jih |Sao Paulo |
| **Evropa** |Severní Evropa, Západní Evropa, Spojené království – západ, Spojené království – jih |Amsterdam, Dublin, Londýn, Newport (Wales), Paříž |
| **Asie** |Východní Asie, Jihovýchodní Asie |Hongkong, Singapur, Singapur2 |
| **Japonsko** |Japonsko – západ, Japonsko – východ |Ósaka, Tokio |
| **Austrálie** |Austrálie – jihovýchod, Austrálie – východ |Melbourne, Sydney |
| **Indie** |Indie – západ, Indie – střed, Indie – jih |Čennaj, Bombaj |
| **Jižní Korea** |Korea – střed, Korea – jih |Busan, Soul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Oblasti a geopolitické hranice pro národní cloudy
Následující tabulka obsahuje informace o oblastech a geopolitických hranicích pro národní cloudy.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- |
| **Cloud vlády USA** |USA (Gov) – Arizona, USA (Gov) – Iowa, USA (Gov) – Texas, USA (Gov) – Virginia, US DoD – střed, US DoD – východ  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **Čína** |Severní Čína, Východní Čína |Peking, Šanghaj |
| **Německo** |Střední Německo, Východní Německo |Berlín, Frankfurt |

Připojení přes geopolitické oblasti není pomocí standardní SKU pro ExpressRoute podporované. Aby bylo podporované globální připojení, budete muset povolit doplněk ExpressRoute Premium. Připojení k prostředím národních cloudů není podporované. Podle potřeby můžete spolupracovat se svým poskytovatelem připojení.

## <a name="locations"></a>Umístění poskytovatele připojení

Následující tabulka uvádí umístění podle poskytovatele služby. Pokud chcete zobrazit dostupné poskytovatele podle umístění, přečtěte si téma [Poskytovatelé služeb podle umístění](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Produkční prostředí Azure
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365 a Dynamics 365** | **Umístění** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[Airtel](http://www.airtel.in/business/connexion)** | Podporuje se | Podporuje se | Čennaj, Bombaj |
| **[Aryaka Networks](http://www.aryaka.com/)** |Podporuje se |Podporuje se |Amsterdam, Dallas, Hongkong, Silicon Valley, Singapur, Tokio, Washington DC |
| **[Datová centra Ascenty](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)** |Připravuje se |Již brzy |Sao Paulo |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Londýn, Silicon Valley, Singapur, Sydney, Tokio, Toronoto, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Podporuje se |Podporuje se |Montreal, Toronto, Quebec City |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Podporuje se |Podporuje se |Amsterdam, Hongkong, Londýn, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[C3ntro](https://c3ntro.com/)** |Již brzy |Již brzy |Miami |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Již brzy |Již brzy |Silicon Valley |
| **China Telecom Global** |Podporuje se |Nepodporuje se |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Podporuje se |Podporuje se |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Podporuje se |Podporuje se |Amsterdam, Dublin, Londýn, Paříž, Tokio |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Podporuje se |Podporuje se |Chicago, Silicon Valley, Washington DC |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Podporuje se |Podporuje se |Chicago, Denver, Los Angeles, New York, Silicon Valley, Washington DC |
| **eir** |Podporuje se |Podporuje se |Dublin|
| **Epsilon Global Communications** |Podporuje se |Podporuje se |Singapur |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Podporuje se |Podporuje se |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, Londýn, Los Angeles, Melbourne, New York, Ósaka, Sao Paulo, Paříž, Seattle, Silicon Valley, Singapur, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Podporuje se |Podporuje se |Amsterdam, Londýn |
| **GÉANT** |Podporuje se |Podporuje se |Amsterdam |
| **[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Podporuje se| Podporuje se | Čennaj, Bombaj |
| **[InterCloud](https://www.intercloud.com/)** |Podporuje se |Podporuje se |Amsterdam, Londýn, Paříž, Singapur, Washington DC |
| **Internet2** |Podporuje se |Podporuje se |Washington DC |
| **[Internet Initiative Japan Inc. – IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Podporuje se |Podporuje se |Ósaka, Tokio |
| **Internet Solutions – Cloud Connect** |Podporuje se |Podporuje se |Amsterdam, Londýn |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Podporuje se |Podporuje se |Amsterdam, Dublin, Londýn, Paříž |
| **[IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)**|Podporuje se |Podporuje se |Toronto
| **Jisc** |Podporuje se |Podporuje se |Londýn |
| **KINX** |Podporuje se |Podporuje se |Soul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Podporuje se | Podporuje se | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Las Vegas, Londýn, Newport, New York, San Antonio, Sao Paulo, Seattle, Silicon Valley, Singapur, Tokio, Washington DC |
| **LG CNS** |Podporuje se |Podporuje se |Busan, Soul |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Dublin, Hongkong, Las Vegas, Londýn, Los Angeles, Melbourne, Miami, New York, Quebec City, San Antonio, Seattle, Silicon Valley, Singapur, Singapur2, Sydney, Toronto, Washington DC |
| **MTN** |Podporuje se |Podporuje se |Londýn |
| **[Neutrona Networks](http://www.neutrona.com/index.php/azure-expressroute/)** |Podporuje se |Podporuje se |Miami, Sao Paulo |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Podporuje se |Podporuje se |Newport(Wales) |
| **NEXTDC** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Podporuje se |Podporuje se |Londýn, Los Angeles, Ósaka, Singapur, Tokio, Washington DC |
| **[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)** |Podporuje se |Podporuje se |Ósaka |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Podporuje se |Podporuje se |Amsterdam, Hongkong, Londýn, Paříž, Silicon Valley, Singapur, Sydney, Washington DC |
| **[PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)** |Podporuje se |Podporuje se |Chicago, Silicon Valley |
| **PCCW Global Limited** |Podporuje se |Podporuje se |Hongkong |
| **Sejong Telecom** |Podporuje se |Podporuje se |Soul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Podporuje se |Podporuje se |Čennaj, Bombaj |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Podporuje se |Podporuje se |Singapur, Singapur2 |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Podporuje se |Podporuje se |Ósaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Podporuje se |Podporuje se |Amsterdam, Čennaj, Hongkong, Londýn, Bombaj, Silicon Valley, Singapur, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Podporuje se |Podporuje se |Amsterdam, Dublin, Londýn |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Podporuje se |Podporuje se |Amsterdam, Sao Paulo |
| **[Telehouse – KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Podporuje se |Podporuje se |Londýn |
| **Telenor** |Podporuje se |Podporuje se |Amsterdam, Londýn |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[Telus](http://www.telus.com)** |Podporuje se |Podporuje se |Montreal, Toronto |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |Podporuje se |Podporuje se |Sao Paulo |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Hongkong, Londýn, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Podporuje se |Nepodporuje se |Londýn |
| **[Zayo](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Londýn, Los Angeles, Montreal, New York, Silicon Valley, Toronto, Washington DC |

 **+** označuje brzké uvedení.

### <a name="national-cloud-environment"></a>Prostředí národního cloudu

### <a name="us-government-cloud"></a>Cloud vlády USA
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365** | **Umístění** |
| --- | --- | --- | --- |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Podporuje se |Podporuje se |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Podporuje se |Podporuje se |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Podporuje se |Podporuje se |Chicago, New York+, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se | Podporuje se | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Podporuje se |Podporuje se |Chicago, Dallas, New York, Washington DC |

### <a name="china"></a>Čína
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365** | **Umístění** |
| --- | --- | --- | --- |
| **China Telecom** |Podporuje se |Nepodporuje se |Peking, Šanghaj |

Další informace najdete v tématu [ExpressRoute v Číně](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Německo
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365** | **Umístění** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Podporuje se |Nepodporuje se |Berlín+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Podporuje se |Nepodporuje se |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Podporuje se |Nepodporuje se |Berlín |
| **Interxion** |Podporuje se |Nepodporuje se |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se  | Nepodporuje se | Berlín |
| **T-Systems** |Podporuje se |Nepodporuje se |Berlín |

## <a name="connectivity-through-exchange-providers"></a>Možnosti připojení prostřednictvím poskytovatelů Exchange

Pokud poskytovatel připojení není v předchozích částech uvedený, můžete přesto vytvořit připojení.

* Zkontrolujte u svého poskytovatele připojení, jestli je připojený k některé z výměn v předchozí tabulce. Můžete zkontrolovat následující odkazy, abyste získali další informace o službách nabízených poskytovateli výměny. Několik poskytovatelů připojení je už připojeno k ethernetovým výměnám.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Rozšířil váš poskytovatel připojení vaši síť ke zvolenému umístění partnerského vztahu?
  * Ujistěte se, že poskytovatel připojení rozšiřuje vaše připojení vysoce dostupným způsobem tak, aby neexistovaly žádné jediné body selhání.
* Abyste se připojili k Microsoftu, objednejte si okruh ExpressRoute s výměnou jako poskytovatel připojení.
  * Při nastavení připojení postupujte podle kroků v tématu [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md).

## <a name="connectivity-through-additional-service-providers"></a>Možnosti připojení prostřednictvím dalších poskytovatelů služeb

| **Poskytovatel připojení** | **Výměna** | **Umístění** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londýn |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokio |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Frankfurt, Hamburk |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silicon Valley, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | Londýn, Singapur, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londýn |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londýn, Slough |
| **LGA Telecom** |Equinix |Singapur|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hongkong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Amsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londýn |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Londýn | 
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington DC |
| **Zain** |Equinix |Londýn|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Možnosti připojení prostřednictvím poskytovatelů datacenter
| **Poskytovatel** | **Výměna** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[Datová centra RagingWire](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach |
| **[Datacentra T5](http://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Možnosti připojení prostřednictvím sítí národního výzkumu a vzdělávání (NREN)

| **Poskytovatel**|
| --- |
| **AARNET**| 
| **DeIC, prostřednictvím GÉANT**|
| **GARR, prostřednictvím GÉANT**|
| **GÉANT**|
| **HEAnet, prostřednictvím GÉANT**|
| **Internet2**|
| **JISC**|
| **RedIRIS, prostřednictvím GÉANT**|
| **SINET**|
| **Surfnet, prostřednictvím GÉANT**|

* Pokud zde není uveden váš poskytovatel připojení, zkontrolujte, jestli není připojen k některému z partnerů ExpressRoute uvedených výše.

## <a name="expressroute-system-integrators"></a>Systémoví integrátoři ExpressRoute
Povolení soukromého připojení podle vlastních potřeb může být náročné, v závislosti na rozsahu vaší sítě. Můžete spolupracovat s kterýmkoli systémovým integrátorem uvedeným v následující tabulce, aby vám pomohl s připojením k ExpressRoute.

| **Systémový integrátor** | **Kontinent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Evropa |
| **[Avanade Inc.](http://www.avanade.com/)** | Asie, Evropa, Severní Amerika, Jižní Amerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Evropa
| **[Ensyst](http://www.ensyst.com.au)** | Asie
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Severní Amerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Severní Amerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Evropa |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Austrálie |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Austrálie |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Evropa (Německo) |
| **[Nelite](http://nelite.com/)** | Evropa |
| **[New Signature](https://www.newsignature.co.uk/)** | Evropa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asie |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Evropa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Severní Amerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Severní Amerika |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Evropa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Austrálie |


## <a name="next-steps"></a>Další kroky
* Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"
