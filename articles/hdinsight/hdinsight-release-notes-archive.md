---
title: "Archivovat poznámky k verzi - komponent systému Hadoop v Azure HDInsight | Microsoft Docs"
description: "Poznámky k verzi archivovaný pro starší verze komponent systému Hadoop pro Azure HDInsight."
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 04278aac85171601b5801b0890d14a9076060444
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="release-notes-archive-for-hadoop-components-on-azure-hdinsight"></a>Poznámky k verzi (archiv) pro komponent systému Hadoop v Azure HDInsight

Tento článek obsahuje informace o **starší** Azure HDInsight verze aktualizace. Informace na novější verze najdete v tématu [poznámky k verzi HDInsight](hdinsight-release-notes.md).

> [!IMPORTANT]
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [HDInsight verze článku](hdinsight-component-versioning.md).



## <a name="notes-for-08302016-release-of-hdinsight"></a>Poznámky k verzi 08/30/2016 HDInsight
Čísla plná verze pro clustery HDInsight se systémem Linux nasazené v této verzi:

| HDI | Verze clusteru HDI | HDP | HDP sestavení | Ambari sestavení |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Čísla plná verze pro nasazení v této verzi clustery HDInsight se systémem Windows:

| HDI | Verze clusteru HDI | HDP | HDP sestavení |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |


## <a name="08172016---release-of-r-server-on-hdinsight"></a>08/17/2016 - verzi R serverem v HDInsight
* R Server 8.0.5 - především verze oprava chyby. Najdete v článku [poznámky k verzi serveru R](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) Další informace.
* Balíček AzureML na uzlu edge - [tento balíček R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) umožňuje R modely, které budou publikovat a využívat jako webové služby Azure ML.  Najdete v článku ["Zprovoznit Model"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) části našich ["Přehled o R serverem v HDInsight"](hdinsight-hadoop-r-server-overview.md) Další informace najdete v článku.
* Linux závislostí [prvních 100 nejoblíbenější balíčky R](https://github.com/metacran/cranlogs) -tyto závislosti balíčků Linux jsou nyní předem nainstalovány.
* Možnost použít úložišti CRAN při přidávání R balíčky do datové uzly. Další informace najdete v tématu ["Začínáme používat R Server v HDInsight"](hdinsight-hadoop-r-server-get-started.md).
* Vylepšení spolehlivosti R Server zřizování při vytváření clusterů.

## <a name="notes-for-08012016-release-of-hdinsight"></a>Poznámky k verzi 08/01/2016 HDInsight
Čísla plná verze pro clustery HDInsight se systémem Linux nasazené v této verzi:

| HDI | Verze clusteru HDI | HDP | HDP sestavení | Ambari sestavení |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8028416 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8028416 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8053402 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Čísla plná verze pro nasazení v této verzi clustery HDInsight se systémem Windows:

| HDI | Verze clusteru HDI | HDP | HDP sestavení |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1005.2488842 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1005.2488842 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1005.2488842 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1005.2488842 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1005.2488842 |2.3 |2.3.3.1-25 |

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Typ clusteru (například Spark, Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Změny ke clusterům HDInsight 3.4 |Výchozí hodnoty pro následující konfigurace hive se změnil pro lepší výkon <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul> |Služba |Všechny |Není k dispozici |
| Tyto opravy jsou součástí této verze |HIVE 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933 |Služba |Všechny |Není k dispozici |

## <a name="notes-for-07142016-release-of-hdinsight"></a>Poznámky k verzi 07/14/2016 HDInsight
Čísla plná verze pro clustery HDInsight se systémem Linux nasazené v této verzi:

| HDI | Verze clusteru HDI | HDP | HDP sestavení | Ambari sestavení |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7932505 |2.2 |2.2.9.1-11 |2.2.1.12-2 |
| 3.3 |3.3.1000.0.7932505 |2.3 |2.3.3.1-18 |2.2.1.12-2 |
| 3.4 |3.4.1000.0.7933003 |2.4 |2.4.2.0 |2.2.1.12-2 |

Čísla plná verze pro nasazení v této verzi clustery HDInsight se systémem Windows:

| HDI | Verze clusteru HDI | HDP | HDP sestavení |
| --- | --- | --- | --- |
| 2.1 |2.1.10.989.2441725 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.989.2441725 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.989.2441725 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.989.2441725 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.989.2441725 |2.3 |2.3.3.1-21 |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Poznámky k verzi 07/07/2016 HDInsight
Čísla plná verze pro clustery HDInsight se systémem Linux nasazené v této verzi:

| HDI | Verze clusteru HDI | HDP | HDP sestavení |
| --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7864996 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.1000.0.7864996 |2.3 |2.3.3.1-18 |
| 3.4 |3.4.1000.0.7861906 |2.4 |2.4.2.0 |

Čísla plná verze pro nasazení v této verzi clustery HDInsight se systémem Windows:

| HDI | Verze clusteru HDI | HDP | HDP sestavení |
| --- | --- | --- | --- |
| 2.1 |2.1.10.977.2413853 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.977.2413853 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.977.2413853 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.977.2413853 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.977.2413853 |2.3 |2.3.3.1-21 |

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Typ clusteru (například Spark, Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| [Nástroje HDInsight pro IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) |Modul plug-in IntelliJ IDEA pro clustery HDInsight Spark je nyní integrována Azure Toolkit pro IntelliJ. Podporuje Azure SDK v2.9.1, nejnovější sady Java SDK a obsahuje všechny funkce z samostatný modul plug-in HDInsight pro IntelliJ. |Nástroje |Spark |Není k dispozici |
| [Nástroje HDInsight pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) |Azure nástrojů pro Eclipse teď podporuje clustery HDInsight Spark. Umožňuje následující funkce: <ul><li>Vytvoření a zápis aplikací Spark Scala a Java snadno s třídou první vytváření podporu pro technologii IntelliSense, automatického formátování, kontrola chyb, atd.</li><li>Testování aplikace Spark místně.</li><li>Odesílání úloh do clusteru HDInsight Spark a načtěte výsledky.</li><li>Přihlaste se k Azure a přístup všechny clustery Spark přidružené k vašemu předplatnému Azure.</li><li>Přejděte všechny prostředky přidruženého úložiště clusteru HDInsight Spark.</li></ul> |Nástroje |Spark |Není k dispozici |

Od této verze, jsme změnili oprav zásady hostovaného operačního systému pro clustery HDInsight se systémem Linux. Cílem nové zásady je výrazně snížit počet restartování z důvodu opravy. Nové zásady opravy virtuální počítače (VM) v systému Linux clusterů každé pondělí a čtvrtek začínající na 12: 00 UTC postupný způsobem mezi uzly v jakémkoliv daného clusteru. Jakékoli dané virtuálních počítačů však restartuje pouze maximálně jednou za 30 dní z důvodu opravy operačního systému hosta. Kromě toho první restartování pro nově vytvořený cluster neodehrává dřív než 30 dní od data vytvoření clusteru.

> [!NOTE]
> Tyto změny se uplatní jenom na nově vytvořený clustery rovna nebo větší než tuto verzi.
>
>

## <a name="notes-for-06062016-release-of-hdinsight"></a>Poznámky k verzi 06/06/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

| HDP | Verze HDI | Spark verze | Číslo sestavení Ambari | Číslo sestavení HDP |
| --- | --- | --- | --- | --- |
| 2.3 |3.3.1000.0.7702215 |1.5.2 |2.2.1.8-2 |2.3.3.1-18 |
| 2.4 |3.4.1000.0.7702224 |1.6.1 |2.2.1.8-2 |2.4.2.0 |

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Typ clusteru (například Spark, Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Spark v HDInsight je obecně k dispozici |Tato verze přináší vylepšení v dostupnost, škálovatelnost a produktivitu otevřít zdroj Apache Spark v HDInsight. <ul><li>Špičkový dostupnost SLA 99,9 %, takže je vhodný pro podnikové úlohy náročné.</li><li>Vrstva škálovatelného úložiště pomocí Azure Data Lake Store.</li><li>Kancelářským nástrojům pro každou fázi zkoumání dat a vývoj. Poznámkové bloky Jupyter s vlastní jádra Spark povolit interaktivní zkoumání dat, integraci s řídicí panely BI jako Power BI a Tableau Qlik je vhodný pro sdílení dat rychlý a průběžné vytváření sestav, modul plug-in IntelliJ spolehlivé možnost pro dlouho termín kód vývoj artefaktů a ladění.</li></ul> |Služba |Spark |Není k dispozici |
| Nástroje HDInsight pro IntelliJ |Toto je modul plug-in IntelliJ IDEA pro clustery HDInsight Spark. Umožňuje následující funkce:<ul><li>Vytvoření a zápis aplikací Spark Scala a Java snadno s třídou první vytváření podporu pro technologii IntelliSense, automatického formátování, kontrola chyb, atd.</li><li>Testování aplikace Spark místně.</li><li>Odesílání úloh do clusteru HDInsight Spark a načtěte výsledky.</li><li>Přihlaste se k Azure a přístup všechny clustery Spark přidružené k vašemu předplatnému Azure.</li><li>Přejděte všechny prostředky přidruženého úložiště clusteru HDInsight Spark.</li><li>Přejděte historie úlohy a informace o úlohách pro váš cluster HDInsight Spark.</li><li>Ladění úloh Spark vzdáleně ze stolního počítače.</li></ul> |Nástroje |Spark |Není k dispozici |

## <a name="notes-for-05132016-release-of-hdinsight"></a>Poznámky k verzi 13/05/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.922.2266903 (HDP 2.2.9.1-11) (Windows)
* HDInsight 3.3.0.922.2266903 (HDP 2.3.3.1-18) (Windows)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Typ clusteru (například Spark, Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Spark verze aktualizace a ostatní opravy chyb |Tato verze 1.6.1 aktualizace na verzi Spark v HDInsight clusteru a řeší jiné chyby |Služba |Spark |Není k dispozici |

## <a name="notes-for-04112016-release-of-hdinsight"></a>Poznámky k verzi 04/11. dubna 2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.889.2191206 (HDP 2.2.9.1-10) (Windows)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-beze změny)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Vlastní metaúložiště upgrade problémy pro HDI 3.4 |Vytvoření clusteru selže, pokud jste použili vlastní metaúložiště, který byl dříve používán na nižší verzi jiný cluster HDInsight. Důvodem byla upgradu chyba skriptu, který teď byl opraven |Vytvoření clusteru |Všechny |Není k dispozici |
| Obnovení při havárii Livy |Poskytuje odolnost stav úlohy pro všechny úlohy odeslat prostřednictvím Livy |Spolehlivost |Spark v Linux |Není k dispozici |
| Obsah Jupyter HA |Poskytuje možnost pro uložení a načtení obsahu poznámkového bloku Jupyter do a z účtu úložiště, který je přidružen ke clusteru. Další informace najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Poznámkové bloky |Spark v Linux |Není k dispozici |
| Odebrání hiveContext v poznámkové bloky Jupyter |Použití `%%sql` magic místo `%%hive` magic. SqlContext je ekvivalentní hiveContext. Další informace najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md) |Poznámkové bloky |Clustery Spark na systému Linux |Není k dispozici |
| Vyřazení starší verze Spark |Starší verze Spark 1.3.1 byla odebrána ze služby na 5/31 |Služba |Clustery Spark na systému Windows |Není k dispozici |

## <a name="notes-for-03292016-release-of-hdinsight"></a>Poznámky k verzi 03/29/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.875.2159884 (HDP 2.2.9.1-7 - beze změny) (Windows)
* HDInsight 3.3.0.875.2159884 (HDP 2.3.3.1-16) (Windows)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - beze změny)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - beze změny)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Přidání verze HDInsight 3.4 a aktualizované verze softwaru HDP pro všechny clustery HDInsight |V této verzi jsme přidali HDInsight v3.4 (podle HDP 2.4) a také aktualizovaly ostatních verzí softwaru HDP. Poznámky k verzi softwaru HDP 2.4 jsou k dispozici [sem](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) a naleznete další informace o verzích HDInsight [zde](hdinsight-component-versioning.md). |Služba |Všechny clustery Linux |Není k dispozici |
| HDInsight Premium |HDInsight je nyní k dispozici ve dvou kategoriích – Standard a Premium. Clustery Spark v Linux HDInsight Premium je aktuálně ve verzi Preview a je k dispozici pouze pro Hadoop. Další informace najdete v tématu [zde](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium). |Služba |Hadoop a Spark v Linux |Není k dispozici |
| Server Microsoft R |HDInsight Premium poskytuje Microsoft R Server, který může být součástí clustery Hadoop a Spark na systému Linux. Další informace najdete v tématu [přehled R serverem v HDInsight](hdinsight-hadoop-r-server-overview.md). |Služba |Hadoop a Spark v Linux |Není k dispozici |
| Spark 1.6.0 |Clustery HDInsight 3.4 nyní zahrnují Spark 1.6.0 |Služba |Clustery Spark na systému Linux |Není k dispozici |
| Vylepšení poznámkového bloku Jupyter |Poznámkové bloky Jupyter s clustery Spark k dispozici nyní poskytují další Spark jádra. Také obsahují vylepšení jako použití %% magic, automaticky vizualizace a integrace s Python vizualizace knihovny (například matplotlib). Další informace najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Služba |Clustery Spark na systému Linux |Není k dispozici |

## <a name="notes-for-03222016-release-of-hdinsight"></a>Poznámky k verzi 03/22/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.875.2159884 (HDP 2.2.9.1-7 - beze změny) (Windows)
* HDInsight 3.3.0.875.2159884 (HDP 2.3.3.1-16) (Windows)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - beze změny)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - beze změny)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight |V této verzi jsme aktualizovaly HDInsight verze pro všechny clustery HDInsight |Služba |Všechny |Není k dispozici |

## <a name="notes-for-03102016-release-of-hdinsight"></a>Poznámky k verzi 03/10/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.859.2123216 (HDP 2.2.9.1-7) (Windows)
* HDInsight 3.3.0.859.2123216 (HDP 2.3.3.1-5 - beze změny) (Windows)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight |V této verzi jsme aktualizovaly HDInsight verze pro všechny clustery HDInsight |Služba |Všechny |Není k dispozici |

## <a name="notes-for-01272016-release-of-hdinsight"></a>Poznámky k verzi 01/27/2016 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.817.2028315 (HDP 2.2.9.1-1) (Windows)
* HDInsight 3.3.0.817.2028315 (HDP 2.3.3.1-5 - beze změny) (Windows)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight |V této verzi jsme aktualizovaly HDInsight verze pro všechny clustery HDInsight |Služba |Všechny |Není k dispozici |

## <a name="notes-for-12022015-release-of-hdinsight"></a>Poznámky k verzi 12/02/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.763.1931434 (HDP 2.2.7.1-34 - beze změny) (Windows)
* HDInsight 3.3.1000.0 (HDP 2.3.3.1-5) (Windows)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - beze změny)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Přidání HDInsight 3.3 verze a aktualizované verze softwaru HDP pro všechny clustery HDInsight |V této verzi jsme přidali HDInsight v3.3 (podle HDP 2.3) a také aktualizovaly ostatních verzí softwaru HDP. Poznámky k verzi softwaru HDP 2.3 jsou k dispozici [sem](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) a naleznete další informace o verzích HDInsight [zde](hdinsight-component-versioning.md). |Služba |Všechny |Není k dispozici |

## <a name="notes-for-11302015-release-of-hdinsight"></a>Poznámky k verzi 11/30/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.757.1923908 (HDP 2.2.7.1-34) (Windows)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight a verze softwaru HDP pro clustery HDInsight 3.2 (Windows a Linux) |V této verzi se byly aktualizovány HDInsight a softwaru HDP verze |Služba |Všechny |Není k dispozici |

## <a name="notes-for-10272015-release-of-hdinsight"></a>Poznámky k verzi 27/10/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - beze změny) (Windows)
* HDInsight 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - beze změny) (Windows)
* HDInsight 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - beze změny) (Windows)
* HDInsight 3.2.7.726.1866228 (HDP 2.2.7.1-33) (Windows)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight (Windows a Linux) |V této verzi se byly aktualizovány HDInsight a softwaru HDP verze |Služba |Všechny |Není k dispozici |
| Opravené clustery Windows Spark pro Jupyter s clustery velkých písmen |Clustery, které měl názvy DNS zadané velkými písmeny měly problémy s poznámkovými bloky Jupyter z důvodu počátečního kontrolu žádostí. Potíže se změna názvu DNS pro konfiguraci na Jupyter na malá písmena. |Služba |HDInsight Spark (Windows) |Není k dispozici |

## <a name="notes-for-10202015-release-of-hdinsight"></a>Poznámky k verzi 20/10/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Výchozí verze softwaru HDP změnit tak, aby HDP 2.2 |Výchozí verze pro clustery HDInsight Windows se změní na HDP 2.2. Od února 2015 byl všeobecně dostupná v HDInsight verze 3.2 (HDP 2.2). Tato změna pouze převrátí clusteru je výchozí verze, pokud nebyla provedena explicitní výběr při zřizování clusteru pomocí portálu Azure, rutiny prostředí PowerShell nebo sady SDK. |Služba |Všechny |Není k dispozici |
| Změny ve formátu názvu virtuálního počítače pro nasazení více HDInsight v systému Linux clusterů v jedné virtuální sítě |Podpora pro nasazení více clusterů HDInsight Linux v jedné virtuální sítě je přidáván v této verzi. Jako součást aktualizace, formát názvy virtuálních počítačů v clusteru se změnila z headnode\*, workernode\* a zookeepernode\* k hn\*, wn\*a zk\* v uvedeném pořadí. Není doporučený postup pro být přímé závislý na formát názvy virtuálních počítačů, protože se jedná, může se změnit. Určení seznamu hostitelů a mapování součástí hostitelům pomocí "hostname -f" na místním počítači nebo rozhraní Ambari API. Můžete najít další informace v [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) a [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). |Služba |Clustery prostředí HDInsight v Linuxu |Není k dispozici |
| Změny konfigurace |Pro clustery HDInsight 3.1 jsou nyní k dispozici následující konfigurace: <ul><li>tez.yarn.ATS.Enabled a yarn.log.server.url. To umožňuje serveru časová osa aplikace a serverem protokolu moct obsluhovat protokoly.</li></ul>Pro clustery HDInsight 3.2 byly upraveny následující konfigurace: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version byla nastavena na hodnotu 2. To umožňuje použití verzi FileOutputCommitter V2.</li></ul> |Služba |Všechny |Není k dispozici |

## <a name="notes-for-09092015-release-of-hdinsight"></a>Poznámky k verzi 09/09/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight |V této verzi se byly aktualizovány HDInsight verze |Služba |Všechny |Není k dispozici |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Poznámky k verzi 31 07. 2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Opravte workflow obnovování uzlu clusteru Spark |Pevné chyb, která je příčinou Spark uzly clusteru, do není obnovit po obnovení z Image |Služba |Spark |Není k dispozici |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Poznámky k verzi 31 07. 2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - beze změny)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze HDInsight pro všechny clustery HDInsight |V této verzi se byly aktualizovány HDInsight verze |Služba |Všechny |Není k dispozici |

## <a name="notes-for-07072015-release-of-hdinsight"></a>Poznámky k verzi 07 07. 2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

| Název | Popis | Ovlivněné oblasti (například služby, komponenty nebo sady SDK) | Cluster typu (například Hadoop, HBase nebo Storm) | JIRA (pokud existuje) |
| --- | --- | --- | --- | --- |
| Aktualizované verze softwaru HDP pro clustery HDInsight 3.2 |V této verzi se nasadí HDInsight 3.2 HDP 2.2.6.1-0012 |Služba |Všechny |Není k dispozici |

## <a name="notes-for-06262015-release-of-hdinsight"></a>Poznámky k verzi 06/26/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - beze změny)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Aktualizované verze softwaru HDP pro clustery HDInsight 3.2</td>
<td>V této verzi se nasadí HDInsight 3.2 HDP 2.2.6.1</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Poznámky k verzi 18. 06/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Další otevřené porty protokolu HTTPS</td>
<td>Cloudové služby teď otevře 5 porty 8001 k 8005 v clusteru, například na https://<clustername>.azurehdinsight.net:8001/. Požadavky na tyto adresy URL se ověřují pomocí stejné heslo mechanizmu základní ověřování, protože port 443. Tyto porty vytvořit vazbu na stejný port, na aktivní headnode. Akce skriptu můžete použít k nastavení zákaznické naslouchat na tyto porty na headnode a trasy, která má mimo cluster.</td>
<td>Cloudové služby</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Občasný problém náhodně MapReduce pro HDInsight 3.2</td>
<td>Oprava výjimečných, přerušované časování v náhodně snížit mapy na velkých clusterech, což vede k selhání příležitostně úkolů. V tématu <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> Další informace.</td>
<td>Základní Hadoop</td>
<td>Všechny</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>
<tr>
<td>Přesunout do nejnovější Azure Java SDK 2.2 pro HDInsight 3.2</td>
<td>Přesunout na nejnovější verzi sady Azure SDK pro jazyk Java používá WASB ovladač. Nejnovější sada SDK obsahuje několik oprav a jsou dostupné v https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt poznámky k verzi pro stejné.</td>
<td>Základní Hadoop</td>
<td>Všechny</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>
<tr>
<td>Přesunout do HDP 2.1.15 pro clustery HDInsight 3.1</td>
<td>Poznámky k verzi Hortonworks pro tuto verzi jsou k dispozici <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">zde</a>.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Poznámky k verzi 06/04/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - beze změny)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Opravte chyby 502 Chybná brána pro clustery Storm</td>
<td>Toto vydání opravuje chyby by to ovlivnilo úlohy odeslání rozhraní API, která způsobila, že web za zastavený po restartu systému.</td>
<td>Služba</td>
<td>Storm</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Poznámky k verzi 06/01/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - beze změny))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - beze změny)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Různé opravy chyb</td>
<td>Toto vydání opravuje chyby související s zřizování clusteru.</td>
<td>Služba</td>
<td>Všechny typy clusteru</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Poznámky k verzi 27/05/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Jiné verze clusteru a sady SDK se nenasadí v rámci této verze.

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Aktualizace softwaru HDP 2.2</td>
<td>Tato verze HDInsight 3.2 obsahuje HDP 2.2.6 a přináší několik důležitých oprav chyb do HDInsight. Jsou k dispozici v poznámkách k verzi úplné <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 poznámky k verzi</a>.</td>
<td>HDP</td>
<td>Všechny typy clusteru</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Změňte výchozí Yarn kontejneru paměti konfiguraci</td>
<td>V této aktualizaci je výchozí dostupná paměť YARN kontejnery (yarn.nodemanager.resource.memory mb a yarn.scheduler.maximum přidělení mb), spuštění uzlu správcem, zvýšena na 5632MB. Dříve to byla snížena na 4608MB, ale podle různých spustí úlohu, nová hodnota musí nabízí lepší spolehlivost a výkon pro většinu úloh, proto je lepší výchozí. Obvyklým způsobem Pokud máte kriticky závislé na této konfiguraci paměti, nastavte ji explicitně při vytváření clusteru.</td>
<td>HDP</td>
<td>Všechny typy clusteru</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Výchozí konfigurace parita pro clustery HBase a Storm</td>
<td>Tato aktualizace obnoví clustery Hbase a Storm použít stejné hodnoty YARN konfigurací jako clusterů systému Hadoop. To se provádí pro paritní všech typů clusteru.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Poznámky k verzi 20/05/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Podpora EventHub SCP.NET</td>
<td>Aktualizované clusteru balíčky pro HDInsight Storm uvede SCP.NET nové funkce. Nyní máte přístup k rozhraním nové API v Tvůrci topologie, které usnadňují použití EventHubSpout nebo Java Spouts. Je třeba aktualizovat váš klient SCP.NET sady SDK pracovat s nových clusterů, jak byly aktualizovány smluv. Podrobnosti o nové rozhraní API, použití a vydání poznámky (včetně oprav chyb) naleznete v souboru Readme součástí balíčku SCP.NET NuGet.</td>
<td>VS nástrojů</td>
<td>Clustery HDInsight 3.2 Storm</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Aktualizovaný ovladač JDBC</td>
<td>Aktualizovat ovladač ve sqljdbc_4.1.5605.100 verzi systému SQL Server podporována.</td>
<td>Metaúložiště.</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Poznámky k verzi 04/27/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Opravte závislostí knihoven DLL</td>
<td>Odebere závislost HDInsight na Unit Test Framework.</td>
<td>Sada SDK</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Oprava chyby pro časování</td>
<td>Cluster vytvořit požadavek nyní čeká na žádosti PUT třeba přijmout před dotazování na stav</td>
<td>Sada SDK</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Poznámky k verzi 04/14. prosince 2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Opravy chyb tez</td>
<td>Opravy pro Apache TEZ 2214 a TEZ 1923 jsou součástí této verze HDI 3.2. Pro některé dotazy Hive v Tez, které vyžadují náhodný výběr významné množství dat je potřeba.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Poznámky k verzi 04/06/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - beze změny)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - beze změny)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - beze změny)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Aktualizace můžete odebrat některé interní třídy pro HDInsight v systému Linux.</td>
<td>Sada SDK</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Knihovna Avro 1.5.6</td>
<td>Přidat <b>KnownTypeAttribute</b> pro metodu <b>GetAllKnownTypes</b>. Pokud má hodnotu null pro metodu GetAllKnownTypes typu pevné NullReferenceException.</td>
<td>Sada SDK</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Opravy chyb</td>
<td>Různé opravy chyb pro službu</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Poznámky k verzi 04/01/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Umožňuje povolit nebo zakázat vzdálené plochy přihlašovací údaje v clusterech Windows pomocí sady .NET SDK</td>
<td>Programové podpory pro povolení nebo zakázání protokolu RDP přihlašovací údaje u clusterů systému Windows.</td>
<td>Sada SDK</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Umožňuje povolit vzdálené plochy přihlašovací údaje u clusterů v době jejich zřízení</td>
<td>Programové podpory pro povolení vzdálené plochy pověření při vytváření clusteru. Touto akcí odeberete dvoustupňový proces pro první zřizování clusteru a pak povolení funkce Vzdálená plocha.</td>
<td>Sada SDK</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Upgradovaná Python k 2.7.8</td>
<td>Clustery prostředí HDInsight k Python 2.7.8, upgradovaná Python, který obsahuje některé důležité zabezpečení opravy pro HDInsight verze 2.1, 3.0, 3.1 a 3.2</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Změna konfigurace YARN</td>
<td>Změněné YARN konfigurace yarn.resourcemanager.max – dokončit aplikace na 1000 pro všechny typy clusteru HDInsight verze 3.1 a 3.2. Tato hodnota řídí pouze seznam dokončené aplikací v uživatelském rozhraní YARN. Pokud chcete získat informace o aplikacích, které byly odeslány před seznam aplikací, na které se zobrazí v Uživatelském rozhraní, můžete přímo přejít k serveru historie.</td>
<td>YARN</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Změna velikosti uzlů v clusteru služby HBase</td>
<td>Clustery HBase teď povolit změnu velikosti uzly (nahoru a dolů) pro HDInsight verze 3.1 a 3.2</td>
<td>Služba</td>
<td>HBase</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>JDBC upgrade</td>
<td>Ovladač SQL JDBC je upgradován na verzi sqljdbc_4.0.2206.100 pro HDInsight verze 3.2. Tato verze obsahuje vylepšení důležité zabezpečení.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Aktualizace konfigurace JVM</td>
<td>Aktualizované JVM konfigurace networkaddress.cache.ttl na 300 sekund z výchozí hodnota-1 pro HDInsight verze 3.1 a 3.2. Tato hodnota konfigurace řídí zásady ukládání do mezipaměti pro hledání úspěšné název ze služby název. To opravy chyby související s zvětšování a zmenšování clustery HBase.</td>
<td>Služba</td>
<td>HBase</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Upgrade na úložiště Azure SDK pro jazyk Java 2.0</td>
<td>HDInsight verze 3.2 je upgradována, aby používali nejnovější verzi sady Azure SDK úložiště pro jazyk Java. Tato položka obsahuje několik důležitých oprav chyb přes 0.6.0 aktuální verze.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Upgradovat na Apache WASB zdrojového kódu</td>
<td>HDInsight verze 3.2 teď používá nejnovější kód ovladač systému souborů WASB z Apache Hadoop. Díky této změně se teď ovladač WASB zabalené jako samostatné jar. Toto je čistě změnu balení a neobsahuje žádné změny chování WASB ovladače. Název tohoto souboru JAR je hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Název souboru aktualizace v HDInsight 3.2 JAR</td>
<td>Tato aktualizace do HDInsight verze 3.2 obsahuje několik oprav chyb a několik interní JAR zabalené jako součást HDP byly upgradovány. Tyto JAR filesare interní balíčku softwaru HDP a ne pro přímé použití aplikacemi zákazníka. Aplikace by měl balíček jejich vlastní verze JAR tak, aby upgradu na interní JAR HDP nedojde k narušení zákazníka aplikace.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Poznámky k verzi 03 03. 2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - beze změny)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-. 2340 - beze změny)
* SDK 1.5.0 (beze změny)

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA</th>
</tr>
<tr>
<td>Vylepšení spolehlivosti</td>
<td>Jsme provedli opravy, které umožňují službu, kterou chcete škálovat lépe s je zvýšená zátěž s ohledem na vytváření clusteru.</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Poznámky k verzi 02/18/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - beze změny)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-. 2340)
* SDK 1.5.0

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Clustery HDInsight 3.2</td>
<td>Hadoop 2.6/HDP2.2 je k dispozici u clusterů HDInsight 3.2. Obsahuje hlavní aktualizace všech součástí open source. Další informace najdete v tématu Co je nového v HDInsight a <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">poznámky k verzi softwaru HDP 2.2.0.0</a>.</td>
<td>Open-source softwaru</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>HDInsight v Linuxu (Preview)</td>
<td>Clustery se dá nasadit systémem Ubuntu Linux. Další informace najdete v tématu Začínáme s HDInsight v systému Linux.</td>
<td>Služba</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Storm obecné dostupnosti</td>
<td>Clustery Apache Storm jsou obecně k dispozici. Další informace najdete v tématu začít používat Storm v HDInsight.</td>
<td>Služba</td>
<td>Storm</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Velikosti virtuálních počítačů</td>
<td>Azure HDInsight je k dispozici pro další typy virtuálního počítače a velikosti. HDInsight můžete využít A2 na velikosti A7, vytvořené pro obecné účely; Uzly série D-Series, které funkce disků SSD (Solid-State Drive) a 60 procent rychlejšími procesory; a velikosti A8 a A9, které mají InfiniBand podporu pro rychlou práci v síti.</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Škálování clusterů</td>
<td>Můžete změnit počet uzlů data pro spuštěného clusteru HDInsight bez nutnosti jej odstranit nebo znovu vytvořit. Tuto možnost, ale podpora pro Apache HBase typ clusteru je brzy postupujte podle mají v současné době pouze typy clusteru Apache Storm a Hadoop dotazu. Další informace najdete v tématu Správa HDInsight clustery.</td>
<td>Služba</td>
<td>Hadoop, Storm</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Nástrojů Visual Studio</td>
<td>Kromě dokončení nástrojů pro Apache Storm, nástrojů pro Apache Hive v sadě Visual Studio se aktualizovala dokončování příkazů, místní ověřování a lepší podporu ladění. Další informace najdete v tématu Začínáme s Hadoop nástroje HDInsight pro Visual Studio.</td>
<td>Nástrojů</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<td>Konektor Hadoop pro Azure Cosmos DB</td>
<td>Pomocí konektoru systému Hadoop pro Azure Cosmos DB můžete provádět komplexní agregace, analýzu a manipulaci s přes vaše dokumenty JSON bez schématu uložené přes Azure Cosmos DB kolekce nebo přes účty databáze. Další informace a kurz najdete v tématu úloh Hadoop spustit pomocí Azure Cosmos DB a HDInsight.</td>
<td>Služba</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Opravy chyb</td>
<td>Jsme provedli různé menšími opravami chyb pro služby HDInsight. Očekává se žádné změny chování zákazníkem.</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Poznámky k verzi 02/06/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SADA SDK NENÍ K DISPOZICI

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Opravy chyb</td>
<td>Jsme provedli různé menšími opravami chyb pro služby HDInsight. Očekává se žádné změny chování zákazníkem.</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Aktualizace softwaru HDP 2.1 údržby</td>
<td>K nasazení softwaru HDP 2.1.10.0 se aktualizuje HDInsight 3.1. Další informace najdete v tématu <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">poznámky k verzi softwaru HDP-2.1.10</a>. </td>
<td>Open-source softwaru</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Binární aktualizace softwaru HDP</td>
<td>V HBase pro souborů, které byly aktualizovány názvy nejsou několik souborů JAR. Tyto soubory JAR se používá interně HBase, takže neočekává se, že zákazníci jsou závislé na názvy těchto souborů JAR. Mezi ně patří:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Open-source softwaru</td>
<td>HBase</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Poznámky k verzi 1/29/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - beze změny)
* SADA SDK NENÍ K DISPOZICI

Tato verze obsahuje následující aktualizace:

<table border="1">

<tr>
<th>Název</th>
<th>Popis</th>
<th>Ovlivněné oblasti (například služby, komponenty nebo sady SDK)</p></th>
<th>Cluster typu (například Hadoop, HBase nebo Storm)</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Opravy chyb</td>
<td>Provedli jsme několik důležitých oprav chyb, které zvyšuje spolehlivost clusterů HDInsight při upgradech Azure.</td>
<td>Služba</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-152015-release-of-hdinsight"></a>Poznámky k verzi 1/5/2015 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - beze změny)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - beze změny)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - beze změny)

Tato verze obsahuje následující aktualizace:

<table border="1">

<tr>
<th>Název</th>
<th>Popis</th>
<th>Komponenta</th>
<th>Typ clusteru</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Ukázky pro analýzu trendů služby Twitter a doporučení na základě Mahout</td>
<td><p>V této verzi konzole dotazu HDInsight má dva další ukázky:</p>

<p><b>Analýza trendu služby Twitter.</b><br>
Veřejná rozhraní API poskytované lokality jako Twitter jsou užitečné zdroje dat pro analýzu a pochopení oblíbených trendy. V tomto kurzu zjistěte, jak získat seznam uživatelů Twitter, které odesílají většina tweetů obsahující určité slovo pomocí Hive. </p>

<p><b>Mahout film doporučení</b><br>
Apache Mahout je Apache Hadoop knihovnou machine learning. Mahout obsahuje algoritmy pro zpracování dat (jako je například filtrování, klasifikace a clustering). V tomto kurzu použijte modul doporučení pro generování doporučení na základě na filmy, které jste viděli vašich přátel.</p></td>
<td>Dotaz konzoly</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Změňte výchozí hodnotu pro konfiguraci Hive: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Tato velikost konfigurace platí pro spojení automaticky převedený mapy. Hodnota představuje součet hodnot velikosti tabulek, které lze převést na hash – mapy splňující v paměti. Tato hodnota se zvyšuje v předchozí verzi, z výchozí hodnotu 10 MB na 128 MB. Nová hodnota 128 MB však byla příčinou k selhání kvůli nedostatku paměti úlohy. Tato verze obnoví výchozí hodnota zpět do 10 MB. Zákazníci stále mohou přepsat tuto hodnotu během vytváření clusteru daného jejich dotazy a tabulky velikosti. Další informace o toto nastavení a jak ji přepsat najdete v tématu <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">optimalizovat automatické připojení k převodu</a> v Hortonworks dokumentaci. </p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Poznámky k verzi 12, 23/2014 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight jsou:

* HDInsight 2.1.10.420.1246783 (beze změny softwaru HDP verze)
* HDInsight 3.0.6.420.1246783 (beze změny softwaru HDP verze)
* HDInsight 3.1.1.420.1246783 (beze změny softwaru HDP verze)

Tato verze obsahuje následující aktualizace:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Komponenta</th>
<th>Typ clusteru</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Chyby při vytváření clusteru přerušované z důvodu nadměrného zatížení</td>
<td><p>Vylepšený algoritmus pro stahování softwaru HDP balíčky během vytváření clusteru umožňuje robustnější zpracování chyb z důvodu nadměrného zatížení.</p></td>
<td>Služba</td>
<td>Hadoop, Hbase, Storm</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Poznámky k verzi 12/18/2014 HDInsight
Tato verze obsahuje aktualizaci následující součásti:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Komponenta</th>
<th>Typ clusteru</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Přizpůsobení obecné dostupnosti clusteru</a></td>
<td><p>Přizpůsobení poskytuje možnost můžete přizpůsobit clusterům Azure HDInsight s projekty, které jsou dostupné v ekosystému Apache Hadoop. Pomocí této nové funkce můžete experimentovat a nasadit do Azure HDInsight Hadoop projekty. Tato možnost je povolena prostřednictvím **akce skriptu** funkci, která lze upravit clusterů systému Hadoop v libovolné způsoby pomocí vlastních skriptů. Toto vlastní nastavení je k dispozici na všech typech clusterů HDInsight, včetně Hadoop, HBase a Storm. K předvedení power tuto funkci, budeme mít zdokumentovaný proces instalace oblíbených <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>, a <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a>moduly. Tato verze přidává také pro zákazníky na jejich vlastní skript akce prostřednictvím portálu Azure poskytuje pokyny a osvědčené postupy o tom, jak vytvořit vlastní skript akce s použitím pomocné metody a poskytuje pokyny o tom, jak otestovat skript akce. </p></td>
<td>Funkce obecné dostupnosti</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Poznámky k verzi 12/05/2014 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight jsou:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK není k dispozici

Tato verze obsahuje následující aktualizace součástí:

<table border="1">
<tr>
<th>Název</th>
<th>Popis</th>
<th>Komponenta</th>
<th>Typ clusteru</th>
<th>JIRA (pokud existuje)</th>
</tr>
<tr>
<td>Oprava chyby: občasných chyb při přidávání velkého počtu oddílů s tabulkou Hive knihovny DLL </td>
<td><p>Pokud dojde k chybě nepřerušované připojení k databázi metaúložiště Hive při přidávání spoustu oddíly pro tabulku Hive, může selhat Hive DDL. Následující příkaz isseen v protokolu chyb Hive, pokud dojde k této chybě: </p><p>"Chyba [hlavní]: ql. Ovladač (SessionState.java:printError(547)) - se nezdařilo: Chyba spuštění, návratový kód 1 z org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction byla volána ale openTransactionCalls = 0. To pravděpodobně znamená, že se nevyváženou volání openTransaction/commitTransaction)"</p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>HIVE-482 (Toto je interní JIRA, takže nemůže být externě uvozovkách. Jsou tady uvedené pro referenci.)</td>
</tr>
<tr>
<td>Oprava chyby: příležitostně zablokování v konzole nástroje HDInsight dotazu</td>
<td>V takovém případě se následující příkaz zobrazí v WebHCat protokolu pro Spouštěče úloh WebHCat: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Nepodařilo se načíst soubor historie {wasb url k souboru historie} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>HIVE-482 (Toto je interní JIRA, takže nemůže být externě uvozovkách. Jsou tady uvedené pro referenci.)</td>
</tr>
<tr>
<td>Oprava chyby: příležitostně Špička latence Hbase dotazů</td>
<td>Pokud k tomu dojde, uživatelé všimnete příležitostně Špička 3 sekund v latenci, Hbase dotazů. </td>
<td>HDInsight Cluster brány</td>
<td>HBase</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>HDP JAR změny názvu souboru</td>
<td>HDI clusteru verze 3.0, existuje několik změny interní souborů JAR ve HDP nainstalovaná. jetty 6.1.26.jar se nahradil údajem jetty 6.1.26.hwx.jar. jetty. util 6.1.26.jar se nahradil údajem jetty. util 6.1.26.hwx.jar. Tyto změny použít na Hadoop, Mahout, WebHCat a Oozie projekty.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-11212014-release-of-hdinsight"></a>Poznámky k verzi 11/21/2014 HDInsight
Čísla plná verze pro nasazení v této verzi clustery služby HDInsight jsou:

* HDInsight 2.1.9.382.1169709 (žádná změna 11/14/2014)
* HDInsight 3.0.5.382.1169709 (žádná změna 11/14/2014)
* HDInsight 3.1.1.382.1169709 (žádná změna 11/14/2014)
* HDInsight SDK 1.4.0

Tato verze obsahuje následující aktualizace součástí:

<table border="1">
<tr><th>Název</th><th>Popis</th><th>Komponenta</th><th>Typ clusteru</th><th>JIRA (pokud existuje)</th></tr>
<tr>
<td>Přístupem protokoly aplikací</td>
<td>Umožňuje prostřednictvím kódu programu výčet aplikací, které byly spuštěny v clusterech služby a stáhnout relevantní specifické pro aplikaci nebo konkrétní kontejner protokolů, které pomáhají ladit problematické aplikace.</td>
<td>Sada SDK</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Umožňuje zadat název oblasti v IHdInsightClient.DeleteCluster </td>
<td>Azure HDInsight SDK poskytuje možnost určit název oblasti je při použití **DeleteCluster**. To pomáhá odblokovat zákazníky, kteří měli dva prostředky se stejným názvem v různých oblastech a nelze odstranit, buď z nich.</td>
<td>Sada SDK</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>ClusterDetails.DeploymentId</td>
<td>**ClusterDetails** objektu vrátí **DeploymentID** pole, která představuje jedinečný identifikátor pro cluster. Je zaručeno být jedinečný v rámci pokusy o vytvoření clusteru se stejnými názvy.</td>
<td>Sada SDK</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Poznámky k verzi 11/14/2014 HDInsight

Čísla plná verze pro nasazení v této verzi clustery služby HDInsight jsou:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Tato verze obsahuje následující nové funkce, součást aktualizace a opravy chyb.

<table border="1">
<tr><th>Název</th><th>Popis</th><th>Komponenta</th><th>Typ clusteru</th><th>JIRA (pokud existuje)</th></tr>
<tr>
<td>Akce skriptu (Preview)</td>
<td>Verze Preview funkci přizpůsobení clusteru, která umožňuje úpravy Hadoop clusterů v libovolné způsoby pomocí vlastních skriptů. Uživatelé s touto funkcí můžete experimentovat s a nasadit projekty, které jsou dostupné v ekosystému Apache Hadoop ke clusterům Azure HDInsight. Tato vlastní nastavení funkce je k dispozici na všech typech clusterů HDInsight, včetně Hadoop, HBase a Storm.</td>
<td>Nová funkce</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Předkompilované úlohy pro Azure weby a úložiště analýzy protokolů</td>
<td>Konzole dotazu HDInsight má Galerie Začínáme, který podporuje řešení, které fungují na vaše data nebo na ukázková data.
<p>**Řešení, které můžete pracovat s daty**:<br>
Vytvořili jsme úlohy pro některé z nejběžnějších scénářů analýzy dat poskytnout výchozí bod pro vytvoření vlastní řešení. Data s úlohou vám pomůže zjistit, jak funguje. Potom pomocí až budete připravení, co jste se naučili vytvářet řešení, která je Modelováno podle předem úlohy.</p>
<p>**Řešení, které fungují na ukázková data**:<br>
Naučte se pracovat s HDInsight pomocí s návodem některé základní scénáře (třeba analýza webových protokolů a data snímačů). Zjistíte, jak používat službu HDInsight k analýze taková data a jak je možné připojit další aplikace a služby k těmto datům. Vizualizace dat připojením k Microsoft Excel poskytuje příklad napájení tohoto přístupu.</p></td>
<td>Dotaz konzoly</td>
<td>Hadoop</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>Oprava nevracení paměti v Templeton</td>
<td>Nevracení paměti v Templeton ovlivňující zákazníci, kteří měli dlouho běžící clustery nebo byly odeslání 100s úlohy požadavky, že druhý byl vyřešen. Restartování služby se problém označované jako Templeton 5xx chyb a alternativní řešení. Alternativní řešení se už nepotřebuje.</td>
<td>Templeton</td>
<td>Všechny</td>
<td>https://issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>

> [!NOTE]
> K předvedení nové funkce, které jsou zpřístupněny přizpůsobení clusteru, byly popsány postupy, pomocí akce skriptu instalace modulů Spark a R na clusteru. Další informace naleznete v tématu:

* [Nainstalovat a používat Spark 1.0 v HDInsight clustery](hdinsight-hadoop-spark-install.md)
* [Na nainstalovat a používat R clusterů systému HDInsight Hadoop](hdinsight-hadoop-r-scripts.md)

## <a name="notes-for-11072014-release-of-hdinsight"></a>Poznámky k verzi 11/07/2014 HDInsight

Čísla plná verze pro clustery HDInsight, které nasazené v této verzi jsou:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Tato verze obsahuje následující aktualizace součástí:

<table border="1">
<tr><th>Název</th><th>Popis</th><th>Komponenta</th><th>Typ clusteru</th><th>JIRA (pokud existuje)</th></tr>
<tr>
<td>HDP 2.1.7</td>
<td>Tato verze je založena na Hortonworks Data Platform (HDP) 2.1.7. Další informace najdete v tématu <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">poznámky k verzi pro HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>YARN časová osa serveru</td>
<td>Ve výchozím nastavení je povoleno serveru časová osa YARN (také označované jako obecná aplikace historie serveru). Časová osa Server poskytuje obecné informace o dokončené aplikace (například ID aplikace, název aplikace, stav aplikace, aplikace odeslání čas a čas dokončení aplikace).

Tyto informace o aplikaci lze načíst z hlavního uzlu přístupu k identifikátoru URI http://headnodehost:8188 nebo spuštěním příkazu YARN: yarn aplikace – seznam - appStates všechny.

Tyto informace můžete také načíst vzdáleně když rozhraní REST API na https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Podrobnější informace najdete v části <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN časová osa serveru</a>.</td>
<td>Služby, YARN</td>
<td>Hadoop, HBase</td>
<td>Není k dispozici</td>
</tr>
<tr>
<td>ID nasazení clusteru</td>
<td>Od verze 1.3.3.1.5426.29232 sady SDK, mohou uživatelé přistupovat jedinečné ID pro každý cluster, který je vydán HDInsight. To umožňuje zákazníkům zjistěte jedinečné instance clusterů, když znovu použít název DNS napříč vytvořit nebo vyřadit scénáře.</td>
<td>Sada SDK</td>
<td>Všechny</td>
<td>Není k dispozici</td>
</tr>
</table>

> [!NOTE]
> V této verzi byl opraven chyb, která zabránila číslo plnou verzi z zobrazovat na portálu nebo nevrátila pomocí sady SDK nebo pomocí prostředí Windows PowerShell.

## <a name="notes-for-10152014-release"></a>Poznámky k verzi 15/10/2014

Tato oprava hotfix verze opravil nevracení paměti v Templeton ovlivňující velkou uživatelé Templeton. V některých případech by uživatelé, kteří provést Templeton výraznou najdete v části chyby označované jako 500 kódy chyb, protože žádosti nebude k dispozici dostatek paměti ke spuštění. Alternativní řešení tohoto problému se restartovat službu Templeton. Tento problém byl opraven.

## <a name="notes-for-1072014-release"></a>Poznámky k verzi 10/7/2014

* Při použití Ambari koncový bod, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *název_hostitele* vrátí pole plně kvalifikovaný název domény (FQDN) uzlu místo pouze název hostitele. Například místo vrácení "**headnode0**", můžete získat plně kvalifikovaný název domény"**headnode0. { ClusterDNS} gt; .azurehdinsight .net**". Tato změna vyžaduje usnadnění scénáře, kde je možné nasadit více typů clusteru (například HBase a Hadoop) v jednu virtuální síť. K tomu dojde, například při použití HBase jako back-end platformy pro Hadoop.

* Uvádíme nové nastavení paměti pro výchozí nasazení clusteru HDInsight. Předchozí nastavení paměti výchozí nezohledňuje adekvátní pokyny pro počet jader procesoru nasazuje. Tato nová nastavení paměti by měl poskytovat lepší výchozí hodnoty (podle doporučení Hortonworks). Tyto změnit, naleznete v dokumentaci k sadě SDK reference o změně konfigurace clusteru. Nové nastavení paměti, které jsou používány clusteru HDInsight výchozí 4 procesoru jádra (8 kontejner) je uvedeno v následující tabulce. (Hodnoty používané před touto verzí najdete taky parenthetically.)

<table border="1">
<tr><th>Komponenta</th><th>Přidělení paměti</th></tr>
<tr><td> yarn.Scheduler.minimum přidělení</td><td>768 MB (dříve 512 MB)</td></tr>
<tr><td> yarn.Scheduler.maximum přidělení</td><td>6144 MB (beze změny)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (beze změny)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (dříve 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>požádá =-Xmx512m (dříve - Xmx410m)</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1536 MB (dříve 1 024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>požádá =-Xmx1024m (dříve - Xmx819m)</td></tr>
<tr><td>yarn.app.mapreduce.am.Resource</td><td>768 MB (dříve 1 024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.am.Command</td><td>požádá =-Xmx512m (dříve - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.Sort</td><td>256 MB (dříve 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (beze změny)</td></tr>
</table>

Další informace o nastavení konfigurace paměti používané YARN a MapReduce na softwaru Hortonworks Data Platform, který se používá v prostředí HDInsight najdete v tématu [určit nastavení konfigurace paměti HDP](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks také poskytuje nástroj k výpočtu nastavení správné paměti.

Ohledně prostředí Azure PowerShell a chybovou zprávou HDInsight SDK: "*clusteru není nakonfigurovaný pro přístup ke službám HTTP*":

* Tato chyba je známá [potíže s kompatibilitou](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , které mohou nastat kvůli rozdílu ve verzi sady SDK HDInsight nebo Azure PowerShell a verzi clusteru. Clustery jsou vytvořené na 8/15 nebo novější je podpora nová funkce zřizování do virtuální sítě. Ale tato funkce není správně interpretovat starší verze sady SDK HDInsight nebo Azure PowerShell. Výsledkem je chyba v některé operace odeslání úlohy. Pokud používáte rozhraní API sady SDK HDInsight nebo Azure PowerShell rutiny (**použití AzureRmHDInsightCluster** nebo **Invoke-AzureRmHDInsightHiveJob**) k odesílání úloh, tyto operace může selhat s chybovou zprávou " *Cluster <clustername> není nakonfigurovaná pro přístup ke službám HTTP*. " Nebo (v závislosti na operaci), může se zobrazit další chybové zprávy, jako například "*nemůže připojit ke clusteru*".
* Tyto problémy s kompatibilitou jsou vyřešené v nejnovější verze sady SDK HDInsight a prostředí Azure PowerShell. Doporučujeme aktualizovat sady SDK HDInsight verze 1.3.1.6 nebo novější a Azure PowerShell nástroje pro verzi 0.8.8 nebo novější. Získáte přístup na nejnovější SDK HDInsight z [Nuget](http://nuget.codeplex.com/wikipage?title=Getting%20Started) a nástroje Azure PowerShell v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Poznámky k verzi HDInsight 3.1 na 12/9/2014
* Tato verze je založena na Hortonworks Data Platform (HDP) 2.1.5. Seznam opravených v této verzi najdete v tématu [pevné v této verzi](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) na webu Hortonworks.
* Ve složce knihovny Pig byl změněn soubor "avro mapred 1.7.4.jar" na "avro-mapred-1.7.4-hadoop2.jar." Obsah tohoto souboru obsahuje menší oprava chyb, který je pevné. Doporučuje se, že zákazníci neprovádějte přímé závislost na název na soubor JAR, abyste se vyhnuli zalomení při přejmenování souborů.

## <a name="notes-for-8212014-release"></a>Poznámky k verzi 8/21/2014
* Přidáváme následující konfigurace WebHCat (HIVE-7155), která nastaví výchozí limit paměti pro úlohu řadiče Templeton na 1 GB. (Předchozí výchozí hodnota je 512 MB.)

     templeton.Mapper.Memory.MB (= 1024)

  * Tato změna řeší s některé dotazy Hive z důvodu nižší limity paměti k následující chybě: "Kontejneru běží mimo hranice fyzické paměti."
  * Můžete obnovit na původní výchozí nastavení, můžete tuto hodnotu můžete nastavit konfiguraci až 512 pomocí prostředí Azure PowerShell v okamžiku vytvoření clusteru pomocí následujícího příkazu:

      Přidat AzureRmHDInsightConfigValues – základní @{"templeton.mapper.memory.mb"="512";}
* Název hostitele roli zookeeper byl změněn na *zookeeper*. Tato akce ovlivní překlad v rámci clusteru, ale neovlivní externí rozhraní REST API. Pokud máte součásti, které používají *zookeepernode* název hostitele, je potřeba aktualizovat, aby uživatelé používali nový název. Nové názvy pro tři uzly zookeeper jsou:

  * zookeeper0
  * zookeeper1
  * zookeeper2
* Matici podpory verze HBase se aktualizuje. Pro produkční prostředí pro úlohy HBase je podporována pouze HDInsight verze 3.1 (HBase verze 0,98). Verze 3.0 (která byla k dispozici pro verzi preview) nepodporuje dál přesunutí.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Poznámky o clusterech vytvořené před 8, 15 nebo 2014
Chybová zpráva Azure PowerShell nebo sady SDK HDInsight, "clusteru <clustername> není nakonfigurovaná pro přístup ke službám HTTP" (nebo v závislosti na operaci, další chybové zprávy, jako: "Nelze se připojit ke clusteru") mohou být zjištěny kvůli rozdílu verze mezi prostředí Azure PowerShell nebo sady SDK HDInsight a cluster. Clustery jsou vytvořené na 8/15 nebo novější je podpora nová funkce zřizování do virtuální sítě. Tato funkce není správně interpretovat starších verzí prostředí Azure PowerShell nebo sady SDK HDInsight, což vede k selhání operace odeslání úlohy. Pokud používáte rozhraní API sady SDK HDInsight nebo Azure PowerShell rutiny (například použití AzureRmHDInsightCluster nebo Invoke-AzureRmHDInsightHiveJob) k odesílání úloh, tyto operace může selhat s některá z chybových zpráv popsaných.

Tyto problémy s kompatibilitou jsou vyřešené v nejnovější verze sady SDK HDInsight a prostředí Azure PowerShell. Doporučujeme aktualizovat sady SDK HDInsight verze 1.3.1.6 nebo novější a Azure PowerShell nástroje pro verzi 0.8.8 nebo novější. Získáte přístup na nejnovější SDK HDInsight z [NuGet][nuget-link]. Máte přístup k prostředí PowerShell nástroje Azure pomocí [instalačního programu webové platformy Microsoft][webpi-link].

## <a name="notes-for-7282014-release"></a>Poznámky k verzi 7/28/2014
* **HDInsight k dispozici v nové oblasti**: jsme rozbalit zeměpisném přítomnosti HDInsight tři oblasti. Zákazníci HDInsight vytvářet clustery v těchto oblastech:
  * Východní Asie
  * Střed USA – sever
  * Střed USA – jih
* HDInsight verze 1.6 (HDP 1.1 a Hadoop 1.0.3) a HDInsight verze 2.1 (HDP1.3 a Hadoop 1.2) jsou odebírán z portálu Azure. Můžete vytvořit clustery Hadoop pro tyto verze pomocí rutiny prostředí Azure PowerShell [New-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) nebo pomocí [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).
* Hortonworks Data Platform (HDP) změny v této verzi:

<table border="1">
<tr><th>HDP</th><th>Změny</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Žádné změny</td></tr>
<tr><td>HDP 2.0 NEBO HDI 3.0</td><td>Žádné změny</td></tr>
<tr><td>HDP 2.1 NEBO HDI 3.1</td><td>zookeeper: [3.4.5.2.1.3.0-1948] -> [3.4.5.2.1.3.2-0002]</td></tr>
</table>

## <a name="notes-for-6242014-release"></a>Poznámky k verzi 6/24/2014
Tato verze obsahuje vylepšení služby HDInsight:

* **HDP 2.1 dostupnosti**: HDInsight 3.1 (který obsahuje HDP 2.1) je obecně k dispozici a je výchozí verze pro nové clustery.
* **HBase - Azure portálu vylepšení**: vydáváme clustery HBase k dispozici ve verzi Preview. Clustery HBase můžete vytvořit z portálu pomocí několika kliknutí.

S HBase můžete vytvořit různé úlohy v reálném čase v HDInsight, z interaktivní weby, které pracují s rozsáhlými datovými sadami pro ukládání dat snímačů a telemetrie z milionů koncových bodů služby. Dalším krokem bude analyzovat data v těchto úloh pomocí úloh Hadoop, a to lze provést v prostředí HDInsight pomocí Azure PowerShell a řídicí panel clusteru Hive.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout předinstalován v HDInsight 3.1
 [Mahout](http://hortonworks.com/hadoop/mahout/) je předem nainstalovaná na clusterů systému HDInsight 3.1 Hadoop, takže Mahout úlohy bez nutnosti konfigurace další clusteru můžete spustit. Například můžete vzdáleného do clusteru Hadoop pomocí protokolu RDP (Remote Desktop) a bez dalších kroků, můžete spustit následující příkaz Hello World Mahout:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Podrobnější vysvětlení tohoto postupu, naleznete v dokumentaci k [Breiman příklad](https://mahout.apache.org/users/classification/breiman-example.html) na Apache Mahout webu.

### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Dotazy Hive v HDInsight 3.1 můžete použít Tez
Je k dispozici v HDInsight 3.1 Hive 0,13 a je schopen spuštění dotazů pomocí Tez, které můžete využít pro zlepšení výkonu podstatné.
Tez není povoleno ve výchozím nastavení pro dotazy Hive. Pokud chcete použít, musí vyjádřit výslovný souhlas. Tez můžete povolit spuštěním následující fragment kódu:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks publikovali podrobné rozpis vylepšení výkonu dotazu Hive s Tez dodaným v standardní srovnávacích testů. Podrobnosti najdete v tématu [srovnávací testy Hive 13 Apache pro Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Další informace najdete v tématu [Hive na Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

### <a name="global-availability"></a>Globální dostupností
Verze služby HDInsight v Hadoop 2.2, společnost Microsoft vyvinula HDInsight k dispozici ve všech hlavních geografických, kde je Azure k dispozici. Konkrétně západní Evropa a jihovýchodní Asie datových centrech mít byla uvést do režimu online. To umožňuje zákazníkům najít clustery v datovém centru, které je zavřít a potenciálně zónu podobné požadavky na dodržování předpisů.

### <a name="dos--donts-between-cluster-versions"></a>DOS & proti mezi verzemi clusteru
**Použít s clusteru služby HDInsight 3.1 metaúložiště Oozie nejsou zpětně kompatibilní s clustery HDInsight 2.1 a nelze je použít tuto předchozí verzi**.

Vlastní databázi metaúložiště Oozie nasazené se službou cluster služby HDInsight 3.1 nemůže znovu pomocí clusteru služby HDInsight 2.1. Toto je tak i v případě, že metaúložiště vytvořena pomocí clusteru služby HDInsight 2.1. Tento scénář není podporován, protože schéma metaúložiště získá upgradovat, je-li použít s clusteru služby HDInsight 3.1, již není kompatibilní s metaúložiště vyžaduje clustery HDInsight 2.1. Pokusy o opakovaně používat metaúložiště Oozie, kterého se cluster služby HDInsight 3.1 vykreslit clusteru HDInsight 2.1 nepoužitelné.

**Metaúložiště Oozie se nedají sdílet mezi clustery.**

Metaúložiště Oozie jsou připojené k určité clustery a jejich se nedají sdílet mezi clustery.

### <a name="breaking-changes"></a>Nejnovější změny
**Předpony syntaxe**: pouze "wasb: / /" v HDInsight 3.1 a 3.0 clustery je podporovaná syntaxe. Starší "asv: / /" v HDInsight 2.1 a 1.6 clustery je podporovaná syntaxe, ale není podporována v HDInsight 3.1 nebo 3.0 clustery. To znamená, že všechny úlohy, odeslané do HDInsight verze 3.1 nebo 3.0 clusteru to explicitně používá "asv: / /" syntaxe se nezdaří. "Wasb: / /" místo toho použita syntaxe. Také úlohy, odeslané do jakékoli HDInsight 3.1 nebo 3.0 clustery, které jsou vytvořeny pomocí existujícího metaúložiště, které obsahuje explicitní odkazy na prostředky, pomocí "asv: / /" syntaxe se nezdaří. Tato metaúložiště musí znovu vytvořit pomocí "wasb: / /" syntaxe pro adresování prostředků.

**Porty**: porty používané službou HDInsight změnily. Čísla portů, které byly používány byly v rámci rozsah dočasných portů operačního systému Windows. Porty jsou automaticky přiděleny z předdefinovaného rozsahu dočasných pro krátkodobou internetové komunikace založené na protokolu. Novou sadu povolených čísla portů služby Hortonworks Data Platform (HDP) jsou mimo tento rozsah, aby se zabránilo zjištění konfliktu, které mohou nastat u portů používaných pro služby spuštěné na hlavního uzlu. Na nová čísla portů by nemělo způsobit změny ukončování řádků. Čísla používaná jsou následující:

 **HDInsight 1.6 (HDP 1.1)**

<table border="1">
<tr><th>Name (Název)</th><th>Hodnota</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

 **HDInsight 3.1 a 3.0 (HDP 2.1 a 2.0)**

<table border="1">
<tr><th>Name (Název)</th><th>Hodnota</th></tr>
<tr><td>Adresa DFS.namenode.http</td><td>namenodehost:30070</td></tr>
<tr><td>Adresa DFS.namenode.HTTPS</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>Adresa DFS.namenode.Secondary.http</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

### <a name="dependencies"></a>Závislosti
V prostředí HDInsight byly přidány následující závislosti 3.x (HDP2.x):

* guice servlet
* optiq jádra
* javax.inject
* Aktivace
* jsr305
* geronimo-jaspic_1.0_spec
* července slf4j
* Java xmlbuilder
* ant
* Commons kompilátoru
* jdo-api
* Commons math3
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* Jersey servlet
* Commons exec
* jaxb-api
* jetty. všechna serveru
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase – vlastnosti
* groovy-all
* hamcrest jádra
* E-mailu
* linq4j
* jpam
* Jersey klienta
* aopalliance
* geronimo-annotation_1.0_spec
* Spouštěč ant
* Jersey guice
* XML – rozhraní API
* stax-api
* asm commons
* asm stromu
* wadl
* geronimo-jta_1.1_spec
* guice
* všechny leveldbjni
* rychlosti postupu
* rychlého vypuštění
* Tenhle java
* všechny jetty
* Commons dbcp

Tyto závislosti již existují v HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* břečťan
* aspectjrt
* JSON
* jádro
* jdo2-api
* avro mapred
* datanucleus vůni
* JSP
* rozhraní api Commons protokolování
* matematické Commons
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* Tenhle

### <a name="version-changes"></a>Změny verze
Mezi HDInsight byly provedeny následující změny verze 2.x (HDP1.x) a prostředí HDInsight 3.x (HDP2.x):

* základní metriky: [2.1.2] -> [3.0.0]
* derbynet: [10.4.2.0] -> [10.10.1.1]
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* jasper kompilátoru: [5.5.12] -> [5.5.23]
* log4j: ['1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: [10.4.2.0] -> [10.10.1.1]
* httpcore: [4.2.4] -> [4.2.5]
* hsqldb: [1.8.0.10] -> [2.0.0]
* jets3t: [0.6.1] -> [0.9.0]
* protobuf java: [2.4.1] -> [2.5.0]
* Derby: [10.4.2.0] -> [10.10.1.1]
* jasper: ['runtime-5.5.12'] -> ['runtime-5.5.23']
* Démon Commons: [1.0.1] -> [1.0.13]
* základní datanucleus: [3.0.9] -> [3.2.10]
* datanucleus-api-jdo: [3.0.7] -> [3.2.6]
* zookeeper: [3.4.5.1.3.9.0-01320] -> [3.4.5.2.1.3.0-1948]
* bonecp: [0.7.1.RELEASE] -> ['
* 0.8.0.RELEASE']

### <a name="drivers"></a>Ovladače
Připojení k databázi v jazyce Java (JDBC) ovladač pro systém SQL Server používá se interně pomocí HDInsight a nepoužívá pro externí operace. Pokud chcete pro připojení k HDInsight pomocí připojení ODBC (Open Database), použijte ovladač Microsoft Hive ODBC. Další informace najdete v tématu [připojení aplikace Excel do HDInsight pomocí ovladače ODBC Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md).

### <a name="bug-fixes"></a>Opravy chyb
V této verzi jsme se aktualizovat, v následujících verzích HDInsight s několik oprav chyb:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)

## <a name="hortonworks-release-notes"></a>Poznámky k verzi Hortonworks
Poznámky k verzi pro Hortonworks Data platformy (HDPs) používané clustery HDInsight verze jsou k dispozici v následujících umístěních:

* HDInsight verze 3.1 používá distribuce Hadoop, která je založena na [softwaru Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Toto je výchozí clusteru Hadoop vytvoří, když pomocí portálu Azure po 11/7/2014. Clustery HDInsight 3.1 vytvořené před 11/7/2014 jsou založené na [softwaru Hortonworks Data Platform 2.1.1][hdp-2-1-1]
* HDInsight verze 3.0 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight verze 2.1 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight verze 1.6 používá distribuce Hadoop, která je založena na [Hortonworks Data Platform 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
