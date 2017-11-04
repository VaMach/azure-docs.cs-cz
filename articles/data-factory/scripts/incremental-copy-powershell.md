---
title: "Skript prostředí PowerShell: přírůstkově načtení dat pomocí Azure Data Factory | Microsoft Docs"
description: "Tento skript prostředí PowerShell ukazuje, jak používat Azure Data Factory ke zkopírování dat přírůstkově z databáze SQL Azure do Azure Blob Storage..."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: 36a8c8c4ed83a56dfd0f487ec4bcf030e3890ef6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>Skript prostředí PowerShell - přírůstkově načtení dat pomocí Azure Data Factory
Tento ukázkový skript prostředí PowerShell načte pouze nové nebo aktualizované záznamy ze zdrojového úložiště dat do úložiště dat podřízený po počáteční úplnou kopii dat ze zdroje jímky.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

V tématu [kurz: přírůstkové kopie](../tutorial-incremental-copy-powershell.md#prerequisites) pro požadavky na spuštění této ukázce. 

## <a name="sample-script"></a>Ukázkový skript

> [!IMPORTANT]
> Tento skript vytvoří soubory JSON, které definují entit služby Data Factory (propojené služby, datové sady a kanál) na vašem pevném disku ve složce c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1 "Incremental copy from Azure SQL Database to Azure Blob Storage")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázkový skript můžete odebrat skupinu prostředků a všechny prostředky, které jsou s ním spojená následující příkaz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Chcete-li odebrat objekt pro vytváření dat ze skupiny prostředků, spusťte následující příkaz: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy: 

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Vytvoření datové továrny |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | V datové továrně vytvoří propojené služby. Propojená služba odkazuje na objekt pro vytváření dat v úložišti dat nebo výpočetní. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Vytvoří datovou sadu v datové továrně. Datové sady představuje vstupu a výstupu pro aktivitu v kanálu. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Vytvoří v objektu pro vytváření dat kanál. Kanál obsahuje jeden nebo více aktivit, které provádí určité operaci. U tohoto kanálu aktivity kopírování kopíruje data z jednoho umístění do jiného umístění v Azure Blob Storage. |
| [Vyvolání AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Vytvoří spustit pro kanál. Jinými slovy spouští kanálu. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Získá informace o spouštění aktivity (aktivity při spuštění) v kanálu. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).

Další ukázky skriptu prostředí PowerShell objekt pro vytváření dat Azure lze nalézt v [skriptů prostředí PowerShell objekt pro vytváření dat Azure](../samples-powershell.md).