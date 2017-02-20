---
title: "Poskytovatelé připojení a umístění: Azure ExpressRoute | Dokumentace Microsoftu"
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
ms.date: 02/13/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0afada6b5ade6c1f246b99e924a972cd7d34fdfa
ms.openlocfilehash: b17defc732be9f1fbdb816727d5602eb037b7e06


---
# <a name="expressroute-partners-and-peering-locations"></a>Partneři ExpressRoute a umístění partnerského vztahu

> [!div class="op_single_selector"]
> * [Umístění podle poskytovatele](expressroute-locations.md)
> * [Poskytovatelé podle umístění](expressroute-locations-providers.md)


Tabulky v tomto článku poskytují informace o poskytovatelích připojení ExpressRoute, zeměpisném pokrytí ExpressRoute, cloudových službách Microsoftu podporovaných přes ExpressRoute a systémových integrátorech (SI) ExpressRoute.

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>Poskytovatelé připojení ExpressRoute
Služba ExpressRoute je podporovaná ve všech umístěních a oblastech Azure. Následující mapa obsahuje seznam oblastí Azure a umístění ExpressRoute. Umístění ExpressRoute odkazují na ty, kde má Microsoft partnerský vztah s několika poskytovateli služeb.

![Mapa umístění][0]

Pokud jste připojení k aspoň jednomu umístění ExpressRoute v rámci geopolitické oblasti, budete mít přístup ke službám Azure napříč všemi oblastmi v rámci geopolitické oblasti.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.
Následující tabulka obsahuje mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- |
| **Severní Amerika** |Východní USA, Západní USA, Východní USA 2, Západní USA 2, Střed USA, Střed USA – jih, Střed USA – sever, Střed USA – západ, Střední Kanada, Východní Kanada |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal, Québec, Toronto |
| **Jižní Amerika** |Brazílie – jih |Sao Paulo |
| **Evropa** |Severní Evropa, Západní Evropa, Spojené království – západ, Spojené království – jih |Amsterdam, Dublin, Londýn, Newport (Wales), Paříž |
| **Asie** |Východní Asie, Jihovýchodní Asie |Hongkong, Singapur |
| **Japonsko** |Japonsko – západ, Japonsko – východ |Ósaka, Tokio |
| **Austrálie** |Austrálie – jihovýchod, Austrálie – východ |Melbourne, Sydney |
| **Indie** |Indie – západ, Indie – střed, Indie – jih |Čennaj, Bombaj |
| **Jižní Korea** |Korea – střed, Korea – jih |Busan, Soul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Oblasti a geopolitické hranice pro národní cloudy
Následující tabulka obsahuje informace o oblastech a geopolitických hranicích pro národní cloudy.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- | --- |
| **Cloud vlády USA** |USA (Gov) – Iowa, USA (Gov) – Virginia, US DoD – střed, US DoD – východ  |Chicago, Dallas, New York, Silicon Valley, Washington DC |
| **Čína** |Severní Čína, Východní Čína |Peking, Šanghaj |
| **Německo** |Střední Německo, Východní Německo |Berlín, Frankfurt |

Připojení přes geopolitické oblasti není pomocí standardní SKU pro ExpressRoute podporované. Aby bylo podporované globální připojení, budete muset povolit doplněk ExpressRoute Premium. Připojení k prostředím národních cloudů není podporované. Podle potřeby můžete spolupracovat se svým poskytovatelem připojení.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Umístění poskytovatele připojení

Následující tabulka uvádí umístění podle poskytovatele služby. Pokud chcete zobrazit dostupné poskytovatele podle umístění, přečtěte si téma [Poskytovatelé služeb podle umístění](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Produkční prostředí Azure
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365 a CRM Online** | **Umístění** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[Aryaka Networks](http://www.aryaka.com/)** |Podporuje se |Podporuje se |Amsterdam, Dallas, Silicon Valley, Singapur, Tokio, Washington DC |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Londýn, Silicon Valley, Singapur, Sydney, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Podporuje se |Podporuje se |Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Podporuje se |Podporuje se |Amsterdam, Hongkong, Londýn, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Připravuje se |Připravuje se |Silicon Valley |
| **China Telecom Global** |Podporuje se |Nepodporuje se |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Podporuje se |Podporuje se |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Podporuje se |Podporuje se |Amsterdam, Dublin, Londýn, Tokio |
| **Comcast** |Podporuje se |Podporuje se |Chicago, Silicon Valley, Washington DC |
| **Console**| Podporuje se | Podporuje se |Silicon Valley |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Podporuje se |Podporuje se |Los Angeles, New York |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Podporuje se |Podporuje se |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, Londýn, Los Angeles, Melbourne, New York, Ósaka, Sao Paulo, Paříž+, Seattle, Silicon Valley, Singapur, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Podporuje se |Podporuje se |Amsterdam |
| **Global Cloud Exchange (GCX)** | Připravuje se | Připravuje se | Čennaj |
| **GÉANT** |Podporuje se |Podporuje se |Amsterdam |
| **[Internet Initiative Japan Inc. – IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Podporuje se |Podporuje se |Ósaka, Tokio |
| **[InterCloud](https://www.intercloud.com/)** |Podporuje se |Podporuje se |Amsterdam, Londýn, Singapur, Washington DC |
| **Internet Solutions – Cloud Connect** |Podporuje se |Podporuje se |Amsterdam, Londýn |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)** |Podporuje se |Podporuje se |Amsterdam, Londýn, Paříž |
| **Jisc** |Podporuje se |Podporuje se |Londýn |
| **KINX** |Podporuje se |Podporuje se |Soul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Podporuje se | Podporuje se | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Podporuje se |Podporuje se |Amsterdam, Chicago, Dallas, Las Vegas+, Londýn, Seattle, Silicon Valley, Singapur, Washington DC |
| **LG CNS** |Připravuje se |Připravuje se |Busan+ |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se |Podporuje se |Dallas, Hong Kong, Las Vegas, Los Angeles, Melbourne, New York, Québec, Seattle, Singapore, Sydney, Toronto, Washington DC |
| **MTN** |Podporuje se |Podporuje se |Londýn |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Podporuje se |Podporuje se |Newport(Wales) |
| **NEXTDC** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Podporuje se |Podporuje se |Londýn, Los Angeles, Ósaka, Singapur, Tokio, Washington DC |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Podporuje se |Podporuje se |Amsterdam, Hongkong, Londýn, Silicon Valley, Singapur, Sydney, Washington DC |
| **PCCW Global Limited** |Podporuje se |Podporuje se |Hongkong |
| **Sejong Telecom** |Podporuje se |Podporuje se |Busan, Soul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Podporuje se |Podporuje se |Čennaj |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Podporuje se |Podporuje se |Singapur |
| **Softbank** |Podporuje se |Podporuje se |Ósaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Podporuje se |Podporuje se |Amsterdam, Čennaj, Hongkong, Londýn, Bombaj, Silicon Valley, Singapur, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Podporuje se |Podporuje se |Amsterdam, Dublin, Londýn |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Podporuje se |Podporuje se |Sao Paulo |
| **Telenor** |Podporuje se |Podporuje se |Amsterdam, Londýn |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Podporuje se |Podporuje se |Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Podporuje se |Podporuje se |Amsterdam, Hongkong, Londýn, Silicon Valley, Singapur, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Podporuje se |Nepodporuje se |Londýn |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Podporuje se |Podporuje se |Chicago, Los Angeles, New York, Silicon Valley, Toronto, Washington DC |

 **+** označuje brzké uvedení.

### <a name="national-cloud-environment"></a>Prostředí národního cloudu

### <a name="us-government-cloud"></a>Cloud vlády USA
| **Poskytovatel služeb** | **Microsoft Azure** | **Office 365** | **Umístění** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Podporuje se |Podporuje se |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Podporuje se |Podporuje se |Chicago, Dallas, New York, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Podporuje se |Podporuje se |Chicago, New York+, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se | Podporuje se | Dallas |
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
| **e-shelter** |Podporuje se |Nepodporuje se |Berlín |
| **Interxion** |Podporuje se |Nepodporuje se |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Podporuje se  | Nepodporuje se | Berlín |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Připojení prostřednictvím neuvedených poskytovatelů služeb
Pokud poskytovatel připojení není v předchozích částech uvedený, můžete přesto vytvořit připojení.

* Zkontrolujte u svého poskytovatele připojení, jestli je připojený k některé z výměn v předchozí tabulce. Můžete zkontrolovat následující odkazy, abyste získali další informace o službách nabízených poskytovateli výměny. Několik poskytovatelů připojení je už připojeno k ethernetovým výměnám.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Rozšířil váš poskytovatel připojení vaši síť ke zvolenému umístění partnerského vztahu?
  * Ujistěte se, že poskytovatel připojení rozšiřuje vaše připojení vysoce dostupným způsobem tak, aby neexistovaly žádné jediné body selhání.
* Abyste se připojili k Microsoftu, objednejte si okruh ExpressRoute s výměnou jako poskytovatel připojení.
  * Při nastavení připojení postupujte podle kroků v tématu [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md).

| **Poskytovatel připojení** | **Výměna** | **Umístění** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londýn |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londýn, Slough |
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix |New York, Washington DC |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix |Chicago, Silicon Valley, Washington DC |
| **[XO Communications](http://www.xo.com/)** |Equinix |Silicon Valley |
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |


## <a name="expressroute-system-integrators"></a>Systémoví integrátoři ExpressRoute
Povolení soukromého připojení podle vlastních potřeb může být náročné, v závislosti na rozsahu vaší sítě. Můžete spolupracovat s kterýmkoli systémovým integrátorem uvedeným v následující tabulce, aby vám pomohl s připojením k ExpressRoute.

| **Systémový integrátor** | **Kontinent** |
| --- | --- |
| **[Avanade Inc.](http://www.avanade.com/)** | Asie, Evropa, Severní Amerika, Jižní Amerika |
| **[Dotnet Solutions](http://www.dotnetsolutions.co.uk/)** | Evropa |
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Severní Amerika |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Austrálie |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Evropa (Německo) |
| **[Nelite](http://nelite.com/)** | Evropa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asie |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Severní Amerika |
| **[Project Leadership](http://www.projectleadership.net/azure)** | Severní Amerika |


## <a name="next-steps"></a>Další kroky
* Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"



<!--HONumber=Feb17_HO2-->


