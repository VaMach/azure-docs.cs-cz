<properties
    pageTitle="Naučte se používat nástroje Visual Studio Hadoop pro HDInsight | Microsoft Azure"
    description="Naučte se instalovat a používat nástroje Visual Studio Hadoop pro HDInsight pro připojení ke clusteru Hadoop a spouštění dotazů Hive."
    keywords="hadoop tools,hive query,visual studio"
    services="HDInsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="05/25/2016"
    ms.author="jgao"/>

# Začněte používat nástroje Visual Studio Hadoop pro HDInsight ke spouštění dotazů Hive

Naučte se používat nástroje HDInsight pro Visual Studio k připojení ke clusterům HDInsight a odesílání dotazů Hive. Další informace o používání HDInsight naleznete v části [Úvod do HDInsight][hdinsight.introduction] a [Začínáme s HDInsight][hdinsight.get.started]. Další informace o připojování ke clusteru Storm naleznete v tématu [Vývoj C# topologií pro Apache Storm v HDInsight pomocí sady Visual Studio][hdinsight.storm.visual.studio.tools].

**Předpoklady**

Pro dokončení tohoto kurzu a používání nástrojů Hadoop v sadě Visual Studio budete potřebovat následující:

- Cluster služby Azure HDInsight: cluster založený na systému Linux nebo Windows bude pracovat s kroky v tomto dokumentu. Viz jedna z následujících informací týkajících se vytvoření clusteru:

    - [Začněte používat HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
    - [Začněte používat HDInsight se systémem Windows](hdinsight-hadoop-tutorial-get-started-windows.md)

- Pracovní stanice s následující softwarem:

    - Windows 8.1, Windows 8 nebo Windows 7
    - Visual Studio (jedna z následujících verzí):
        - Visual Studio 2013 Community/Professional/Premium/Ultimate s [aktualizací 4](https://www.microsoft.com/download/details.aspx?id=44921)
        - Visual Studio 2015 (Community/Enterprise)

    >[AZURE.NOTE] Nástroje HDInsight pro Visual Studio jsou v současné době dostupné pouze v angličtině.


## Instalace nástrojů HDInsight pro Visual Studio

Nástroje HDInsight pro Visual Studio a ovladač ODBC Microsoft Hive se dodávají s Microsoft Azure SDK pro rozhraní .NET verze 2.5.1 nebo novější. Můžete ho nainstalovat pomocí [Instalačního programu webové platformy](http://go.microsoft.com/fwlink/?LinkId=255386). Je nutné zvolit ten, který se shoduje s vaší verzí sady Visual Studio. Pokud nemáte nainstalované Visual Studio, můžete nainstalovat poslední verzi nástroje Visual Studio Community a Azure SDK pomocí [Instalačního programu webové platformy](http://go.microsoft.com/fwlink/?LinkId=255386) nebo pomocí následujících odkazů:

- [Visual Studio Community 2015 s Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2015CommunityAzurePack.appids)
- [Visual Studio Community 2013 s Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2013CommunityAzurePack.appids)
- [Microsoft Azure SDK pro .NET (VS 2015)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2015AzurePack.appids)
- [Microsoft Azure SDK pro .NET (VS 2013)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2013AzurePack.appids)

![Nástroje Hadoop: instalační program nástrojů HDinsight pro webovou platformu Visual Studia.][1]

## Připojení k předplatným služby Azure
Nástroje HDInsight pro Visual Studio umožňují připojení ke clusterům HDInsight, provádění některých operací základní správy a spouštění dotazů Hive.

>[AZURE.NOTE] Informace o připojení k obecnému clusteru Hadoop naleznete v tématu [Zapisování a odesílání dotazů Hive pomocí sady Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).


**Postup připojení k předplatnému služby Azure**

1.  Otevřete sadu Visual Studio.
2.  V nabídce **Zobrazení** klikněte na tlačítko **Průzkumník serveru** a otevřete okno Průzkumníka serveru.
3.  Rozbalte položku **Azure** a pak rozbalte **HDInsight**.

    >[AZURE.NOTE]Všimněte si, že by se mělo otevřít okno **Seznam úloh HDInsight**. Pokud ho nevidíte, klikněte na tlačítko **Ostatní okna** v nabídce **Zobrazení** a pak klikněte na tlačítko **Okno seznam úloh HDInsight**.  
4.  Zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **Přihlásit**. Tento postup je nezbytný pouze v případě, že jste se ještě nikdy nepřipojili k předplatnému Azure ze sady Visual Studio na této pracovní stanici.
5.  V Průzkumníku serveru se zobrazí seznam stávajících clusterů HDInsight. Pokud nemáte žádné clustery, můžete jeden vytvořit pomocí portálu Azure, Azure PowerShell nebo sady SDK HDInsight. Další informace naleznete v tématu [Vytváření clusterů HDInsight][hdinsight-provision].

    ![Nástroje Hadoop: nástroje HDInsight pro seznam clusterů Průzkumníka serveru Visual Studia][5]
6.  Rozbalte cluster služby HDInsight. Zobrazí se **Databáze Hive**, výchozí účet úložiště, propojené účty úložiště a **protokol služby Hadoop**. Entity můžete dále rozšířit.

Po připojení k předplatnému Azure budete moci provádět následující akce:

**Pro připojení k portálu Azure ze sady Visual Studio**

- Z Průzkumníka serveru rozbalte **Azure** > **HDInsight**, klikněte pravým tlačítkem na cluster HDInsight a pak klikněte na tlačítko **Spravovat Cluster portálu Azure**.

**Ze sady Visual Studio můžete klást otázky a poskytovat zpětnou vazbu**

- V nabídce **Nástroje** klikněte na tlačítko **HDInsight** a pak klikněte na tlačítko **Fórum MSDN** a pokládejte otázky, nebo klikněte na položku **Odeslat zpětnou vazbu**.

## Procházejte propojené prostředky

V Průzkumníkovi serveru můžete zobrazit výchozí účet úložiště a všechny propojené účty úložiště. Pokud rozbalíte výchozí účet úložiště, uvidíte kontejnery na účtu úložiště. Jsou označeny jako výchozí účet úložiště a výchozí kontejner. Pro zobrazení obsahu můžete také kliknout pravým tlačítkem myši na kontejnery.

![Nástroje HDInsight pro seznam clusterů Průzkumníka serveru Visual Studia][2]

## Spuštění dotazu Hive
[Apache Hive][apache.hive] je infrastruktura datového skladu postavená na Hadoop pro poskytování souhrnů dat, dotazů a analýz. Nástroje HDInsight pro Visual Studio podporují spouštění dotazů Hive ze sady Visual Studio. Další informace o Hive naleznete v tématu [Používání Hive s HDInsight][hdinsight.hive].

Otestování skriptu Hive proti clusteru služby HDInsight je časově náročné. Může to trvat několik minut nebo déle. Nástroje HDInsight pro Visual Studio jsou schopny ověřování skriptu místně bez připojení k živému clusteru.

Nástroje HDInsight pro Visual Studio také umožňují uživatelům zobrazit, co se nachází uvnitř úlohy Hive díky shromažďování a zpřístupnění protokolů YARN určitých úloh Hive.

### Zobrazení **hivesampletable**
Všechny clustery HDInsight se vzorovou tabulkou Hive se nazývají *hivesampletable*. Tuto tabulku použijeme, abychom vám zobrazit postup, jak zobrazit seznam tabulek Hive, zobrazit schémata tabulek a zobrazit seznam řádků v tabulce Hive.



**Postup zobrazení seznamu tabulek Hive a zobrazení schématu tabulek Hive**

1.  V části **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight** > zvoleného clusteru > **Databáze Hive** > **Výchozí** > **hivesampletable** a zobrazte schéma tabulky.
4.  Klikněte pravým tlačítkem na položku **hivesampletable** a pak kliknutím na tlačítko **Zobrazit prvních 100 řádků** zobrazíte seznam řádků. Jedná se o ekvivalent procesu spuštění následujícího dotazu Hive pomocí ovladače ODBC Hive:

        SELECT * FROM hivesampletable LIMIT 100

    Počet řádků můžete přizpůsobit.

    ![Nástroje Hadoop: dotaz schématu HDinsight Hive sady Visual Studio][6]

### Vytváření tabulek Hive

Grafické uživatelské rozhraní můžete použít k vytvoření tabulky Hive nebo dotazů Hive. Informace o použití dotazů Hive naleznete v tématu [Spouštění dotazů Hive](#run.queries).

**Chcete-li vytvořit tabulku Hive**

1. V nabídce **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight clustery** cluster služby HDInsight > **Databáze Hive**, klikněte pravým tlačítkem myši na položku **výchozí** a klikněte na tlačítko **Vytvořit tabulku**.
2. Konfigurace tabulky.
3. Klikněte na tlačítko **Vytvořit tabulku** a odešlete úlohu chcete-li vytvořit novou tabulku Hive.

    ![Nástroje Hadoop: nástroje hdinsight sady visual studio vytvoří tabulku hive][7]

### <a name="run.queries"></a>Ověřování a spouštění dotazů Hive
Existují dva způsoby, jak vytvářet a spouštět dotazy Hive:

- Vytváření dotazů ad-hoc
- Vytvoření aplikace Hive

**Postup vytváření, ověřování a spouštění dotazů ad hoc**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **clustery HDInsight**.
2. Klikněte pravým tlačítkem na cluster, kde chcete spustit dotaz a pak klikněte na tlačítko **Napsat dotaz Hive**.
3. Zadejte dotazy Hive. Všimněte si, že editor Hive podporuje technologii IntelliSense. Nástroje HDInsight pro Visual Studio podporují načítání vzdálených metadat při úpravách skriptu Hive. Například když zadáte „VYBRAT*Z“ IntelliSense zobrazí seznam všech navrhovaných tabulek. Pokud zadáte název tabulky, zobrazí technologie IntelliSense seznam názvů sloupců. Nástroj podporuje téměř všechny příkazy DML Hive, poddotazy a vestavěné UDF.

    ![Nástroje Hadoop: Nástroje HDInsight Visual Studio IntelliSense][13]

    ![Nástroje Hadoop: Nástroje HDInsight Visual Studio IntelliSense][14]

    > [AZURE.NOTE] Navrhne pouze metadata clusterů, které jsou vybrán v panelu nástrojů HDInsight.
4. (Volitelné): klikněte na tlačítko **Ověření skriptu** a zkontrolujte chyby syntaxe skriptu.

    ![Nástroje Hadoop: nástroje hdinsight pro místní ověřování Visual Studia][10]

4. Klikněte na tlačítko **Odeslat** nebo **Odeslat (rozšířené)**. Pomocí rozšířených možnosti nakonfigurujete **Název úlohy**, **Argumenty**, **Další konfigurace**, a **Stavový adresář** pro skript:

    ![hdinsight hadoop hive dotaz][9]

    Po odeslání úlohy se zobrazí okno **Souhrn úlohy Hive**.

    ![Souhrn dotazů HDInsight Hadoop Hive][8]
5. Pomocí tlačítka **Aktualizovat** aktualizujte stav, dokud se stav úlohy nezmění na **Dokončeno**.
6. Kliknutím na odkazy v dolní části zobrazíte následující: **úloha dotaz**, **výstup úlohy**, **protokol úloh** nebo **protokolu Yarn**.



**Vytvoření a spuštění řešení Hive**

1. V nabídce **Soubor** klikněte na tlačítko **Nový** a pak klikněte na tlačítko **Projekt**.
2. Vyberte možnost **HDInsight** v levém podokně, vyberte možnost **Aplikace Hive** v prostředním podokně, zadejte vlastnosti a pak klikněte na tlačítko **OK**.

    ![Nástroje Hadoop: nový projekt hive nástroje hdinsight sady visual studio][11]
3. V části **Průzkumník řešení** dvakrát klikněte na možnost **Script.hql** a otevřete ji.
4. Pro ověření skriptu Hive můžete kliknout na tlačítko **Ověření skriptu** nebo kliknout pravým tlačítkem na skript v editoru Hive a pak kliknout na tlačítko **Ověření skriptu** z kontextové nabídky.


### Zobrazení úloh Hive
Můžete zobrazit dotazy úlohy, výstup úlohy, protokoly úlohy a protokoly Yarn pro úlohy Hive. Další informace najdete v předchozím snímku obrazovky.

Nejnovější verze nástroje vám umožní vidět co je uvnitř vaší úlohy Hive díky shromažďování a zpřístupnění protokolů YARN. Protokol YARN vám může pomoci prozkoumat problémy s výkonem. Další informace o tom, jak HDInsight shromažďuje protokoly YARN naleznete v části [Programový přístup do protokolů aplikace HDInsight ][hdinsight.access.application.logs].

**Zobrazení úloh Hive**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **HDInsight**.
2. Klikněte pravým tlačítkem cluster HDInsight a potom klikněte na tlačítko **Zobrazit úlohy**. Zobrazí se seznam úloh Hive, které byly spuštěny v clusteru.
3. Klikněte na úlohu v seznamu úloh, vyberte ji a pak použijte okno **Souhrn úlohy Hive** k otevření **Dotazu úlohy**, **Výstupu úlohy**, **Protokolu úloh** nebo **Protokolu Yarn**.

    ![Nástroje Hadoop: Zobrazení úloh Hive v nástrojích HDInsight Visual Studio][12]

### Rychlejší cesta ke spouštění Hive prostřednictvím HiveServer2

>[AZURE.NOTE] Tato funkce funguje pouze na verzi clusteru HDInsight 3.2 a novější.

Nástroje HDInsight se používají k odeslání úloh Hive prostřednictvím [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (také známé jako Templeton). Trvalo dlouhou dobu vrátit podrobnosti úlohy a informace o chybě.
Chcete-li vyřešit tento problém s výkonem, nástroje HDInsight spustí úlohy Hive přímo v clusteru prostřednictvím HiveServer2 tak, aby obešly RDP/SSH.
Kromě lepšího výkonu mohou uživatelé také zobrazit Hive na grafech Tez a podrobnosti úlohy.

U clusteru HDInsight verze 3.2 nebo vyšší můžete vidět tlačítko **Spustit prostřednictvím HiveServer2**:

![spuštění nástrojů sady visual studio hdinsight prostřednictvím hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png)

A můžete si zobrazit protokoly vysílané datovým proudem ve skutečném čase a zobrazit grafy úloh, pokud se dotaz Hive spouští v Tez.

![rychlá cesta spuštění nástrojů visual studio hdinsight hive](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png)

**Rozdíl mezi spouštěním dotazů prostřednictvím HiveServer2 a odesíláním dotazů prostřednictvím WebHCat**

I když má zpracování dotazů prostřednictvím HiveServer2 mnoho výkonnostních výhod, má několik omezení. Některá omezení nejsou vhodná pro produkční účely. V následující tabulce jsou uvedeny rozdíly:

| |Spouštění prostřednictvím HiveServer2 |Odesílání prostřednictvím WebHCat|
|---|---|---|
|Provádění dotazů|Eliminuje režijní náklady v WebHCat (které spustí úlohu MapReduce s názvem „TempletonControllerJob“).|Dokud je dotaz proveden prostřednictvím WebHCat, spustí WebHCat úlohu MapReduce, která zavádí další latence.|
|Zpětné protokolování datovými proudy|Téměř v reálném čase.|Protokoly spouštění úlohy jsou k dispozici pouze v případě, že je úloha dokončena.|
|Zobrazení historie úlohy|Pokud je dotaz proveden prostřednictvím HiveServer2, není zachována jeho historie úlohy (protokol úlohy, výstup úlohy). Aplikaci lze zobrazit v uživatelském rozhraní YARN s omezenými informacemi.|Pokud je dotaz proveden prostřednictvím WebHCat, jeho historie úloh (protokol úlohy, výstup úlohy) je zachována a lze ji zobrazit pomocí aplikace Visual Studio/HDInsight SDK/PowerShell. |
|Zavření okna|  Spouštění prostřednictvím HiveServer2 je „synchronní“ způsob, takže je nutné zachovat otevřená okna. Pokud okna zavřete, spouštění dotazu bude zrušeno.|Odesílání prostřednictvím WebHCat je „asynchronní“ způsob, aby bylo možné odeslat dotaz prostřednictvím WebHCat a zavřít aplikaci Visual Studio. Kdykoliv se můžete vrátit a zobrazit výsledky.|


### Graf výkonu úlohy Tez Hive

Nástroje HDInsight Visual Studio podporují zobrazení grafů výkonu pro úlohy Hive spuštěné prostřednictvím modulu Tez. Informace o povolení Tez naleznete v tématu [Používání Hive v HDInsight][hdinsight.hive]. Po odeslání úlohy Hive v sadě Visual Studio zobrazí Visual Studio po dokončení úlohy graf.  Možná budete muset kliknout na tlačítko **Aktualizovat** a získat nejnovější stav úlohy.

> [AZURE.NOTE] Tato funkce je dostupná pouze pro verze clusteru HDInsight nad 3.2.4.593 a může fungovat pouze pro dokončené úlohy (pokud jste odeslali úlohu prostřednictvím WebHCat; tento grafu se zobrazí při spuštění dotazu prostřednictvím HiveServer2). Tento postup funguje pro clustery založené na systému Windows a Linux.

![graf výkonu hadoop hive tez](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

Abychom vám pomohli lépe pochopit dotaz Hive, nástroj v této verzi přidává zobrazení Operátora Hive. Jednoduše dvakrát klikněte na vrcholy grafu úlohy a zobrazí se všechny operátory uvnitř vrcholu. Můžete také najet na konkrétní operátor a zobrazit další podrobnosti tohoto operátoru.

### Zobrazení spouštění úlohy pro Hive na úlohách Tez

Zobrazení spouštění úlohy pro Hive na úlohách Tez lze použít k získání strukturovaných a vizualizovaných informací pro úlohy Hive a získání dalších podrobností úlohy. Pokud dojde k problémům s výkonem, můžete zobrazení použít k získání dalších podrobností. Například způsobu, jakým úloha pracuje a podrobných informací o jednotlivých úlohách (čtení a zápis dat, plánovaný/počáteční/koncový čas atd.) tak, abyste mohli vyladit úlohu konfigurace nebo architekturu systému na základě vizualizovaných informací.

![zobrazení spouštění úlohy nástrojů hdinsight sady visual studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png)

## Spouštění skriptů Pig

Nástroje HDInsight pro Visual Studio podporují vytváření a odesílání skriptů Pig do clusterů HDInsight. Uživatelé mohou vytvořit projekt Pig ze šablony a pak odeslat skript do clusteru HDInsight.

## Názory a známé problémy

- Aktuálně jsou výsledky HiveServer2 zobrazeny čistě textovým způsobem, což není ideální. Pracujeme na řešení.

- Pokud výsledky začínají s hodnotami NULL, momentálně se nezobrazí. Tento problém jsme opravili a pokud budete tímto problémem blokováni, neváhejte nám zaslat e-mail nebo se obrátit na tým podpory.

- Skript HQL vytvořených pomocí Visual Studia je zakódován v závislosti na nastavení pro místní oblast uživatele. Nemusí se správně spustit, pokud uživatel nahraje skript do clusteru jako binární.

Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při používání tohoto nástroje, neváhejte nám zaslat e-mail na adresu hdivstool@microsoft.com.

## Další kroky
V tomto článku jste se dozvěděli, jak se připojit ke clusterům HDInsight ze sady Visual Studio, používat balíček nástroje Hadoop a spouštět dotazy Hive. Další informace naleznete v tématu:

- [Používání Hive Hadoop v HDInsight][hdinsight.hive]
- [Začínáme používat Hadoop v HDInsight][hdinsight.get.started]
- [Odesílání úloh Hadoop v HDInsight][hdinsight.submit.jobs]
- [Analýza dat Twitteru pomocí Hadoop v HDInsight][hdinsight.analyze.twitter.data]


<!--Anchors-->
[Instalace]: #installation
[Připojení k předplatnému služby Azure]: #connect-to-your-azure-subscription
[Procházejte propojené prostředky]: #navigate-the-linked-resources
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
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight.introduction]: hdinsight-hadoop-introduction.md
[hdinsight.get.started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight.hive]: hdinsight-use-hive.md
[hdinsight.submit.jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org



<!--HONumber=Jun16_HO2-->


