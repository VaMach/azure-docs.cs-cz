---
title: "Použití PowerShellu k vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure | Dokumentace Microsoftu"
description: "Použijte PowerShell k vytvoření přiřazení zásady Azure Policy pro identifikaci prostředků, které nedodržují předpisy."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 5c00d50817e40de0a43d05eb85662b494247d8fa
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure pomocí PowerShellu

Prvním krokem k porozumění dodržování předpisů v Azure je vědět, jak jste na tom s vašimi stávajícími prostředky. Tento rychlý start vás provede procesem vytvoření přiřazení zásady pro identifikaci virtuálních počítačů, které nepoužívají spravované disky.

Na konci tohoto procesu úspěšně identifikujete virtuální počítače, které nepoužívají spravované disky, a proto *nedodržují předpisy*.


PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje použití PowerShellu k vytvoření přiřazení zásady pro identifikaci prostředků, které nedodržují předpisy, v prostředí Azure.

Tato příručka vyžaduje modul Azure PowerShell verze 4.0 nebo novější. Verzi zjistíte spuštěním příkazu  ```Get-Module -ListAvailable AzureRM``` . Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Než začnete, ujistěte se, že je nainstalovaná nejnovější verze PowerShellu. Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="opt-in-to-azure-policy"></a>Přihlášení ke službě Azure Policy

Služba Azure Policy je nyní dostupná ve verzi Public Preview a pro vyžádání přístupu se musíte zaregistrovat.

1. Přejděte do služby Azure Policy na adrese https://aka.ms/getpolicy a v levém podokně vyberte **Zaregistrovat se**.

   ![Hledání zásad](media/assign-policy-definition/sign-up.png)

2. Přihlaste se ke službě Azure Policy tak, že v seznamu **Předplatné** vyberete předplatná, se kterými chcete pracovat. Potom vyberte **Zaregistrovat**.

   ![Přihlášení k používání služby Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   Vaše žádost se pro verzi Preview schválí automaticky. Počkejte, systému může zpracování registrace trvat až 30 minut.

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tomto rychlém startu vytvoříme přiřazení zásady a přiřadíme definici *Audit virtuálních počítačů bez spravovaných disků*. Tato definice zásady bude identifikovat prostředky, které nesplňují podmínky nastavené v definici zásady.

Podle těchto pokynů vytvořte nové přiřazení zásady.

Spuštěním následujícího příkazu zobrazte všechny definice zásad a vyhledejte definici zásady, kterou chcete přiřadit:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Služba Azure Policy obsahuje již integrované definice zásad, které můžete použít. Zobrazí se například následující integrované definice zásad:

- Vynucovat použití značky a její hodnoty
- Použít značku a její hodnotu
- Vyžadovat SQL Server verze 12.0

Dále přiřaďte definici zásady k požadovanému oboru pomocí rutiny `New-AzureRmPolicyAssignment`.

Pro účely tohoto kurzu zadáváme pro příkaz následující informace:
- **Name** – Zobrazovaný název přiřazení zásady. V tomto případě použijeme Audit virtuálních počítačů bez spravovaných disků.
- **Policy** – Toto je definice zásady založená na té, kterou používáte k vytvoření tohoto přiřazení. V tomto případě je to definice zásady *Audit virtuálních počítačů bez spravovaných disků*.
- **Scope** – Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat. Může sahat od předplatného až po skupinu prostředků. V tomto příkladu přiřazujeme definici zásady ke skupině prostředků **FabrikamOMS**.
- **$definition** – Je potřeba zadat ID prostředku definice zásady. V tomto případě používáme ID pro definici zásady *Audit virtuálních počítačů bez spravovaných disků*.

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

   Pokud nějaké stávající prostředky nedodržují předpisy tohoto nového přiřazení, zobrazí se na kartě **Neodpovídající prostředky**, jak je znázorněno výše.

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
