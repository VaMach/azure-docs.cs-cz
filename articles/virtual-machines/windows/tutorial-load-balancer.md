---
title: "Jak načíst vyvážit virtuální počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o použití nástroje pro vyrovnávání zatížení Azure k vytvoření vysoce dostupné a zabezpečené aplikace napříč tři virtuální počítače Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6738d88d5a0430abaf3855dbf97a618e4c83617f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Jak načíst vyvážit virtuální počítače s Windows v Azure k vytvoření vysoce dostupné aplikace
Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů. V tomto kurzu informace o různé součásti nástroje pro vyrovnávání zatížení Azure, které distribuci přenosů a zajištění vysoké dostupnosti. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použití rozšíření vlastních skriptů pro vytvoření základní webu služby IIS
> * Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení Azure
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.

Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy. Tuto konfiguraci front-end IP adresy umožňuje Vyrovnávání zatížení a aplikace přístupné přes Internet. 

Virtuální počítače připojit k nástroji pro vyrovnávání zatížení pomocí jejich virtuální síťová karta (NIC). K distribuci provoz na virtuální počítače, fond back-end adres obsahuje IP adresy virtuální (NIC) připojené ke službě Vyrovnávání zatížení.

Pokud chcete řídit tok přenosů dat, definujete pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které jsou mapovány na virtuální počítače.


## <a name="create-azure-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení Azure
Tato část podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení. Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v *EastUS* umístění:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
Pro přístup k vaší aplikace v síti Internet, musíte nástroj pro vyrovnávání zatížení veřejnou IP adresu. Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v *myResourceGroupLoadBalancer* skupiny prostředků:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení
Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Následující příklad vytvoří na front-endovou IP adresu s názvem *myFrontEndPool*: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Následující příklad vytvoří fond back-end adresy s názvem *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Nyní, vytvoří se službou Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* pomocí *myPublicIP* adresa:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Vytvoření test stavu
Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu. Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund. Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci. 

Následující příklad vytvoří sondou TCP. Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu. Pokud používáte vlastní sondu HTTP, musíte vytvořit stránka pro kontrolu stavu, jako například *healthcheck.aspx*. Sondy musí vrátit **HTTP 200 OK** odpovědi pro vyrovnávání zatížení na hostiteli mějte otočení.

K vytvoření stavu sondou TCP, použijete [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Následující příklad vytvoří kontrolu stavu s názvem *myHealthProbe* který monitoruje každý virtuální počítač:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.
Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům. Můžete definovat front-endové konfiguraci protokolu IP pro příchozí provoz a fond back-end IP příjem provozu, společně s požadovaný zdrojový a cílový port. Pokud chcete mít jistotu, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat test stavu použít.

Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem *myLoadBalancerRule* a vyrovnává přenosy na portu *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě. Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Následující příklad vytvoří virtuální síť s názvem *myVnet* s *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Vytvoření pravidla skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), pak vytvořte skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Přidat skupinu zabezpečení sítě pro podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) a aktualizujte virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroup* a použije je k *mySubnet*:

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuální síťové adaptéry jsou vytvořeny pomocí [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů
Pokud chcete zvýšit vysokou dostupnost vaší aplikace, umístíte virtuální počítače v nastavení dostupnosti.

Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Nastavte správce uživatelské jméno a heslo pro virtuální počítače s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nyní můžete vytvořit virtuálních počítačů s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Následující příklad vytvoří tři virtuální počítače:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Trvá několik minut vytvořit a nakonfigurovat všechny tři virtuální počítače.

### <a name="install-iis-with-custom-script-extension"></a>Nainstalovat službu ISS s rozšíření vlastních skriptů
V předchozích kurz [postup přizpůsobení virtuálního počítače s Windows](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače pomocí ovládacího prvku vlastní skript rozšíření pro Windows. Stejnou metodu můžete použít k instalaci a konfiguraci služby IIS na virtuální počítače.

Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů. Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS a aktualizací *Default.htm* stránku a zobrazit název hostitele virtuálního počítače:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Nástroj pro vyrovnávání zatížení testu
Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Potom můžete zadat veřejnou IP adresu v do webového prohlížeče. Zobrazí se na webu, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:

![Spuštění webu IIS](./media/tutorial-load-balancer/running-iis-website.png)

Nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny tři virtuální počítače spuštěné aplikace najdete můžete můžete vynutit obnovení webového prohlížeče.


## <a name="add-and-remove-vms"></a>Přidání a odebrání virtuálních počítačů
Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů. V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení.

### <a name="remove-a-vm-from-the-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení
Získat karty síťového rozhraní s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), nastavte *pravidlo LoadBalancerBackendAddressPools* vlastnost virtuálního síťového adaptéru do *$null*. Nakonec aktualizujte virtuální síťový adaptér.:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče. Nyní můžete provést údržbu na virtuální počítač, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.

### <a name="add-a-vm-to-the-load-balancer"></a>Přidat virtuální počítač ke službě Vyrovnávání zatížení
Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, nastavte *pravidlo LoadBalancerBackendAddressPools* vlastnost virtuálního síťového adaptéru do *BackendAddressPool* z [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Získáte nástroje pro vyrovnávání zatížení:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili pro vyrovnávání zatížení a je připojený virtuální počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použití rozšíření vlastních skriptů pro vytvoření základní webu služby IIS
> * Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Přechodu na v dalším kurzu se dozvíte, jak ke správě sítí virtuálních počítačů.

> [!div class="nextstepaction"]
> [Správa virtuálních počítačů a virtuálních sítí](./tutorial-virtual-network.md)
