---
title: "Rychlý start Azure – Vytvoření virtuálního počítače s Windows pomocí PowerShellu | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvářet virtuální počítače s Windows pomocí PowerShellu."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/14/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 9c14a009de1bfa6b3c5baa5cd0c09498760c9549
ms.lasthandoff: 03/22/2017


---

# <a name="create-a-windows-virtual-machine-with-powershell"></a>Vytvoření virtuálního počítače s Windows pomocí PowerShellu

Modul Azure PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku PowerShellu nebo ve skriptech. Tento průvodce podrobně popisuje vytvoření virtuálního počítače s Windows Serverem 2016 pomocí PowerShellu. 

Než začnete, ujistěte se, že je nainstalovaná nejnovější verze modulu Azure PowerShell. Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-networking-resources"></a>Vytvoření síťových prostředků

Vytvořte virtuální síť, podsíť a veřejnou IP adresu. Tyto prostředky slouží k zajištění síťového připojení virtuálnímu počítači a k jeho připojení k internetu.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location westeurope `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location westeurope `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Vytvořte skupinu zabezpečení sítě a pravidlo skupiny zabezpečení sítě. Skupina zabezpečení sítě zabezpečuje virtuální počítač pomocí příchozích a odchozích pravidel. V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location westeurope `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
```

Vytvořte pro virtuální počítač síťovou kartu. Síťová karta připojuje virtuální počítač k podsíti, skupině zabezpečení sítě a veřejné IP adrese.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location westeurope `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte konfiguraci virtuálního počítače. Tato konfigurace zahrnuje nastavení, která se používají při nasazení virtuálního počítače, jako je image virtuálního počítače, jeho velikost a konfigurace ověřování. Při spuštění tohoto kroku se zobrazí výzva k zadání přihlašovacích údajů. Hodnoty, které zadáte, se nakonfigurují jako uživatelské jméno a heslo pro virtuální počítač.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Vytvořte virtuální počítač.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Připojení k virtuálnímu počítači

Po dokončení nasazení vytvořte připojení ke vzdálené ploše virtuálního počítače.

Spuštění následujících příkazů vrátí veřejnou IP adresu virtuálního počítače.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci vzdálené plochy. IP adresu nahraďte veřejnou IP adresou vašeho virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje, které jste použili při vytváření virtuálního počítače.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="delete-virtual-machine"></a>Odstranění virtuálního počítače

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít následující příkaz.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz instalace role a konfigurace brány firewall](./virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Prozkoumejte ukázky nasazení virtuálního počítače pomocí PowerShellu](./virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
