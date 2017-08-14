---
title: "Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit server služby Azure Analysis Services pomocí PowerShellu."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 43700de27ca643bd699159b86fe80c2e45e4acc7
ms.contentlocale: cs-cz
ms.lasthandoff: 08/04/2017

---

# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu

Tento rychlý start popisuje použití PowerShellu z příkazového řádku k vytvoření serveru Azure Analysis Services ve [skupině prostředků Azure](../azure-resource-manager/resource-group-overview.md) ve vašem předplatném Azure.

Tato úloha vyžaduje modul Azure PowerShell verze 4.0 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud chcete provést instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba. Další informace najdete v tématu [Ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto rychlého startu je potřeba:

* **Předplatné Azure:** Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatný zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
* **Azure Active Directory:** Vaše předplatné musí být přidružené k tenantovi Azure Active Directory a musíte mít účet v tomto adresáři. Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Import modulu AzureRm.AnalysisServices
K vytvoření serveru ve vašem předplatném můžete použít modul komponenty [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Načtěte modul AzureRm.AnalysisServices do relace PowerShellu.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se ke svému předplatnému Azure pomocí příkazu [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount). Postupujte podle pokynů na obrazovce.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
 
[Skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) je logický kontejner, ve kterém se nasazují a spravují prostředky Azure jako skupina. Při vytváření serveru je potřeba zadat skupinu prostředků ve vašem předplatném. Pokud skupinu prostředků ještě nemáte, můžete vytvořit novou pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků `myResourceGroup` v oblasti USA – západ.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Vytvoření serveru

Vytvořte nový server pomocí příkazu [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Následující příklad vytvoří server myServer ve skupině prostředků myResourceGroup v oblasti USA – západ na úrovni D1 a určí philipc@adventureworks.com jako správce serveru.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Server můžete z předplatného odebrat pomocí příkazu [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Pokud budete pokračovat dalšími rychlými starty a kurzy v této kolekci, server neodebírejte. Následující příklad odebere server vytvořený v předchozím kroku.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Další kroky
[Správa služby Azure Analysis Services pomocí PowerShellu](analysis-services-powershell.md)   
[Nasazení modelu ze sady SSDT](analysis-services-deploy.md)   
[Vytvoření modelu na webu Azure Portal](analysis-services-create-model-portal.md)
