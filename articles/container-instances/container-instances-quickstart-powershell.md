---
title: "Rychlý start – Vytvoření prvního kontejneru služby Azure Container Instances pomocí PowerShellu"
description: "Začněte se službou Azure Container Instances vytvořením instance kontejneru Windows pomocí PowerShellu."
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
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances

Služba Azure Container Instances usnadňuje vytváření a správu kontejnerů Dockeru v Azure, aniž byste museli zřizovat virtuální počítače nebo používat službu vyšší úrovně.

V tomto rychlém startu vytvoříte kontejner Windows v Azure a zveřejníte ho na internetu s použitím veřejné IP adresy. K dokončení této operace stačí jediný příkaz. Za chvíli se ve vašem prohlížeči zobrazí spuštěná aplikace:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][qs-powershell-01]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure.

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure do rutiny [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Volitelně můžete kontejner zveřejnit na internetu s použitím veřejné IP adresy. V tomto případě použijeme kontejner Windows Nano Server se spuštěnou Internetovou informační službou (IIS).

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Během několika sekund obdržíte odpověď na váš požadavek. Zpočátku je kontejner ve stavu **Vytváření**, ale během několika minut by se měl spustit. Stav můžete zkontrolovat pomocí rutiny [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Ve výstupu rutiny se zobrazí stav zřizování kontejneru a jeho IP adresa:

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

Jakmile se **stav zřizování** kontejneru změní na `Succeeded`, můžete k němu přejít v prohlížeči pomocí uvedené IP adresy.

![Služba IIS nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][qs-powershell-01]

## <a name="delete-the-container"></a>Odstranění kontejneru

Až s kontejnerem skončíte, můžete ho odebrat pomocí rutiny [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste spustili předem připravený kontejner Windows ve službě Azure Container Instances. Pokud byste si chtěli sami vyzkoušet sestavení kontejneru a jeho nasazení do služby Azure Container Instances pomocí služby Azure Container Registry, pokračujte na kurz služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurz služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png