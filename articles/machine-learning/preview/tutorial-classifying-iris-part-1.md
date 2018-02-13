---
title: "Příprava dat pro kurz klasifikace Iris ve službách Azure Machine Learning | Dokumentace Microsoftu"
description: "V tomto kurzu na pokračování se dozvíte, jak komplexně používat služby Azure Machine Learning (Preview). Toto je první část, ve které se probírá příprava dat."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 09/28/2017
ms.openlocfilehash: 4e558518a5a1fb7b4cd0a58fe2453fd4c083b46a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="classify-iris-part-1-prepare-the-data"></a>Klasifikace Iris – Část 1: Příprava dat
Služby Azure Machine Learning (Preview) představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

Tento kurz je první částí z třídílné série. V tomto kurzu vás provedeme základy služeb Azure Machine Learning (Preview). Získáte informace o těchto tématech:
> [!div class="checklist"]
> * Vytvoření projektu v aplikaci Azure Machine Learning Workbench
> * Vytvoření balíčku pro přípravu dat
> * Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

Tento kurz používá nadčasovou [datovou sadu Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set). Snímky obrazovky jsou specifické pro systém Windows, ale prostředí v systému Mac OS je téměř shodné.

## <a name="prerequisites"></a>Požadavky
- Vytvoření účtu Experimentování ve službě Azure Machine Learning
- Instalace aplikace Azure Machine Learning Workbench

Aplikaci Azure Machine Learning Workbench můžete nainstalovat podle pokynů v článku [Rychlý start k instalaci a vytvoření](quickstart-installation.md). Tato instalace zahrnuje také nástroj příkazového řádku pro více platforem neboli Azure CLI.

## <a name="create-a-new-project-in-azure-machine-learning-workbench"></a>Vytvoření nového projektu v aplikaci Azure Machine Learning Workbench
1. Otevřete aplikaci Azure Machine Learning Workbench a v případě potřeby se přihlaste. V podokně **PROJEKTY** vyberte symbol plus (**+**) a vytvořte **Nový projekt**.

   ![Nový pracovní prostor](media/tutorial-classifying-iris/new_ws.png)

2. Vyplňte podrobnosti v části **Vytvořit nový projekt**: 

   ![Nový projekt](media/tutorial-classifying-iris/new_project.png)

   - Do pole **Název projektu** zadejte název projektu. Použijte například hodnotu **myIris**.
   - Vyberte **Adresář projektu**, ve kterém se projekt vytvoří. Použijte například hodnotu `C:\Temp\`. 
   - Zadejte volitelný **Popis projektu**. 
   - Pole **Úložiště Git** je také volitelné a můžete ho nechat prázdné. Můžete zadat existující prázdné úložiště Git (úložiště bez hlavní větve) ve Visual Studio Team Services. Pokud používáte již existující úložiště Git, můžete scénáře roamingu a sdílení povolit později. Další informace najdete v tématu [Použití úložiště Git](using-git-ml-project.md). 
   - Vyberte **Pracovní prostor**, například tento kurz používá **IrisGarden**. 
   - Ze seznamu šablon projektů vyberte šablonu **Klasifikace Iris**. 

3. Vyberte tlačítko **Vytvořit**. Projekt se teď vytvoří a otevře.

## <a name="create-a-data-preparation-package"></a>Vytvoření balíčku pro přípravu dat
1. V **zobrazení souborů** otevřete soubor **iris.csv**. Tento soubor představuje tabulku s 5 sloupci a 150 řádky. Obsahuje čtyři sloupce s čísly a cílový sloupec řetězcového typu. Neobsahuje záhlaví sloupců.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Nezahrnujte datové soubory do složky projektu, zejména pokud jsou soubory velké. Do této šablony jsme pro demonstrační účely soubor **iris.csv** zahrnuli, protože je malý. Další informace najdete v tématu [Čtení a zápis velkých datových souborů](how-to-read-write-files.md).

2. V **Zobrazení dat** vyberte symbol plus (**+**) a přidejte nový zdroj dat. Otevře se stránka **Přidat zdroj dat**. 

   ![Zobrazení dat](media/tutorial-classifying-iris/data_view.png)

3. Vyberte **Textové soubory (*.csv, .json, .txt.,... )** a klikněte na **Další**.
   ![Zdroj dat](media/tutorial-classifying-iris/data-source.png)
   

4. Přejděte k souboru **iris.csv** a klikněte na **Další**.  
 
   ![Výběr iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Zkontrolujte, že jste pro účely tohoto cvičení vybrali soubor **iris.csv** z aktuálního adresáře projektu. Jinak se pozdější kroky nemusí podařit.
   
5. Ponechte výchozí hodnoty a klikněte na **Dokončit**.

6. Vytvoří se nový soubor s názvem **iris-1.dsource**. Soubor má jedinečný název s „-1“, protože ukázkový projekt už obsahuje neočíslovaný soubor **iris.dsource**.  

   Soubor se otevře a zobrazí se data. Do této datové sady se automaticky přidá řada záhlaví sloupců od **Column1** po **Column5**. Přejděte do dolní části a všimněte si, že poslední řádek datové sady je prázdný. Řádek je prázdný, protože v souboru CSV je zalomení řádku navíc.

   ![Zobrazení dat iris](media/tutorial-classifying-iris/iris_data_view.png)

7. Vyberte tlačítko **Metriky**. Podívejte se na histogramy. Pro každý sloupec se vypočítala úplná sada statistik. Můžete také vybrat tlačítko **Data** a znovu zobrazit data. 

   ![Zobrazení dat iris](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Vyberte tlačítko **Připravit**. Otevře se dialogové okno **Připravit**. 

   Ukázkový projekt již obsahuje soubor **iris.dprep**. Ve výchozím nastavení se zobrazí výzva k vytvoření nového toku dat v již existujícím balíčku pro přípravu dat **iris.dprep**. 

   Z rozevírací nabídky vyberte **+ Nový balíček pro přípravu dat**, jako název balíčku zadejte novou hodnotu **iris-1** a pak vyberte **OK**.

   ![Zobrazení dat iris](media/tutorial-classifying-iris/new_dprep.png)

   Vytvoří se nový balíček pro přípravu dat s názvem **iris-1.dprep** a otevře se v editoru přípravy dat.

9. Teď provedeme základní přípravu dat. Přejmenujte sloupce. Výběrem záhlaví jednotlivých sloupců umožníte úpravu jejich textu. 

   Pro jednotlivé sloupce zadejte **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** a **Species**.

   ![Přejmenování sloupců](media/tutorial-classifying-iris/rename_column.png)

10. Pokud chcete zjistit počet jedinečných hodnot, vyberte sloupec **Species** a kliknutím pravým tlačítkem jej vyberte. Z rozevírací nabídky vyberte **Četnost hodnot**. 

   ![Výběr možnosti Četnost hodnot](media/tutorial-classifying-iris/value_count.png)

   Tato akce otevře podokno **Kontroly** a zobrazí histogram se čtyři pruhy. Cílový sloupec obsahuje tři různé hodnoty **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** a hodnotu **(null)**.

11. Pokud chcete vyfiltrovat hodnoty null, vyberete v grafu pruh reprezentující hodnotu null. Hodnotu **(null)** obsahuje jeden řádek. Pokud chcete tento řádek odebrat, vyberte symbol minus (**-**).

   ![Histogram počtu hodnot](media/tutorial-classifying-iris/filter_out.png)

12. Všimněte si jednotlivých kroků uvedených v podokně **KROKY**. Při přejmenování sloupců a vyfiltrování řádků s hodnotou null se každá akce zaznamenala jako krok přípravy dat. Jednotlivé kroky můžete upravit a tím upravit nastavení, změnit pořadí kroků a odebrat jednotlivé kroky.

   ![Kroky](media/tutorial-classifying-iris/steps.png)

13. Zavřete editor přípravy dat. Vyberte **Zavřít** (x) na kartě **iris-1** s ikonou grafu. Vaše práce se automaticky uloží do souboru **iris-1.dprep** zobrazeného pod nadpisem **Příprava dat**.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

1. Klikněte pravým tlačítkem na soubor **iris-1.dprep**, abyste vyvolali místní nabídku, a pak vyberte **Vygenerovat soubor s kódem přístupu k datům**. 

   ![Generování kódu](media/tutorial-classifying-iris/generate_code.png)

2. Otevře se nový soubor s názvem **iris-1.py**, který obsahuje následující řádky kódu:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   Tento fragment kódu vyvolává logiku, kterou jste vytvořili jako balíček pro přípravu dat. V závislosti na kontextu, ve kterém je tento kód spuštěný, může `df` představovat různé druhy datových rámců. [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) se používá při spuštění v modulu runtime Pythonu a [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) se používá při spuštění v kontextu Sparku. 

   Další informace o přípravě dat v aplikaci Azure Machine Learning Workbench najdete v příručce [Začínáme s přípravou dat](data-prep-getting-started.md).

## <a name="next-steps"></a>Další kroky
V této první části třídílné série kurzů jste použili aplikaci Azure Machine Learning Workbench k provedení těchto úloh:
> [!div class="checklist"]
> * Vytvoření nového projektu 
> * Vytvoření balíčku pro přípravu dat
> * Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

Jste připraveni přejít k další části série, ve které se naučíte sestavit model ve službě Azure Machine Learning:
> [!div class="nextstepaction"]
> [Sestavení modelu](tutorial-classifying-iris-part-2.md)
