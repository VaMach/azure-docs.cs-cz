---
title: "Připojení ke službě Azure HDInsight pomocí nástrojů Data Lake pro Visual Studio | Dokumentace Microsoftu"
description: "Naučte se instalovat a používat nástroje Data Lake pro Visual Studio pro připojení ke clusterům Hadoop ve službě Azure HDInsight a spouštění dotazů Hive."
keywords: hadoop tools, hive query, visual studio, visual studio hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: jgao
ms.openlocfilehash: 1e9d5ca475424c99b30c62252f4b0abc9bd09078
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="connect-to-azure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Připojení ke službě Azure HDInsight a spouštění dotazů Hive pomocí nástrojů Data Lake pro Visual Studio

Naučte se používat nástroje Data Lake pro Visual Studio (označují se také jako nástroje Azure Data Lake nebo Stream Analytics) pro připojení ke clusterům Hadoop ve službě [Azure HDInsight](../hdinsight-hadoop-introduction.md) a odesílání dotazů Hive. Další informace o používání služby HDInsight najdete v tématech [Úvod do služby HDInsight](../hdinsight-hadoop-introduction.md) a [Začínáme se službou HDInsight](apache-hadoop-linux-tutorial-get-started.md). Další informace o připojování ke clusteru Storm najdete v tématu [Vývoj topologií C# Storm pomocí sady HDInsight Tools pro Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Nástroje Data Lake Tools pro Visual Studio můžete použít pro přístup k Data Lake Analytics i HDInsight.  Informace o nástrojích Data Lake Tools najdete v tématu [Kurz: Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Požadavky**

Pro dokončení tohoto kurzu a používání nástrojů Data Lake v sadě Visual Studio potřebujete následující položky:

* Cluster Azure HDInsight: Informace o jeho vytvoření najdete v tématu [Začínáme používat Hadoop ve službě Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Pokud se chcete naučit používat interaktivní dotazy Hive, potřebujete cluster [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).
* Pracovní stanici se sadou Visual Studio 2013, 2015 nebo 2017.
    
    > [!NOTE]
    > Nástroje Data Lake Tools pro Visual Studio jsou v současnosti dostupné jenom v angličtině.
    > 
    > 

## <a name="install-and-upgrade-data-lake-tools-for-visual-studio"></a>Instalace a upgrade nástrojů Data Lake pro Visual Studio

Data Lake Tools se pro sadu Visual Studio 2017 instalují ve výchozím nastavení. Pro starší verze sady Visual Studio je můžete nainstalovat pomocí [Instalace webové platformy](https://www.microsoft.com/web/downloads/). Je nutné zvolit ten, který se shoduje s vaší verzí sady Visual Studio. Pokud nemáte nainstalované Visual Studio, můžete nainstalovat poslední verzi nástroje Visual Studio Community a Azure SDK pomocí [Instalace webové platformy](https://www.microsoft.com/web/downloads/):

![Instalace webové platformy pro Data Lake Tools pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Instalace nástrojů Data Lake pro Visual Studio pomocí instalace webové platformy")

**Aktualizace nástrojů**
1. Otevřete sadu Visual Studio.
2. V nabídce **Nástroje** klikněte na **Rozšíření a aktualizace**.
3. Rozbalte **Aktualizace** a aktualizujte **Nástroje Azure Data Lake a Stream Analytics**, pokud existují.

> [!NOTE]
>
> Připojení ke clusterům Interactive Query a spouštění interaktivních dotazů Hive podporují pouze verze 2.3.0.0 nebo novější.

## <a name="connect-to-azure-subscriptions"></a>Připojení k předplatným služby Azure
Data Lake Tools pro Visual Studio umožňují připojení ke clusterům HDInsight, provádění některých operací základní správy a spouštění dotazů Hive.

> [!NOTE]
> Informace o připojení k obecnému clusteru Hadoop naleznete v tématu [Zapisování a odesílání dotazů Hive pomocí sady Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**Postup připojení k předplatnému Azure**

1. Otevřete sadu Visual Studio.
2. V nabídce **Zobrazení** klikněte na tlačítko **Průzkumník serveru** a otevřete okno Průzkumníka serveru.
3. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.
   
   > [!NOTE]
   > Všimněte si, že by se mělo otevřít okno **Seznam úloh HDInsight**. Pokud ho nevidíte, klikněte na tlačítko **Ostatní okna** v nabídce **Zobrazení** a pak klikněte na tlačítko **Okno seznam úloh HDInsight**.  
   > 
   > 
4. Zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **Přihlásit**. Ověření je nezbytné pouze v případě, že jste se ještě nikdy nepřipojili k předplatnému Azure ze sady Visual Studio na této pracovní stanici.
5. V Průzkumníku serveru se zobrazí seznam stávajících clusterů HDInsight. Pokud nemáte žádné clustery, můžete nějaký vytvořit pomocí webu Azure Portal, Azure PowerShellu nebo sady SDK HDInsight. Další informace najdete v tématu [Vytvoření clusterů HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Seznam clusterů v Průzkumníku serveru pro Data Lake pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Průzkumník serveru pro Data Lake pro Visual Studio")
6. Rozbalte cluster služby HDInsight. Zobrazí se **Databáze Hive**, výchozí účet úložiště, propojené účty úložiště a **protokol služby Hadoop**. Entity můžete dále rozšířit.

Po připojení k předplatnému Azure budete moci provádět následující úlohy:

**Připojení k portálu Azure Portal ze sady Visual Studio**

* Z Průzkumníka serveru rozbalte **Azure** > **HDInsight**, klikněte pravým tlačítkem na cluster HDInsight a pak klikněte na tlačítko **Spravovat cluster webu Azure Portal**.

**Kladení otázek a poskytování zpětné vazby ze sady Visual Studio**

* V nabídce **Nástroje** klikněte na tlačítko **HDInsight** a pak klikněte na tlačítko **Fórum MSDN** a pokládejte otázky, nebo klikněte na položku **Odeslat zpětnou vazbu**.

## <a name="navigate-the-linked-resources"></a>Procházejte propojené prostředky
V Průzkumníkovi serveru můžete zobrazit výchozí účet úložiště a všechny propojené účty úložiště. Pokud rozbalíte výchozí účet úložiště, uvidíte kontejnery na účtu úložiště. Jsou označeny jako výchozí účet úložiště a výchozí kontejner. Pro zobrazení obsahu můžete také kliknout pravým tlačítkem myši na kontejnery.

![Seznam propojených prostředků v Průzkumníku serveru pro Data Lake pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Seznam propojených prostředků")

Po otevření kontejneru můžete k nahrání, odstranění a stažení objektů blob použít následující tlačítka:

![Operace s objekty blob v Průzkumníku serveru pro Data Lake Tools pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Nahrání, odstranění a stažení objektů blob")

## <a name="run-interactive-hive-queries"></a>Spouštění interaktivních dotazů Hive
[Apache Hive](http://hive.apache.org) je infrastruktura datového skladu postavená na Hadoop pro poskytování souhrnů dat, dotazů a analýz. Data Lake Tools pro Visual Studio podporují spouštění dotazů Hive ze sady Visual Studio. Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight](hdinsight-use-hive.md).

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) využívá [Hive s funkcí LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) v Apache Hive 2.1 a přináší interaktivitu do složitých dotazů ve stylu datového skladu nad uloženými rozsáhlými datovými sadami. Spouštění dotazů Hive v Interactive Query je v porovnání s tradičními dávkovými úlohami Hive mnohem rychlejší.  Další informace o spouštění dávkových úloh Hive najdete v části [Spouštění dávkových úloh Hive](#run-hive-batch-jobs).

> [!note]
>
> Spouštění interaktivních dotazů Hive se podporuje pouze při připojení ke clusteru [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

Data Lake Tools pro Visual Studio také umožňují uživatelům zobrazit, co se nachází uvnitř úlohy Hive díky shromažďování a zpřístupnění protokolů YARN určitých úloh Hive.

### <a name="view-the-hivesampletable"></a>Zobrazení **hivesampletable**
Všechny clustery HDInsight se vzorovou tabulkou Hive se nazývají *hivesampletable*. Tato tabulka Hive slouží k názornému předvedení výpisu tabulek Hive, zobrazení schémat tabulek a výpisu řádků v tabulce Hive.

**Postup zobrazení seznamu tabulek Hive a zobrazení schématu tabulek Hive**

1. V části **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight** > zvoleného clusteru > **Databáze Hive** > **Výchozí** > **hivesampletable** a zobrazte schéma tabulky.
2. Klikněte pravým tlačítkem na položku **hivesampletable** a pak kliknutím na tlačítko **Zobrazit prvních 100 řádků** zobrazíte seznam řádků. Jedná se o ekvivalent procesu spuštění následujícího dotazu Hive pomocí ovladače ODBC Hive:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Počet řádků můžete přizpůsobit.
   
   ![Data Lake Tools: dotaz schématu HDinsight Hive sady Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Výsledky dotazu Hive")

### <a name="create-hive-tables"></a>Vytváření tabulek Hive
Grafické uživatelské rozhraní můžete použít k vytvoření tabulky Hive nebo dotazů Hive. Informace o použití dotazů Hive naleznete v tématu [Spouštění dotazů Hive](#run.queries).

**Vytvoření tabulky Hive**

1. V nabídce **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight clustery** cluster služby HDInsight > **Databáze Hive**, klikněte pravým tlačítkem myši na položku **výchozí** a klikněte na tlačítko **Vytvořit tabulku**.
2. Konfigurace tabulky.
3. Klikněte na tlačítko **Vytvořit tabulku** a odešlete úlohu chcete-li vytvořit novou tabulku Hive.
   
    ![Data Lake Tools: nástroje HDInsight sady Visual Studio vytvoří tabulku Hive](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Vytvoření tabulky Hive")

### <a name="run.queries"></a>Ověřování a spouštění dotazů Hive
Existují dva způsoby, jak vytvářet a spouštět dotazy Hive:

* Vytváření dotazů ad hoc
* Vytvoření aplikace Hive

**Vytváření, ověřování a spouštění dotazů ad hoc**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **clustery HDInsight**.
2. Klikněte pravým tlačítkem na cluster, kde chcete spustit dotaz a pak klikněte na tlačítko **Napsat dotaz Hive**.
3. Zadejte dotazy Hive. Všimněte si, že editor Hive podporuje technologii IntelliSense. Data Lake Tools pro Visual Studio podporují načítání vzdálených metadat při úpravách skriptu Hive. Například když zadáte „VYBRAT*Z“ IntelliSense zobrazí seznam všech navrhovaných tabulek. Pokud zadáte název tabulky, zobrazí technologie IntelliSense seznam názvů sloupců. Nástroje podporují skoro všechny příkazy DML Hive, poddotazy a vestavěné UDF.
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Navrhnou se pouze metadata clusteru vybraného na panelu nástrojů služby HDInsight.
   > 
   > 
4. (Volitelné) Klikněte na tlačítko **Ověření skriptu** a zkontrolujte chyby syntaxe skriptu.
   
    ![Data Lake Tools: místní ověřování Data Lake Tools pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Ověření skriptu")
5. Klikněte na tlačítko **Odeslat** nebo **Odeslat (rozšířené)**. Pomocí rozšířených možnosti odeslání nakonfigurujete **Název úlohy**, **Argumenty**, **Další konfigurace** a **Stavový adresář** pro skript:
   
    ![Dotaz Hive v HDInsight Hadoop](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Odeslání dotazů")
   
    Po odeslání úlohy se zobrazí okno **Souhrn úlohy Hive**.
   
    ![Souhrn dotazu Hive v HDInsight Hadoop](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Souhrn úlohy Hive")
6. Pomocí tlačítka **Aktualizovat** aktualizujte stav, dokud se stav úlohy nezmění na **Dokončeno**.
7. Kliknutím na odkazy v dolní části můžete zobrazit: **Dotaz úlohy**, **Výstup úlohy**, **Protokol úloh** nebo **Protokol Yarn**.

**Vytvoření a spuštění řešení Hive**

1. V nabídce **Soubor** klikněte na tlačítko **Nový** a pak klikněte na tlačítko **Projekt**.
2. Vyberte možnost **HDInsight** v levém podokně, vyberte možnost **Aplikace Hive** v prostředním podokně, zadejte vlastnosti a pak klikněte na tlačítko **OK**.
   
    ![Data Lake Tools: nový projekt Hive v HDInsight sady Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Vytvoření aplikací Hive v sadě Visual Studio")
3. V části **Průzkumník řešení** dvakrát klikněte na možnost **Script.hql** a otevřete ji.
4. Pro ověření skriptu Hive můžete kliknout na tlačítko **Ověření skriptu** nebo kliknout pravým tlačítkem na skript v editoru Hive a pak kliknout na tlačítko **Ověření skriptu** z kontextové nabídky.

### <a name="view-hive-jobs"></a>Zobrazení úloh Hive
Můžete zobrazit dotazy úlohy, výstup úlohy, protokoly úlohy a protokoly Yarn pro úlohy Hive. Další informace najdete v předchozím snímku obrazovky.

Nejnovější verze nástrojů vám umožní vidět, co je uvnitř vaší úlohy Hive, díky shromažďování a zpřístupnění protokolů YARN. Protokol YARN vám může pomoci prozkoumat problémy s výkonem. Další informace o tom, jak služba HDInsight shromažďuje protokoly YARN, najdete v tématu [Programový přístup k protokolům aplikací v prostředí HDInsight](../hdinsight-hadoop-access-yarn-app-logs.md).

**Zobrazení úloh Hive**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **HDInsight**.
2. Klikněte pravým tlačítkem cluster HDInsight a potom klikněte na tlačítko **Zobrazit úlohy**. Zobrazí se seznam úloh Hive, které byly spuštěny v clusteru.
3. Klikněte na úlohu v seznamu úloh, vyberte ji a pak použijte okno **Souhrn úlohy Hive** k otevření **Dotazu úlohy**, **Výstupu úlohy**, **Protokolu úloh** nebo **Protokolu Yarn**.
   
    ![Data Lake Tools: zobrazení úloh Hive v HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Zobrazení úloh Hive")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Rychlejší cesta ke spouštění Hive prostřednictvím HiveServer2
> [!NOTE]
> Tato funkce funguje pouze na verzi clusteru HDInsight 3.2 a novější.
> 
> 

Data Lake Tools dříve odesílaly úlohy Hive prostřednictvím [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (také známé jako Templeton). Trvalo dlouhou dobu vrátit podrobnosti úlohy a informace o chybě.
Aby se tento problém s výkonem vyřešil, Data Lake Tools spouští úlohy Hive přímo v clusteru prostřednictvím HiveServer2 tak, aby obešly RDP/SSH.
Kromě lepšího výkonu mohou uživatelé také zobrazit Hive na grafech Tez a podrobnosti úlohy.

U clusteru HDInsight verze 3.2 nebo vyšší můžete vidět tlačítko **Spustit prostřednictvím HiveServer2**:

![Data Lake Visual Studio Tools spouští úlohy prostřednictvím HiveServer2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Spouštění dotazů Hive pomocí HiveServer2")

Zároveň můžete vidět protokoly streamované v reálném čase zpět a grafy úloh, pokud se dotaz Hive spouští v Tez.

![Rychlá cesta spuštění Hive v Data Lake Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Zobrazení grafů úloh")

**Rozdíl mezi spouštěním dotazů prostřednictvím HiveServer2 a odesíláním dotazů prostřednictvím WebHCat**

I když má zpracování dotazů prostřednictvím HiveServer2 mnoho výkonnostních výhod, má několik omezení. Některá omezení nejsou vhodná pro produkční účely. V následující tabulce jsou uvedeny rozdíly:

|  | Spouštění prostřednictvím HiveServer2 | Odesílání prostřednictvím WebHCat |
| --- | --- | --- |
| Provádění dotazů |Eliminuje režijní náklady v WebHCat (které spustí úlohu MapReduce s názvem „TempletonControllerJob“). |Pokud se dotaz provádí prostřednictvím WebHCat, spustí WebHCat úlohu MapReduce, která zavádí další latenci. |
| Zpětné protokolování datovými proudy |Téměř v reálném čase. |Protokoly spouštění úlohy jsou k dispozici pouze v případě, že je úloha dokončena. |
| Zobrazení historie úlohy |Pokud je dotaz proveden prostřednictvím HiveServer2, nezachová se jeho historie úloh (protokol úloh, výstup úloh). Aplikaci lze zobrazit v uživatelském rozhraní YARN s omezenými informacemi. |Pokud se dotaz provádí prostřednictvím WebHCat, jeho historie úloh (protokol úloh, výstup úloh) je zachována a lze ji zobrazit pomocí sady Visual Studio, sady HDInsight SDK nebo PowerShellu. |
| Zavření okna |Spouštění prostřednictvím HiveServer2 je „synchronní“ způsob, takže je nutné nechat okna otevřená. Pokud okna zavřete, provádění dotazu se zruší. |Odesílání prostřednictvím WebHCat je „asynchronní“ způsob, aby bylo možné odeslat dotaz prostřednictvím WebHCat a zavřít aplikaci Visual Studio. Kdykoliv se můžete vrátit a zobrazit výsledky. |

### <a name="tez-hive-job-performance-graph"></a>Graf výkonu úlohy Tez Hive
Data Lake Tools podporují zobrazení grafů výkonu pro úlohy Hive spuštěné prostřednictvím modulu Tez. Informace o povolení Tez najdete v tématu [Používání Hive ve službě HDInsight](hdinsight-use-hive.md). Po odeslání úlohy Hive v sadě Visual Studio zobrazí Visual Studio po dokončení úlohy graf.  Možná budete muset kliknout na tlačítko **Aktualizovat** a získat nejnovější stav úlohy.

> [!NOTE]
> Tato funkce je dostupná pouze pro verze clusteru HDInsight nad 3.2.4.593 a může fungovat pouze pro dokončené úlohy (pokud jste odeslali úlohu prostřednictvím WebHCat; tento graf se zobrazí při spuštění dotazu prostřednictvím HiveServer2). 
> 
> 

![graf výkonu hadoop hive tez](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Stav úlohy")

Abychom vám pomohli lépe pochopit dotaz Hive, nástroje v této verzi přidávají zobrazení Operátoru Hive. Stačí dvakrát kliknout na vrcholy grafu úlohy a zobrazí se všechny operátory uvnitř vrcholu. Můžete také najet na konkrétní operátor a zobrazit další podrobnosti tohoto operátoru.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Zobrazení spouštění úlohy pro Hive na úlohách Tez
Zobrazení spouštění úlohy pro Hive na úlohách Tez lze použít k získání strukturovaných a vizualizovaných informací pro úlohy Hive a získání dalších podrobností úlohy. Pokud dojde k problémům s výkonem, můžete zobrazení použít k získání dalších podrobností. Například způsobu, jakým úloha pracuje a podrobných informací o jednotlivých úlohách (čtení a zápis dat, plánovaný/počáteční/koncový čas atd.) tak, abyste mohli vyladit úlohu konfigurace nebo architekturu systému na základě vizualizovaných informací.

![Zobrazení spouštění úlohy v nástrojích Data Lake pro Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Zobrazení spuštění úlohy")

## <a name="run-hive-batch-jobs"></a>Spouštění dávkových úloh Hive
[Apache Hive](http://hive.apache.org) je infrastruktura datového skladu postavená na Hadoop pro poskytování souhrnů dat, dotazů a analýz. Data Lake Tools pro Visual Studio podporují spouštění dotazů Hive ze sady Visual Studio. Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight](hdinsight-use-hive.md).

Testování skriptu Hive na clusteru HDInsight je časově náročné, s výjimkou clusteru Interactive Query. Může to trvat několik minut nebo déle. Data Lake Tools pro Visual Studio dokáží ověřovat skript Hive místně bez připojení k živému clusteru. Další informace o spouštění interaktivních dotazů najdete v části [Spouštění interaktivních dotazů Hive](#run-interactive-hive-queries).

Data Lake Tools pro Visual Studio také umožňují uživatelům zobrazit, co se nachází uvnitř úlohy Hive díky shromažďování a zpřístupnění protokolů YARN určitých úloh Hive.

Další informace o spouštění dávkových úloh Hive najdete v části [Spouštění interaktivních dotazů Hive](#run-interactive-hive-queries). Informace v této části se týkají spouštění déletrvajících dávkových úloh Hive.

## <a name="run-pig-scripts"></a>Spouštění skriptů Pig
Data Lake Tools pro Visual Studio podporují vytváření a odesílání skriptů Pig do clusterů HDInsight. Uživatelé mohou vytvořit projekt Pig ze šablony a pak odeslat skript do clusteru HDInsight.

## <a name="feedbacks--known-issues"></a>Názory a známé problémy
* Aktuálně se výsledky HiveServer2 zobrazují čistě textovým způsobem, což není ideální. Microsoft pracuje na řešení.
* Pokud výsledky začínají s hodnotami NULL, momentálně se nezobrazí. Tento problém jsme opravili a pokud budete tímto problémem blokováni, kontaktujte tým podpory.
* Skript HQL vytvořených pomocí Visual Studia je zakódován v závislosti na nastavení pro místní oblast uživatele. Nemusí se správně spustit, pokud uživatel nahraje skript do clusteru jako binární.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak se připojit ke clusterům HDInsight ze sady Visual Studio, používat balíček Data Lake (HDInsight) Tools a spustit dotaz Hive. Další informace naleznete v tématu:

* [Použití Hadoop Hive ve službě HDInsight](hdinsight-use-hive.md)
* [Začínáme používat Hadoop ve službě HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Odesílání úloh Hadoop do služby HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Analýza dat Twitteru pomocí softwaru Hadoop ve službě HDInsight](../hdinsight-analyze-twitter-data.md)

