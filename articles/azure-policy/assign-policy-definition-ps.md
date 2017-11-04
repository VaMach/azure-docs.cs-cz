---
title: "Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí PowerShell vytvořit přiřazení zásad Azure k identifikaci nekompatibilní prostředky."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 02afe946e5e1ad9730ab07df19676e90485ecf98
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Vytvoření přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure pomocí prostředí PowerShell

Prvním krokem při pochopení dodržování předpisů v Azure je zároveň budete vědět, kde stát s aktuální prostředky. Tento rychlý start vás provede procesem vytvoření přiřazení zásady můžete identifikovat virtuální počítače, které nepoužívají spravované disky.

Na konci tohoto procesu se úspěšně zjistilo, jaké virtuální počítače nejsou pomocí spravovaných disků a jsou tedy *nevyhovující*.


PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka údaje vytváření přiřazení zásad k identifikaci nekompatibilní prostředky v prostředí Azure pomocí prostředí PowerShell.

Tato příručka vyžaduje prostředí Azure PowerShell verze modulu 4.0 nebo novější. Spustit ```Get-Module -ListAvailable AzureRM``` najít verzi. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Než začnete, ujistěte se, že je nainstalovaná nejnovější verze PowerShellu. Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="opt-in-to-azure-policy"></a>Vyjádřit výslovný souhlas Azure zásad

Azure zásad je nyní k dispozici ve verzi Public Preview a je nutné zaregistrovat k požádat o přístup.

1. Přejděte do zásad Azure v https://aka.ms/getpolicy a vyberte **zaregistrovat** v levém podokně.

   ![Vyhledejte zásad](media/assign-policy-definition/sign-up.png)

2. Vyjádřit výslovný souhlas do zásad Azure tak, že vyberete odběry ve **předplatné** seznamu chcete pracovat. Potom vyberte **zaregistrovat**.

   ![Vyjádřit výslovný souhlas pomocí zásad Azure](media/assign-policy-definition/preview-opt-in.png)

   Vaše žádost je automaticky schváleny pro verzi Preview. Může trvat až 30 minut pro systém ke zpracování registrace.

## <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

V tento rychlý start, vytvoření přiřazení zásady a přiřadit *auditu virtuální počítače bez spravované disků* definice. Tato definice zásady určují prostředky, které nejsou v souladu s podmínkami, nastavte v definici zásady.

Postupujte podle těchto kroků můžete vytvořit nové přiřazení zásad.

Spusťte následující příkaz, vyhledat a zobrazit všechny definice zásady, že kterou chcete přiřadit:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure zásad se dodává s definice již předdefinovaných zásad, které můžete použít. Definice předdefinované zásady se zobrazí jako například:

- Vynutit značku a její hodnota
- Použít značku a její hodnota
- Vyžadovat verze serveru SQL 12.0

V dalším kroku definice zásady přiřazení požadovaný rozsah pomocí `New-AzureRmPolicyAssignment` rutiny.

V tomto kurzu společnost Microsoft poskytuje následující informace pro příkaz:
- Zobrazení **název** pro přiřazení zásad. V tomto případě použijeme auditu virtuální počítače bez spravované disků.
- **Zásady** – to je definice zásady, na základě vypnout, který používáte k vytvoření přiřazení. V takovém případě je definice zásady – *auditu virtuální počítače bez spravované disků*
- A **oboru** – obor Určuje, jaké prostředky nebo seskupení prostředků v získá vynucena přiřazení zásad. Může rozsahu z odběru do skupiny prostředků. V tomto příkladu jsme se definice zásady pro přiřazení **FabrikamOMS** skupinu prostředků.
- **$definition** – je třeba zadat ID prostředku definice zásady – v tomto případě používáme ID definice zásady - *auditu virtuální počítače bez spravované disků*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Nyní jste připraveni určete nekompatibilní prostředky pro informace o stavu dodržování předpisů vašeho prostředí.

## <a name="identify-non-compliant-resources"></a>Určete nekompatibilní prostředky

1. Přejděte zpět na cílovou stránku zásad Azure.
2. Vyberte **dodržování předpisů** v levém podokně a Hledat **přiřazení zásady** jste vytvořili.

   ![Zásady dodržování předpisů](media/assign-policy-definition/policy-compliance.png)

   Pokud jsou všechny existující prostředky, které nejsou kompatibilní s toto přiřazení nové, budou se zobrazí na **nekompatibilní prostředky** kartě, jak je uvedeno výše.

## <a name="clean-up-resources"></a>Vyčištění prostředků

V této kolekci dalších příručkách stavět na tento rychlý start. Pokud budete chtít pokračovat v práci s další kurzy, neprovádí vyčištění prostředky vytvořené v tento rychlý start. Pokud neplánujete, chcete-li pokračovat, odstraňte přiřazení, které jste vytvořili spuštěním tohoto příkazu:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste přiřadili definici zásady k identifikaci nekompatibilní prostředky v prostředí Azure.

Další informace o přiřazení zásad, aby **budoucí** prostředky, které jsou vytvářeny předpisům, pokračovat v kurzu pro:

> [!div class="nextstepaction"]
> [Vytváření a Správa zásad](./create-manage-policy.md)
