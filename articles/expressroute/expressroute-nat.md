---
title: "Požadavky na překlad adres (NAT) pro okruhy ExpressRoute | Dokumentace Microsoftu"
description: "Tato stránka obsahuje podrobné požadavky pro konfiguraci a správu překladu adres (NAT) pro okruhy ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d16326340f668285e185a6e8adb1a8316d8bd0eb


---
# <a name="expressroute-nat-requirements"></a>Požadavky na překlad adres (NAT) služby ExpressRoute
Pokud se chcete připojit ke cloudovým službám Microsoftu pomocí služby ExpressRoute, budete muset nastavit a spravovat překlady adres (NAT). Někteří poskytovatelé připojení nabízejí nastavení a správu NAT jako spravovanou službu. Zeptejte se svého poskytovatele připojení, jestli tuto službu nabízí. Pokud ne, je nutné splnit požadavky popsané dál. 

Přehled různých domén směrování získáte na stránce [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md). Pokud chcete splnit požadavky na veřejné IP adresy pro veřejný partnerský vztah Azure a partnerský vztah Microsoftu, doporučujeme mezi vaší sítí a Microsoftem zřídit překlad adres NAT. Tato část poskytuje podrobný popis infrastruktury NAT, kterou je potřeba nastavit.

## <a name="nat-requirements-for-azure-public-peering"></a>Požadavky NAT pro veřejný partnerský vztah Azure
Cesta veřejného partnerského vztahu Azure vám umožňuje připojení ke všem službám, které jsou hostovány v Azure, přes jejich veřejné IP adresy. Sem patří služby uvedené v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) a všechny služby hostované nezávislými dodavateli softwaru v Microsoft Azure. Připojení ke službám Microsoft Azure ve veřejném partnerském vztahu je vždycky iniciováno z vaší sítě do sítě Microsoftu. Přenosy určené do Microsoft Azure ve veřejném partnerském vztahu musí být před jejich vstupem do sítě Microsoftu platné IPv4 adresy přeložené pomocí překladu SNAT. Následující obrázek poskytuje základní přehled o způsobu nastavení překladu síťových adres NAT, aby splňoval předchozí požadavek.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>Inzerování tras a fondu IP adres NAT
Je nutné zajistit, aby provoz vstupující do cesty veřejného partnerského vztahu Azure měl platnou veřejnou IPv4 adresu. Microsoft musí být schopný ověřit vlastnictví fondu IPv4 adres NAT proti regionálnímu registru RIR nebo registru IRR. Kontrola bude provedená na základě čísla AS s partnerským vztahem a adres IP použitých pro NAT. Informace o registrech směrování najdete na stránce [Požadavky na směrování služby ExpressRoute](expressroute-routing.md).

Neexistují žádná omezení délky předpon adres IP pro NAT inzerovaných prostřednictvím tohoto partnerského vztahu. Musíte monitorovat fond NAT a zajistit, že nemáte nedostatek relací NAT.

> [!IMPORTANT]
> Fond IP adres NAT inzerovaný Microsoftu nesmí být inzerovaný do internetu. Tím by došlo k přerušení připojení k jiným službám Microsoftu.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>Požadavky NAT pro partnerský vztah Microsoftu
Cesta partnerského vztahu Microsoftu vám umožní připojit se ke cloudovým službám Microsoftu, které nejsou podporované prostřednictvím cesty veřejného partnerského vztahu Azure. Mezi tyto služby patří služby Office 365, jako je Exchange Online, SharePoint Online, Skype pro firmy a CRM Online. Microsoft v partnerském vztahu Microsoftu očekává podporu obousměrného připojení. Přenosy určené cloudovým službám Microsoftu musí být před jejich vstupem do sítě Microsoftu platné IPv4 adresy přeložené pomocí překladu SNAT. Přenosy určené do vaší sítě z cloudových služeb Microsoftu musí být před vstupem do vaší sítě přeložené pomocí překladu SNAT. Následující obrázek poskytuje základní přehled o způsobu nastavení překladu síťových adres NAT pro partnerský vztah Microsoftu.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

#### <a name="traffic-originating-from-your-network-destined-to-microsoft"></a>Přenosy pocházející z vaší sítě určené do Microsoftu
* Je nutné zajistit, aby provoz vstupující do cesty partnerského vztahu Microsoftu měl platnou veřejnou IPv4 adresu. Microsoft musí být schopný ověřit vlastníka fondu IPv4 adres NAT proti regionálnímu registru RIR nebo registru IRR. Kontrola bude provedená na základě čísla AS s partnerským vztahem a adres IP použitých pro NAT. Informace o registrech směrování najdete na stránce [Požadavky na směrování služby ExpressRoute](expressroute-routing.md).
* Adresy IP použité pro nastavení veřejného partnerského vztahu Azure a jiné okruhy ExpressRoute nesmí být inzerované Microsoftu prostřednictvím relace protokolu BGP. Neexistuje žádné omezení délky předpon adres IP pro NAT inzerovaných prostřednictvím tohoto partnerského vztahu.
  
  > [!IMPORTANT]
  > Fond IP adres NAT inzerovaný Microsoftu nesmí být inzerovaný do internetu. Tím by došlo k přerušení připojení k jiným službám Microsoftu.
  > 
  > 

#### <a name="traffic-originating-from-microsoft-destined-to-your-network"></a>Přenosy pocházející z Microsoftu určené do vaší sítě
* Některé scénáře vyžadují, aby Microsoft zahájil připojení ke koncovým bodům služby hostovaným v rámci vaší sítě. Typickým příkladem takového scénáře je připojení k serverům služby AD FS hostovaným ve vaší síti ze služeb Office 365. V takových případech musíte nechat uniknout příslušné předpony z vaší sítě do partnerského vztahu Microsoftu. 
* Je nutné přenosy určené na IP adresy v rámci vaší sítě z Microsoftu překládat pomocí SNAT. 

## <a name="next-steps"></a>Další kroky
* Přečtěte si požadavky pro [směrování](expressroute-routing.md) a [technologii QoS](expressroute-qos.md).
* Informace o pracovním postupu najdete v tématu [Pracovní postupy zřizování okruhů ExpressRoute a stavy okruhu](expressroute-workflows.md).
* Nakonfigurujte připojení ExpressRoute.
  
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Konfigurace směrování](expressroute-howto-routing-classic.md)
  * [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Nov16_HO2-->


