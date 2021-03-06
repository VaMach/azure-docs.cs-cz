---
title: "Použití PowerShellu k vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure | Dokumentace Microsoftu"
description: "Použijte PowerShell k vytvoření přiřazení zásady Azure Policy pro identifikaci prostředků, které nedodržují předpisy."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 1/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 67c779b96dab088d810d22ad3053ade106aec56a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure pomocí PowerShellu

Prvním krokem k porozumění dodržování předpisů v Azure je zjištění stavu vašich prostředků. Tento rychlý start vás provede procesem vytvoření přiřazení zásady pro identifikaci virtuálních počítačů, které nepoužívají spravované disky.

Na konci tohoto procesu úspěšně identifikujete virtuální počítače, které nepoužívají spravované disky. *Neodpovídají* přiřazení zásad.

PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje použití PowerShellu k vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure.

Tato příručka vyžaduje modul Azure PowerShell verze 4.0 nebo novější. Verzi zjistíte spuštěním příkazu  `Get-Module -ListAvailable AzureRM` . Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Než začnete, ujistěte se, že je nainstalovaná nejnovější verze PowerShellu. Podrobné informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.


## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tomto rychlém startu vytvoříte přiřazení zásady a přiřadíme definici *Audit virtuálních počítačů bez spravovaných disků*. Tato definice zásady identifikuje prostředky, které nesplňují podmínky nastavené v definici zásady.

Podle těchto pokynů vytvořte nové přiřazení zásady.

1. Aby se zajistila správná funkce vašeho předplatného s poskytovatelem prostředků, zaregistrujte poskytovatele prostředků Policy Insights. Když chcete registrovat poskytovatele prostředků, musíte mít oprávnění k provedení operace akce registrace pro poskytovatele prostředků. Tato operace je součástí rolí Přispěvatel a Vlastník.

    Spuštěním následujícího příkazu zaregistrujte poskytovatele prostředků:

    ```
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
```

    Registraci poskytovatele prostředků nejde zrušit, dokud máte ve vašem předplatném typy prostředků od tohoto poskytovatele prostředků.

    Další podrobnosti o registraci a zobrazení poskytovatelů prostředků najdete v tématu [Poskytovatelé a typy prostředků](../azure-resource-manager/resource-manager-supported-services.md).

2. Po registraci poskytovatele prostředků spuštěním následujícího příkazu zobrazte všechny definice zásad a vyhledejte definici zásady, kterou chcete přiřadit:

    ```powershell
$definition = Get-AzureRmPolicyDefinition
```

    Služba Azure Policy obsahuje již integrované definice zásad, které můžete použít. Zobrazí se například následující integrované definice zásad:

    - Vynucovat použití značky a její hodnoty
    - Použít značku a její hodnotu
    - Vyžadovat SQL Server verze 12.0

3. Dále přiřaďte definici zásady k požadovanému oboru pomocí rutiny `New-AzureRmPolicyAssignment`.

Pro účely tohoto kurzu použijte pro příkaz následující informace:

- **Name** – Zobrazovaný název přiřazení zásady. V tomto případě použijte Audit virtuálních počítačů bez spravovaných disků.
- **Policy** – Definice zásady založená na té, kterou používáte k vytvoření tohoto přiřazení. V tomto případě je to definice zásady *Audit virtuálních počítačů bez spravovaných disků*.
- **Scope** – Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat. Může sahat od předplatného až po skupinu prostředků. V tomto příkladu přiřazujete definici zásady ke skupině prostředků **FabrikamOMS**.
- **$definition** – Je potřeba zadat ID prostředku definice zásady. V tomto případě používáte ID pro definici zásady *Audit virtuálních počítačů bez spravovaných disků*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Nyní jste připraveni identifikovat prostředky, které nedodržují předpisy, abyste porozuměli stavu dodržování předpisů ve vašem prostředí.

## <a name="identify-non-compliant-resources"></a>Identifikace prostředků, které nedodržují předpisy

1. Přejděte zpět na úvodní stránku služby Azure Policy.
2. V levém podokně vyberte **Dodržování předpisů** a vyhledejte **Přiřazení zásady**, které jste vytvořili.

   ![Dodržování zásad](media/assign-policy-definition/policy-compliance.png)

   Pokud nějaké stávající prostředky nedodržují předpisy tohoto nového přiřazení, zobrazí se na kartě **Nekompatibilní prostředky**.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další příručky v této kolekci vycházejí z tohoto rychlého startu. Pokud chcete pokračovat v práci s dalšími kurzy, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, spuštěním následujícího příkazu odstraňte přiřazení, která jste vytvořili:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste přiřadili definici zásady pro identifikaci prostředků, které nedodržují předpisy, ve vašem prostředí Azure.

Další informace o přiřazování zásad pro zajištění, aby **budoucí** vytvořené prostředky dodržovaly předpisy, najdete v tomto kurzu:

> [!div class="nextstepaction"]
> [Vytváření a správa zásad](./create-manage-policy.md)
