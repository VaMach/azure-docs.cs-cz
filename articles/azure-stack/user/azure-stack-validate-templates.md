---
title: "Používat šablony ověření ke kontrole šablony Azure zásobníku | Microsoft Docs"
description: "Zkontrolujte šablony pro nasazení do Azure zásobníku"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6a77efb3ef4236048ff08b14346175b592493982
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Zkontrolujte vaše šablony pro Azure zásobník validátoru šablony

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Nástroj ověření šablony můžete zaškrtněte, pokud správce Azure Resource Manager [šablony](azure-stack-arm-templates.md) jsou připraveny pro Azure zásobníku. Nástroj ověření šablony je k dispozici jako součást nástroje Azure zásobníku. Stažení nástroje Azure zásobníku pomocí kroků popsaných v [stáhnout z webu GitHub nástroje](azure-stack-powershell-download.md) článku. 

Ověření šablony, můžete používat následující moduly prostředí PowerShell v **TemplateValidator** a **CloudCapabilities** složky: 

 - AzureRM.CloudCapabilities.psm1 vytvoří soubor JSON cloudu možnosti představují služby a verzí v cloudu, jako je Azure zásobníku.
 - AzureRM.TemplateValidator.psm1 používá soubor JSON možností cloudu k testování šablony pro nasazení v zásobníku Azure.
 
V tomto článku Vytvoření souboru schopnosti cloudu a poté spusťte nástroj validátoru.

## <a name="build-cloud-capabilities-file"></a>Vytvoření souboru možností cloudu
Než použijete validátor šablony, spusťte modul AzureRM.CloudCapabilities PowerShell k vytvoření souboru JSON. Pokud aktualizaci integrované systému, nebo přidejte nové služby nebo rozšíření virtuálního počítače je nutné spustit také Tenhle modul znovu.

1.  Ujistěte se, že máte připojení k Azure zásobníku. Tyto kroky lze provést z hostitele zásobník Azure development kit, nebo můžete použít [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) pro připojení z pracovní stanice. 
2.  Naimportujte modul Powershellu AzureRM.CloudCapabilities:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Použijte rutinu Get-CloudCapabilities načtení verze aktualizace service a vytvořte soubor JSON možnosti cloudu. Pokud neurčíte - OutputPath, soubor AzureCloudCapabilities.Json je vytvořen v aktuálním adresáři. Použijte aktuálního umístění:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```             

## <a name="validate-templates"></a>Ověření šablony
V následujícím postupu ověření šablony s použitím modulu AzureRM.TemplateValidator PowerShell. Můžete použít vlastní šablony, nebo ověřit [šablony Azure Quickstart zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates).

1.  Naimportujte modul Powershellu AzureRM.TemplateValidator.psm1:
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  Spusťte validátor šablony:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Ověření šablony nějakým chybám jsou protokolovány konzole prostředí PowerShell a soubor ve formátu HTML v zdrojový adresář. Tady je příklad sestavy ověření:

![ukázkové sestavy ověření](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametry

| Parametr | Popis | Požaduje se |
| ----- | -----| ----- |
| TemplatePath | Určuje cestu k rekurzivnímu najít šablony Azure Resource Manager | Ano | 
| TemplatePattern | Určuje název šablony souborů tak, aby odpovídaly. | Ne |
| CapabilitiesPath | Určuje cestu k souboru JSON možnosti cloudu | Ano | 
| IncludeComputeCapabilities | Zahrnuje vyhodnocení prostředky infrastruktury jako velikosti virtuálních počítačů a rozšíření virtuálního počítače | Ne |
| IncludeStorageCapabilities | Zahrnuje vyhodnocení prostředků úložiště, jako jsou typy SKU | Ne |
| Sestava | Určuje název vygenerovanou sestavu ve formátu HTML | Ne |
| Podrobné | Protokoly chyb a varování do konzoly nástroje | Ne|


### <a name="examples"></a>Příklady
Tento příklad ověří všechny [šablony Azure Quickstart zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates) stažen místně a také ověří velikosti virtuálních počítačů a rozšíření Azure zásobníku Development Kit možnostmi.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```


## <a name="next-steps"></a>Další postup
 - [Nasazení šablon do Azure zásobníku](azure-stack-arm-templates.md)
 - [Vývoj šablon pro Azure zásobníku](azure-stack-develop-templates.md)

