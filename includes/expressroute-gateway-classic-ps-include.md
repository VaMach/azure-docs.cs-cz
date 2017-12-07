Je nutné vytvořit virtuální síť a podsíť brány nejprve, před prací na následující úlohy.

> [!NOTE]
> Tyto příklady se nevztahují na S2S nebo ExpressRoute existovat vedle sebe konfigurace.
> Další informace o práci s bránami v konfiguraci coexist najdete v tématu [konfigurace koexistujících připojení](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Přidat bránu

Použijte následující příkaz k vytvoření brány. Nezapomeňte nahradit všechny hodnoty vlastními.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ověřte, že vytvoření brány

Použijte následující příkaz k ověření, zda byl vytvořen brány. Tento příkaz načte také ID brány, které potřebujete pro jiné operace.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Změnit velikost brány

Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Tento příkaz můžete kdykoli změnit skladová položka brány.

> [!IMPORTANT]
> Tento příkaz nefunguje pro UltraPerformance bránu. Chcete-li změnit bránu pro bránu UltraPerformance, nejprve odeberte existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance. Přejít na starší verzi bránu z bránu UltraPerformance, nejprve odeberte UltraPerformance brány a poté vytvořit novou bránu. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Odebrat bránu

Pomocí následujícího příkazu odeberte brány

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```