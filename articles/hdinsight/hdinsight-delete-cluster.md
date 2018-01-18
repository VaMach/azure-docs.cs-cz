---
title: "Postup odstranění clusteru služby HDInsight - Azure | Microsoft Docs"
description: "Informace o různých způsobů, jak můžete odstranit cluster služby HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 04b89b2cceb18a0e3c88d0d1deada1a05b8187f6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Odstranění clusteru služby HDInsight pomocí prohlížeče, prostředí PowerShell nebo rozhraní příkazového řádku Azure

Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru, začne běžet fakturace clusteru HDInsight. Účtuje se poměrnou částí po minutách, takže byste cluster měli odstranit vždy, když už se nepoužívá. V tomto dokumentu zjistěte, jak odstranit cluster pomocí portálu Azure, Azure PowerShell a Azure CLI 1.0.

> [!IMPORTANT]
> Odstranění clusteru služby HDInsight neodstraní účty Azure Storage nebo Data Lake Store, které jsou přidruženy ke clusteru. Data uložená v těchto služeb v budoucnu můžete znovu použít.

## <a name="azure-portal"></a>Azure Portal

1. Přihlaste se k [portál Azure](https://portal.azure.com) a vyberte clusteru HDInsight. Pokud není clusteru HDInsight připnuli k řídicímu panelu, můžete je vyhledat podle názvu pomocí pole hledání.
   
    ![hledání portálu](./media/hdinsight-delete-cluster/navbar.png)

2. Nastavení pro cluster, vyberte **odstranit** ikonu. Po zobrazení výzvy vyberte **Ano** cluster odstranit.
   
    ![Odstranit ikonu](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Příkazovém řádku prostředí PowerShell můžete cluster odstranit následující příkaz:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Nahraďte **CLUSTERNAME** názvem clusteru HDInsight.

## <a name="azure-cli-10"></a>Azure CLI 1.0

K odstranění clusteru z řádku, použijte následující:

    azure hdinsight cluster delete CLUSTERNAME

Nahraďte **CLUSTERNAME** názvem clusteru HDInsight.

> [!NOTE]
> Azure CLI 2.0 nepodporuje odstraňování clusterů HDInsight v tuto chvíli (23 říjen 2017).