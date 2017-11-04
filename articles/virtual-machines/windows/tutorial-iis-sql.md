---
title: "Vytvořte virtuální počítače se systémem SQL &#92; IIS &#92;. NET zásobníku v Azure | Microsoft Docs"
description: "Kurz – instalace Azure SQL, služby IIS, .NET zásobníku na virtuální počítače Windows."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6f7ef46d9c40138c211427845423783fefde5dc3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Nainstalujte SQL &#92; IIS &#92;. NET zásobníku v Azure

V tomto kurzu jsme nainstalovat SQL &#92; IIS &#92;. NET zásobníku pomocí Azure PowerShell. Tato zásobníku se skládá z dva virtuální počítače se systémem Windows Server 2016 s služby IIS a rozhraní .NET a dalších s SQL serverem.

> [!div class="checklist"]
> * Vytvoření virtuálního počítače pomocí New-AzVM
> * Do virtuálního počítače nainstalujte službu IIS a .NET Core SDK
> * Vytvoření virtuálního počítače s SQL serverem
> * Nainstalujte rozšíření systému SQL Server



## <a name="create-a-iis-vm"></a>Vytvoření virtuálního počítače služby IIS 

V tomto příkladu používáme [New-AzVM](https://www.powershellgallery.com/packages/AzureRM.Compute.Experiments) rutiny v prostředí PowerShell cloudu se rychle vytvořit virtuální počítač s Windows Server 2016 a potom nainstalovat službu IIS a rozhraní .NET Framework. Službu IIS a virtuálním počítačům systému SQL sdílet skupinu prostředků a virtuální síti, proto jsme vytvořit proměnných pro tyto názvy.

Klikněte na **zkuste ho** tlačítko pravém horním rohu stránky blok kódu spusťte cloudové prostředí v tomto okně. Zobrazí se výzva k zadání přihlašovacích údajů pro virtuální počítač na příkazový řádek.

```azurepowershell-interactive
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzVm -Name myIISVM -ResourceGroupName $resourceGroup -VirtualNetworkName $vNetName 
```

Instalace IIS a rozhraní .NET framework pomocí rozšíření vlastních skriptů.

```azurepowershell-interactive

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName myIISVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="azure-sql-vm"></a>Virtuální počítač Azure SQL

Chcete-li vytvořit virtuální počítač SQL používáme předem nakonfigurovaná Azure marketplace bitovou kopii systému SQL server. Nejdříve vytvoříme virtuální počítač a potom jsme do virtuálního počítače nainstalujte rozšíření serveru SQL. 


```azurepowershell-interactive
# Create user object. You get a pop-up prompting you to enter the credentials for the VM.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroup
Add-AzureRmVirtualNetworkSubnetConfig -Name mySQLSubnet -VirtualNetwork $vNet -AddressPrefix "192.168.2.0/24"
Set-AzureRmVirtualNetwork -VirtualNetwork $vNet


# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location eastus `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow


# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location eastus `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name mySQLNic -ResourceGroupName $resourceGroup -Location eastus `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName mySQLVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName mySQLVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftSQLServer -Offer SQL2014SP2-WS2012R2 -Skus Enterprise -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM -ResourceGroupName $resourceGroup -Location eastus -VM $vmConfig
```

Použití [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) přidat [rozšíření systému SQL Server](/sql/virtual-machines-windows-sql-server-agent-extension.md) do virtuálního počítače SQL.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension -ResourceGroupName $resourceGroup -VMName mySQLVM -name "SQLExtension"
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nainstalovali SQL &#92; IIS &#92;. NET zásobníku pomocí Azure PowerShell. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače pomocí New-AzVM
> * Do virtuálního počítače nainstalujte službu IIS a .NET Core SDK
> * Vytvoření virtuálního počítače s SQL serverem
> * Nainstalujte rozšíření systému SQL Server

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webového serveru IIS s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru IIS s certifikáty protokolu SSL](tutorial-secure-web-server.md)

