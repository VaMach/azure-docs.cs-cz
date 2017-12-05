---
title: "Rychlý start - vytvořit privátní registru Docker v Azure pomocí prostředí PowerShell"
description: "Rychle se Naučte se vytvořit privátní registru kontejner Docker pomocí prostředí PowerShell."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
ms.service: container-registry
ms.devlang: na
ms.topic: quicksart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d165cc159f7da2c8e7f0be4f0fa7d353997ed5f9
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Vytvoření registru kontejneru služby Azure pomocí prostředí PowerShell

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka údaje vytvoření instance registru kontejner Azure pomocí prostředí PowerShell.

Tento rychlý start vyžaduje prostředí Azure PowerShell verze modulu 3,6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Je také nutné mít Docker nainstalovány místně. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

Vytvořte instanci ACR pomocí [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry) příkaz.

Název registru **musí být jedinečné**. V následujícím příkladu *myContainerRegistry007* se používá. Aktualizujte na jedinečnou hodnotu.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Přihlaste se k ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. Nejprve pomocí [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) získat přihlašovací údaje správce pro instanci ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Pak pomocí [docker přihlášení](https://docs.docker.com/engine/reference/commandline/login/) příkaz k přihlášení do ACR instance.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Příkaz po dokončení vrátí zprávu Login Succeeded (Přihlášení proběhlo úspěšně).

## <a name="push-image-to-acr"></a>Nabízená ACR bitové kopie

Tak, aby nabízel bitovou kopii registru kontejneru Azure, musíte nejprve mít bitovou kopii. V případě potřeby spusťte následující příkaz načítat z úložiště Docker Hub předem vytvořené bitové kopie.

```bash
docker pull microsoft/aci-helloworld
```

Obrázek musí být označené pomocí názvu serveru ACR přihlášení. Spustit [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) příkaz vrátí název serveru přihlášení instance ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Značka pomocí bitové kopie [docker značky](https://docs.docker.com/engine/reference/commandline/tag/) příkaz. Nahraďte *acrLoginServer* s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Nakonec použijte [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) pro uložení bitové kopie do ACR instance. Nahraďte *acrLoginServer* s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz k odebrání skupiny prostředků, ACR instanci a všechny Image kontejneru.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start vytvořili kontejner registru Azure pomocí Azure CLI. Pokud chcete používat registru kontejner Azure s Azure kontejner instancí, nadále kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurz pro Azure instancí kontejnerů](../container-instances/container-instances-tutorial-prepare-app.md)