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
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: faf17bcac66a70fc78bb171e172886fd2dcadca8
ms.contentlocale: cs-cz
ms.lasthandoff: 06/16/2017


---
# Začínáme s Azure Data Lake Analytics s využitím Azure PowerShellu
<a id="get-started-with-azure-data-lake-analytics-using-azure-powershell" class="xliff"></a>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučíte se, jak pomocí Azure PowerShellu vytvořit účty Azure Data Lake Analytics a následně odeslat a spustit úlohy U-SQL. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

## Požadavky
<a id="prerequisites" class="xliff"></a>

Před zahájením tohoto kurzu musíte mít následující informace:

* **Účet služby Azure Data Lake Analytics**. Zobrazit téma [Začínáme s Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Pracovní stanice s prostředím Azure PowerShell**. Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## Přihlaste se k Azure.
<a id="log-in-to-azure" class="xliff"></a>

Tento kurz předpokládá, že jste už s používáním Azure Powershellu obeznámení. Konkrétně musíte vědět, jak se k Azure přihlásit. Pokud potřebujete pomoc, přejděte na téma [Začínáme s Azure PowerShellem](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps).

Přihlášení pomocí názvu předplatného:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Místo názvu předplatného můžete také pro přihlášení použít ID předplatného:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Pokud se vám to podaří, výstup tohoto příkazu vypadá jako následující text:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## Příprava pro tento kurz
<a id="preparing-for-the-tutorial" class="xliff"></a>

Fragment kódu PowerShellu v tomto kurzu používá následující proměnné k ukládání příslušných informací:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## Získání informací o účtu Data Lake Analytics
<a id="get-information-about-a-data-lake-analytics-account" class="xliff"></a>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## Odeslání úlohy U-SQL
<a id="submit-a-u-sql-job" class="xliff"></a>

Vytvořte proměnnou Powershellu, aby uchovala skript U-SQL.

```
$script = @"
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

"@
```

Odešlete skript.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Nebo můžete tento skript uložit jako soubor a odeslat ho následujícím příkazem:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Získejte stav konkrétní úlohy. Tuto rutinu používejte, dokud neuvidíte, že se úloha dokončila.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Namísto opakovaného volání rutiny Get-AdlAnalyticsJob, dokud se úloha nedokončí, můžete použít rutinu Wait-AdlJob.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Stáhněte výstupní soubor.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## Viz také
<a id="see-also" class="xliff"></a>
* Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
* Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).

