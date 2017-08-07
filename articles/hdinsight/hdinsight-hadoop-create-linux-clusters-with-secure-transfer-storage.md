---
title: "Vytvoření clusteru Hadoop s účty úložiště s bezpečným přenosem ve službě Azure HDInsight | Dokumentace Microsoftu"
description: "Naučte se vytvářet clustery HDInsight s účty úložiště Azure s povoleným zabezpečeným přístupem."
keywords: hadoop getting started, hadoop linux, hadoop quickstart, secure transfer, azure storage account
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: f89595c6721c00435e714368905ae48a542f8cb9
ms.contentlocale: cs-cz
ms.lasthandoff: 08/03/2017

---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Vytvoření clusteru Hadoop s účty úložiště s bezpečným přenosem ve službě Azure HDInsight

Funkce [Vyžadovat zabezpečený přenos](../storage/storage-require-secure-transfer.md) zvyšuje zabezpečení účtu služby Azure Storage tím, že vynucuje přenos všech požadavků na účet přes zabezpečené připojení. Tuto funkci a schéma wasbs podporují pouze clustery HDInsight verze 3.6 nebo novější. 

>[!NOTE] 
> Vytváření clusterů s účty úložiště s povoleným zabezpečeným přenosem pomocí sady .NET SDK se aktuálně nepodporuje. Alternativním řešením je nastavení wasbs ve vlastnosti fs.defaultFS v konfiguraci základního webu jako součásti ClusterCreateParametersExtended.

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít:

* **Předplatné Azure**: pro vytvoření bezplatného zkušebního účet na jeden měsíc otevřete web [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Účet služby Azure Storage s povoleným zabezpečeným přenosem**. Pokyny najdete v tématech popisujících [vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) a funkci [Vyžadovat zabezpečený přenos](../storage/storage-require-secure-transfer.md).
* **Kontejner objektů blob v účtu úložiště**. 
## <a name="create-cluster"></a>Vytvoření clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Šablona se nachází ve [veřejném kontejneru](https://hditutorialdata.blob.core.windows.net/securetransfer/azuredeploy-new.json). Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány. Další metody vytváření clusterů a principy vlastnosti používaných v tomto kurzu, naleznete v části [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na webu Azure Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fsecuretransfer%2Fazuredeploy-new.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Postupujte podle pokynů a vytvořte cluster s následujícími specifikacemi: 

    - Zadejte verzi služby HDInsight 3.6.  Výchozí verze je 3.5. Vyžaduje se verze 3.6 nebo novější.
    - Zadejte účet úložiště s povoleným zabezpečeným přenosem.
    - Použijte krátký název účtu úložiště.
    - Účet úložiště i kontejner objektů blob je potřeba vytvořit předem. 

    Pokyny najdete v tématu popisujícím [vytvoření clusteru](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Pokud k zadání vlastních konfiguračních souborů použijete akci skriptu, musíte v následujících nastaveních použít wasbs:

- fs.defaultFS (základní web)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Přidání dalších účtů úložiště

Existuje několik možností, jak přidat další účty úložiště s povoleným zabezpečeným přenosem:

- Úprava šablony Azure Resource Manageru v poslední části.
- Vytvoření clusteru pomocí webu [Azure Portal](https://portal.azure.com) a zadání propojeného účtu úložiště.
- Použití akce skriptu k přidání dalších účtů úložiště s povoleným zabezpečeným přenosem do existujícího clusteru HDInsight.  Další informace najdete v tématu [Přidání dalších účtů úložiště do služby HDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili vytvořit cluster HDInsight a povolit zabezpečený přenos pro účty úložiště.

Další informace o analýze dat pomocí HDInsight naleznete v následujících článcích:

* Další informace o používání Hivu se službou HDInsight, včetně postupu provádění dotazů Hivu ze sady Visual Studio, najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].
* Další informace o jazyce Pig používaném k transformaci dat najdete v tématu [Použití Pigu se službou HDInsight][hdinsight-use-pig].
* Další informace o MapReduce, způsobu psaní programů, které zpracovávají data v Hadoopu, najdete v tématu [Použití MapReduce se službou HDInsight][hdinsight-use-mapreduce].
* Další informace o použití nástrojů HDInsight pro Visual Studio k analýze dat na HDInsight naleznete v části [Začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Další informace o způsobu, jakým služba HDInsight ukládá data, nebo jak přenést data do služby HDInsight, najdete v následujících článcích:

* Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Informace o tom, jak nahrát data do služby HDInsight, najdete v tématu [Nahrání dat do služby HDInsight][hdinsight-upload-data].

Další informace o vytvoření a správě clusteru HDInsight najdete v následujících článcích:

* Další informace o správě clusteru HDInsight se systémem Linux naleznete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).
* Další informace o možnostech, které můžete vybrat při vytváření clusteru služby HDInsight, naleznete v tématu [Vytváření HDInsight na Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).
* Je-li obeznámeni s Linux a Hadoop, ale chcete znát podrobnosti o Hadoop na HDInsight, prostudujte si část [Práce s HDInsight v systému Linux](hdinsight-hadoop-linux-information.md). Tento článek obsahuje informace o:
  
  * Adresách URL služeb hostovaných v clusteru, například Ambari a WebHCat
  * Umístění souborů Hadoop a příkladech v místním systému souborů
  * Používání Azure Storage (WASB) namísto HDFS jako výchozího datového úložiště

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md



