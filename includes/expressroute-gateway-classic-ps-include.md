Je nutné vytvořit virtuální síť a podsíť brány nejprve, před prací na následující úlohy. Najdete v článku [konfigurace virtuální sítě pomocí portálu classic](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Další informace.   

## <a name="add-a-gateway"></a>Přidat bránu
Použijte následující příkaz k vytvoření brány. Nezapomeňte nahradit všechny hodnoty vlastními.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Ověřte, že vytvoření brány
Použijte následující příkaz k ověření, zda byl vytvořen brány. Tento příkaz načte také ID brány, které potřebujete pro jiné operace.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Změnit velikost brány
Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Tento příkaz můžete kdykoli změnit skladová položka brány.

> [!IMPORTANT]
> Tento příkaz nefunguje pro UltraPerformance bránu. Chcete-li změnit bránu pro bránu UltraPerformance, nejprve odeberte existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance. Přejít na starší verzi bránu z bránu UltraPerformance, nejprve odeberte UltraPerformance brány a poté vytvořit novou bránu. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Odebrat bránu
Pomocí následujícího příkazu odeberte brány

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>