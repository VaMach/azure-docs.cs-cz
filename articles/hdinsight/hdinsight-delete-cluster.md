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
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 65dac529df15d2dd43eec17673d82a2832f7692e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Odstranění clusteru služby HDInsight pomocí prohlížeče, prostředí PowerShell nebo rozhraní příkazového řádku Azure

Jakmile clusteru je vytvořen a zastaví, když odstranění clusteru, začne běžet fakturace clusteru HDInsight. Fakturuje se fakturují za minutu, proto byste měli vždy odstranit cluster, když je již používán. V tomto dokumentu zjistěte, jak odstranit cluster pomocí portálu Azure, Azure PowerShell a Azure CLI 1.0.

> [!IMPORTANT]
> Odstranění clusteru služby HDInsight neodstraní účty Azure Storage nebo Data Lake Store, které jsou přidruženy ke clusteru. Data uložená v těchto služeb v budoucnu můžete znovu použít.

## <a name="azure-portal"></a>portál Azure

1. Přihlaste se k [portál Azure](https://portal.azure.com) a vyberte clusteru HDInsight. Pokud není clusteru HDInsight připnuli k řídicímu panelu, můžete je vyhledat podle názvu pomocí pole hledání.
   
    ![hledání portálu](./media/hdinsight-delete-cluster/navbar.png)

2. Po otevření okna pro cluster, vyberte **odstranit** ikonu. Po zobrazení výzvy vyberte **Ano** cluster odstranit.
   
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
> Azure CLI 2.0 nepodporuje odstraňování clusterů HDInsight v tuto chvíli (31 července 2017).