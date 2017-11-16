---
title: "Rychlý start - vytvoření vaší první kontejner instancí kontejnerů Azure pomocí prostředí PowerShell"
description: "Začínáme s Azure kontejner instancí vytvoření instance kontejneru systému Windows v prostředí PowerShell."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: ca10274fc6a23d7f5e7436dbaf72a6e7a918f275
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances

Azure instancí kontejnerů umožňuje snadno vytvářet a spravovat Docker kontejnerů v Azure, aniž by museli zřizovat virtuální počítače nebo přijmou vyšší úrovně služby.

V tento rychlý start vytvoření kontejneru systému Windows v Azure a umístěte ji do internet s veřejnou IP adresu. K dokončení této operace stačí jediný příkaz. V několika situacích můžete zjistit běžící aplikaci prohlížeče:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][qs-powershell-01]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure.

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření skupiny prostředků Azure s [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, bitovou kopii Docker a skupinu prostředků Azure k [New-AzureRmContainerGroup] [ New-AzureRmContainerGroup] rutiny. Volitelně můžete kontejner zveřejnit na internetu s použitím veřejné IP adresy. V tomto případě použijeme kontejner Windows Nano Server spuštění Internetové informační služby (IIS).

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Během pár sekund získáte odpovědi na požadavek. Standardně je kontejner v **vytváření** stavu, ale by se měl spustit v rámci minutu nebo dvě. Můžete zkontrolovat stav pomocí [Get-AzureRmContainerGroup] [ Get-AzureRmContainerGroup] rutiny:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Stav zřizování kontejneru a IP adresa se zobrazí ve výstupu rutiny:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Jednou kontejneru **ProvisioningState** přesune do `Succeeded`, dosáhnout v prohlížeči pomocí je zadaná adresa IP.

![Nasazení pomocí Azure kontejner instancí služby IIS zobrazit v prohlížeči][qs-powershell-01]

## <a name="delete-the-container"></a>Odstranění kontejneru

Až skončíte s kontejneru, můžete odebrat pomocí [odebrat AzureRmContainerGroup] [ Remove-AzureRmContainerGroup] rutiny:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Další kroky

V této rychlé spuštění spuštění předdefinovaných kontejneru systému Windows v Azure kontejner instancí. Pokud jste chtěli zkuste jej sestavit kontejner sami a jeho nasazení do Azure kontejner instancí pomocí klíče registru kontejner Azure, pokračovat v kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurz pro Azure instancí kontejnerů](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png