---
title: "Objekt JSON předat runbook služby Azure Automation | Microsoft Docs"
description: "Jak předat parametry sady runbook jako objekt JSON"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell, sady runbook, json, služby azure automation"
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: e9352ac1a346537d6214590be6dbc9db7ca0f461
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Předat objekt JSON do runbooku automatizace Azure

Může být užitečné k ukládání dat, který chcete předat do sady runbook v souboru JSON.
Například může vytvořit soubor JSON, který obsahuje všechny parametry, které chcete předat k sadě runbook.
K tomuto účelu, budete muset převést na řetězec ve formátu JSON a pak převést řetězec na objektu prostředí PowerShell před předáním její obsah do runbooku.

V tomto příkladu vytvoříme skript prostředí PowerShell, který volá [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) pro spuštění sady runbook PowerShell předávání obsah JSON do runbooku.
Powershellový runbook spustí virtuální počítač Azure, získávání parametrů pro virtuální počítač z formátu JSON, který byl předán v.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-sec-configure-azure-runas-account.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.  Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.
* Azure Powershell nainstalovaný v místním počítači. V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tom, jak získat prostředí Azure PowerShell.

## <a name="create-the-json-file"></a>Vytvořte soubor JSON

Zadejte následující testu do textového souboru a uložte ho jako `test.json` někde v místním počítači.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Vytvoření sady runbook

Vytvořte nový runbook prostředí PowerShell s názvem "Test-Json" ve službě Azure Automation.
Naučte se vytvořit novou sadu runbook Powershellu, najdete v tématu [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).

Přijmout JSON data, sada runbook vyžaduje objekt jako vstupní parametr.

Sady runbook pak můžete použít vlastnosti definované ve formátu JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Uložte a publikovat tuto sadu runbook v účtu Automation.

## <a name="call-the-runbook-from-powershell"></a>Volání sady runbook z prostředí PowerShell

Nyní můžete volat sadu runbook z místního počítače pomocí prostředí Azure PowerShell.
Spusťte následující příkazy prostředí PowerShell:

1. Přihlaste se k Azure:
   ```powershell
   Login-AzureRmAccount
   ```
    Zobrazí se výzva k zadání přihlašovacích údajů Azure.
1. Získat obsah souboru JSON a převeďte ho na řetězec:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`je cesta, kam jste uložili soubor JSON.
1. Převést řetězec obsah `$json` na objekt prostředí PowerShell:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Vytvořit tabulku hash pro parametry `Start-AzureRmAutomationRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Všimněte si, že nastavíte hodnotu `Parameters` objekt prostředí PowerShell, který obsahuje hodnoty ze souboru JSON. 
1. Spuštění runbooku
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Sada runbook používá hodnoty ze souboru JSON pro spuštění virtuálního počítače.

## <a name="next-steps"></a>Další kroky

* Další informace o úpravách prostředí PowerShell a pracovní postup prostředí PowerShell sad runbook s textový editor, najdete v části [úpravy textovou sady runbook ve službě Azure Automation](automation-edit-textual-runbook.md) 
* Další informace o vytváření a import sad runbook najdete v tématu [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)


