---
title: "Příklad Machine learning s MLlib Spark v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit aplikaci machine learning, která analyzuje datové sady používá klasifikaci prostřednictvím logistic regression pomocí Spark MLlib."
keywords: "Spark strojového učení, spark machine learning příklad"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: jgao
ms.openlocfilehash: 864d34306dad2915a15b032a27600cefdc632bb9
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="use-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Použít Spark MLlib sestavení machine learning aplikací a analýza datové sady

Další informace o použití Spark **MLlib** vytvořit machine learning aplikaci dělat jednoduché prediktivní analýzy pro otevřete datovou sadu. Z Spark předdefinované strojového učení knihovny, tento příklad používá *klasifikace* prostřednictvím logistic regression. 

> [!TIP]
> V tomto příkladu je také k dispozici jako poznámkový blok Jupyter v clusteru Spark (Linux), který vytvoříte v HDInsight. Poznámkový blok prostředí vám umožní spustit fragmenty Python ze samotného poznámkového bloku. Chcete-li postupovat v kurzu z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Potom spusťte poznámkového bloku **Spark Machine Learning - prediktivní analýzy dat kontroly potravin pomocí MLlib.ipynb** pod **Python** složky.
>
>

MLlib je základní knihovna Spark, která poskytuje řadu nástrojů, které jsou užitečné pro úkoly strojového učení, včetně nástroje, které jsou vhodné pro:

* Klasifikace
* Regrese
* Clustering
* Téma modelování
* Hodnota singulární rozložením (SVD) a analýzu hlavní součásti (PCA)
* Předpoklad testování a výpočet ukázka statistiky

## <a name="what-are-classification-and-logistic-regression"></a>Co jsou klasifikace a logistic regression?
*Klasifikace*, oblíbených strojového učení úloh, je proces řazení do kategorií vstupní data. Je úloha klasifikační algoritmus zjistit, jak přiřadit "popisky" pro vstupní data, která zadáte. Například může úvahách o algoritmu strojového učení, které přijímá uložené informace jako vstup a rozděluje se stav do dvou kategorií: které by měl prodeje a populací, které byste měli mít.

Logistic regression je algoritmus, který používáte pro klasifikaci. Spark logistic regression rozhraní API je užitečné pro *binární klasifikace*, nebo do jedné ze dvou skupin klasifikace vstupní data. Další informace o logistic regresí najdete v tématu [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

V souhrnu, vytvoří proces logistic regression *logistic funkce* , mohou být použity k předpovědi pravděpodobnost, že vstupní vektoru patří v jedné skupině nebo dalších.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Příklad prediktivní analýzy dat kontroly potravin
V tomto příkladu použijete k provedení některých prediktivní analýzy dat kontroly potravin Spark (**Food_Inspections1.csv**), byl získán v rámci [města Chicagu datovém portálu](https://data.cityofchicago.org/). Tato datová sada obsahuje informace o kontroly potravin zařízení, které byly provedeny v Chicagu, včetně informací o každé zařízení, porušení nalezen (pokud existuje) a výsledky kontroly. Datový soubor CSV je již k dispozici v účtu úložiště přidruženého k clusteru na **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

V následujících krocích vyvinout model zobrazíte trvá na úspěch nebo selhání kontroly potravin.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Začněte vytvářet Spark MMLib strojového učení aplikace
1. Z portálu [Azure Portal](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel). Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.   
1. Z okna clusteru Spark klikněte na **Řídicí panel clusteru** a poté na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   > [!NOTE]
   > Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření poznámkového bloku Jupyter](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "vytvoření nového poznámkového bloku Jupyter")
1. Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb. Klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu poznámkového bloku](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Zadání názvu poznámkového bloku")
1. Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu. Můžete začít vytvářet svůj počítač učení aplikace importem typů nezbytných pro tento scénář. Uděláte to tak, umístěte kurzor do buňky a stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Vytvoření vstupní dataframe
Můžeme použít `sqlContext` k provádění transformací na strukturovaná data. První úlohou je načíst ukázková data ((**Food_Inspections1.csv**)) do Spark SQL *dataframe*.

1. Protože nezpracovaná data ve formátu CSV, je potřeba použít Spark kontext pro každý řádek souboru pro vyžádání obsahu do paměti jako nestrukturovaných text; potom použít jazyka Python sdíleného svazku clusteru knihovnu analyzovat každý řádek zvlášť.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. Jako RDD máme teď soubor CSV.  Zjistit schéma dat načteme z RDD jeden řádek.

        inspections.take(1)

    Zobrazený výstup by měl vypadat asi takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. Tento výstup nám poskytuje základní přehled o schéma vstupní soubor. Obsahuje název každé zařízení, typ zařízení, adresu, data z kontroly a umístění, mimo jiné. Umožňuje vybrat několik sloupců, které jsou užitečné pro naše prediktivní analýzy a výsledky jsou seskupeny dataframe, který jsme potom použít k vytvoření dočasné tabulky.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Nyní je k dispozici *dataframe*, `df` na kterém jsme můžete provádět analýzy. Máme také volání dočasné tabulky **CountResults**. Jsme zahrnuli čtyři sloupce zájem o dataframe: **id**, **název**, **výsledky**, a **porušení**.

    Pojďme malé ukázkové dat:

        df.show(5)

    Zobrazený výstup by měl vypadat asi takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Pochopení dat
1. Začněme získáte přehled o co obsahuje naší datové sadě. Co jsou například různé hodnoty v **výsledky** sloupec?

        df.select('results').distinct().show()

    Zobrazený výstup by měl vypadat asi takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. Rychlé vizualizace pomůžete nám důvod informace o distribuci tyto výstupy. Připravili jsme data v dočasné tabulce **CountResults**. Spuštěním následujícího dotazu SQL s tabulkou získat lepší pochopení, jak jsou distribuovány výsledky.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql` Magic následuje `-o countResultsdf` zajistí, že je na serveru Jupyter (obvykle headnode clusteru) místně zachován výstup tohoto dotazu. Výstup je uchován jako [Pandas](http://pandas.pydata.org/) dataframe se zadaným názvem **countResultsdf**.

    Zobrazený výstup by měl vypadat asi takto:

    ![Výstup dotazu SQL](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "výstupu dotazu SQL")

    Další informace o magických příkazech `%%sql` a dalších magických příkazech, které jsou k dispozici s jádrem PySpark, najdete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. Matplotlib knihovnu použitý k vytvoření vizualizaci dat, můžete taky vytvořit vykreslení. Protože musí být vytvořeny vykreslení z místně trvalou **countResultsdf** dataframe, fragment kódu musí začínat `%%local` magic. To zajistí, že kód běží místně na serveru Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Zobrazený výstup by měl vypadat asi takto:

    ![Výstup Spark strojového učení aplikace - výsečového grafu s pěti výsledky kontroly odlišné](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark strojového učení výstup výsledků")
1. Můžete zjistit, že jsou 5 odlišné výsledky, které může mít kontrola:

   * Obchodní není umístěný.
   * Chyba
   * Předat
   * PSS s podmínky
   * Mimo firmy

     Dejte nám vyvinout model, který lze snadno uhodnout výsledek kontroly potravin zadané porušení zásad. Vzhledem k tomu, že logistic regression je metoda binární klasifikace, má smysl seskupovat naše data do dvou kategorií: **nezdaří** a **předat**. "Předat s podmínky" je stále průchodu, takže když jsme trénování modelu, jsme vezměte v úvahu oba výsledky ekvivalentní. Data s jinými výsledky ("Obchodní není umístěný" nebo "mimo firemní") nejsou užitečné, proto jsme odebrat z našich trénovací sady. Toto musí být v pořádku, vzhledem k tomu, že tyto dvě kategorie tvoří velmi malá část tohoto výsledky přesto.
1. Dejte nám pokračujte a převést naše stávající dataframe (`df`) do nového dataframe, kde je každý kontroly reprezentován jako pár porušení popisek. V našem případě štítek z `0.0` představuje selhání, popisek `1.0` představuje úspěšné a popisek `-1.0` představuje některé výsledky kromě těchto dvou. Jsme odfiltrovat jiných výsledků při výpočtu nové datové rámce.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    Pokud chcete zjistit, jaké označených datech zdá, můžeme načíst jeden řádek.

        labeledData.take(1)

    Zobrazený výstup by měl vypadat asi takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Vytvoření modelu logistic regression ze vstupní dataframe
Naše poslední je s popiskem data převést do formátu, který lze analyzovat pomocí logistic regression. Vstup logistic regresní algoritmus musí být sadu *popisek funkce vector páry*, kde je "funkce vector" vektor čísel představující vstupní bod. Ano musíme převést sloupci "porušení", který je částečně strukturovaných a obsahuje mnoho komentáře v prostém textu, na pole reálná čísla, které je počítač by mohl snadno porozumíte.

Jeden standardní strojového učení přístup pro zpracování přirozeného jazyka je přiřadit všech jedinečných slov "index" a poté předat vektor strojového učení algoritmu tak, aby každý index hodnota obsahuje relativní četnost dané slovo v textovém řetězci.

MLlib poskytuje snadný způsob, jak provést tuto operaci. Nejprve "tokenizaci" každé porušení řetězec k získání jednotlivých slov v každé řetězec. Poté použijte `HashingTF` k převedení každou sadu tokeny do funkce vektor, který může být předána do algoritmus logistic regression vytvořit model. Můžeme provést všechny tyto kroky v pořadí pomocí "kanál".

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Vyhodnocení modelu na samostatné testovací datové sady
Můžeme použít model, který jsme vytvořili dříve do *předpovědi* co výsledky kontrol nové budou, podle porušení zásad, která měla dodržen. Jsme natrénovali tento model na datovou sadu **Food_Inspections1.csv**. Dejte nám použít druhý datovou sadu, **Food_Inspections2.csv**do *vyhodnotit* sílu tento model na nová data. Druhé sadě dat (**Food_Inspections2.csv**) by už měla být ve výchozím kontejneru úložiště přidružen ke clusteru.

1. Následující fragment vytváří nové dataframe, **predictionsDf** obsahující předpovědi generované modelu. Fragment vytváří také dočasné tabulky nazývané **předpovědi** podle dataframe.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Zobrazený výstup by měl vypadat asi takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. Podívejte se na jednu z jsou předpovědi. Spusťte tento fragment kódu:

        predictionsDf.take(1)

   Není předpovědi pro první položku v sadě dat testu.
1. `model.transform()` Metoda používá stejný transformace žádná nová data se stejným schématem a přicházejí na předpovědi jak klasifikovat data. Provedeme jednoduchý statistikami získat představu o jak přesný byly naše předpovědi:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Výstup vypadá takto:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Pomocí Spark logistic regression nám poskytuje model přesné vztahu mezi popisy porušení v angličtině a zda se daný obchodní úspěch nebo selhání kontroly potravin.

## <a name="create-a-visual-representation-of-the-prediction"></a>Vytvoření vizuální reprezentace předpovědi
Jsme můžete nyní vytvořit konečné vizualizace nám pomáhají důvod o výsledcích tento test.

1. Začneme extrahováním různých předpovědi a výsledky z **předpovědi** dočasné tabulky vytvořili dříve. Následující dotazy oddělit výstup jako *true_positive*, *false_positive*, *true_negative*, a *false_negative*. V následující dotazy jsme vypnout vizualizaci pomocí `-q` a také uložte si výstup (s použitím `-o`) jako dataframes, který lze potom použít s `%%local` magic.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Nakonec použijte následující fragment kódu ke generování výkresu pomocí **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Byste měli vidět následující výstup:

    ![Spark strojového učení výstupu aplikace - výsečového grafu procenta kontroly potravin se nezdařilo. ] (./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark strojového učení výstup výsledků")

    V tomto grafu "pozitivní" výsledek odkazuje na kontroly potravin se nezdařila, zatímco záporný výsledek odkazuje na předaný kontroly.

## <a name="shut-down-the-notebook"></a>Vypnout poznámkového bloku
Po dokončení spuštění aplikace byste měli vypínat poznámkového bloku k uvolnění prostředků. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku. Tím se vypne a zavření poznámkového bloku.

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](apache-spark-job-debugging.md)
