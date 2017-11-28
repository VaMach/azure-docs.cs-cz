---
title: "Používání Hadoop Hive v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell ke spouštění dotazů Hive v Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.openlocfilehash: 95bfab18a6a8a9ad9eb547179a3205ae4b186213
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="run-hive-queries-using-powershell"></a>Spouštění dotazů Hive pomocí prostředí PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Tento dokument poskytuje příklad použití Azure PowerShell v režimu skupiny prostředků Azure ke spouštění dotazů Hive v Hadoop na clusteru HDInsight.

> [!NOTE]
> Tento dokument neposkytuje podrobný popis co dělat, příkazy HiveQL, které se používají v příkladech. Informace o HiveQL, který se používá v tomto příkladu najdete v tématu [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md).

**Požadavky**

* **Cluster Azure HDInsight**: nezáleží, jestli je clusteru systému Windows nebo linuxu.

  > [!IMPORTANT]
  > HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Pracovní stanice s prostředím Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Spouštění dotazů Hive pomocí Azure PowerShell

Prostředí Azure PowerShell poskytuje *rutiny* které umožňují vzdáleně spouštět dotazy Hive v HDInsight. Interně rutiny volat REST [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) v clusteru HDInsight.

Při spouštění dotazů Hive v vzdáleného clusteru HDInsight, se používají následující rutiny:

* **Přidat-AzureRmAccount**: ověřuje prostředí Azure PowerShell k předplatnému Azure.
* **Nové AzureRmHDInsightHiveJobDefinition**: vytvoří *úlohy definice* pomocí zadané příkazy HiveQL.
* **Spuštění AzureRmHDInsightJob**: odešle definici úlohy do HDInsight a spuštění úlohy. A *úlohy* se vrátí objekt.
* **Počkejte AzureRmHDInsightJob**: používá objekt úlohy a zkontrolujte stav úlohy. Se čeká na dokončení úlohy nebo je překročena doba čekání.
* **Get-AzureRmHDInsightJobOutput**: používá se k načtení výstup úlohy.
* **Vyvolání AzureRmHDInsightHiveJob**: používá ke spouštění příkazy HiveQL. Tato rutina bloky dotaz dokončení a potom vrátí výsledky.
* **Použití AzureRmHDInsightCluster**: Nastaví aktuální cluster pro **Invoke-AzureRmHDInsightHiveJob** příkaz.

Následující kroky ukazují, jak tyto rutiny použít ke spuštění úlohy v clusteru HDInsight:

1. Pomocí editoru, uložte následující kód jako **hivejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Otevřete nový **prostředí Azure PowerShell** příkazového řádku. Změňte adresáře na umístění **hivejob.ps1** souboru a potom použijte následující příkaz pro spuštění skriptu:

        .\hivejob.ps1

    Při spuštění skriptu, zobrazí se výzva k zadání názvu clusteru a přihlašovací údaje účtu HTTPS nebo Správce clusteru. Také můžete být vyzváni k přihlášení k předplatnému Azure.

3. Po dokončení úlohy vrátí informace podobná následující text:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Jak už bylo zmíněno dříve, **Invoke-Hive** lze použít ke spuštění dotazu a čekat na odpověď. Chcete-li zjistit, jak funguje Invoke-Hive pomocí následujícího skriptu:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Výstup vypadá následující text:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Pro delší HiveQL dotazy, můžete použít prostředí Azure PowerShell **sem řetězce** rutina nebo HiveQL soubory skriptů. Následující fragment kódu ukazuje způsob použití **Invoke-Hive** můžete spustit soubor skriptu HiveQL. Soubor skriptu HiveQL musí být odeslán do wasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Další informace o **sem řetězce**, najdete v části <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">pomocí Windows PowerShell zde-řetězce</a>.

## <a name="troubleshooting"></a>Řešení potíží

Pokud žádné informace se vrátí po dokončení úlohy, zobrazte protokoly chyb. Chcete-li zobrazit informace o chybě pro tuto úlohu, přidejte na konec následující **hivejob.ps1** souboru, uložit jej a znovu jej spusťte.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Tato rutina vrátí informace, které jsou zapsány do STDERR během zpracování úlohy.

## <a name="summary"></a>Souhrn

Jak vidíte, Azure PowerShell poskytuje snadný způsob, jak spouštět dotazy Hive v clusteru služby HDInsight, monitorovat stav úlohy a načíst výstup.

## <a name="next-steps"></a>Další kroky

Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
