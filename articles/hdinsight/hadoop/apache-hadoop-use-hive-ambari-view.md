---
title: "Při práci s Hive v HDInsight (Hadoop) - Azure používají zobrazení Ambari | Microsoft Docs"
description: "Naučte se používat k odesílání dotazů Hive zobrazení Hive z webového prohlížeče. Zobrazení Hive je součástí webové uživatelské rozhraní Ambari poskytuje k vašemu clusteru HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/23/2017
ms.author: larryfr
ms.openlocfilehash: 8293da8c77725d051f295826d9a78bf81055dcb3
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Použití zobrazení Ambari Hive se systémem Hadoop v HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informace o spouštění dotazů Hive pomocí zobrazení Ambari Hive. Ambari je správy a monitorování nástroj s clustery HDInsight se systémem Linux. Jedna z funkcí poskytované prostřednictvím Ambari je webového uživatelského rozhraní, který slouží ke spouštění dotazů Hive.

> [!NOTE]
> Ambari má mnoho možností, které nejsou popsané v tomto dokumentu. Další informace najdete v tématu [Správa clusterů HDInsight pomocí webového uživatelského rozhraní Ambari](../hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight se systémem Linux. Informace o vytváření clusterů najdete v tématu [začněte s Hadoop v HDInsight](apache-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Kroky v tomto dokumentu vyžadují clusteru Azure HDInsight, který používá Linux. Linux je jenom operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-the-hive-view"></a>Otevření zobrazení Hive

Zobrazení Ambari můžete otevřít z portálu Azure. Vyberte HDInsight cluster a pak vyberte **zobrazení Ambari** z **rychlé odkazy** části.

![Rychlé odkazy sekci portálu](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

Ze seznamu zobrazení, vyberte __zobrazení Hive__.

![Vybrané zobrazení Hive](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Pokud získáváte přístup k Ambari, se zobrazí výzva k ověření na webu. Zadejte správce (výchozí `admin`) účet jméno a heslo, které jste použili při vytvoření clusteru.

Měli byste vidět stránka podobná na následujícím obrázku:

![Obrázek listu dotazu pro zobrazení Hive](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="run-a-query"></a>Spuštění dotazu

Ke spuštění dotazu Hive, použijte následující kroky v zobrazení Hive.

1. Z __dotazu__ kartě, vložte následující příkazy HiveQL do listu:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Tyto příkazy provádět následující akce:

   * `DROP TABLE`: Odstraní tabulku a datový soubor v v případě, že v tabulce již existuje.

   * `CREATE EXTERNAL TABLE`: Vytvoří novou tabulku "externí" v Hive.
   Externí tabulky uložit pouze definici tabulky Hive. Data je ponechán v původním umístění.

   * `ROW FORMAT`: Ukazuje způsob formátování data. V takovém případě polí v každém protokolu jsou oddělené mezerou.

   * `STORED AS TEXTFILE LOCATION`: Zobrazí se uloží data a že je uloženo jako text.

   * `SELECT`: Vybere počet všech řádků, kde t4 sloupec obsahuje hodnotu [Chyba].

     > [!NOTE]
     > Použijte externí tabulky, pokud očekáváte, že v základních datech aktualizovat externí zdroj, například automatizované dat nahrát procesu nebo jiná operace MapReduce. Vyřazení externí tabulku nemá *není* odstranit data, jenom definici tabulky.

    > [!IMPORTANT]
    > Ponechte __databáze__ výběr v __výchozí__. V příkladech v tomto dokumentu použijte výchozí databázi součástí HDInsight.

2. Pokud chcete spustit dotaz, použít **Execute** tlačítko pod listu. Tlačítko se změní oranžová a se text změní na **Zastavit**.

3. Po dokončení dotazu **výsledky** karta zobrazuje výsledky operace. Tento text je výsledkem dotazu:

        sev       cnt
        [ERROR]   3

    Můžete použít **protokoly** a zobrazit informace o protokolování, které úloha vytvořena.

   > [!TIP]
   > Stahování nebo uložení výsledků **výsledky uložit** dialogové rozevírací seznam v horní pravé **výsledky zpracování dotazu** části.

4. Vyberte první čtyři řádky tohoto dotazu a pak vyberte **Execute**. Všimněte si, že nebyly nalezeny žádné výsledky po dokončení úlohy. Pomocí **Execute** tlačítko, pokud je vybraná část dotazu lze spustit pouze vybrané příkazy. V takovém případě výběr nezahrnuli poslední příkaz, který načte řádky z tabulky. Pokud vyberete jenom daného řádku a používat **Execute**, měli byste vidět očekávané výsledky.

5. Pokud chcete přidat do listu, použijte **nový list** tlačítko v dolní části **Editor dotazů**. Do nového listu zadejte následující příkazy HiveQL:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Tyto příkazy provádět následující akce:

   * **Vytvoření tabulka není v případě existuje**: vytvoří tabulku, pokud ještě neexistuje. Protože **externí** – klíčové slovo nepoužívá, k vytvoření interní tabulky. Interní tabulku je uložena v datovém skladu Hive a je zcela spravuje Hive. Na rozdíl od s externích tabulek se odstranit interní tabulku odstraní v základních datech.

   * **ULOŽENÉ ORC AS**: ukládá data ve formátu optimalizované řádek sloupcovém (ORC). ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.

   * **PŘEPSAT INSERT... Vyberte**: vybere řádky z **log4jLogs** tabulku, která obsahují `[ERROR]`a potom vkládá data do **errorLogs** tabulky.

Použití **Execute** tlačítko ke spuštění tohoto dotazu. **Výsledky** karta neobsahuje žádné informace, pokud dotaz vrátí nulový počet řádků. Měl by být stav jako **úspěšné** po dokončení dotazu.

### <a name="visual-explain"></a>Vysvětlují Visual

Chcete-li zobrazit vizualizaci plán dotazu, vyberte **vysvětlují, Visual** kartě níže listu.

**Visual vysvětlují** zobrazení dotazu může být užitečné porozumět toku složitých dotazů. Textové ekvivalent v tomto zobrazení můžete zobrazit pomocí **vysvětlit** tlačítko v editoru dotazů.

### <a name="tez-ui"></a>Tez uživatelského rozhraní

Pokud chcete zobrazit uživatelské rozhraní Tez pro dotaz, vyberte **Tez** kartě níže listu.

> [!IMPORTANT]
> Chcete-li vyřešit všechny dotazy se nepoužije tez. Abyste mohli vyřešit mnoho dotazů bez použití Tez. 

Pokud Tez byl použit k vyřešení dotazu, zobrazí se přesměruje Acyklické grafu (DAG). Pokud chcete zobrazit DAG pro dotazy, které jste si vyzkoušeli v minulosti, nebo pokud chcete ladit proces Tez, použijte [Tez zobrazení](../hdinsight-debug-ambari-tez-view.md) místo.

## <a name="view-job-history"></a>Zobrazení historie úlohy

__Úlohy__ karta se zobrazuje historie dotazů Hive.

![Obrázek historie úlohy](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Databázové tabulky

Můžete použít __tabulky__ kartě práce s tabulkami v rámci databáze Hive.

![Obrázek kartu tabulky](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Uložené dotazy

Z **dotazu** kartě, můžete volitelně uložit dotazy. Po uložení dotazu se můžete znovu použít z __uložené dotazy__ kartě.

![Obrázek karty uložené dotazy](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Uživatelem definované funkce

Můžete také rozšířit Hive prostřednictvím uživatelem definovaných funkcí (UDF). Implementace funkce nebo logiky, která není snadno modelován v HiveQL pomocí uživatelem definovanou FUNKCI.

Deklarace a ukládání sadu UDF pomocí **UDF** karty v horní části zobrazení Hive. Tyto funkce lze použít s **Editor dotazů**.

![Obrázek karty UDF](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Po přidání uživatelem definovanou FUNKCI k zobrazení Hive **vložit UDF** tlačítko se zobrazí v dolní části **Editor dotazů**. Výběrem této položky se zobrazí rozevírací seznam UDF definované v zobrazení Hive. Výběr UDF přidá příkazy HiveQL do dotazu povolit UDF.

Například pokud jste definovali UDF s následujícími vlastnostmi:

* Název prostředku: myudfs

* Cesta prostředku: /myudfs.jar

* Název UDF: myawesomeudf

* Název třídy UDF: com.myudfs.Awesome

Pomocí **vložit UDF** tlačítko zobrazí položka s názvem **myudfs**, s jinou rozevíracího seznamu pro každý UDF definované pro tento prostředek. V takovém případě je **myawesomeudf**. Výběrem této položky přidáte následující na začátek dotazu:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Potom můžete UDF v dotazu. Například, `SELECT myawesomeudf(name) FROM people;`.

Další informace o těchto funkcích pomocí Hive v HDInsight naleznete v následujících článcích:

* [Python pomocí Hive a Pig v HDInsight](python-udf-hdinsight.md)
* [Jak přidat vlastní Hive UDF do HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Nastavení Hive

Můžete změnit různá nastavení Hive, jako je například změna spouštěcímu modulu pro Hive z Tez (výchozí) na MapReduce.

## <a id="nextsteps"></a>Další kroky

Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
