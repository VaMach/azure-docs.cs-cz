---
title: "Kurz sady dostupnosti pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o skupiny dostupnosti pro virtuální počítače Windows v Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2345b434a51b768793c2dea4587dc0a49ab35b70
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-use-availability-sets"></a>Jak používat skupiny dostupnosti

V tomto kurzu zjistěte, jak zvýšit dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti. Skupiny dostupnosti Ujistěte se, že virtuálních počítačů nasadit v Azure jsou rozmístěny v několika izolované hardware do uzlů v clusteru. To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, má vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstávají dostupné a funkční. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů
> * Zkontrolujte Azure Advisor

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Přehled sady dostupnosti

Skupiny dostupnosti je logické seskupení funkci, která můžete použít v Azure k zajištění, že prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure. Azure zajistí, že virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače. Pokud dojde k selhání Azure softwaru nebo hardwaru, má vliv pouze podmnožinu virtuální počítače a celkové aplikace zůstává až a nadále k dispozici pro vaše zákazníky. Skupiny dostupnosti jsou nezbytné funkce, pokud chcete vytvářet spolehlivé cloudové řešení.

Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze. S Azure, by chcete definovat dvě sady dostupnosti před nasazením virtuálních počítačů: jednu sadu dostupnosti pro webovou vrstvu a jednu sadu dostupnosti pro databázové vrstvy. Když vytvoříte nový virtuální počítač, pak můžete vytvořit skupinu dostupnosti jako parametr k virtuálnímu počítači az příkaz, a Azure automaticky zajistí, aby virtuální počítače, vytvořte v rámci sady k dispozici jsou izolovány napříč několika prostředcích fyzický hardware. Pokud fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že ostatní instance webového serveru a databáze virtuální počítače zůstat spuštěné, protože jsou na jiný hardware.

Použijte skupiny dostupnosti, pokud chcete nasadit spolehlivé řešení založená na virtuálních počítačů v Azure.

## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Můžete vytvořit pomocí sadu dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). V tomto příkladu jsme nastavit počet aktualizací a chyby domén v *2* pro skupinu dostupnosti s názvem *myAvailabilitySet* v *myResourceGroupAvailability* Skupina prostředků.

Vytvořte skupinu prostředků.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Vytvoření spravované dostupnosti nastavit pomocí [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) s **- sku zarovnán** parametr.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Vytvořit virtuální počítače uvnitř skupiny dostupnosti

Virtuální počítače musí být vytvořen v rámci skupinu dostupnosti a ujistěte se, že jsou správně rozmístěny v hardwaru. Nelze přidat existující virtuální počítač po vytvoření sadu dostupnosti. 

Hardware v umístění je rozděleno v několika aktualizace domén a domén selhání. **Aktualizace domény** je skupina virtuální počítače a základní fyzický hardware, který může být restartován ve stejnou dobu. Virtuální počítače ve stejné **doména selhání** sdílejí společné úložiště, jakož i běžné zdroje a sítě vypínač. 

Při vytváření konfigurace virtuálního počítače pomocí [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) použijete `-AvailabilitySetId` parametru určete ID skupiny dostupnosti.

Vytvořte dva virtuální počítače s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ve dostupnosti nastavit.

```azurepowershell-interactive
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig
    
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup `
    -Location eastus `
    -Name myNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAvailability `
    -SecurityRules $nsgRuleRDP
    
# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name mySubnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -ComputerName myVM$i `
        -Credential $cred `
        -VM $vm `
        -Windows `
        -EnableAutoUpdate `
        -ProvisionVMAgent
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
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Trvá několik minut vytvořit a nakonfigurovat oba virtuální počítače. Po dokončení, budete mít dva virtuální počítače distribuované ve základní hardware. 

Pokud se podíváte na skupinu dostupnosti na portálu tak, že přejdete na skupiny prostředků > myResourceGroupAvailability > myAvailabilitySet, měli byste najdete v části Jak virtuální počítače distribuované ve 2 odolnost a aktualizaci domény.

![Na portálu pro sadu dostupnosti](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Zkontrolujte dostupné velikosti virtuálních počítačů 

Můžete přidat další virtuální počítače pro skupinu dostupnosti později, ale musíte vědět, jaké velikosti virtuálních počítačů jsou k dispozici na hardware. Použití [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) seznam všech dostupných velikostí na hardwaru clusteru pro skupinu dostupnosti.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="check-azure-advisor"></a>Zkontrolujte Azure Advisor 

Azure Advisor také můžete získat další informace o způsobů, jak zlepšit dostupnost virtuálních počítačů. Azure Advisor pomáhá dodržujte doporučené postupy, chcete-li optimalizovat nasazení Azure. Ho analyzuje konfigurace prostředků a telemetrii využití a pak doporučuje řešení, které vám pomůžou líp finanční efektivita, výkon, vysokou dostupnost a zabezpečení vašich prostředků Azure.

Přihlaste se k [portál Azure](https://portal.azure.com), vyberte **další služby**a typ **Advisor**. Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné. Další informace najdete v tématu [Začínáme s Azure Advisor](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů
> * Zkontrolujte Azure Advisor

Přechodu na v dalším kurzu se dozvíte o sady škálování virtuálního počítače.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)


