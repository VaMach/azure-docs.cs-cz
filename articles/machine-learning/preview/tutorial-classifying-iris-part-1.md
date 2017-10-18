---
title: "Příprava dat pro kurz klasifikace Iris ve službách Machine Learning | Dokumentace Microsoftu"
description: "V tomto kurzu na pokračování se dozvíte, jak komplexně používat služby Azure Machine Learning (Preview). Toto je 1. část, která se zabývá přípravou dat."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/28/2017
ms.openlocfilehash: 975a86c1f9d9692f6eadd232177f33cbbbeeff2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="classifying-iris-part-1-prepare-data"></a>Klasifikace Iris – Část 1: Příprava dat
Služby Azure Machine Learning (Preview) představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

Tento kurz je první částí z třídílné série. V tomto kurzu se názorně seznámíte se základy služeb Azure Machine Learning (Preview). V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Vytvoření projektu v aplikaci Azure Machine Learning Workbench
> * Vytvoření balíčku pro přípravu dat
> * Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

Tento kurz používá kvůli zjednodušení nadčasovou [datovou sadu Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set). Snímky obrazovky jsou specifické pro systém Windows, ale prostředí v systému macOS je téměř shodné.

## <a name="prerequisites"></a>Požadavky
- Vytvoření účtu Experimentování ve službě Azure Machine Learning
- Instalace aplikace Azure Machine Learning Workbench

Aplikaci Azure Machine Learning Workbench můžete nainstalovat postupem podle kurzu [Rychlý start k instalaci a vytvoření](quickstart-installation.md). Tato instalace zahrnuje také rozhraní příkazového řádku (CLI).

## <a name="create-a-new-project-in-azure-ml-workbench"></a>Vytvoření nového projektu v aplikaci Azure ML Workbench
1. Spusťte aplikaci Azure Machine Learning Workbench a v případě potřeby se přihlaste. V podokně **PROJEKTY** klikněte na ikonu **+** a vytvořte **Nový projekt**.

   ![nový pracovní prostor](media/tutorial-classifying-iris/new_ws.png)

2. Vyplňte podrobnosti v části **Vytvořit nový projekt**. 

   ![Nový projekt](media/tutorial-classifying-iris/new_project.png)

   - Do pole **Název projektu** zadejte název projektu. Použijte například hodnotu **myIris**.
   - Zvolte **Adresář projektu**, ve kterém se projekt vytvoří. Použijte například hodnotu **C:\Temp**. 
   - Zadejte **Popis projektu**. 
   - Pole **Úložiště Git** je volitelné a můžete ho nechat prázdné. Můžete zadat existující prázdné úložiště Git (úložiště bez hlavní větve) ve VSTS. Pokud to uděláte, můžete později povolit scénáře roamingu a sdílení. Další informace najdete v článku [Používání úložiště Git](using-git-ml-project.md). 
   - Zvolte **Pracovní prostor**, například tento kurz používá **IrisGarden**. 
   - Ze seznamu šablon projektů vyberte šablonu **Klasifikace Iris**. 

3. Kliknutím na tlačítko **Vytvořit** vytvořte nový projekt. Projekt se teď vytvoří a otevře.

## <a name="create-a-data-preparation-package"></a>Vytvoření balíčku pro přípravu dat
1. V **zobrazení souborů** otevřete soubor **iris.csv**. Tento soubor představuje jednoduchou tabulku s 5 sloupci a 150 řádky. Obsahuje čtyři sloupce s čísly a cílový sloupec řetězcového typu. Neobsahuje záhlaví sloupců.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Nedoporučujeme zahrnovat datové soubory do složky projektu, zejména pokud jsou soubory velké. Do této šablony pro demonstrační účely jsme soubor **iris.csv** zahrnuli, protože je malý. Další informace najdete v článku [Čtení a zapisování velkých datových souborů](how-to-read-write-files.md).

2. V **Zobrazení dat** klikněte na ikonu **+** a přidejte nový zdroj dat. Spustí se průvodce **Přidat zdroj dat**. 

   ![zobrazení dat](media/tutorial-classifying-iris/data_view.png)

3. Dokončete průvodce přípravou dat. 
   - Na první obrazovce vyberte možnost **Soubory/adresář** a potom klikněte na **Další**.
   - Na druhé obrazovce zvolte místní soubor **iris.csv**, například C:\Temp\myIris\iris.csv.
   - Na třetí obrazovce **Podrobnosti o souboru** přijměte výchozí hodnoty.
   - Na čtvrté obrazovce **Datové typy**, změňte **DATOVÝ TYP** ze _String_ na _Numeric_ pro sloupce _Column1_ až _Column4_, protože tyto sloupce obsahují numerické hodnoty. 
   - Na páté a šesté obrazovce přijměte výchozí nastavení.
   - Klikněte na tlačítko **Dokončit**.

   ![výběr iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Zkontrolujte, že jste pro účely tohoto cvičení vybrali soubor **iris.csv** z aktuálního adresáře projektu, jinak se pozdější kroky nemusí podařit. 

4. Vytvoří se nový soubor **iris-1.dsource**. Soubor má jedinečný název s „-1“, protože ukázkový projekt už obsahuje neočíslovaný soubor **iris.dsource**.  

   Soubor se otevře a zobrazí se data. Do této datové sady se automaticky přidá řada záhlaví sloupců od **Column1** po **Column5**. Přejděte do dolní části a všimněte si, že poslední řádek datové sady je prázdný. To je z důvodu zalomení řádku navíc v souboru csv.

   ![zobrazení dat iris](media/tutorial-classifying-iris/iris_data_view.png)

5. Teď klikněte na tlačítko **Metriky**. Prohlédněte si histogramy a úplnou sadu statistik, které se počítají pro každý sloupec. Můžete také kliknout na tlačítko **Data** a znovu zobrazit data. 

   ![zobrazení dat iris](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Klikněte na tlačítko **Připravit**. Otevře se dialogové okno **Připravit**. 

   Ukázkový projekt obsahuje soubor **iris.dprep**, proto vás ve výchozím nastavení vyzve k vytvoření nového toku dat v tomto existujícím balíčku pro přípravu dat **iris.dprep**. 

   Změňte hodnotu rozevíracího seznamu na **+ Nový balíček pro přípravu dat**, zadejte novou hodnotu iris-1 a pak klikněte na **OK**.

   ![Zobrazení dat iris](media/tutorial-classifying-iris/new_dprep.png)

   Vytvoří se nový balíček pro přípravu dat s názvem **iris-1.dprep** a otevře se v editoru přípravy dat.

7. Teď provedeme základní přípravu dat. Kliknutím na záhlaví jednotlivých sloupců umožněte úpravu textu záhlaví a přejmenujte názvy sloupců. 

   Pro jednotlivé sloupce zadejte **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** a **Species**.

   ![Přejmenování sloupců](media/tutorial-classifying-iris/rename_column.png)

8. Pokud chcete zjistit počet jedinečných hodnot, vyberte sloupec **Species** a klikněte na něj pravým tlačítkem. Zvolte **Četnost hodnot**. 

   ![Klikněte na Četnost hodnot.](media/tutorial-classifying-iris/value_count.png)

   Tato akce otevře podokno **Kontroly** a zobrazí histogram se čtyři pruhy. Všimněte si že cílový sloupec má tři různé hodnoty **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** a hodnotu **(null)**.

9. Hodnoty null vyfiltrujete tak, že v grafu vyberete pruh reprezentující hodnotu null. Hodnotu **(null)** obsahuje jeden řádek. Pokud se chcete tohoto řádku zbavit, klikněte na tlačítko filtru **-**.

   ![Histogram počtu hodnot](media/tutorial-classifying-iris/filter_out.png)

10. Všimněte si jednotlivých kroků uvedených v podokně **KROKY**. Když jste prováděli přejmenování sloupců a filtrování řádků s hodnotou null, všechny akce se zaznamenaly jako kroky přípravy dat. Jednotlivé kroky můžete upravit. Můžete upravovat nastavení, měnit pořadí kroků a dokonce i odstraňovat jednotlivé kroky.

   ![Kroky](media/tutorial-classifying-iris/steps.png)

11. Teď editor pro přípravu dat zavřete kliknutím na **X** na kartě s pojmenované **iris-1** s ikonou grafu. Vaše práce se automaticky uloží do souboru **iris-1.dprep** zobrazeného pod nadpisem **Příprava dat**.

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

1. Klikněte pravým tlačítkem na soubor **iris-1.dprep**, abyste vyvolali místní nabídku, a zvolte **Vygenerovat soubor s kódem přístupu k datům**. 

   ![generování kódu](media/tutorial-classifying-iris/generate_code.png)

2. Otevře se nový soubor s názvem **iris-1.py**, který obsahuje následující řádky kódu:

   ```python
   # This code snippet will load the referenced package and return a DataFrame.
   # If the code is run in a PySpark environment, the code will return a
   # Spark DataFrame. If not, the code will return a Pandas DataFrame.

   from azureml.dataprep.package import run
   df = run('iris.dprep', dataflow_idx=0)
   ```

   Tento fragment kódu vyvolává logiku, kterou jste vytvořili jako balíček pro přípravu dat. V závislosti na kontextu, ve kterém běží tento kód, může `df` představovat různé druhy datových rámců. [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) se používá při spuštění v modulu Python runtime a [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) se používá při spuštění v kontextu Sparku. 

   Další informace o přípravě dat v aplikaci Azure Machine Learning Workbench najdete v příručce [Začínáme s přípravou dat](data-prep-getting-started.md).

## <a name="next-steps"></a>Další kroky
V této první části třídílné série kurzů jste použili aplikaci Azure Machine Learning Workbench k provedení těchto úloh:
> [!div class="checklist"]
> * Vytvoření nového projektu 
> * Vytvoření balíčku pro přípravu dat
> * Vygenerování kódu Pythonu/PySpark k vyvolání balíčku pro přípravu dat

Jste připraveni přejít k další části série, ve které sestavíte model machine learningu.
> [!div class="nextstepaction"]
> [Sestavení modelu](tutorial-classifying-iris-part-2.md)
