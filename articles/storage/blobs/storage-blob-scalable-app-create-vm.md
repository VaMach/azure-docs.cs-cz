---
title: "Vytvořte účet virtuálních počítačů a úložiště pro škálovatelná aplikaci v Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit virtuální počítač, který se má použít ke spuštění škálovatelné aplikace pomocí Azure blob storage"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 011812f5e32537321301dad0c654bca341b3606d
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="create-a-virtual-machine-and-storage-account-for-a-scalable-application"></a>Vytvoření virtuálního počítače a účet úložiště pro škálovatelná aplikace

V tomto kurzu je součástí, jednu z řady. Tento kurz ukazuje, že nasazujete aplikaci, která nahrávání a stahování velkých objemů náhodná data s účtem úložiště Azure. Jakmile budete hotovi, máte konzolové aplikace běžící na virtuálním počítači, že jste nahrávání a stahování velkých objemů dat do účtu úložiště.

V rámci jedna řada, zjistíte, jak:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Vytvoření virtuálního počítače
> * Konfigurovat rozšíření vlastních skriptů

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
 
Ukázka ukládání 50 velkých souborů na kontejner objektů blob v účtu Azure Storage. Účet úložiště poskytuje jedinečný obor názvů pro ukládání a přístup k vaší datové objekty Azure storage. Vytvořit účet úložiště ve skupině prostředků, který jste vytvořili pomocí [nový AzureRmStorageAccount](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccount) příkaz.

V následujícím příkazu nahraďte vlastní globálně jedinečný název pro účet úložiště Blob, kde uvidíte `<blob_storage_account>` zástupný symbol.

```powershell-interactive
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
  -Name "<blob_storage_account>" `
  -Location EastUS `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte konfiguraci virtuálního počítače. Tato konfigurace zahrnuje nastavení, která se používají při nasazení virtuálního počítače, jako je image virtuálního počítače, jeho velikost a konfigurace ověřování. Při spuštění tohoto kroku se zobrazí výzva k zadání přihlašovacích údajů. Hodnoty, které zadáte, se nakonfigurují jako uživatelské jméno a heslo pro virtuální počítač.

Vytvořte virtuální počítač pomocí příkazu [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "eastus"
$vmName = "myVM"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create a virtual network card and associate with public IP address
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS14_v2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

Write-host "Your public IP address is $($pip.IpAddress)"
```

## <a name="deploy-configuration"></a>Konfigurace nasazení

V tomto kurzu jsou předpoklady, které je třeba nainstalovat na virtuální počítač. Rozšíření vlastních skriptů se používá ke spuštění skriptu prostředí PowerShell, který dokončí následující úkoly:

> [!div class="checklist"]
> * Nainstalujte základní rozhraní .NET 2.0
> * Instalace chocolatey
> * Nainstalovat GIT
> * Naklonujte úložiště ukázka
> * Obnovení balíčků NuGet
> * Vytvoří 50 1 GB soubory s náhodná data

Spusťte následující rutinu pro dokončení konfigurace virtuálního počítače. Tento krok bude dokončeno 5 až 15 minut.

```azurepowershell-interactive
# Start a CustomScript extension to use a simple PowerShell script to instal .NET core, dependancies, and pre-create the files to upload.
Set-AzureRMVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location EastUS `
    -FileUri https://raw.githubusercontent.com/azure-samples/storage-dotnet-perf-scale-app/master/setup_env.ps1 `
    -Run 'setup_env.ps1' `
    -Name DemoScriptExtension
```

## <a name="next-steps"></a>Další kroky

V rámci jedna řada jste se dozvěděli o vytvoření účtu úložiště, nasazení virtuálního počítače a konfigurace virtuálního počítače s požadované požadavky například jak:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Vytvoření virtuálního počítače
> * Konfigurovat rozšíření vlastních skriptů

Přechodu na dva součástí řady nahrát velké objemy dat na účet úložiště pomocí exponenciální opakování a stupně paralelního zpracování.

> [!div class="nextstepaction"]
> [Nahrát velkých objemů velkých souborů paralelně na účet úložiště](storage-blob-scalable-app-upload-files.md)
