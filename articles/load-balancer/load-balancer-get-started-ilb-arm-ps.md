---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení pomocí PowerShellu v Resource Manageru | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí prostředí PowerShell v Resource Manageru"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 07ea7f3529d5d2e6fde07805663da7c0f3e65d86

---

# <a name="create-an-internal-load-balancer-using-powershell"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

V následujícím postupu se dozvíte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru s prostředím PowerShell. S Azure Resource Managerem se jednotlivé položky k vytvoření interního nástroje pro vyrovnávání zatížení nakonfigurují zvlášť, následně se spojí dohromady a vytvoří nástroj pro vyrovnávání zatížení.

Pokud chcete nasadit nástroj pro vyrovnávání zatížení, je nutné vytvořit a nakonfigurovat následující objekty:

* Konfigurace front-endové IP adresy – nakonfiguruje privátní IP adresu pro příchozí síťový provoz.
* Back-endový fond adres – nakonfiguruje síťová rozhraní, která budou přijímat provoz s vyrovnáváním zatížení přicházející z front-endového fondu IP adres.
* Pravidla vyrovnávání zatížení – konfigurace zdrojového a cílového portu pro nástroj pro vyrovnávání zatížení.
* Testy – konfiguruje test stavu pro instance virtuálních počítačů.
* Pravidla příchozího překladu adres (NAT) – konfiguruje pravidla portů pro přímý přístup k některé z instancí virtuálních počítačů.

Další informace o komponentách nástroje pro vyrovnávání zatížení s Azure Resource Managerem najdete v tématu [Podpora nástroje Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

V následujícím postupu se dozvíte, jak nakonfigurovat nástroj pro vyrovnávání zatížení mezi dvěma virtuálními počítači.

## <a name="setup-powershell-to-use-resource-manager"></a>Nastavení prostředí PowerShell pro použití Resource Manageru

Ujistěte se, že máte nejnovější produkční verzi modulu Azure pro prostředí PowerShell, a že je prostředí PowerShell správně nastavené pro přístup k vašemu předplatnému Azure.

### <a name="step-1"></a>Krok 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro příslušný účet.

```powershell
Get-AzureRmSubscription
```

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

### <a name="step-3"></a>Krok 3

Zvolte předplatné Azure, které chcete použít.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Vytvoření skupiny prostředků pro nástroj pro vyrovnávání zatížení

Vytvořte novou skupinu prostředků (tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků).

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření nástroje pro vyrovnávání zatížení používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem NRP-RG a umístěním Západní USA.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Vytvoření virtuální sítě a privátní IP adresy pro front-endový fond IP adres

Vytvoří podsíť virtuální sítě a přiřadí ji do proměnné $backendSubnet.

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Vytvořte virtuální síť:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Vytvoří virtuální síť NRPVNet, přidá do ní podsíť lb-subnet-be, a přiřadí ji do proměnné $vnet.

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Vytvoření front-endového fondu IP adres a back-endového fondu adres

Nastavení front-endového fondu IP adres pro příchozí síťový provoz nástroje pro vyrovnávání zatížení, a back-endového fondu adres pro přijímání provozu s vyrovnáváním zatížení.

### <a name="step-1"></a>Krok 1

Vytvořte front-endový fond IP adres pomocí privátní IP adresy 10.0.2.5 pro podsíť 10.0.2.0/24, která bude koncovým bodem příchozího síťového provozu.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>Krok 2

Nastavte back-endový fond adres, který slouží k přijímání příchozího provozu z front-endového fondu IP adres:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT), testu a nástroje pro vyrovnávání zatížení

Po vytvoření front-endového fondu IP adres a back-endového fondu adres bude třeba vytvořit pravidla, která budou patřit do prostředku nástroje pro vyrovnávání zatížení:

### <a name="step-1"></a>Krok 1

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

Výše uvedený příklad vytváří následující položky:

* Pravidlo překladu adres (NAT), které bude veškerý příchozí provoz na portu 3441 předávat na port 3389.
* Druhé pravidlo překladu adres (NAT), které bude veškerý příchozí provoz na portu 3442 předávat na port 3389.
* Pravidlo nástroje pro vyrovnávání zatížení, které bude vyrovnávat zatížení veškerého příchozího provozu na veřejném portu 80 na port 80 v back-endovém fondu adres.
* Pravidlo testu, které bude kontrolovat stav na stránce HealthProbe.aspx.

### <a name="step-2"></a>Krok 2

Vytvořte nástroj pro vyrovnávání zatížení spojením všech objektů (pravidla překladu adres (NAT), pravidla nástroje pro vyrovnávání zatížení, konfigurace testů) dohromady:

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Vytvoření síťových rozhraní

Po vytvoření interního nástroje pro vyrovnávání zatížení je třeba definovat, která síťová rozhraní budou přijímat příchozí síťový provoz s vyrovnáváním zatížení, pravidla překladu adres (NAT) a testy. Síťové rozhraní je v tomto případě nakonfigurované samostatně a k virtuálnímu počítači jej lze přiřadit později.

### <a name="step-1"></a>Krok 1

Získejte virtuální síť a podsíť prostředku pro vytvoření síťových rozhraní:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

V tomto kroku se vytvoří síťové rozhraní, které bude patřit do back-endového fondu nástroje pro vyrovnávání zatížení, a k němu se přidruží první pravidlo překladu adres (NAT) pro protokol RDP:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>Krok 2

Vytvořte druhé síťové rozhraní s názvem: LB-Nic2-BE:

V tomto kroku se vytvoří druhé síťové rozhraní, přiřadí se ke stejnému back-endovému fondu nástroje pro vyrovnávání zatížení a přidruží se k němu druhé pravidlo překladu adres (NAT) vytvořené pro protokol RDP:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

Konečný výsledek zobrazí následující:

    $backendnic1

Očekávaný výstup:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Krok 3

Pomocí příkazu Add-AzureRmVMNetworkInterface přiřaďte síťové rozhraní k virtuálnímu počítači.

Podrobné pokyny k vytvoření virtuálního počítače a jeho přiřazení k síťovému rozhraní najdete v dokumentaci: [Vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface"></a>Přidání síťového rozhraní

Pokud již máte vytvořený virtuální počítač, můžete síťové rozhraní přidat následujícím postupem:

### <a name="step-1"></a>Krok 1

Načtěte prostředek nástroje pro vyrovnávání zatížení do proměnné (pokud jste tak ještě neučinili). Použijte proměnnou s názvem $lb a stejné názvy z prostředku nástroje pro vyrovnávání zatížení, který jste vytvořili v předchozích krocích.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>Krok 2

Načtěte konfiguraci back-endu do proměnné.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>Krok 3

Načtěte již vytvořené síťové rozhraní do proměnné. Název této proměnné je $nic. Název síťového rozhraní je stejný, jako v předchozím příkladu.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>Krok 4

Změňte konfiguraci back-endu na síťovém rozhraní.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>Krok 5

Uložte objekt síťového rozhraní.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Síťového rozhraní po přidání do back-endového fondu nástroje pro vyrovnávání zatížení začne přijímat síťový provoz na základě pravidel vyrovnávání zatížení pro daný prostředek nástroje pro vyrovnávání zatížení.

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávajícího nástroje pro vyrovnávání zatížení

### <a name="step-1"></a>Krok 1
Použijte nástroj pro vyrovnávání zatížení z předchozího příkladu k přiřazení objektu nástroje pro vyrovnávání zatížení do proměnné $slb pomocí příkazu Get-AzureRmLoadBalancer.

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>Krok 2

V následujícím příkladu do stávajícího nástroje pro vyrovnávání zatížení přidáte nové pravidlo příchozího překladu adres (NAT) pomocí portu 81 ve front-endovém fondu a portu 8181 pro back-endový fond.

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>Krok 3

Uložte novou konfiguraci pomocí příkazu Set-AzureLoadBalancer.

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Odebrání nástroje pro vyrovnávání zatížení

Použijte příkaz Remove-AzureRmLoadBalancer k odstranění dříve vytvořeného nástroje pro vyrovnávání zatížení s názvem NRP-LB ve skupině prostředků s názvem NRP-RG.

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Můžete použít volitelný přepínač -Force, abyste se vyhnuli zobrazení výzvy k odstranění.

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO5-->


