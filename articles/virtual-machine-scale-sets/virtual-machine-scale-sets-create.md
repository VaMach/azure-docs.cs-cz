---
title: "Vytvoření sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Vytvoření a nasazení škálování virtuálních počítačů Linux nebo Windows Azure nastavit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, šablonu nebo Visual Studio."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 32af01aa545c541688128a7ae6bbb82a0e046f2d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Vytvoření a nasazení škálovací sadu virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další informace o sadách škálování najdete v tématu [sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).

V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů **bez** pomocí portálu Azure. Informace o tom, jak pomocí portálu Azure najdete v tématu [nastavit postup vytvoření škálování virtuálních počítačů pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Další informace o prostředky Azure Resource Manager najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Pokud používáte Azure CLI 2.0 nebo Azure PowerShell k vytvoření sady škálování, musíte nejdřív přihlásit k vašemu předplatnému.

Další informace o tom, jak nainstalovat, nastavit a přihlaste se k Azure pomocí Azure CLI nebo prostředí PowerShell najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) nebo [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Nejprve musíte vytvořit skupinu prostředků, který je přidružený škálovací sadu virtuálních počítačů.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Vytvoření z rozhraní příkazového řádku Azure

Pomocí rozhraní příkazového řádku Azure můžete vytvořit škálování virtuálního počítače s minimálním úsilím. Pokud nezadáte výchozí hodnoty, poskytnou jsou pro vás. Například pokud nezadáte žádné informace o virtuální síti, se pro vás vytvoří virtuální síť. Pokud vynecháte následujících částí, jsou vytvořeny pro můžete: 
- Nástroj pro vyrovnávání zatížení
- Virtuální síť
- Veřejnou IP adresu

Při výběru image virtuálního počítače, který chcete použít na škálovací sadu virtuálních počítačů, máte několik možností:

- NÁZEV URN  
Identifikátor prostředku:  
**Win2012R2Datacenter**

- Název URN alias  
Popisný název název URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Vlastní prostředek id  
Cesta k prostředek Azure:  
**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/Images/MyImage**

- Webové prostředky  
Cesta k identifikátor URI HTTP:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**

>[!TIP]
>Můžete získat seznam dostupných imagí s `az vm image list`.

Pokud chcete vytvořit škálovací sadu virtuálních počítačů, je nutné zadat následující:

- Skupina prostředků 
- Name (Název)
- Bitové kopie operačního systému
- Informace o ověřování 
 
Následující příklad vytvoří sada škálování základní virtuální počítač (Tento krok může trvat několik minut).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Po dokončení příkazu budete mít teď vaše sad vytvořený škálování virtuálního počítače. Musíte získat IP adresu virtuálního počítače tak, aby k nim mohla připojit. Pomocí následujícího příkazu můžete získat spoustu různé informace o virtuálním počítači (včetně IP adresa). 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Vytvoření z prostředí PowerShell

PowerShell je složitější než rozhraní příkazového řádku Azure. Zatímco rozhraní příkazového řádku Azure poskytuje výchozí hodnoty pro souvisejícím se sítí prostředky (například nástroje pro vyrovnávání zatížení, IP adresy a virtuální sítě), prostředí PowerShell nemá. Odkazování na bitovou kopii pomocí prostředí PowerShell je něco víc složitá příliš. Můžete získat obrázků pomocí následující rutiny:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Rutiny pracovní lze předat v pořadí. Tady je příklad toho, jak získat všechny bitové kopie pro **západní USA 2** oblasti s vydavatele, který má název **microsoft** v ní.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

Pracovní postup pro vytváření škálovací sadu virtuálních počítačů je následující:

1. Vytvořte objekt konfigurace, který obsahuje informace o sadě škálování.
2. Odkazovat na základní bitovou kopii operačního systému.
3. Nakonfigurujte nastavení operační systém: ověřování, předpona názvu virtuálního počítače a uživatele nebo heslo.
4. Konfigurace sítí.
5. Vytvoření sady škálování.

Tento příklad vytvoří základní dvě instance škále nastavení v počítači, který má nainstalovaný Windows Server 2016.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a>Pomocí bitové kopie vlastního virtuálního počítače
Pokud vytváříte nastavit od svoji vlastní image, místo odkazující na bitovou kopii virtuálního počítače z galerie, škálování _Set-AzureRmVmssStorageProfile_ příkaz bude vypadat takto:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Vytvoření ze šablony

Můžete nasadit škálování virtuálních počítačů, nastavit pomocí šablony Azure Resource Manager. Můžete vytvořit vlastní šablonu nebo použijte jednu z [šablony úložiště](https://azure.microsoft.com/resources/templates/?term=vmss). Tyto šablony se dá nasadit přímo do vašeho předplatného Azure.

>[!NOTE]
>Pokud chcete vytvořit vlastní šablonu, vytvořte textový soubor JSON. Obecné informace o tom, jak vytvořit a přizpůsobení šablony najdete v tématu [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Vzorové šablony je k dispozici [na Githubu](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Další informace o tom, jak vytvořit a použít tuto ukázku najdete v tématu [minimální přijatelná škálovací sadu](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Vytvořit ze sady Visual Studio

Pomocí sady Visual Studio můžete vytvořit projekt skupiny prostředků Azure a přidat že škálovací sady virtuálních počítačů šablony do ní. Můžete zvolit, jestli chcete importovat z webu GitHub nebo Galerie webových aplikací Azure. Nasazení skript prostředí PowerShell je vytvořena také za vás. Další informace najdete v tématu [jak vytvořit škálování virtuálních počítačů sady pomocí sady Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Vytvoření z portálu Azure

Portál Azure poskytuje pohodlný způsob, jak rychle vytvořit škálovací sadu. Další informace najdete v tématu [nastavit postup vytvoření škálování virtuálních počítačů pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Další kroky

Další informace o [datových disků](virtual-machine-scale-sets-attached-disks.md).

Zjistěte, jak [správě aplikací](virtual-machine-scale-sets-deploy-app.md).
