---
title: "Rychlý start – Vytvoření privátního registru Dockeru v Azure pomocí PowerShellu"
description: "Rychle se naučíte, jak vytvořit privátní registr Dockeru pomocí PowerShellu."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 10/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c7d74395b1c8b386ce190906aa5b63b48c1bb1bf
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Vytvoření služby Azure Container Registry pomocí PowerShellu

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka podrobně popisuje vytvoření instance služby Azure Container Registry pomocí PowerShellu.

Tento rychlý start vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Je také nutné mít Docker nainstalovaný místně. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Přihlášení k Azure

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

Vytvořte instanci ACR pomocí příkazu [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry).

Název registru musí být jedinečný v rámci Azure a musí obsahovat 5 až 50 alfanumerických znaků. V následujícím příkladu se používá *myContainerRegistry007*. Aktualizujte název na jedinečnou hodnotu.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Přihlášení ke službě ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. Nejprve pomocí příkazu [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) získejte přihlašovací údaje správce pro instanci ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Potom se pomocí příkazu [docker login](https://docs.docker.com/engine/reference/commandline/login/) přihlaste k instanci ACR.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Příkaz po dokončení vrátí zprávu Login Succeeded (Přihlášení proběhlo úspěšně).

## <a name="push-image-to-acr"></a>Nasdílení image do služby ACR

Pokud chcete nasdílet image do služby Azure Container Registry, musíte nejprve mít nějakou image. V případě potřeby si přetáhněte předem vytvořenou image z Docker Hubu spuštěním následujícího příkazu.

```bash
docker pull microsoft/aci-helloworld
```

Image musí být označená pomocí názvu přihlašovacího serveru ACR. Spuštěním příkazu [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) získáte název přihlašovacího serveru instance ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Označte image pomocí příkazu [docker tag](https://docs.docker.com/engine/reference/commandline/tag/). Nahraďte *acrLoginServer* názvem přihlašovacího serveru vaší instance ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Nakonec pomocí příkazu [docker push](https://docs.docker.com/engine/reference/commandline/push/) nasdílejte image do instance ACR. Nahraďte *acrLoginServer* názvem přihlašovacího serveru vaší instance ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, instance ACR a všech imagí kontejneru použít příkaz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili službu Azure Container Registry pomocí Azure CLI. Pokud chcete používat službu Azure Container Registry se službou Azure Container Instances, přejděte na kurz služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurz služby Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)