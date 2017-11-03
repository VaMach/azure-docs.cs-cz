---
title: "Spravovat řešení Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Ke správě prostředků pomocí Azure PowerShell a správce prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: ae5ccb83a0088cb7c9668f18620b74f9f3f1e9b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Správa prostředků pomocí Azure PowerShell a správce prostředků

V tomto článku zjistěte, jak spravovat vaše řešení pomocí Azure PowerShell a Azure Resource Manager. Pokud nejste obeznámeni s Resource Managerem, přečtěte si téma [Přehled služby Správce prostředků](resource-group-overview.md). Tento článek se zaměřuje na úlohy správy. Vaším úkolem je:

1. Vytvoření skupiny prostředků
2. Přidání prostředku do skupiny prostředků
3. Přidání značky k prostředku
4. Zadat dotaz na prostředky na základě názvy nebo hodnoty značky
5. Použít a odebere se zámek na prostředek
6. Odstranit skupinu prostředků.

Tento článek nezobrazuje postup nasazení šablony Resource Manageru do vašeho předplatného. Podrobnosti naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Začínáme s Azure PowerShell

Pokud jste prostředí Azure PowerShell nenainstalovali, přečtěte si téma [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Pokud máte nainstalovaný Azure PowerShell v minulosti, ale nebyly aktualizovány nedávno, zvažte instalaci nejnovější verze. Verze můžete aktualizovat pomocí stejného postupu, které jste použili k jeho instalaci. Například pokud jste použili služby instalace webové platformy, spusťte znovu a vyhledejte aktualizaci.

Pokud chcete zkontrolovat vaší verzi modulu prostředky Azure, použijte následující rutinu:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Tento článek byl aktualizován pro verzi 3.3.0. Pokud máte starší verzi, nemusí odpovídat prostředí kroky uvedené v tomto článku. Dokumentaci o rutinách v této verzi najdete v tématu [AzureRM.Resources modulu](/powershell/module/azurerm.resources).

## <a name="log-in-to-your-azure-account"></a>Přihlaste se k účtu Azure
Před zahájením práce na řešení, musíte být přihlášení ke svému účtu.

Přihlásit se k účtu Azure, použijte **Login-AzureRmAccount** rutiny.

```powershell
Login-AzureRmAccount
```

Tato rutina vás vyzve k zadání přihlašovacích údajů k vašemu účtu Azure. Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v Azure PowerShellu.

Rutina vrátí informace o váš účet a předplatné pro použití pro úlohy.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Pokud máte více než jedno předplatné, můžete přepnout do jiného předplatného. První Podíváme se, Všechna předplatná pro váš účet.

```powershell
Get-AzureRmSubscription
```

Vrátí povolených a zakázaných odběrů.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

Chcete-li přepnout do jiného předplatného, zadejte název odběru s **Set-AzureRmContext** rutiny.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Před nasazením žádné prostředky k vašemu předplatnému, musíte vytvořit skupinu prostředků, která bude obsahovat prostředky.

Chcete-li vytvořit skupinu prostředků, použijte **New-AzureRmResourceGroup** rutiny. Příkaz používá **název** parametr zadejte název pro skupinu prostředků a **umístění** parametr k určení jeho umístění.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

Výstup je v následujícím formátu:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Pokud budete potřebovat později načíst skupinu prostředků, použijte následující rutinu:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

Chcete-li získat všechny skupiny prostředků v rámci vašeho předplatného, nezadávejte název:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a>Přidat prostředky do skupiny prostředků

Chcete-li přidat prostředek do skupiny prostředků, můžete použít **New-AzureRmResource** rutina nebo rutiny, které jsou specifické pro typ prostředku, kterou vytváříte (jako **AzureRmStorageAccount nový**). Vám může být snadněji použít rutinu, která je specifická pro typ prostředku, protože obsahuje parametry pro vlastnosti, které jsou potřebné pro nový prostředek. Použít **New-AzureRmResource**, musíte znát všechny vlastnosti, které chcete nastavit bez výzvy pro ně.

Přidání prostředku pomocí rutin však může být matoucí budoucí protože nový prostředek neexistuje v šabloně Resource Manager. Společnost Microsoft doporučuje definování infrastrukturu pro vaše řešení Azure v šabloně Resource Manager. Šablony umožňují spolehlivě a opakovaného nasazování svého řešení. Pro tohoto článku vytvořit účet úložiště pomocí rutiny prostředí PowerShell, ale později generování šablonu z vaší skupiny prostředků.

Následující rutina vytvoří účet úložiště. Místo použití názvu v příkladu, zadejte jedinečný název pro účet úložiště. Název musí být v rozmezí 3 až 24 znaků a použít pouze čísla a malá písmena. Pokud použijete název v příkladu, zobrazí chybu, protože tento název je již používán.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Pokud budete potřebovat později načíst tento prostředek, použijte následující rutinu:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Přidání značky

Značky umožňují uspořádání prostředků podle jiné vlastnosti. Například může mít několik prostředků v různých prostředků skupiny, které patří do stejného oddělení. Můžete použít oddělení značky a hodnoty pro tyto prostředky označit je jako náležící do stejné kategorii. Nebo můžete označit, zda je prostředek použít v produkčním i testovacím prostředí. V tomto článku pouze jeden prostředek použijete značky, ale ve vašem prostředí nejpravděpodobnější má smysl použití značek ke všem prostředkům.

Následující rutiny platí dvě značky pro váš účet úložiště:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Značky jsou aktualizovány jako jednoho objektu. Postup přidání značky k prostředku, který již obsahuje značky, nejdřív načtěte stávající značky. Přidejte novou značku k objektu, který obsahuje stávající značky a znovu použít všechny značky k prostředku.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Hledat prostředky

Použití **najít AzureRmResource** rutiny k načtení prostředků pro různé vyhledávací podmínky.

* Chcete-li získat prostředek podle názvu, zadejte **ResourceNameContains** parametr:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* Chcete-li získat všechny prostředky ve skupině prostředků, zadejte **ResourceGroupNameContains** parametr:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* Chcete-li získat všechny prostředky se název značky a hodnotou, zadat **TagName** a **TagValue** parametry:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* Na všechny prostředky s konkrétní typ prostředku, zadejte **ResourceType** parametr:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="get-resource-id"></a>Získání ID prostředku

Mnoho příkazů trvat ID prostředku jako parametr. Pokud chcete načíst ID prostředků a úložiště do proměnné, použijte:

```powershell
$webappID = (Get-AzureRmResource -ResourceGroupName exampleGroup -ResourceName exampleSite).ResourceId
```

## <a name="lock-a-resource"></a>Zamknout prostředku

Pokud potřebujete zajistit kritické prostředků je náhodně odstraněna nebo upravena, použije zámek k prostředku. Můžete buď zadat **CanNotDelete** nebo **jen pro čtení**.

Vytvořit nebo odstranit zámky správy, musíte mít přístup k `Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce. Z předdefinovaných rolí pouze vlastník a správce přístupu uživatelů mají tyto akce.

Použít zámek, použijte následující rutinu:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Uzamčení prostředků v předchozím příkladu nelze odstranit, dokud se neodstraní zámek. Chcete-li odebrat zámek, použijte:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Další informace o nastavení zámky najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Odebrat prostředky nebo skupinu prostředků
Můžete odebrat prostředek nebo skupina prostředků. Když odeberete skupinu prostředků, je také odstranit všechny prostředky v příslušné skupině prostředků.

* Pokud chcete odstranit prostředek ze skupiny prostředků, použijte **odebrat AzureRmResource** rutiny. Tato rutina Odstraní prostředek, ale nedojde k odstranění skupiny prostředků.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* Chcete-li odstranit skupinu prostředků a všechny její prostředky, použijte **Remove-AzureRmResourceGroup** rutiny.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Obě rutin, zobrazí se výzva k potvrzení, že chcete odebrat prostředek nebo skupina prostředků. Pokud se operace úspěšně Odstraní prostředek nebo skupina prostředků, vrátí **True**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Spusťte skripty správce prostředků se Azure Automation.

Tento článek ukazuje, jak provádět základní operace na vašich prostředků Azure PowerShell. Pro pokročilejší scénáře správy obvykle chcete vytvořit skript a znovu použít tento skript podle potřeby nebo podle plánu. [Služby Azure Automation](../automation/automation-intro.md) poskytuje způsob, jak často automatizaci použít skripty, které spravují řešení Azure.

Následující témata ukazují, jak používat Azure Automation, Resource Manageru a prostředí PowerShell k efektivní provádění úloh správy:

- Informace o vytvoření sady runbook najdete v tématu [Můj první Powershellový runbook](../automation/automation-first-runbook-textual-powershell.md).
- Informace o práci s galerií skriptů, najdete v tématu [Galerie Runbooků a modulů pro Azure Automation](../automation/automation-runbook-gallery.md).
- Sady runbook, které spuštění a zastavení virtuálních počítačů, najdete v části [scénáře Azure Automation: formátu JSON pomocí značky se mají vytvořit plán pro virtuální počítač Azure spuštění a vypnutí](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Sady runbook, které spuštění a zastavení počítačem nepracujete virtuální počítače, naleznete v části [spuštění a zastavení virtuálních počítačů během řešení počítačem nepracujete v automatizaci](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Další kroky
* Další informace o vytváření šablon Resource Manageru, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Další informace o nasazení šablony najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).
* Existující prostředky můžete přesunout do nové skupiny prostředků. Příklady najdete v tématu [přesunutí prostředků do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

