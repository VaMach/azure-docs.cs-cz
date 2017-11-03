---
title: "Virtuální sítě Azure a virtuální počítače s Windows | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/12/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 21f2d586a4c468071bec55c65005b35baf323fe7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Správa virtuálních sítí Azure a virtuální počítače s Windows pomocí Azure Powershellu

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače. V příkladech v tomto kurzu předpokládá, že virtuální počítače hostují webové aplikace s databáze back-end, ale není v tomto kurzu nasazené aplikace. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuální sítě a podsítě
> * Vytvoření veřejné IP adresy
> * Vytvoření virtuálního počítače s front-endu
> * Zabezpečení provozu sítě
> * Vytvoření virtuálního počítače s back-end

Při dokončení tohoto kurzu, zobrazí se tyto prostředky vytvořené:

![Virtuální síť se dvěma podsítěmi](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -virtuální síť, která virtuální počítače používat ke komunikaci s sebe navzájem a Internetem.
- *myFrontendSubnet* -podsítě v *myVNet* používá front-endové prostředky.
- *myPublicIPAddress* -veřejnou IP adresu použitou pro přístup k *myFrontendVM* z Internetu.
- *myFrontentNic* -síťové rozhraní používá *myFrontendVM* ke komunikaci s *myBackendVM*.
- *myFrontendVM* -virtuálního počítače používá ke komunikaci mezi Internetu a *myBackendVM*.
- *myBackendNSG* -skupina zabezpečení sítě, které řídí komunikaci mezi *myFrontendVM* a *myBackendVM*.
- *myBackendSubnet* -podsíť k *myBackendNSG* a používané prostředky back-end.
- *myBackendNic* -síťové rozhraní používá *myBackendVM* ke komunikaci s *myFrontendVM*.
- *myBackendVM* -virtuální počítač, který používá port 1433 ke komunikaci s *myFrontendVM*.

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Přehled sítě virtuálních počítačů

Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, internet a jinými službami Azure, jako je například Azure SQL database. Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě. Podsítě se používají k řízení toku sítě a jako hranice zabezpečení. Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, který je připojen k podsíti.

## <a name="create-a-virtual-network-and-subnet"></a>Vytvoření virtuální sítě a podsítě

V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi. Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.

Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků pomocí [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v *EastUS* umístění:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Vytvoření konfigurací podsítě

Vytvoření konfigurace podsíť s názvem *myFrontendSubnet* pomocí [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

A vytvořit konfiguraci podsíť s názvem *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Vytvoření virtuální sítě s názvem *myVNet* pomocí *myFrontendSubnet* a *myBackendSubnet* pomocí [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby. V další části jsou virtuální počítače vytvořené a připojené k tyto podsítě.

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy

Veřejná IP adresa umožňuje prostředků Azure bude přístupný na Internetu. Metoda přidělení veřejné IP adresy můžete konfigurovat jako dynamické nebo statické. Ve výchozím nastavení se přidělí dynamicky veřejnou IP adresu. Dynamické IP adresy vydávají, když virtuální počítač je navrácena. To způsobí, že IP adresa, chcete-li změnit během všechny operace, která zahrnuje navrácení virtuálních počítačů.

Statický, které zajišťuje, že IP adresa zůstane i během deallocated stavu přiřazené k virtuálnímu počítači, může být nastavena metoda přidělení. Pokud používáte staticky přidělená adresa IP, nelze zadat IP adresu. Místo toho je přidělen z fondu adres k dispozici.

Vytvoření veřejné IP adresy s názvem *myPublicIPAddress* pomocí [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Můžete změnit pomocí parametru - AllocationMethod `Static` přiřadit statickou veřejnou IP adresu.

## <a name="create-a-front-end-vm"></a>Vytvoření virtuálního počítače s front-endu

Pro virtuální počítač pro komunikaci ve virtuální síti musí být virtuální síťové rozhraní (NIC). Vytvoření síťový adaptér pomocí [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Nastavte uživatelské jméno a heslo, které potřebuje pro účet správce na virtuální počítač pomocí [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Použít tyto přihlašovací údaje pro připojení k virtuálnímu počítači v dalších krocích:

```azurepowershell-interactive
$cred = Get-Credential
```

Vytvořit virtuální počítače pomocí [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="secure-network-traffic"></a>Zabezpečení provozu sítě

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která prostředkům připojeným k virtuálním sítím Azure povolují nebo odpírají síťový provoz. Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivých síťových rozhraní. Když skupinu NSG je spojen s síťovým rozhraním, bude se vztahovat jenom přidružený virtuální počítač. Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla se vztahují na všechny prostředky, které jsou připojené k příslušné podsíti.

### <a name="network-security-group-rules"></a>Pravidla skupiny zabezpečení sítě

Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný. Pravidla mohou obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě. Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096). Pravidla jsou vyhodnocovány v pořadí podle priority. Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.

Všechny skupiny NSG obsahují sadu výchozích pravidel. Výchozí pravidla se nedají odstranit, ale protože je jim přiřazená nejnižší priorita, dají se přepsat pravidly, která vytvoříte.

- **Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.
- **Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.
- **Nástroj pro vyrovnávání zatížení** – nástroj pro vyrovnávání zatížení povolit Azure testovat stav virtuálních počítačů a instancí rolí. Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.

### <a name="create-network-security-groups"></a>Vytvoření skupiny zabezpečení sítě

Vytvoření příchozího pravidla s názvem *myFrontendNSGRule* na Povolit příchozí webové přenosy *myFrontendVM* pomocí [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Můžete omezit interní provoz *myBackendVM* z jenom *myFrontendVM* tak, že vytvoříte skupinu NSG pro podsíť back-end. Následující příklad vytvoří pravidlo NSG s názvem *myBackendNSGRule*:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Přidat skupinu zabezpečení sítě s názvem *myFrontendNSG* pomocí [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Nyní přidejte skupinu zabezpečení sítě s názvem *myBackendNSG* pomocí New-AzureRmNetworkSecurityGroup:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Přidání skupin zabezpečení sítě k podsítím provozu:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Vytvoření virtuálního počítače s back-end

Nejjednodušší způsob, jak vytvořit virtuální počítač back-end pro účely tohoto kurzu je pomocí bitové kopie systému SQL Server. V tomto kurzu pouze vytvoří virtuální počítač s databázovým serverem, ale neposkytuje informace o přístupu k databázi.

Vytvoření *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Nastavte uživatelské jméno a heslo pro účet správce ve virtuálním počítači s Get-Credential potřebné:

```azurepowershell-interactive
$cred = Get-Credential
```

Vytvoření *myBackendVM*:

```azurepowershell-interactive
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

Obrázek, který je použit nainstalován server SQL, ale není použit v tomto kurzu. Je zahrnutá ukázat vám, jak můžete nakonfigurovat virtuální počítač pro zpracování webových přenosů a virtuální počítač pro správu databáze.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure. 

> [!div class="checklist"]
> * Vytvoření virtuální sítě a podsítě
> * Vytvoření veřejné IP adresy
> * Vytvoření virtuálního počítače s front-endu
> * Zabezpečení provozu sítě
> * Vytvoření virtuálního počítače s back-end

Přechodu na v dalším kurzu se dozvíte o monitorování zabezpečení dat na virtuální počítače pomocí zálohování Azure.

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Windows v Azure](./tutorial-backup-vms.md)
