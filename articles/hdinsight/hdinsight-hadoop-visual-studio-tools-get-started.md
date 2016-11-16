---
title: "Naučte se používat nástroje Data Lake (HDInsight) Tools pro Visual Studio | Dokumentace Microsoftu"
description: "Naučte se instalovat a používat nástroje Data Lake (HDInsight) Tools pro Visual Studio pro připojení ke clusteru Hadoop a spuštění dotazu Hive."
keywords: "nástroje hadoop,dotaz hive,visual studio"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8bd214c166190248662634d488d8e7b1427b5738


---
# <a name="get-started-using-azure-data-lake-hdinsight-tools-for-visual-studio-to-run-a-hive-query"></a>Začněte používat nástroje Azure Data Lake (HDInsight) Tools pro Visual Studio ke spuštění dotazu Hive
Naučte se používat nástroje Data Lake (HDInsight) Tools pro Visual Studio k připojení ke clusterům HDInsight a odesílání dotazů Hive. Další informace o používání HDInsight naleznete v části [Úvod do HDInsight][hdinsight.introduction] a [Začínáme s HDInsight][hdinsight.get.started]. Další informace o připojování ke clusteru Storm naleznete v tématu [Vývoj C# topologií pro Apache Storm v HDInsight pomocí sady Visual Studio][hdinsight.storm.visual.studio.tools].

Nástroje Data Lake Tools pro Visual Studio můžete použít pro přístup k Data Lake Analytics i HDInsight.  Informace o nástrojích Data Lake Tools najdete v tématu [Kurz: Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Požadavky**

Pro dokončení tohoto kurzu a používání nástrojů Data Lake Tools v sadě Visual Studio budete potřebovat:

* Cluster služby Azure HDInsight: cluster založený na systému Linux nebo Windows bude pracovat s kroky v tomto dokumentu. Viz jedna z následujících informací týkajících se vytvoření clusteru:
  
  * [Začněte používat HDInsight s Linuxem](hdinsight-hadoop-linux-tutorial-get-started.md)
  * [Začněte používat HDInsight s Windows](hdinsight-hadoop-tutorial-get-started-windows.md)
* Pracovní stanice s následující softwarem:
  
  * Windows 8.1, Windows 8 nebo Windows 7
  * Visual Studio (jedna z následujících verzí):
    
    * Visual Studio 2013 Community/Professional/Premium/Ultimate s [aktualizací 4](https://www.microsoft.com/download/details.aspx?id=44921)
    * Visual Studio 2015 (Community/Enterprise)
    
    > [!NOTE]
    > Nástroje Data Lake Tools pro Visual Studio jsou v současnosti dostupné jenom v angličtině.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalace nástrojů Data Lake Tools pro Visual Studio
Data Lake Tools můžete nainstalovat pomocí [Instalace webové platformy](https://www.microsoft.com/web/downloads/). Je nutné zvolit ten, který se shoduje s vaší verzí sady Visual Studio. Pokud nemáte nainstalované Visual Studio, můžete nainstalovat poslední verzi nástroje Visual Studio Community a Azure SDK pomocí [Instalace webové platformy](https://www.microsoft.com/web/downloads/):

![Instalace webové platformy pro Data Lake Tools pro Visual Studio][1]

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
4. Zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **Přihlásit**. Tento postup je nezbytný pouze v případě, že jste se ještě nikdy nepřipojili k předplatnému Azure ze sady Visual Studio na této pracovní stanici.
5. V Průzkumníku serveru se zobrazí seznam stávajících clusterů HDInsight. Pokud nemáte žádné clustery, můžete nějaký vytvořit pomocí webu Azure Portal, Azure PowerShellu nebo sady SDK HDInsight. Další informace najdete v tématu [Vytváření clusterů HDInsight][hdinsight-create-clusters].
   
   ![Seznam clusterů v Průzkumníku serveru pro Data Lake Tools pro Visual Studio][5]
6. Rozbalte cluster služby HDInsight. Zobrazí se **Databáze Hive**, výchozí účet úložiště, propojené účty úložiště a **protokol služby Hadoop**. Entity můžete dále rozšířit.

Po připojení k předplatnému Azure budete moci provádět následující akce:

**Připojení k portálu Azure Portal ze sady Visual Studio**

* Z Průzkumníka serveru rozbalte **Azure** > **HDInsight**, klikněte pravým tlačítkem na cluster HDInsight a pak klikněte na tlačítko **Spravovat cluster webu Azure Portal**.

**Kladení otázek a poskytování zpětné vazby ze sady Visual Studio**

* V nabídce **Nástroje** klikněte na tlačítko **HDInsight** a pak klikněte na tlačítko **Fórum MSDN** a pokládejte otázky, nebo klikněte na položku **Odeslat zpětnou vazbu**.

## <a name="navigate-the-linked-resources"></a>Procházejte propojené prostředky
V Průzkumníkovi serveru můžete zobrazit výchozí účet úložiště a všechny propojené účty úložiště. Pokud rozbalíte výchozí účet úložiště, uvidíte kontejnery na účtu úložiště. Jsou označeny jako výchozí účet úložiště a výchozí kontejner. Pro zobrazení obsahu můžete také kliknout pravým tlačítkem myši na kontejnery.

![Seznam clusterů v Průzkumníku serveru pro Data Lake Tools pro Visual Studio][2]

Po otevření kontejneru můžete k nahrání, odstranění a stažení objektů blob použít následující tlačítka:

![Operace s objekty blob v Průzkumníku serveru pro Data Lake Tools pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png)

## <a name="run-a-hive-query"></a>Spuštění dotazu Hive
[Apache Hive][apache.hive] je infrastruktura datového skladu postavená na Hadoop pro poskytování souhrnů dat, dotazů a analýz. Data Lake Tools pro Visual Studio podporují spouštění dotazů Hive ze sady Visual Studio. Další informace o Hive naleznete v tématu [Používání Hive s HDInsight][hdinsight.hive].

Otestování skriptu Hive proti clusteru služby HDInsight je časově náročné. Může to trvat několik minut nebo déle. Data Lake Tools pro Visual Studio dokáží ověřovat skript Hive místně bez připojení k živému clusteru.

Data Lake Tools pro Visual Studio také umožňují uživatelům zobrazit, co se nachází uvnitř úlohy Hive díky shromažďování a zpřístupnění protokolů YARN určitých úloh Hive.

### <a name="view-the-hivesampletable"></a>Zobrazení **hivesampletable**
Všechny clustery HDInsight se vzorovou tabulkou Hive se nazývají *hivesampletable*. Tuto tabulku použijeme, abychom vám zobrazit postup, jak zobrazit seznam tabulek Hive, zobrazit schémata tabulek a zobrazit seznam řádků v tabulce Hive.

**Postup zobrazení seznamu tabulek Hive a zobrazení schématu tabulek Hive**

1. V části **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight** > zvoleného clusteru > **Databáze Hive** > **Výchozí** > **hivesampletable** a zobrazte schéma tabulky.
2. Klikněte pravým tlačítkem na položku **hivesampletable** a pak kliknutím na tlačítko **Zobrazit prvních 100 řádků** zobrazíte seznam řádků. Jedná se o ekvivalent procesu spuštění následujícího dotazu Hive pomocí ovladače ODBC Hive:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Počet řádků můžete přizpůsobit.
   
   ![Data Lake Tools: dotaz schématu HDinsight Hive sady Visual Studio][6]

### <a name="create-hive-tables"></a>Vytváření tabulek Hive
Grafické uživatelské rozhraní můžete použít k vytvoření tabulky Hive nebo dotazů Hive. Informace o použití dotazů Hive naleznete v tématu [Spouštění dotazů Hive](#run.queries).

**Vytvoření tabulky Hive**

1. V nabídce **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight clustery** cluster služby HDInsight > **Databáze Hive**, klikněte pravým tlačítkem myši na položku **výchozí** a klikněte na tlačítko **Vytvořit tabulku**.
2. Konfigurace tabulky.
3. Klikněte na tlačítko **Vytvořit tabulku** a odešlete úlohu chcete-li vytvořit novou tabulku Hive.
   
    ![Data Lake Tools: nástroje HDInsight sady Visual Studio vytvoří tabulku Hive][7]

### <a name="a-namerunqueriesavalidate-and-run-hive-queries"></a><a name="run.queries"></a>Ověřování a spouštění dotazů Hive
Existují dva způsoby, jak vytvářet a spouštět dotazy Hive:

* Vytváření dotazů ad-hoc
* Vytvoření aplikace Hive

**Postup vytváření, ověřování a spouštění dotazů ad hoc**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **clustery HDInsight**.
2. Klikněte pravým tlačítkem na cluster, kde chcete spustit dotaz a pak klikněte na tlačítko **Napsat dotaz Hive**.
3. Zadejte dotazy Hive. Všimněte si, že editor Hive podporuje technologii IntelliSense. Data Lake Tools pro Visual Studio podporují načítání vzdálených metadat při úpravách skriptu Hive. Například když zadáte „VYBRAT*Z“ IntelliSense zobrazí seznam všech navrhovaných tabulek. Pokud zadáte název tabulky, zobrazí technologie IntelliSense seznam názvů sloupců. Nástroje podporují skoro všechny příkazy DML Hive, poddotazy a vestavěné UDF.
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio][13]
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio][14]
   
   > [!NOTE]
   > Navrhne pouze metadata clusterů, které jsou vybrán v panelu nástrojů HDInsight.
   > 
   > 
4. (Volitelné): klikněte na tlačítko **Ověření skriptu** a zkontrolujte chyby syntaxe skriptu.
   
    ![Data Lake Tools: místní ověřování Data Lake Tools pro Visual Studio][10]
5. Klikněte na tlačítko **Odeslat** nebo **Odeslat (rozšířené)**. Pomocí rozšířených možnosti nakonfigurujete **Název úlohy**, **Argumenty**, **Další konfigurace**, a **Stavový adresář** pro skript:
   
    ![Dotaz Hive v HDInsight Hadoop][9]
   
    Po odeslání úlohy se zobrazí okno **Souhrn úlohy Hive**.
   
    ![Souhrn dotazů HDInsight Hadoop Hive][8]
6. Pomocí tlačítka **Aktualizovat** aktualizujte stav, dokud se stav úlohy nezmění na **Dokončeno**.
7. Kliknutím na odkazy v dolní části zobrazíte následující: **úloha dotaz**, **výstup úlohy**, **protokol úloh** nebo **protokolu Yarn**.

**Vytvoření a spuštění řešení Hive**

1. V nabídce **Soubor** klikněte na tlačítko **Nový** a pak klikněte na tlačítko **Projekt**.
2. Vyberte možnost **HDInsight** v levém podokně, vyberte možnost **Aplikace Hive** v prostředním podokně, zadejte vlastnosti a pak klikněte na tlačítko **OK**.
   
    ![Data Lake Tools: nový projekt Hive v HDInsight sady Visual Studio][11]
3. V části **Průzkumník řešení** dvakrát klikněte na možnost **Script.hql** a otevřete ji.
4. Pro ověření skriptu Hive můžete kliknout na tlačítko **Ověření skriptu** nebo kliknout pravým tlačítkem na skript v editoru Hive a pak kliknout na tlačítko **Ověření skriptu** z kontextové nabídky.

### <a name="view-hive-jobs"></a>Zobrazení úloh Hive
Můžete zobrazit dotazy úlohy, výstup úlohy, protokoly úlohy a protokoly Yarn pro úlohy Hive. Další informace najdete v předchozím snímku obrazovky.

Nejnovější verze nástrojů vám umožní vidět, co je uvnitř vaší úlohy Hive, díky shromažďování a zpřístupnění protokolů YARN. Protokol YARN vám může pomoci prozkoumat problémy s výkonem. Další informace o tom, jak HDInsight shromažďuje protokoly YARN naleznete v části [Programový přístup do protokolů aplikace HDInsight ][hdinsight.access.application.logs].

**Zobrazení úloh Hive**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **HDInsight**.
2. Klikněte pravým tlačítkem cluster HDInsight a potom klikněte na tlačítko **Zobrazit úlohy**. Zobrazí se seznam úloh Hive, které byly spuštěny v clusteru.
3. Klikněte na úlohu v seznamu úloh, vyberte ji a pak použijte okno **Souhrn úlohy Hive** k otevření **Dotazu úlohy**, **Výstupu úlohy**, **Protokolu úloh** nebo **Protokolu Yarn**.
   
    ![Data Lake Tools: zobrazení úloh Hive v HDInsight Visual Studio Tools][12]

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Rychlejší cesta ke spouštění Hive prostřednictvím HiveServer2
> [!NOTE]
> Tato funkce funguje pouze na verzi clusteru HDInsight 3.2 a novější.
> 
> 

Data Lake Tools dříve odesílaly úlohy Hive prostřednictvím [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (také známé jako Templeton). Trvalo dlouhou dobu vrátit podrobnosti úlohy a informace o chybě.
Aby se tento problém s výkonem vyřešil, Data Lake Tools spouští úlohy Hive přímo v clusteru prostřednictvím HiveServer2 tak, aby obešly RDP/SSH.
Kromě lepšího výkonu mohou uživatelé také zobrazit Hive na grafech Tez a podrobnosti úlohy.

U clusteru HDInsight verze 3.2 nebo vyšší můžete vidět tlačítko **Spustit prostřednictvím HiveServer2**:

![Data Lake Visual Studio Tools spouští úlohy prostřednictvím HiveServer2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png)

A můžete si zobrazit protokoly vysílané datovým proudem v reálném čase a zobrazit grafy úloh, pokud se dotaz Hive spouští v Tez.

![Rychlá cesta spuštění Hive v Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png)

**Rozdíl mezi spouštěním dotazů prostřednictvím HiveServer2 a odesíláním dotazů prostřednictvím WebHCat**

I když má zpracování dotazů prostřednictvím HiveServer2 mnoho výkonnostních výhod, má několik omezení. Některá omezení nejsou vhodná pro produkční účely. V následující tabulce jsou uvedeny rozdíly:

|  | Spouštění prostřednictvím HiveServer2 | Odesílání prostřednictvím WebHCat |
| --- | --- | --- |
| Provádění dotazů |Eliminuje režijní náklady v WebHCat (které spustí úlohu MapReduce s názvem „TempletonControllerJob“). |Dokud je dotaz proveden prostřednictvím WebHCat, spustí WebHCat úlohu MapReduce, která zavádí další latence. |
| Zpětné protokolování datovými proudy |Téměř v reálném čase. |Protokoly spouštění úlohy jsou k dispozici pouze v případě, že je úloha dokončena. |
| Zobrazení historie úlohy |Pokud je dotaz proveden prostřednictvím HiveServer2, nezachová se jeho historie úloh (protokol úloh, výstup úloh). Aplikaci lze zobrazit v uživatelském rozhraní YARN s omezenými informacemi. |Pokud je dotaz proveden prostřednictvím WebHCat, jeho historie úloh (protokol úlohy, výstup úlohy) je zachována a lze ji zobrazit pomocí aplikace Visual Studio/HDInsight SDK/PowerShell. |
| Zavření okna |Spouštění prostřednictvím HiveServer2 je „synchronní“ způsob, takže je nutné zachovat otevřená okna. Pokud okna zavřete, spouštění dotazu se zruší. |Odesílání prostřednictvím WebHCat je „asynchronní“ způsob, aby bylo možné odeslat dotaz prostřednictvím WebHCat a zavřít aplikaci Visual Studio. Kdykoliv se můžete vrátit a zobrazit výsledky. |

### <a name="tez-hive-job-performance-graph"></a>Graf výkonu úlohy Tez Hive
Data Lake Tools podporují zobrazení grafů výkonu pro úlohy Hive spuštěné prostřednictvím modulu Tez. Informace o povolení Tez naleznete v tématu [Používání Hive v HDInsight][hdinsight.hive]. Po odeslání úlohy Hive v sadě Visual Studio zobrazí Visual Studio po dokončení úlohy graf.  Možná budete muset kliknout na tlačítko **Aktualizovat** a získat nejnovější stav úlohy.

> [!NOTE]
> Tato funkce je dostupná pouze pro verze clusteru HDInsight nad 3.2.4.593 a může fungovat pouze pro dokončené úlohy (pokud jste odeslali úlohu prostřednictvím WebHCat; tento grafu se zobrazí při spuštění dotazu prostřednictvím HiveServer2). Tento postup funguje pro clustery založené na Windows i Linuxu.
> 
> 

![graf výkonu hadoop hive tez](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

Abychom vám pomohli lépe pochopit dotaz Hive, nástroje v této verzi přidávají zobrazení Operátoru Hive. Stačí dvakrát kliknout na vrcholy grafu úlohy a zobrazí se všechny operátory uvnitř vrcholu. Můžete také najet na konkrétní operátor a zobrazit další podrobnosti tohoto operátoru.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Zobrazení spouštění úlohy pro Hive na úlohách Tez
Zobrazení spouštění úlohy pro Hive na úlohách Tez lze použít k získání strukturovaných a vizualizovaných informací pro úlohy Hive a získání dalších podrobností úlohy. Pokud dojde k problémům s výkonem, můžete zobrazení použít k získání dalších podrobností. Například způsobu, jakým úloha pracuje a podrobných informací o jednotlivých úlohách (čtení a zápis dat, plánovaný/počáteční/koncový čas atd.) tak, abyste mohli vyladit úlohu konfigurace nebo architekturu systému na základě vizualizovaných informací.

![Zobrazení spouštění úlohy v Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png)

## <a name="run-pig-scripts"></a>Spouštění skriptů Pig
Data Lake Tools pro Visual Studio podporují vytváření a odesílání skriptů Pig do clusterů HDInsight. Uživatelé mohou vytvořit projekt Pig ze šablony a pak odeslat skript do clusteru HDInsight.

## <a name="feedbacks-known-issues"></a>Názory a známé problémy
* Aktuálně jsou výsledky HiveServer2 zobrazeny čistě textovým způsobem, což není ideální. Pracujeme na řešení.
* Pokud výsledky začínají s hodnotami NULL, momentálně se nezobrazí. Tento problém jsme opravili a pokud budete tímto problémem blokováni, neváhejte nám zaslat e-mail nebo se obrátit na tým podpory.
* Skript HQL vytvořených pomocí Visual Studia je zakódován v závislosti na nastavení pro místní oblast uživatele. Nemusí se správně spustit, pokud uživatel nahraje skript do clusteru jako binární.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak se připojit ke clusterům HDInsight ze sady Visual Studio, používat balíček Data Lake (HDInsight) Tools a spustit dotaz Hive. Další informace naleznete v tématu:

* [Používání Hive Hadoop v HDInsight][hdinsight.hive]
* [Začínáme používat Hadoop v HDInsight][hdinsight.get.started]
* [Odesílání úloh Hadoop v HDInsight][hdinsight.submit.jobs]
* [Analýza dat Twitteru pomocí Hadoop v HDInsight][hdinsight.analyze.twitter.data]

<!--Anchors-->
[Instalace]: #installation
[Připojení k předplatnému Azure]: #connect-to-your-azure-subscription
[Procházení propojených prostředků]: #navigate-the-linked-resources
[Spuštění dotazů Hive]: #run-hive-queries
[Další kroky]: #next-steps

<!--Image references-->
[1]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png
[2]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png
[5]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png
[6]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png
[7]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png
[8]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png
[9]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png
[10]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png
[11]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png
[12]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png
[13]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png
[14]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png


<!--Link references-->
[hdinsight-create-clusters]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight.introduction]: hdinsight-hadoop-introduction.md
[hdinsight.get.started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight.hive]: hdinsight-use-hive.md
[hdinsight.submit.jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org



<!--HONumber=Nov16_HO2-->


