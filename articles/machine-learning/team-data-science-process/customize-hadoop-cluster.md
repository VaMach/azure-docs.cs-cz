---
title: "Přizpůsobení clusterů systému Hadoop pro proces vědecké účely dat Team | Microsoft Docs"
description: "K dispozici ve vlastní clusterů systému Azure HDInsight Hadoop oblíbených modulů Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 522e33b399f2648427464b439bc4405e9e8097cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Přizpůsobení clusterů Azure HDInsight Hadoop pro vědecké zpracování týmových dat
Tento článek popisuje, jak přizpůsobit clusteru HDInsight Hadoop nainstalováním Anaconda 64-bit (Python 2.7) na každém uzlu při zřízení clusteru jako služba HDInsight. Také ukazuje, jak pro přístup k headnode vlastní úlohy do clusteru. Toto vlastní nastavení díky mnoha oblíbených Python moduly, které jsou součástí Anaconda pohodlně k dispozici pro použití v uživatelsky definované funkce (UDF), které jsou určené ke zpracování záznamů Hive v clusteru. Pokyny o postupech použitých v tomto scénáři najdete v tématu [postup odesílání dotazů Hive](move-hive-tables.md#submit).

Následující nabídky odkazy na témata, které popisují, jak nastavit různé vědě prostředí datových používané [tým datové vědy procesu (TDSP)](overview.md).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Přizpůsobení clusteru Azure HDInsight Hadoop
Chcete-li vytvořit vlastní cluster HDInsight Hadoop, spusťte protokolování na [ **portál Azure classic**](https://manage.windowsazure.com/), klikněte na tlačítko **nový** v levém dolním rohu a pak vyberte DATA služby -> -> HDINSIGHT **vytvořit vlastní** se zprovoznit **podrobnosti o clusteru** okno. 

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Zadejte název clusteru, který má být vytvořena na stránku konfigurace 1 a přijměte výchozí hodnoty pro ostatní pole. Klikněte na šipku přejděte na další stránku konfigurace. 

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Na stránce konfigurace 2, vstupní počet **datové uzly**, vyberte **oblasti nebo virtuální síť**a vyberte velikosti **hlavní uzel** a **datový uzel**. Klikněte na šipku přejděte na další stránku konfigurace.

> [!NOTE]
> **Oblasti nebo virtuální síť** musí být stejný jako oblast účtu úložiště, který se bude používat pro cluster HDInsight Hadoop. Na čtvrté stránce konfigurace, jinak hodnota účet úložiště se nezobrazí na rozevírací seznam **název účtu**.
> 
> 

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/customize-cluster-img3.png)

Na stránce konfigurace 3 zadejte uživatelské jméno a heslo pro cluster HDInsight Hadoop. **Nechcete** vyberte *zadejte Metaúložiště Hive nebo Oozie*. Potom klikněte na šipku přejděte na další stránku konfigurace. 

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/customize-cluster-img4.png)

Na stránce konfigurace 4 zadejte název účtu úložiště, výchozí kontejner clusteru HDInsight Hadoop. Pokud vyberete *vytvořit výchozí kontejner* v **výchozí kontejner** rozevíracího seznamu, kontejner se stejným názvem jako bude vytvořen cluster. Klikněte na šipku a přejít na poslední stránku konfigurace.

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/customize-cluster-img5.png)

V konečné **akcí skriptů** stránku konfigurace, klikněte na tlačítko **přidat akce skriptu** tlačítko a vyplníte pole text následující hodnoty.

* **NÁZEV** -libovolný řetězec jako název této akce skriptu
* **Typ uzlu** – vyberte **všechny uzly**
* **Identifikátor URI skriptu** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* je veřejném kontejneru v účtu úložiště 
  * *getgoing* budeme používat ke sdílení souborů skriptů prostředí PowerShell pro usnadnění práce uživatelů v Azure
* **Parametry** -(ponechte prázdné)

Nakonec kliknutím na značku zaškrtnutí zahájíte vytváření vlastní cluster HDInsight Hadoop. 

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Přístup k hlavní uzel clusteru Hadoop
Vzdálený přístup ke clusteru Hadoop v Azure musíte povolit, než se dostanete k hlavnímu uzlu clusteru Hadoop prostřednictvím protokolu RDP. 

1. Přihlaste se k [ **portál Azure classic**](https://manage.windowsazure.com/), vyberte **HDInsight** na levé straně, vyberte ze seznamu clusterů Hadoop cluster, klikněte na **konfigurace** a pak klikněte **povolit vzdálené** ikona v dolní části stránky.
   
    ![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/enable-remote-access-1.png)
2. V **konfigurace vzdálené plochy** okno, zadejte uživatelské jméno a heslo pole a vyberte datum vypršení platnosti pro vzdálený přístup. Klikněte na tlačítko zaškrtnutí políčka Povolit vzdálený přístup k hlavnímu uzlu clusteru Hadoop.
   
    ![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> Uživatelské jméno a heslo pro vzdálený přístup nejsou uživatelské jméno a heslo, které používáte při vytváření clusteru Hadoop. Toto je samostatnou sadu přihlašovacích údajů. Datum vypršení platnosti pro vzdálený přístup také musí být v rámci 7 dní od dnešního data.
> 
> 

Po povolení vzdáleného přístupu klikněte na tlačítko **CONNECT** v dolní části stránky na vzdálený do hlavního uzlu. Přihlásíte se k hlavnímu uzlu clusteru Hadoop zadáním pověření pro uživatele vzdáleného přístupu, který jste zadali dříve.

![Vytvořit pracovní prostor](./media/customize-hadoop-cluster/enable-remote-access-3.png)

Další kroky v procesu pokročilou analýzu, jsou namapované v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, pak zpracovat a ukázkové ho existuje v rámci přípravy learning z dat pomocí Azure Machine Learning.

V tématu [postup odesílání dotazů Hive](move-hive-tables.md#submit) pokyny, jak získat přístup k moduly jazyka Python, které jsou součástí Anaconda z hlavního uzlu clusteru v uživatelsky definované funkce (UDF) používané ke zpracování záznamů Hive, které jsou uložené v clusteru.

