---
title: "Azure json zásady ukázkové – Ujistěte se, šifrování souborů úložiště | Microsoft Docs"
description: "Tato ukázková zásada json vyžaduje že pro účty úložiště je povolené šifrování tohoto souboru."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 24f6d3e16ae3e98019f97c725960d329cb687555
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="ensure-storage-file-encryption"></a>Ujistěte se, šifrování souborů úložiště

Tato zásada vyžaduje že pro účty úložiště je povolené šifrování tohoto souboru.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Ukázka šablony

[!code-json[main](../../../policy-templates/samples/Storage/storage-account-file-encryption/azurepolicy.json "Ensure storage file encryption")]


Můžete nasadit pomocí této šablony [portál Azure](#deploy-with-the-portal), s [prostředí PowerShell](#deploy-with-powershell) nebo pomocí [rozhraní příkazového řádku Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Nasazení s využitím portálu

[![Nasazení do Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade)

## <a name="deploy-with-powershell"></a>Nasazení s využitím PowerShellu

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]



````powershell
$definition = New-AzureRmPolicyDefinition -Name "storage-account-file-encryption" -DisplayName "Ensure storage file encryption" -description "Ensures file encryption for storage accounts" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-file-encryption/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-file-encryption/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
````

### <a name="clean-up-powershell-deployment"></a>Vyčištění nasazení prostředí PowerShell

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```


## <a name="deploy-with-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]


````cli

az policy definition create --name 'storage-account-file-encryption' --display-name 'Ensure storage file encryption' --description 'Ensures file encryption for storage accounts' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-file-encryption/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-file-encryption/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "storage-account-file-encryption"

````

### <a name="clean-up-azure-cli-deployment"></a>Vyčištění nasazení rozhraní příkazového řádku Azure

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Další kroky

- Další ukázky šablony zásad Azure jsou [šablon pro Azure zásad](../json-samples.md).
