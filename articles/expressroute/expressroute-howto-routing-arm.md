---
title: Postup konfigurace směrování pro okruh ExpressRoute | Microsoft Docs
description: Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit.
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: expressroute
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/05/2016
ms.author: ganesr

---
# Vytvoření a úprava směrování pro okruh ExpressRoute
> [!div class="op_single_selector"]
> [Azure Portal – Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
> [PowerShell – Resource Manager](expressroute-howto-routing-arm.md)
> [PowerShell – Classic](expressroute-howto-routing-classic.md)
> 
> 

Tento článek vás provede kroky k vytvoření a správě konfigurace směrování pro okruhu ExpressRoute pomocí PowerShellu a modelu nasazení Azure Resource Manager.  Dál uvedené kroky také ukazují, jak kontrolovat stav partnerských vztahů pro okruh ExpressRoute, aktualizovat je nebo je odstranit a zrušit jejich zřízení. 

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Předpoklady konfigurace
* Budete potřebovat nejnovější verzi modulů prostředí Azure PowerShell, verze 1.0 nebo novější. 
* Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).
* Musí mít aktivní okruh ExpressRoute. Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení. Abyste mohli spouštět rutiny popsané dál, musí být okruh ExpressRoute zřízený a povolený.

Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás.

> [!IMPORTANT]
> Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP se poraďte se svým poskytovatelem služeb.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili. 

## Soukromý partnerský vztah Azure
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci soukromého partnerského vztahu Azure pro okruh ExpressRoute. 

### Vytvoření soukromého partnerského vztahu Azure
1. Naimportujte modul PowerShellu pro ExpressRoute.
   
    Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu. Musíte spustit PowerShell jako správce.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Naimportujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.
   
        Import-AzureRM
   
    Můžete také naimportovat jenom modul select v rozsahu známé sémantické verze. 
   
        Import-Module AzureRM.Network 
   
    Přihlaste se k účtu Azure.
   
        Login-AzureRmAccount
   
    Vyberte předplatné, pro které chcete vytvořit okruh ExpressRoute.
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Vytvořte okruh ExpressRoute.
   
    Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení. 
   
    Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás. V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech. Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál. 
3. Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený.
   
    Nejdřív musíte zkontrolovat, že stav okruhu ExpressRoute je Zřízený a také Povolený. Viz následující příklad.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Odpověď bude vypadat přibližně jako v uvedeném příkladu:
   
        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []
4. Nakonfigurujte soukromý partnerský vztah Azure pro okruh.
   
    Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:
   
   * Podsíť /30 pro primární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * Podsíť /30 pro sekundární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
     
     Spuštěním následující rutiny můžete nakonfigurovat soukromý partnerský vztah Azure pro váš okruh.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     Pokud se rozhodnete použít hodnotu hash MD5, můžete použít následující rutinu.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     > [!IMPORTANT]
     > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.
     > 
     > 

### Zobrazení podrobností soukromého partnerského vztahu Azure
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### Aktualizace konfigurace soukromého partnerského vztahu Azure
Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny. V následujícím příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 100 na hodnotu 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### Odstranění soukromého partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.

> [!WARNING]
> Před spuštěním této rutiny je nutné zajistit, že všechny virtuální sítě jsou od okruhu ExpressRoute odpojené. 
> 
> 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## Veřejný partnerský vztah Azure
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci veřejného partnerského vztahu Azure pro okruh ExpressRoute.

### Vytvoření veřejného partnerského vztahu Azure
1. Naimportujte modul PowerShellu pro ExpressRoute.
   
    Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu. Musíte spustit PowerShell jako správce.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Naimportujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.
   
        Import-AzureRM
   
    Můžete také naimportovat jenom modul select v rozsahu známé sémantické verze. 
   
        Import-Module AzureRM.Network 
   
    Přihlaste se k účtu Azure.
   
        Login-AzureRmAccount
   
    Vyberte předplatné, pro které chcete vytvořit okruh ExpressRoute.
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Vytvořte okruh ExpressRoute.
   
    Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení. 
   
    Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil veřejný partnerský vztah Azure za vás. V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech. Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál.
3. Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený.
   
    Nejdřív musíte zkontrolovat, že stav okruhu ExpressRoute je Zřízený a také Povolený. Viz následující příklad.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Odpověď bude vypadat přibližně jako v uvedeném příkladu:
   
        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Nakonfigurujte veřejný partnerský vztah Azure pro okruh.
   
    Před pokračováním se ujistěte, že máte k dispozici následující informace.
   
   * Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy.
   * Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
     
     Spuštěním následující rutiny můžete nakonfigurovat veřejný partnerský vztah Azure pro váš okruh.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
     
     Pokud se rozhodnete použít hodnotu hash MD5, můžete použít následující rutinu.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, a ne zákaznické číslo ASN.

### Zobrazení podrobností veřejného partnerského vztahu Azure
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### Aktualizace konfigurace veřejného partnerského vztahu Azure
Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny.

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

V předchozím příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 200 na hodnotu 600.

### Odstranění veřejného partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## Partnerský vztah Microsoftu
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute. 

### Vytvoření partnerského vztahu Microsoftu
1. Naimportujte modul PowerShellu pro ExpressRoute.
   
    Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu. Musíte spustit PowerShell jako správce.
   
        Install-Module AzureRM
   
        Install-AzureRM
   
    Naimportujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.
   
        Import-AzureRM
   
    Můžete také naimportovat jenom modul select v rozsahu známé sémantické verze. 
   
        Import-Module AzureRM.Network 
   
    Přihlaste se k účtu Azure.
   
        Login-AzureRmAccount
   
    Vyberte předplatné, pro které chcete vytvořit okruh ExpressRoute.
   
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
2. Vytvořte okruh ExpressRoute.
   
    Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení. 
   
    Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás. V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech. Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál.
3. Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený.
   
    Nejdřív musíte zkontrolovat, že stav okruhu ExpressRoute je Zřízený a také Povolený. Viz následující příklad.
   
        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   
    Odpověď bude vypadat přibližně jako v uvedeném příkladu:
   
        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Nakonfigurujte partnerský vztah Microsoftu pro okruh.
   
    Před pokračováním se ujistěte, že máte k dispozici následující informace.
   
   * Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP. Přijímají se jenom předpony veřejných IP adres. Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být v RIR/IRR zaregistrované na vás.
   * Zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, na které jsou registrované. **Tato položka je nepovinná.**
   * Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
     
     Spuštěním následující rutiny můžete nakonfigurovat partnerský vztah Microsoftu pro váš okruh.
     
       Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"
     
       Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### Získání podrobností partnerského vztahu Microsoftu
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### Aktualizace konfigurace partnerského vztahu Microsoftu
Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### Odstranění partnerského vztahu Microsoftu
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## Další kroky
Dalším krokem je [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-arm.md).

* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).

<!--HONumber=Oct16_HO3-->


