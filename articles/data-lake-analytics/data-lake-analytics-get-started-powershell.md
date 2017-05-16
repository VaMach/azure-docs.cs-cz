---
title: "Začínáme s Azure Data Lake Analytics pomocí Azure PowerShellu | Dokumentace Microsoftu"
description: "Použijte Azure PowerShell k vytvoření účtu Data Lake Analytics, vytvořte úlohu Data Lake Analytics pomocí U-SQL a úlohu odešlete. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 6985dff332928d704f30e167c3bddb62bcc6cac1
ms.contentlocale: cs-cz
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Kurz: Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučíte se, jak pomocí Azure PowerShellu vytvořit účty Azure Data Lake Analytics a následně odeslat a spustit úlohy U-SQL. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu musíte mít následující informace:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Pracovní stanice s prostředím Azure PowerShell**. Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="preparing-for-the-tutorial"></a>Příprava pro tento kurz
Pokud chcete vytvořit účet Data Lake Analytics, nejprve je nutné definovat:

* **Skupina prostředků Azure:** Účet Data Lake Analytics musí být vytvořen v rámci skupiny prostředků Azure.
* **Název účtu Data Lake Analytics:** Název účtu Data Lake musí obsahovat jenom malá písmena a číslice.
* **Umístění**: jedno z datových center Azure podporující Data Lake Analytics.
* **Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store. Tyto účty musí být ve stejném umístění.

Fragment kódu PowerShellu v tomto kurzu používá následující proměnné k ukládání příslušných informací:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics

Pokud ještě nemáte skupinu prostředků, kterou můžete použít, vytvořte ji. 

```
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Každý účet Data Lake Analytics vyžaduje výchozí účet Data Lake Store, který slouží k ukládání protokolů. Můžete použít existující účet nebo vytvořit nový. 

```
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Jakmile budete mít k dispozici skupinu prostředků a účet Data Lake Store, vytvořte účet Data Lake Analytics.

```
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Získání informací o účtu Data Lake Analytics

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Odeslání úlohy U-SQL

Vytvořte textový soubor s následujícím skriptem U-SQL.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Odešlete skript.

```
Submit-AdlJob -AccountName $adla –ScriptPath "d:\test.usql"Submit
```

## <a name="monitor-u-sql-jobs"></a>Monitorování úloh U-SQL

Vypište všechny úlohy v rámci účtu. Výstup zahrnuje aktuálně spuštěné úlohy a nedávno dokončené úlohy.

```
Get-AdlJob -Account $adla
```

Získejte stav konkrétní úlohy.

```
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Namísto opakovaného volání rutiny Get-AdlAnalyticsJob, dokud se úloha nedokončí, můžete použít rutinu Wait-AdlJob.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Po dokončení úlohy zkontrolujte výpisem souborů ve složce, jestli existuje výstupní soubor.

```
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Zkontrolujte, jestli soubor existuje.

```
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading-files"></a>Nahrání a stažení souborů

Stáhněte si výstup skriptu U-SQL.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv"  -Destination "D:\data.csv"
```


Nahrajte soubor, který se použije jako vstup skriptu U-SQL.

```
Import-AdlStoreItem -AccountName $adls -Path "D:\data.tsv" -Destination "/data_copy.csv" 
```

## <a name="see-also"></a>Viz také
* Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
* Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).

