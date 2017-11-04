---
title: "Vytvoření virtuálního počítače s SQL serverem v prostředí Azure PowerShell (Resource Manager) | Microsoft Docs"
description: "Obsahuje kroky a skriptů prostředí PowerShell pro vytvoření virtuálního počítače Azure s obrázky Galerie virtuálního počítače systému SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/29/2017
ms.author: jroth
ms.openlocfilehash: 33c306258b6be40f2c5cbc016e3c84e36bf61e0d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Přehled
V tomto kurzu se dozvíte, jak vytvořit jeden virtuální počítač Azure pomocí **Azure Resource Manager** modelu nasazení pomocí rutin prostředí Azure PowerShell. V tomto kurzu vytvoříme jednoho virtuálního počítače pomocí jednoho disku z bitové kopie v galerii SQL. Vytvoříme nové zprostředkovatele pro úložiště, síť a výpočetní prostředky, které se použijí pro virtuální počítač. Pokud máte existující zprostředkovatele pro kterýkoli z těchto prostředků, můžete místo toho použít těchto zprostředkovatelů.

Pokud potřebujete classic verzi tohoto tématu, přečtěte si [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Classic](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Požadavky
Pro účely tohoto kurzu budete potřebovat:

* Účet Azure a předplatné před zahájením. Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* [Azure PowerShell)](/powershell/azure/overview), minimální verze 1.4.0 nebo novější (Tento kurz vytvořené pomocí verze 1.5.0).
  * Pokud chcete načíst vaše verze, zadejte **Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Konfigurovat předplatné
Otevřete prostředí Windows PowerShell a spuštěním následující rutiny vytvořit přístup k účtu Azure. Zobrazí se přihlašovací obrazovka k zadání pověření. Použijte stejnou e-mailu a heslo, které používáte pro přihlášení k portálu Azure.

```PowerShell
Add-AzureRmAccount
```

Po úspěšném přihlášení zobrazte některé informace na obrazovce, která obsahuje název odběru a ID předplatného výchozí. Toto je předplatné, ve kterém se vytvoří prostředky pro tento kurz, pokud nezměníte do jiného předplatného. Pokud máte více předplatných, spusťte následující rutinu k zobrazení seznamu všech předplatných:

```PowerShell
Get-AzureRmSubscription
```
Chcete-li změnit do jiného předplatného, spusťte následující příkaz s cílových Název_předplatného.

```PowerShell
Select-AzureRmSubscription -SubscriptionName YourTargetSubscriptionName
```

## <a name="define-image-variables"></a>Definování proměnné bitovou kopii
Pro zjednodušení použitelnost a pochopení dokončené skriptu z tohoto kurzu, spustíme definováním počet proměnných. Podle svých potřeb, ale pozor omezení týkající se názvu délky a speciální znaky, při úpravě hodnoty poskytnuté pojmenování změně hodnot parametrů.

### <a name="location-and-resource-group"></a>Umístění a skupiny prostředků
Dvě proměnné, které slouží k určení oblasti dat a skupině prostředků, do kterého chcete vytvořit další prostředky pro virtuální počítač.

Upravte požadovaným způsobem a potom spusťte následující rutiny k chybě při inicializaci tyto proměnné.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"
```

### <a name="storage-properties"></a>Úložiště vlastností
Použijte následující proměnné zadat účet úložiště a typ úložiště, který se má použít pro virtuální počítač.

Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné. Všimněte si, že v tomto příkladu používáme [Storage úrovně Premium](../premium-storage.md), který se doporučuje pro úlohy v produkčním prostředí. Podrobnosti na tyto pokyny a dalších doporučeních najdete v tématu [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Vlastnosti sítě
Použijte následující proměnné k definování síťového rozhraní, metoda přidělení TCP/IP, název virtuální sítě, název virtuální podsítě, rozsah IP adres pro virtuální síť, rozsah IP adres pro podsíť a popisek názvu veřejné domény pro použití sítě ve virtuálním počítači.

Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = "sqlvm1"
```

### <a name="virtual-machine-properties"></a>Vlastnosti virtuálního počítače
Zadat název virtuálního počítače, název počítače, velikost virtuálního počítače a název disku operačního systému pro virtuální počítač, použijte následující proměnné.

Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="image-properties"></a>Vlastnosti bitové kopie
Chcete-li definovat obrázek, který má používat pro virtuální počítač, použijte následující proměnné. V tomto příkladu se používá bitovou kopii SQL serveru 2016 Developer edition. Developer edition je volně licenci pro vývoj a testování a platíte jenom pro náklady na provozování virtuálního počítače.

Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.

```PowerShell
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "SQLDEV"
$Version = "latest"
```

Všimněte si, že můžete získat úplný seznam nabídky bitovou kopii systému SQL Server pomocí příkazu Get-AzureRmVMImageOffer:

```PowerShell
Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'
```

A zobrazí se dostupné pro nabídky pomocí příkazu Get-AzureRmVMImageSku SKU. Následující příkaz ukazuje všech skladových položek k dispozici pro **SQL2016SP1 WS2016** nabízejí.

```PowerShell
Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer 'SQL2016SP1-WS2016' | Select Skus
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Pomocí modelu nasazení Resource Manager je první objekt, který vytvoříte skupinu prostředků. Budeme používat [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) vytvořte skupinu prostředků Azure a jeho prostředků se název skupiny prostředků a umístění definované proměnné, které jste dříve inicializován.

Spusťte následující rutinu k vytvoření nové skupiny prostředků.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Virtuální počítač vyžaduje prostředků úložiště pro disk operačního systému a souborů protokolu a data systému SQL Server. Pro jednoduchost vytvoříme jediný disk pro obojí. Můžete připojit další disky později pomocí [disku Azure přidat](/powershell/module/azure/add-azuredisk) rutiny, aby bylo možné umístit data systému SQL Server a protokolu se soubory ve vyhrazených disků. Budeme používat [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) vytvořte standardní účet úložiště v nové skupiny prostředků a název účtu úložiště, název Sku úložiště a umístění definovat pomocí proměnné, které jste dříve inicializován.

Spusťte následující rutinu k vytvoření nového účtu úložiště.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location
```

## <a name="create-network-resources"></a>Vytvoření síťové prostředky
Virtuální počítač vyžaduje počet síťovým prostředkům pro připojení k síti.

* Každý virtuální počítač vyžaduje virtuální síť.
* Virtuální síť musí mít alespoň jednu podsíť definované.
* Síťové rozhraní musí být definovány se veřejné nebo privátní IP adresy.

### <a name="create-a-virtual-network-subnet-configuration"></a>Vytvoření konfigurace podsítě virtuální sítě
Spustíme vytvořením konfigurace pro naše virtuální síť s podsítí. V našem kurzu vytvoříme výchozí podsíť pomocí [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) rutiny. Pomocí názvu a adresy předponu podsítě definovat pomocí proměnné, které jste dříve inicializovat vytvoříme naše konfigurace podsítě virtuální sítě.

> [!NOTE]
> Můžete definovat další vlastnosti konfigurace podsítě virtuální sítě pomocí této rutiny, ale který je nad rámec tohoto kurzu.

Spusťte následující rutiny můžete vytvořit konfiguraci virtuální podsítě.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
V dalším kroku vytvoříme naše pomocí virtuální sítě [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) rutiny. Vytvoříme naše virtuální sítě v nové skupiny prostředků, název, umístění a adresu předpona definovaná pomocí proměnné, které jste dříve inicializovat a pomocí konfigurace podsítě, který jste zadali v předchozím kroku.

Spusťte následující rutinu pro vytvoření svojí virtuální sítě.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Vytvoření veřejné IP adresy
Teď, když máme naše virtuální síť definován, je potřeba nakonfigurovat IP adresu pro připojení k virtuálnímu počítači. V tomto kurzu vytvoříme veřejnou IP adresu pomocí dynamických IP adres pro podporu připojení k Internetu. Budeme používat [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) vytvořte veřejnou IP adresu v prevously vytvořenou skupinu prostředků a název, umístění, metoda přidělení a popisek názvu domény DNS definovat pomocí proměnné, které jste dříve inicializován.

> [!NOTE]
> Můžete definovat další vlastnosti veřejné IP adresy pomocí této rutiny, ale který je nad rámec tohoto počáteční kurzu. Můžete také vytvořit privátní adresu nebo adresu se statickou adresou, ale který je také nad rámec tohoto kurzu.

Spusťte následující rutiny můžete vytvořit veřejnou IP adresu.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-interface"></a>Vytvořit rozhraní sítě
Nyní jsme připraveni k vytvoření síťového rozhraní, které budou používat naše virtuálního počítače. Budeme používat [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) předtím definovaný vytvořte naše síťové rozhraní, ve skupině prostředků vytvořili dříve a název, umístění, podsíť a veřejnou IP adresu.

Spusťte následující rutinu k vytvoření síťového rozhraní.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id
```

## <a name="configure-a-vm-object"></a>Konfigurace objektu virtuálního počítače
Teď, když máme úložiště a síťové prostředky, které jsou definované, jsme připraveni k definování výpočetní prostředky pro virtuální počítač. V našem kurzu jsme zadejte velikost virtuálního počítače a různé vlastnosti operačního systému, zadejte definovat úložiště objektů blob síťového rozhraní, které jsme dříve vytvořili, a pak zadejte disku operačního systému.

### <a name="create-the-vm-object"></a>Vytvořit objekt virtuálního počítače
Spustíme zadáním velikost virtuálního počítače. V tomto kurzu zadáváme DS13. Budeme používat [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) vytvořte objekt konfigurovat virtuální počítač s názvem a velikostí, které jsou definované pomocí proměnné, které jste dříve inicializován.

Spusťte následující rutinu k vytvoření objektu virtuálního počítače.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Vytvořit objekt přihlašovacích údajů k uchování jméno a heslo pro přihlašovací údaje místního správce
Než budeme moct nastavit vlastnosti operačního systému pro virtuální počítač, je potřeba zadat přihlašovací údaje pro účet místního správce jako zabezpečený řetězec. K tomu, budeme používat [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.

Spusťte následující rutinu a v okně požadavku pověření prostředí Windows PowerShell, zadejte jméno a heslo pro účet místního správce ve virtuálním počítači Windows.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Nastavit vlastnosti operačního systému pro virtuální počítač
Nyní jsme připravení nastavit vlastnosti operačního systému virtuálního počítače. Budeme používat [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) vyžadují rutiny nastavte typ operačního systému jako Windows, [agenta virtuálního počítače](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) k instalaci, zadejte, že rutina umožňuje automatickou aktualizaci a nastavte název virtuálního počítače, název počítače a přihlašovacích údajů pomocí proměnné, které jste dříve inicializován.

Spusťte následující rutinu a nastavte vlastnosti operačního systému pro virtuální počítač.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Přidání rozhraní sítě k virtuálnímu počítači
Potom přidáme síťového rozhraní, které jsme vytvořili dříve k virtuálnímu počítači. Budeme používat [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) přidejte síťové rozhraní, pomocí proměnné rozhraní sítě, která jste definovali dříve.

Spusťte následující rutiny můžete nastavit rozhraní sítě pro virtuální počítač.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Nastavení umístění úložiště objektů blob pro disk, který se má použít pro virtuální počítač
V dalším kroku se nastaví umístění úložiště objektů blob pro disk, který se má použít pro virtuální počítač pomocí proměnné, které jste definovali dříve.

Spusťte následující rutiny můžete nastavit umístění úložiště objektů blob.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Nastavit vlastnosti disku pro virtuální počítač operační systém
Operační systém v dalším kroku bude nastaví vlastnosti disku pro virtuální počítač. Budeme používat [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) rutiny k určení, že operační systém pro virtuální počítač bude pocházet z bitové kopie, nastavení ukládání do mezipaměti pro čtení, (protože SQL Server je instalován na stejném disku) a zadejte název virtuálního počítače a disku operačního systému definovaná pomocí proměnné, které definovaného dříve.

Spusťte následující rutiny můžete nastavit vlastnosti disku pro virtuální počítač operační systém.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Zadat image platformy pro virtuální počítač
Naše poslední krok konfigurace slouží k zadání image platformy pro naše virtuální počítač. V našem kurzu se používá nejnovější bitovou kopii systému SQL Server 2016 CTP. Budeme používat [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny k použití této image podle definice proměnné, které jste definovali dříve.

Spusťte následující rutiny můžete zadat image platformy pro virtuální počítač.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>Vytvoření virtuálního počítače SQL
Teď, když dokončíte kroky konfigurace, jste připraveni k vytvoření virtuálního počítače. Budeme používat [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) rutiny k vytvoření virtuálního počítače pomocí proměnné, které jsme definovali.

Spusťte následující rutinu k vytvoření virtuálního počítače.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Při vytvoření virtuálního počítače. Všimněte si, že standardní účet úložiště je pro Diagnostika spouštění vytvořit, protože zadaný účet úložiště pro disk virtuálního počítače je prémiový účet úložiště.

Tento počítač nyní můžete zobrazit na portálu Azure najdete v části [jeho veřejnou IP adresu a jeho plně kvalifikovaný název domény](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Ukázkový skript
Následující skript obsahuje dokončení skriptu prostředí PowerShell pro účely tohoto kurzu. Se předpokládá, že máte již instalace předplatné Azure pro použití s **Add-AzureRmAccount** a **Select-AzureRmSubscription** příkazy.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = "sqlvm1"

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "Enterprise"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

## <a name="next-steps"></a>Další kroky
Po vytvoření virtuálního počítače, jste připraveni k připojení k virtuálnímu počítači pomocí RDP a nastavení připojení. Další informace najdete v tématu [připojení SQL serveru virtuálnímu počítači na platformě Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
