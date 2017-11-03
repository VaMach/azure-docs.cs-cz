---
title: "Zamknout prostředky Azure, aby se změny | Microsoft Docs"
description: "Zabráníte uživatelům v aktualizaci nebo odstranění důležité prostředky Azure s použitím zámek pro všechny uživatele a role."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Zamknutí prostředků, aby se zabránilo neočekávané změny 
Jako správce musíte k uzamčení předplatné, skupinu prostředků nebo prostředek zabránit ostatním uživatelům ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků. Můžete nastavit zámek na úrovni **CanNotDelete** nebo **jen pro čtení**. 

* **CanNotDelete** znamená Autorizovaní uživatelé stále mohou číst a upravovat prostředek, ale jejich nelze odstranit prostředek. 
* **Jen pro čtení** znamená Autorizovaní uživatelé mohou číst prostředek, ale jejich nelze odstranit nebo aktualizovat prostředek. Je podobná omezení všem oprávněným uživatelům oprávnění udělují použití této zámku **čtečky** role. 

## <a name="how-locks-are-applied"></a>Jak se používají zámky.

Když použijete zámku v nadřazeném oboru, zdědí všechny prostředky v rámci tohoto oboru stejné zámek. I prostředky, které přidáte později zámek dědí z nadřazeného objektu. Nejvíc omezující zámek v dědičnosti přednost.

Na rozdíl od řízení přístupu na základě rolí použít zámky správy pro aplikaci omezení ve všech uživatelů a rolí. Další informace o nastavení oprávnění pro uživatele a rolí najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).

Zámky správce prostředků se vztahují pouze na operace, které dojít v rovině řízení, sestávající operací posílá `https://management.azure.com`. Zámky neomezují jak prostředky provádět vlastní funkce. Změny prostředku jsou s omezeným přístupem, ale operace prostředků nejsou s omezeným přístupem. Například jen pro čtení zámku v databázi SQL zabráníte odstranění nebo úprava databázi, ale nezabrání můžete z vytvoření, aktualizace a odstraňování dat v databázi. Data transakce jsou povolené, protože tyto operace se neodesílají na `https://management.azure.com`.

Použití **jen pro čtení** může vést k neočekávaným výsledkům, protože některé operace, které vypadají podobně jako pro čtení, operace ve skutečnosti vyžadují další akce. Například umístění **jen pro čtení** výpis klíčů všem uživatelům zabrání zámku na účet úložiště. Seznam klíčů operaci je zpracováván prostřednictvím požadavek POST, protože vrácený klíče jsou k dispozici pro operace zápisu. Další příklad umístění **jen pro čtení** zámku na prostředek aplikace služby zabrání zobrazení souborů pro daný prostředek, protože interakce vyžaduje oprávnění k zápisu Průzkumníka serveru Visual Studia.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kdo může vytvářet nebo odstraňovat zámky ve vaší organizaci
Vytvořit nebo odstranit zámky správy, musíte mít přístup k `Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce. Z předdefinovaných rolí pouze **vlastníka** a **správce přístupu uživatelů** mají tyto akce.

## <a name="portal"></a>Portál
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Šablona
Následující příklad ukazuje šablonu, která vytvoří zámek na účet úložiště. Účet úložiště, na kterých chcete použít zámek je zadat jako parametr. Název zámku se vytvoří zřetězením názvu prostředku s **/Microsoft.Authorization/** a název zámku, v tomto případě **myLock**.

Zadaný typ je specifické pro daný typ prostředku. Pro úložiště nastavte typ, který má "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
Zámek můžete nasadit pomocí prostředků pomocí Azure PowerShell [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) příkaz.

K uzamčení prostředku, zadejte název prostředku, její typ prostředku a jeho název skupiny prostředků.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Zamknout skupinu prostředků, zadejte název skupiny prostředků.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

Chcete-li získat informace o zámek, použijte [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). Všechny zámky v rámci vašeho předplatného, použijte:

```powershell
Get-AzureRmResourceLock
```

Všechny zámky pro prostředek, použijte:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Všechny zámky pro skupinu prostředků, použijte:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Prostředí Azure PowerShell poskytuje jinými příkazy pro pracovní zámků, jako například [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) aktualizovat o zámku a [odebrat AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) odstranit zámek.

## <a name="azure-cli"></a>Azure CLI

Zámek můžete nasadit pomocí prostředků pomocí Azure CLI [az zámku vytvořit](/cli/azure/lock#create) příkaz.

K uzamčení prostředku, zadejte název prostředku, její typ prostředku a jeho název skupiny prostředků.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

Zamknout skupinu prostředků, zadejte název skupiny prostředků.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

Chcete-li získat informace o zámek, použijte [az zámku seznamu](/cli/azure/lock#list). Všechny zámky v rámci vašeho předplatného, použijte:

```azurecli
az lock list
```

Všechny zámky pro prostředek, použijte:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

Všechny zámky pro skupinu prostředků, použijte:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Rozhraní příkazového řádku Azure poskytuje jinými příkazy pro pracovní zámků, jako například [az zámek aktualizace](/cli/azure/lock#update) aktualizovat o zámku a [odstranit zámek az](/cli/azure/lock#delete) odstranit zámek.

## <a name="rest-api"></a>REST API
Zamknete nasazené prostředky s [REST API pro správu zámky](https://docs.microsoft.com/rest/api/resources/managementlocks). Rozhraní REST API umožňuje vytvářet a umožňuje odstranit zámky a načíst informace o existující zámky.

Chcete-li vytvořit zámek, spusťte:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Obor může být předplatné, skupinu prostředků nebo prostředek. Název zámku je všechno volání zámek. Verze rozhraní api, používat **2015-01-01**.

V žádosti o zahrnují objekt JSON, který určuje vlastnosti pro zámek.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Další kroky
* Další informace o práci s uzamčení prostředků najdete v tématu [zámku dolů vaše prostředky Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Další informace o logicky organizování vašich prostředků najdete v tématu [pomocí značek k uspořádání prostředků](resource-group-using-tags.md)
* Ke změně prostředku se nachází v prostředku skupiny, najdete v části [přesunutím prostředků do nové skupiny prostředků](resource-group-move-resources.md)
* Můžete použít omezení a pravidla týkající se vašeho předplatného pomocí vlastních zásad. Další informace najdete v tématu [Použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

