---
title: "Korelovat události v čase pomocí nástrojů Storm a HBase v HDInsight"
description: "Zjistěte, jak ke korelaci událostí, které přicházejí v různou dobu pomocí nástrojů Storm a HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 06630096383601e48e8f69f8553314cee42f5f3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Korelovat události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase

Pomocí úložiště trvalé dat s Apache Storm mohou korelovat datové položky, které přicházejí v různých časech. Například propojení události přihlášení a odhlášení pro uživatelské relace vypočítat, jak dlouho už bylo relace.

V tomto dokumentu zjistěte, jak vytvořit základní topologie C# Storm, který sleduje události přihlášení a odhlášení pro relace uživatelů a vypočítá dobu trvání relace. Topologie používá HBase jako úložiště dat trvalé. HBase můžete také provádět batch dotazy na historická data k vytvoření další statistiky. Například kolik uživatelských relací byly spuštěna, nebo skončila v konkrétním časovém období.

## <a name="prerequisites"></a>Požadavky

* Visual Studio a nástroje HDInsight pro Visual Studio. Další informace najdete v tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Apache Storm v HDInsight clusteru (založené na Windows).

  > [!WARNING]
  > Když SCP.NET topologie jsou podporované u clusterů Storm se systémem Linux vytvořené po 10/28/2016, sada SDK HBase pro balíček .NET, které jsou k dispozici od 10/28/2016 nebude správně fungovat na HDInsight se systémem Linux

* Apache HBase v clusteru HDInsight (Linux nebo na základě Windows).

  > [!IMPORTANT]
  > HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Java](https://java.com) 1.7 nebo vyšší na vašem vývojovém prostředí. Java se používá k balíčku topologii při odeslání do clusteru HDInsight.

  * **JAVA_HOME** proměnnou prostředí musí odkazovat na adresář, který obsahuje Java.
  * **%JAVA_HOME%/bin** adresář se musí nacházet v cestě

## <a name="architecture"></a>Architektura

![Diagram toku dat prostřednictvím topologie](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Obecný identifikátor korelace událostí vyžaduje pro zdroje událostí. For example ID uživatele, ID relace nebo další část dat, který je a) jedinečný a b) součástí v všechna data odesílaná do Storm. Tento příklad používá hodnotu GUID představují ID relace.

V tomto příkladu se skládá ze dvou clustery HDInsight:

* HBase: úložiště dat pro historických dat
* Storm: používá k ingestování příchozích dat

Data je náhodně generované topologie Storm a skládá se z následujících položek:

* ID relace: identifikátor GUID, který jednoznačně identifikuje každou relaci
* Událost: počáteční nebo koncové události. V tomto příkladu počáteční dochází vždy konce
* Čas: čas události.

Tato data jsou zpracovány a uložená v HBase.

### <a name="storm-topology"></a>Topologie Storm

Když se spustí relaci, **spustit** událostí je přijatých topologie a do HBase. Při **END** události, načte topologii **spustit** událostí a vypočítá dobu mezi dvěma událostmi. To **doba trvání** hodnoty jsou pak uloženy v HBase spolu s **END** informací o události.

> [!IMPORTANT]
> Při této topologii ukazuje základní vzor, produkční řešení by potřeba provést návrhu pro následující scénáře:
>
> * Příchozích mimo pořadí událostí
> * Duplicitní události
> * Vyřazené události

Ukázková topologie se skládá z následujících součástí:

* Session.cs: simuluje uživatelské relace vytvořením ID náhodných relace, počáteční čas a jak dlouho trvá relace.

* Spout.cs: Vytvoří 100 relací, vysílá události spuštění, čeká náhodných časový limit pro každou relaci a potom vydá KONCOVÁ událost. Potom recykluje skončila generovat nové relace.

* HBaseLookupBolt.cs: používá k vyhledání informací o relaci z HBase ID relace. Při zpracování KONCOVÁ událost najde odpovídající události spuštění a vypočítá dobu trvání relace.

* HBaseBolt.cs: Ukládá informace o HBase.

* TypeHelper.cs: Pomáhá s převod typů při čtení / zápisu do HBase.

### <a name="hbase-schema"></a>Schéma HBase

V HBase jsou data uložena v tabulce s následující schéma nebo nastavení:

* Klíč řádku: relace ID slouží jako klíč pro řádky v této tabulce.

* Rodin sloupců: název rodiny je 'CR'. Jsou uložené v této rodině sloupce:

  * událost: počáteční nebo KONCOVÝ.

  * čas: čas v milisekundách, kdy došlo k události.

  * Doba trvání: délka mezi počáteční a koncové události.

* VERZE: rodiny 'CR' nastavena na uchování 5 verzích každý řádek.

  > [!NOTE]
  > Verze jsou protokolu předchozí hodnot uložených pro klíč specifickým řádkem. Ve výchozím nastavení HBase pouze vrací hodnotu pro nejnovější verzi řádek. V takovém případě se používá pro stejný řádek pro každou verzi řádek je identifikována hodnotu časového razítka všechny události (START a END.). Verze poskytují Historický přehled události evidované pro konkrétní ID.

## <a name="download-the-project"></a>Stažení projektu

Ukázkový projekt si můžete stáhnout z [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).

Tento soubor ke stažení obsahuje následující projekty C#:

* CorrelationTopology: Topologie C# Storm, který náhodně vysílá počáteční a koncové události pro relace uživatelů. Každý relace trvá od 1 do 5 minut.

* SessionInfo: C# konzolovou aplikaci, která vytvoří tabulku HBase a poskytuje příklady dotazů k vrácení informací o datech uložených relací.

## <a name="create-the-table"></a>Vytvoření tabulky

1. Otevřete **SessionInfo** projektu v sadě Visual Studio.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **SessionInfo** projektu a vyberte **vlastnosti**.

    ![Snímek obrazovky nabídky s vlastnostmi vybrané](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Vyberte **nastavení**, nastavte následující hodnoty:

   * HBaseClusterURL: adresa URL pro váš cluster HBase. Například https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: Správce/HTTP uživatelský účet pro váš cluster

   * HBaseClusterPassword: heslo pro uživatelský účet správce nebo HTTP

   * HBaseTableName: název tabulku, kterou chcete použít v tomto příkladu

   * HBaseTableColumnFamily: Rodiny název sloupce

     ![Obrázek dialogového okna nastavení](./media/hdinsight-storm-correlation-topology/settings.png)

4. Spusťte řešení. Po zobrazení výzvy vyberte "c" klíč, který chcete vytvořit cluster HBase v tabulce.

## <a name="build-and-deploy-the-storm-topology"></a>Vytváření a nasazování topologie Storm

1. Otevřete **CorrelationTopology** řešení v sadě Visual Studio.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **CorrelationTopology** projektu a vyberte možnost Vlastnosti.

3. V okně vlastností vyberte **nastavení** a zadejte hodnoty konfigurace pro tento projekt. První 5 jsou stejné hodnoty používané **SessionInfo** projektu:

   * HBaseClusterURL: adresa URL pro váš cluster HBase. Například https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: Správce/HTTP uživatelský účet pro váš cluster.

   * HBaseClusterPassword: heslo pro uživatelský účet správce nebo HTTP.

   * HBaseTableName: název tabulku, kterou chcete použít v tomto příkladu. Tato hodnota je stejný název tabulky jako použité v SessionInfo projektu.

   * HBaseTableColumnFamily: Sloupec Název rodiny. Tato hodnota se stejným názvem rodiny sloupec jako použité v SessionInfo projektu.

   > [!IMPORTANT]
   > Neměňte HBaseTableColumnNames, jako výchozí hodnoty jsou názvy používané **SessionInfo** načíst data.

4. Uložte vlastnosti a sestavte projekt.

5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslání do Storm v HDInsight**. Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.

   ![Obrázek odeslání na položku nabídky storm](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. V **odeslání topologie** dialogovém okně, vyberte cluster Storm, který chcete nasadit tuto topologii.

   > [!NOTE]
   > Při prvním odeslání topologii, může trvat několik sekund načíst název clusterům HDInsight.

7. Jakmile topologii byl nahrán a odeslaných do clusteru, **zobrazení topologie Storm** otevře a zobrazí spuštěné topologie. Chcete-li obnovit data, vyberte **CorrelationTopology** a použijte tlačítko Aktualizovat v horní pravé části stránky.

   ![Obrázek topologie zobrazení](./media/hdinsight-storm-correlation-topology/topologyview.png)

   Když začne topologii generování dat, je hodnota v **Emitted** sloupec přírůstcích.

   > [!NOTE]
   > Pokud **zobrazení topologie Storm** neobsahuje automaticky otevře, otevřete ho pomocí následujících kroků:
   >
   > 1. V **Průzkumníku řešení**, rozbalte položku **Azure**a potom rozbalte **HDInsight**.
   > 2. Klikněte pravým tlačítkem na cluster Storm, který topologie běží na a pak vyberte **topologie Storm zobrazení**

## <a name="query-the-data"></a>Dotaz na data

Po data byla vygenerované, použijte následující postup se dotázat na údaje.

1. Vraťte se na **SessionInfo** projektu. Pokud není spuštěná, spusťte novou instanci.

2. Po zobrazení výzvy vyberte **s** k vyhledání počáteční událostí. Zobrazí se výzva k zadání počáteční a koncový čas definovat časové rozmezí - se vrátí jenom události mezi těmito časy.

    Při zadávání počáteční a koncový čas, použijte následující formát: hh: mm a "" m"nebo 'pm'. Například 23:20:00.

    Pokud chcete vrátit protokolované události, použijte čas zahájení z před nasazená topologie Storm a koncový čas z nyní. Vracených dat obsahuje položky podobné následujícím textem:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Hledání koncové události funguje stejně jako počáteční události. Ale koncové události jsou generovány, náhodně rozmezí 1 až 5 minut po události spuštění. Možná budete muset zkuste několik časových rozsahů najít koncové události. KONCOVÉ události obsahovat také trvání relace - rozdíl mezi čas události počáteční a KONCOVÝ čas události. Tady je příklad dat pro koncové události:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Hodnoty času, kterou zadáte, jsou v místním čase, je čas vrácených dotazem ve standardu UTC.

## <a name="stop-the-topology"></a>Zastavení topologie

Pokud budete chtít zastavit topologii, vraťte se do **CorrelationTopology** projektu v sadě Visual Studio. V **zobrazení topologie Storm**, vyberte topologii a potom pomocí **Kill** tlačítka v horní části zobrazení topologie.

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

Další příklady Storm naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).
