---
title: "Prostředí PowerShell skriptu transformace dat v cloudu pomocí služby Data Factory | Microsoft Docs"
description: "Tento skript prostředí PowerShell transformuje dat v cloudu spuštěním programu Spark v clusteru Azure HDInsight Spark."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: spelluru
ms.openlocfilehash: 195b7276346827479fbbe10dfaaaa9ed1d754967
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---transform-data-in-cloud-using-azure-data-factory"></a>Skript prostředí PowerShell - transformace dat v cloudu pomocí Azure Data Factory

Tento ukázkový skript prostředí PowerShell vytvoří kanál, který transformuje dat v cloudu spuštěním programu Spark v clusteru Azure HDInsight Spark. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Požadavky
* **Účet služby Azure Storage**. Vytvořte python skriptu a vstupní soubor a jejich nahrávání do úložiště Azure. V tomto účtu úložiště se ukládá výstup z programu Sparku. Cluster Spark na vyžádání používá stejný účet úložiště jako primární úložiště.  

### <a name="upload-python-script-to-your-blob-storage-account"></a>Uložení skriptu Pythonu do účtu služby Blob Storage
1. Vytvořte soubor Pythonu s názvem **WordCount_Spark.py** s následujícím obsahem: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Nahraďte **&lt;storageAccountName&gt;** názvem vašeho účtu služby Azure Storage. Pak soubor uložte. 
3. Ve službě Azure Blob Storage, vytvořte kontejner **adftutorial**, pokud ještě neexistuje. 
4. Vytvořte složku s názvem **spark**.
5. Ve složce **spark** vytvořte podsložku s názvem **script**. 
6. Do podsložky **script** uložte soubor **WordCount_Spark.py**. 


### <a name="upload-the-input-file"></a>Nahrání vstupního souboru
1. Vytvořte soubor s názvem **minecraftstory.txt** a nějakým textem. Program Sparku spočítá slova v tomto textu. 
2. Vytvořit podsložku s názvem `inputfiles` v `spark` složky v kontejneru objektů blob. 
3. Do podsložky `inputfiles` uložte soubor `minecraftstory.txt`. 

## <a name="sample-script"></a>Ukázkový skript
> [!IMPORTANT]
> Tento skript vytvoří soubory JSON, které definují entit služby Data Factory (propojené služby, datové sady a kanál) na vašem pevném disku ve složce c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/transform-data-using-spark/transform-data-using-spark.ps1 "Transform data using Spark")]

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
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2) | Vytvoření datové továrny |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedservice) | V datové továrně vytvoří propojené služby. Propojená služba odkazuje na objekt pro vytváření dat v úložišti dat nebo výpočetní. |
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactorv2ypipeline) | Vytvoří v objektu pro vytváření dat kanál. Kanál obsahuje jeden nebo více aktivit, které provádí určité operaci. U tohoto kanálu spark aktivity transformace dat spuštěním programu na clusteru Azure HDInsight Spark. |
| [Vyvolání AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2pipelinerun) | Vytvoří spustit pro kanál. Jinými slovy spouští kanálu. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Získá informace o spouštění aktivity (aktivity při spuštění) v kanálu. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).

Další ukázky skriptu prostředí PowerShell objekt pro vytváření dat Azure lze nalézt v [ukázky Azure Data Factory PowerShell](../samples-powershell.md).