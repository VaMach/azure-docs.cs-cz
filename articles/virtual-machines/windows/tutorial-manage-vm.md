---
title: "Vytvoření a správa virtuálních počítačů Windows s modulu Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvořit a spravovat virtuální počítače Windows s modulu Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2237f2e5cb67df019d0975e764602babe7f4c8f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Vytvoření a správa virtuálních počítačů Windows s modulu Azure PowerShell

Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí. Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit a připojit k virtuálnímu počítači
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače


[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure. 

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v *EastUS* oblast. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

Skupina prostředků je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Virtuální počítač musí být připojen k virtuální síti. Byste komunikovat s virtuálním počítačem pomocí veřejnou IP adresu prostřednictvím karty síťového rozhraní.

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Vytvoření veřejné IP adresy

Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Vytvoření karty síťového rozhraní

Vytvoření síťová karta s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Vytvořit skupinu zabezpečení sítě

Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů. Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů. Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem. Pro přístup k webovém serveru IIS, který se instaluje, je nutné přidat příchozího pravidla NSG.

Vytvoření příchozího pravidla NSG, použijte [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Následující příklad vytvoří pravidlo NSG s názvem *myNSGRule* , otevře port *3389* pro virtuální počítač:

```azurepowershell-interactive
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Vytvořte pomocí NSG *myNSGRule* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Přidat NSG k podsíti v dané virtuální síti s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Aktualizace virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```azurepowershell-interactive
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností. V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* s nejnovější verzí systému Windows Server 2016 Datacenter.

Nastavte uživatelské jméno a heslo, které potřebuje pro účet správce na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Vytvoření počáteční konfigurace pro virtuální počítač s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Přidat informace operačního systému do konfigurace virtuálního počítače s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```azurepowershell-interactive
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Přidat informace o obrázku v konfiguraci virtuálního počítače s [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Přidejte nastavení disku operačního systému do konfigurace virtuálního počítače s [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Přidat síťový adaptér, který jste předtím vytvořili pro konfiguraci virtuálního počítače s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```azurepowershell-interactive
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Vytvořte virtuální počítač pomocí příkazu [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a>Připojit k virtuálnímu počítači

Po dokončení nasazení vytvořte připojení ke vzdálené ploše virtuálního počítače.

Spuštění následujících příkazů vrátí veřejnou IP adresu virtuálního počítače. Poznamenejte si tuto IP adresu, abyste se k ní v dalším kroku mohli pomocí prohlížeče připojit a otestovat připojení k webu.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Pomocí následujícího příkazu na svém místním počítači vytvořte s virtuálním počítačem relaci vzdálené plochy. IP adresu nahraďte veřejnou IP adresou (*publicIPAddress*) vašeho virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje, které jste použili při vytváření virtuálního počítače.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Pochopení Image virtuálních počítačů

Azure marketplace obsahuje mnoho bitové kopie virtuálních počítačů, které lze použít k vytvoření nového virtuálního počítače. V předchozích krocích byl vytvořen virtuální počítač pomocí bitové kopie systému Windows Server 2016-Datacenter. V tomto kroku modulu PowerShell se používá pro vyhledávání na webu marketplace pro jiné Image systému Windows, které může taky jako základ pro nové virtuální počítače. Tento proces se skládá z hledání vydavatele, nabídky a název bitové kopie (Sku). 

Použití [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) příkaz vrátí seznam vydavatelů bitové kopie.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Použití [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) vrácení seznamu nabízí bitové kopie. Pomocí tohoto příkazu vrácený seznam je filtrován na zadaný vydavatel. 

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

[Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) příkazu vyfiltruje klikněte na název vydavatele a nabídky k zobrazení seznamu názvů bitové kopie.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Tyto informace slouží k nasazení virtuálních počítačů s konkrétní image. Tento příklad nastaví název bitové kopie na objekt virtuálního počítače. Naleznete v předchozích příkladech v tomto kurzu postup dokončení nasazení.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Pochopení velikosti virtuálních počítačů

Velikost virtuálního počítače určuje množství výpočetní prostředky, jako je například procesoru, grafického procesoru a paměti, které jsou k dispozici pro virtuální počítač. Virtuální počítače je potřeba vytvořit s velikostí odpovídající expect pracovní zátěže. Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.

### <a name="vm-sizes"></a>Velikosti virtuálních počítačů

V následující tabulce velikostí rozděluje do případy použití.  

| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Obecné účely         |DSv2, Dv2, DS, D, Av2, A0 7| Vyrovnáváním procesoru do paměti. Ideální pro vývoj / testování a malé a střední řešení aplikacím a datům.  |
| Optimalizované z hlediska výpočetních služeb      | Služby FS, F             | Vysoké využití procesoru do paměti. Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.        |
| Optimalizované z hlediska paměti       | GS, G, DSv2, DS, Dv2, D   | Vysoká paměti na core. Výborně hodí pro relačních databází, středních a velkých mezipaměti a analýzy v paměti.                 |
| Optimalizované z hlediska úložiště       | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| GPU           | VS, NC            | Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.       |
| Vysoký výkon | H, A8 11          | Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA). 


### <a name="find-available-vm-sizes"></a>Najít dostupných velikostí virtuálních počítačů

Pokud chcete zobrazit seznam velikostí virtuálních počítačů v určité oblasti, použijte [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz.

```azurepowershell-interactive
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Změna velikosti virtuálního počítače

Po nasazení virtuálního počítače, můžete změnit velikost můžete zvýšit nebo snížit přidělení prostředků.

Před změnou velikosti virtuálního počítače, zkontrolujte, jestli požadovaná velikost je dostupná na aktuální cluster virtuálních počítačů. [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz vrátí seznam velikostí. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Pokud požadovaná velikost je k dispozici, virtuální počítač velikost lze změnit ze stavu na zapnuté, ale po restartu během operace.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Pokud požadovaná velikost není v aktuální clusteru, musí být navrácena, než dojde k operace změny velikosti virtuálního počítače. Všimněte si, když virtuální počítač je zapnutý zpět, se odeberou všechna data na dočasné disku a veřejné IP adresa změnu, pokud se používá statickou IP adresu. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Stavy spotřeby virtuálních počítačů

Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie. Tento stav představuje aktuální stav virtuálního počítače z hlediska hypervisoru. 

### <a name="power-states"></a>Stavy spotřeby.

| Stav napájení | Popis
|----|----|
| Spouštění | Označuje, že virtuální počítač se spouští. |
| Běžící (Spuštěno) | Určuje, zda je virtuální počítač spuštěn. |
| Zastavování | Označuje, že je zastavení virtuálního počítače. | 
| Zastaveno | Označuje, že virtuální počítač je zastavená. Všimněte si, že virtuální počítače v zastaveném stavu stále platit poplatky výpočty.  |
| Rušení přidělování | Označuje, že virtuální počítač je navrácena. |
| Zrušeno | Označuje, že virtuální počítač je úplně odebrána z hypervisor, ale stále k dispozici v rovině řízení. Virtuální počítače ve stavu Deallocated nevznikají poplatky za výpočty. |
| - | Označuje, že stav napájení virtuálního počítače neznámý. |

### <a name="find-power-state"></a>Najít stav napájení

Chcete-li načíst stav konkrétní virtuální počítač, použijte [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz. Je nutné zadat platný název pro virtuální počítač a skupinu prostředků. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName myResourceGroupVM `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Výstup:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Úlohy správy

Během životního cyklu virtuálního počítače můžete spustit úlohy správy, jako je například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete vytvořit skripty pro automatizaci úloh opakovaných nebo komplexní. Pomocí Azure PowerShell, mnoho běžné úlohy správy lze spustit z příkazového řádku nebo ve skriptech.

### <a name="stop-virtual-machine"></a>Zastavit virtuální počítač

Zastavení a zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Pokud chcete zachovat virtuální počítač ve stavu, zřízený, použijte parametr - StayProvisioned.

### <a name="start-virtual-machine"></a>Spustit virtuální počítač

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Odstranění skupiny prostředků

Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:

> [!div class="checklist"]
> * Vytvořit a připojit k virtuálnímu počítači
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače

Přechodu na v dalším kurzu se dozvíte o disky virtuálních počítačů.  

> [!div class="nextstepaction"]
> [Vytvoření a správa virtuálních počítačů disky](./tutorial-manage-data-disk.md)
