<properties
   pageTitle="Konfigurace Expressroute a připojení VPN typu site-to-site, která mohou existovat vedle sebe, pro model nasazení Resource Manager | Microsoft Azure"
   description="Tento článek vás provede konfigurací ExpressRoute a připojení VPN typu site-to-site, která mohou v modelu nasazení Resource Manager existovat vedle sebe."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="charleywen"/>

# Konfigurace ExpressRoute a připojení typu site-to-site, která mohou v modelu nasazení Resource Manager existovat vedle sebe

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell – Classic](expressroute-howto-coexist-classic.md)

Možnost konfigurace VPN typu site-to-site a ExpressRoute má několik výhod. Můžete nakonfigurovat VPN typu site-to-site jako zabezpečenou cestu převzetí služeb při selhání pro ExressRoute, nebo použít VPN typu site-to-site pro připojení k webům, které nejsou připojené prostřednictvím ExpressRoute. V tomto článku vám nabídneme postupy konfigurace pro oba scénáře. Tento článek se týká modelu nasazení Resource Manager. Tato konfigurace není k dispozici na webu Azure Portal.


**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] Než budete postupovat podle dál uvedených pokynů, musí být předem nakonfigurované okruhy ExpressRoute. Před provedením následujících kroků zkontrolujte, že jste provedli postupy pro [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfiguraci směrování](expressroute-howto-routing-arm.md).

## Omezení

- **Směrování provozu není podporováno:** Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou pomocí VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.
- **Na bráně VPN typu site-to-site nejde povolit vynucené tunelování:** Můžete jenom „vynutit“ veškerý provoz vázaný na Internet do místní sítě prostřednictvím ExpressRoute. 
- **Jenom standardní nebo vysoce výkonné brány:** Je potřeba použít standardní nebo vysoce výkonnou bránu pro bránu ExpressRoute i bránu VPN typu site-to-site. Informace o SKU brány najdete v tématu [SKU brány](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Jenom brána VPN založená na trasách:** Musíte použít bránu VPN založenou na trasách. Informace o bráně VPN založené na trasách najdete v tématu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Požadavek na statické trasy:** Pokud je vaše místní síť připojená k ExpressRoute a VPN typu site-to-site, musíte mít v místní síti konfigurovanou statickou trasu, abyste mohli směrovat připojení VPN typu site-to-site do veřejného internetu.
- **Nejdřív musí být nakonfigurovaná brána ExpressRoute:** Bránu ExpressRoute musíte vytvořit předtím, než přidáte bránu VPN typu site-to-site.


## Návrhy konfigurace

### Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute

Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute. To platí jenom pro virtuální sítě, které jsou propojené s cestou soukromého partnerského vztahu Azure. Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu. Okruh ExpressRoute je vždy primárním propojením. Data budou procházet cestou VPN typu site-to-site jenom v případě, když okruh ExpressRoute selže. 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### Konfigurace VPN typu site-to-site pro připojení webů, které nejsou připojené prostřednictvím ExpressRoute

Svoji síť můžete nakonfigurovat tak, že některé weby jsou připojené přímo k Azure prostřednictvím VPN typu site-to-site a některé weby přes ExpressRoute. 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

>[AZURE.NOTE] Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.

## Výběr kroků k použití

Existují dvě sady postupů, ze kterých si můžete vybrat, když konfigurujete připojení, která můžou existovat společně. Postup konfigurace, který vyberete, bude záviset na tom, jestli máte existující virtuální síť, ke které se chcete připojit, nebo chcete vytvořit novou virtuální síť.


- Nemám virtuální síť a potřebuji ji vytvořit.
    
    Pokud ještě nemáte virtuální síť, tento postup vás provede procesem vytvoření nové virtuální sítě pomocí modelu nasazení Resource Manager a vytvoření nových připojení ExpressRoute a VPN typu site-to-site. Konfiguraci provedete podle kroků v části [Vytvoření nové virtuální sítě a koexistujících připojení](#new).

- Už mám virtuální síť modelu nasazení Resource Manager.

    Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute. V části [Konfigurace koexistujících připojení pro už existující virtuální síť](#add) najdete postup odstranění brány a následného vytvoření nových připojení ExpressRoute a VPN typu site-to-site. Uvědomte si, že při vytváření nových připojení musí být kroky provedené ve velmi specifickém pořadí. Nepoužívejte pro vytvoření připojení a bran pokyny z jiných článků.

    V tomto postupu bude vytvoření připojení, která mohou existovat společně, vyžadovat, abyste odstranili bránu a pak nakonfigurovali nové brány. To znamená, že budete mít během odstraňování a opětného vytváření brány a připojení výpadek připojení mezi místy, ale nebude nutné migrovat žádné virtuální počítače a služby do nové virtuální sítě. Virtuální počítače a služby budou během konfigurace brány stále schopné komunikovat prostřednictvím nástroje pro vyrovnávání zatížení, pokud jsou tak nakonfigurované.


## <a name="new"></a>Vytvoření nové virtuální sítě a koexistujících připojení

Tento postup vás provede procesem vytvoření virtuální sítě a vytvoření připojení ExpressRoute a VPN site-to-site, která budou existovat společně.
    
1. Budete potřebovat nainstalovat nejnovější verzi rutin Azure PowerShellu. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md). Všimněte si, že rutiny, které budete používat pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte. Ujistěte se, že používáte rutiny určené v těchto pokynech.

2. Přihlaste se ke svému účtu a nastavte prostředí.
    
        login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
        $location = "Central US"
        $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location

3. Vytvořte virtuální síť včetně podsítě brány. Další informace o konfiguraci virtuální sítě najdete v tématu [Konfigurace Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).

    >[AZURE.IMPORTANT] Podsíť brány musí být /27 nebo kratší předpona (například /26 nebo /25).
    
    Vytvořte novou virtuální síť.

        $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16" 

    Přidejte podsítě.

        Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Uložte konfiguraci virtuální sítě.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

4. <a name="gw"></a>Vytvořte bránu ExpressRoute. Další informace o konfiguraci brány ExpressRoute najdete v tématu [Konfigurace brány ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). GatewaySKU musí být *Standard* nebo *HighPerformance*.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard 

5. Propojte bránu ExpressRoute s okruhem ExpressRoute. Po dokončení tohoto kroku bude připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno. Další informace o operaci propojení najdete v tématu [Propojení virtuálních sítí s ExpressRoute](expressroute-howto-linkvnet-arm.md).

        $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
        New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute

6. <a name="vpngw"></a>Dál vytvořte bránu VPN typu site-to-site. Další informace o konfiguraci brány VPN najdete v tématu [Konfigurace připojení mezi virtuálními sítěmi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). GatewaySKU musí být *Standard* nebo *HighPerformance*. VpnType musí být *RouteBased*.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"

7. Vytvořte entitu brány VPN místního webu. Tento příkaz neprovede konfiguraci vaší místní brány VPN. Místo toho umožní zadat nastavení místní brány, jako je například veřejná IP adresa a místní adresní prostor, aby se brána Azure VPN k nim mohla připojit. 
    >[AZURE.NOTE] Pokud vaše místní síť obsahuje víc tras, můžete je předat všechny najednou jako pole.  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")  

        $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix '10.100.0.0/16'

8. Nakonfigurujte místní zařízení VPN pro připojení k nové bráně Azure VPN. Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Propojte bránu VPN typu site-to-site v Azure s místní bránou.

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>


## <a name="add"></a>Konfigurace koexistujících připojení pro už existující virtuální síť

Pokud máte ve existující virtuální síť, zkontrolujte velikost podsítě brány. Pokud podsíť brány je /28 nebo /29, musíte nejdřív bránu virtuální sítě odstranit a zvýšit velikost podsítě brány. Postup v této části ukazuje, jak to provést.

Pokud podsíť brány je /27 nebo větší a virtuální síť je připojená přes ExpressRoute, můžete přeskočit následující kroky a přejít ke [kroku 6 – Vytvoření brány VPN typu site-to-site](#vpngw) v předchozí části. 

>[AZURE.NOTE] Pokud odstraníte existující bránu, místní místo ztratí během práce na této konfiguraci připojení k virtuální síti. 

1. Budete potřebovat nainstalovat nejnovější verzi rutin Azure PowerShellu. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md). Všimněte si, že rutiny, které budete používat pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte. Ujistěte se, že používáte rutiny určené v těchto pokynech. 

2. Odstraňte existující bránu ExpressRoute nebo VPN typu site-to-site. 

        Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>

3. Odstraňte podsíť brány.
        
        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> 
        Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet

4. Přidejte podsíť brány, který je /27 nebo větší.
    >[AZURE.NOTE] Pokud vám ve virtuální síti nezbylo dost IP adres pro zvětšení velikosti podsítě brány, budete muset přidat další adresní prostor IP adres.

        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Uložte konfiguraci virtuální sítě.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

5. V tuto chvíli máte virtuální síť, která nemá žádné brány. Abyste vytvořili nové brány a dokončili připojení, můžete pokračovat [krokem 4 – Vytvoření brány ExpressRoute](#gw), který se nachází v předchozí sadě kroků.

## Přidání konfigurace point-to-site k bráně VPN
Podle následujících pokynů můžete k bráně VPN v nastavení koexistence přidat konfiguraci point-to-site.

1. Přidejte fond adres klienta VPN. 

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"

2. Odešlete kořenový certifikát VPN pro bránu VPN do Azure. V tomto příkladu se předpokládá, že kořenový certifikát je uložený v místním počítači, kde se spustí následující rutiny PowerShellu. 

        $p2sCertFullName = "RootErVpnCoexP2S.cer"
        $p2sCertMatchName = "RootErVpnCoexP2S"
        $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName}
        if ($p2sCertToUpload.count -eq 1){
            write-host "cert found"
        } else {
            write-host "cert not found"
            exit
        } 
        $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData)
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData

Další informace o VPN typu point-to-site najdete v tématu [Konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## Další kroky

Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).



<!---HONumber=Aug16_HO4-->


