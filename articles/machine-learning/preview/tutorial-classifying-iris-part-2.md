---
title: "Vytvoření modelu pro služby Azure Machine Learning (verze Preview) | Dokumentace Microsoftu"
description: "V tomto kurzu na pokračování se dozvíte, jak komplexně používat služby Azure Machine Learning (verze Preview). Toto je 2. část, která se zabývá experimentováním."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/25/2017
ms.openlocfilehash: 5d86f3bdf19603d2f92fc1a704376beefd7323c0
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="classifying-iris-part-2-build-a-model"></a>Klasifikace Iris, část 2: Sestavení modelu
Služby Azure Machine Learning (verze Preview) představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

Tento kurz je druhou částí z třídílné série. V této části kurzu se naučíte, jak pomocí služeb Azure Machine Learning (verze Preview) provádět následující kroky:

> [!div class="checklist"]
> * Práce v aplikaci Azure Machine Learning Workbench
> * Otevírání skriptů a kontrola kódu
> * Spouštění skriptů v místním prostředí
> * Kontrola historie spuštění
> * Spouštění skriptů v místním prostředí Dockeru
> * Spouštění skriptů v místním okně Azure CLI
> * Spouštění skriptů ve vzdáleném prostředí Dockeru
> * Spouštění skriptů v cloudovém prostředí HDInsight

Tento kurz používá kvůli zjednodušení nadčasovou [datovou sadu Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set). Snímky obrazovky jsou specifické pro systém Windows, ale prostředí v systému macOS je téměř shodné.

## <a name="prerequisites"></a>Požadavky
Měli byste si nejdříve projít první kurz z této série. Před zahájením kroků v tomto kurzu vytvořte na základě [kurzu přípravy dat](tutorial-classifying-iris-part-1.md) prostředky služby Azure Machine Learning a nainstalujte aplikaci Azure Machine Learning Workbench.

Volitelně můžete experimentovat se spouštěním skriptů proti místnímu kontejneru Dockeru. V takovém případě budete potřebovat nainstalovaný modul Docker (Community Edition je dostačující) spuštěný místně na počítači s Windows nebo macOS. Další informace o [pokynech k instalaci Dockeru](https://docs.docker.com/engine/installation/).

Pokud chcete experimentovat s odesláním skriptu ke spuštění v kontejneru Dockeru na vzdáleném virtuálním počítači Azure nebo v clusteru HDInsight Spark, můžete postupovat podle [pokynů k vytvoření virtuálního počítače Azure pro datové vědy založeného na Ubuntu nebo clusteru HDInsight](how-to-create-dsvm-hdi.md).

## <a name="review-irissklearnpy-and-configuration-files"></a>Kontrola skriptu iris_sklearn.py a konfiguračních souborů
1. Spusťte aplikaci **Azure Machine Learning Workbench** a otevřete projekt **myIris**, který jste vytvořili v předchozí části této série kurzů.

2. Jakmile se projekt otevře, klikněte v aplikaci Azure Machine Learning Workbench na tlačítko **Soubory** (ikona složky) na levém panelu nástrojů a otevřete seznam souborů ve složce projektu.

3. Vyberte soubor **iris_sklearn.py** a v aplikaci Workbench se na nové kartě textového editoru otevře kód Python.

   ![otevření souboru](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Kód, který se zobrazí, nemusí být přesně stejný jako předchozí kód, protože tento ukázkový projekt se často aktualizuje.

4. Projděte si kód skriptu Python, abyste se seznámili se stylem kódování. Všimněte si, že skript provádí tyto úlohy:

   - Načte přípravný datový balíček **iris.dprep** pro vytvoření struktury [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Používáme přípravný datový balíček `iris.dprep`, který je součástí ukázkového projektu a který by měl být totožný se souborem `iris-1.dprep` vytvořeným v 1. části tohoto kurzu.

   - Přidá náhodné funkce, aby bylo obtížnější problém vyřešit. (Náhodnost je potřeba, protože Iris je malá datová sada, která se dá snadno klasifikovat s téměř 100% přesností.)

   - Pomocí knihovny strojového učení [scikit-learn](http://scikit-learn.org/stable/index.html) sestaví jednoduchý model logistické regrese. 

   - Pomocí knihovny [pickle](https://docs.python.org/2/library/pickle.html) serializuje model do souboru ve složce `outputs`, pak ho načte a deserializuje zpátky do paměti.

   - Pomocí deserializovaného modelu vytvoří předpověď nového záznamu. 

   - Pomocí knihovny [matplotlib](https://matplotlib.org/) vytvoří dva grafy – chybovou matici a křivku ROC s více třídami – a uloží je do složky `outputs`.

   - V celém průběhu se pomocí objektu `run_logger` zaznamenává do protokolů míra regularizace a přesnost modelu a protokoly se automaticky promítají do historie spouštění.


## <a name="execute-irissklearnpy-script-in-local-environment"></a>Spuštění skriptu iris_sklearn.py v místním prostředí

Teď se připravíme na první spuštění skriptu **iris_sklearn.py**. Tento skript vyžaduje balíčky scikit-learn a matplotlib. Balíček **scikit-learn** už nainstalovala aplikace Azure ML Workbench. Ještě ale zbývá nainstalovat balíček **matplotlib**. 

1. V aplikaci Azure Machine Learning Workbench klikněte na nabídku **Soubor** a výběrem možnosti **Otevřít příkazový řádek** spusťte příkazový řádek. Toto okno rozhraní příkazového řádku označujeme jako okno CLI aplikace Azure Machine Learning Workbench nebo zkráceně jenom jako okno CLI.

2. Do okna CLI zadejte následující příkaz, který slouží k instalaci balíčku **matplotlib** jazyka Python. Mělo by to trvat necelou minutu.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Pokud výše uvedený příkaz `pip install` přeskočíte, kód ve skriptu `iris_sklearn.py` se úspěšně spustí, ale nevytvoří výstup v podobě grafů chybové matice a křivky ROC s více třídami, který je vidět ve vizualizacích historie.

3. Vraťte se do okna aplikace Workbench. 

4. V levém horním rohu karty **iris_sklearn.py** vedle ikony uložení klikněte na rozevírací nabídku a vyberte možnost **Konfigurace spuštění**.  Jako prostředí pro spuštění zvolte možnost **local** a vyberte ke spuštění skript `iris_sklearn.py`.

5. Pak se přesuňte do pravé části stejné karty a do pole **Argumenty** zadejte hodnotu `0.01`. 

   ![obrázek](media/tutorial-classifying-iris/run_control.png)

6. Klikněte na tlačítko **Spustit**. Okamžitě se naplánuje úloha. Tato úloha se objeví na panelu **Úlohy** na pravé straně okna aplikace Workbench. 

7. Po chvíli se stav úlohy změní z **Odesílání** na **Spuštěno** a za chvíli na **Dokončeno**.

   ![spuštění skriptu sklearn](media/tutorial-classifying-iris/run_sklearn.png)

8. V textu stavu úlohy na panelu Úlohy klikněte na slovo **Dokončeno**. Zobrazí se automaticky otevírané okno s textem standardního výstupu (stdout) spuštěného skriptu. Pokud chcete text standardního výstupu zavřít, klikněte na tlačítko **X** v pravém horním rohu automaticky otevíraného okna.

9. Ve stejném stavu úlohy na panelu Úlohy klikněte na modrý text **iris_sklearn.py [n]** (_n_ je číslo spuštění) hned nad stavem **Dokončeno** a časem spuštění. Otevře se stránka **vlastností spuštění** a zobrazí se informace o vlastnostech spuštění, **výstupní** soubory, všechny **vizualizace** a **protokoly** z tohoto konkrétního spuštění. 

   Když se spuštění dokončí, v automaticky otevíraném okně se zobrazí tyto výsledky:

   >[!NOTE]
   >Vzhledem k tomu, že jsme u cvičné sady použili náhodnost, se můžou vaše přesné výsledky mírně lišit.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details page.
   ```

10. Zavřete kartu **vlastností spuštění** a vraťte se na kartu **iris_sklearn.py**. 

11. Opakujte další spuštění. 

    Do pole **Argumenty** zadejte řadu různých číselných hodnot od `0.001` do `10`. Kliknutím na tlačítko **Spustit** ještě několikrát spusťte kód. Hodnota argumentu, kterou pokaždé změníte, se předává algoritmu logistické regrese v kódu, takže se pokaždé zobrazí jiné výsledky.

## <a name="review-run-history-in-detail"></a>Podrobná kontrola historie spuštění
V aplikaci Azure Machine Learning Workbench se každé spuštění skriptu zaznamená v podobě záznamu historie spuštění. Historii spuštění určitého skriptu si můžete prohlédnout v zobrazení **Spuštění**.

1. Kliknutím na tlačítko **Spuštění** (ikona hodin) na levém panelu nástrojů otevřete seznam **Spuštění**. Potom kliknutím na možnost **iris_sklearn.py** zobrazíte **řídicí panel spuštění** skriptu `iris_sklearn.py`.

   ![obrázek](media/tutorial-classifying-iris/run_view.png)

2. Otevře se karta **řídicího panelu spuštění**. Zkontrolujte statistiky zaznamenané během různých spuštění. Grafy se vykreslují v horní části karty a tabulka ve spodní části stránky obsahuje všechna očíslovaná spuštění s podrobnostmi.

   ![obrázek](media/tutorial-classifying-iris/run_dashboard.png)

3. Tabulku můžete filtrovat a interaktivně klikat na grafy, abyste zobrazili stav, dobu trvání, přesnost a míru regularizace jednotlivých spuštění. 

4. V tabulce **Spuštění** vyberte dvě nebo tři spuštění a kliknutím na tlačítko **Porovnat** otevřete stránku podrobného porovnání. Projděte si porovnání údajů vedle sebe. Kliknutím na tlačítko Zpět pro **seznam spuštění** v levém horním rohu stránky porovnání se vraťte na **řídicí panel spuštění**.

5. Kliknutím na určité spuštění otevřete jeho podrobné zobrazení. Všimněte si, že v části _vlastností spuštění_ se zobrazí statistika vybraného spuštění. Část **Výstup** obsahuje soubory zapsané do složky výstupu a je možné je stáhnout.

   ![obrázek](media/tutorial-classifying-iris/run_details.png)

   V sekci **Vizualizace** se vykreslí dva grafy, chybová matice a křivka ROC s více třídami. Všechny soubory protokolu najdete také v části **Protokoly**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Spouštění skriptů v místním prostředí Dockeru

Služba Azure ML umožňuje snadno konfigurovat další prostředí pro spouštění, jako je Docker, a spouštět v těchto prostředích skripty. 

>[!IMPORTANT]
>K provedení tohoto kroku je potřeba mít v místním počítači nainstalovaný a spuštěný modul Docker. Další podrobnosti najdete v instalační příručce.

1. Výběrem ikony složky na levém panelu nástrojů otevřete seznam **Soubory** pro váš projekt. Rozbalte složku `aml_config`. 

2. Všimněte si, že je tu několik předkonfigurovaných prostředí, například **docker-python**, **docker-spark** a **local**. 

   Každé prostředí má dva soubory, například `docker-python.compute` a `docker-python.runconfig`. Otevřete oba typy souborů a všimněte si, že v textovém editoru se dají konfigurovat některé možnosti.  

   Zavřete (X) karty všech otevřených textových editorů.

3. Spusťte skript **iris_sklearn.py** v prostředí **docker-python**. 

   - Na levém panelu nástrojů otevřete kliknutím na ikonu hodin panel **Spuštění**. Klikněte na **Všechna spuštění**. 
   - V horní části karty **Všechna spuštění** vyberte místo výchozího prostředí **local** cílové prostředí **docker-python**. 
   - Potom se posuňte doprava a jako skript ke spuštění vyberte možnost **iris_sklearn.py**. 
   - Pole **Argumenty** nechte prázdné, protože skript určuje výchozí hodnotu. 
   - Klikněte na tlačítko **Spustit**.

4. Všimněte si, že na panelu **Úlohy** v pravé části okna aplikace Workbench se zobrazí spuštění nové úlohy.

   Pokud spouštíte skript v prostředí Docker poprvé, bude zpracování trvat o několik minut déle. 

   Aplikace Azure Machine Learning Workbench na pozadí sestaví nový soubor Docker odkazující na základní image Dockeru zadanou v souboru `docker.compute` a závislé balíčky Pythonu zadané v souboru `conda_dependencies.yml`. Modul Docker potom stáhne základní image z Azure, nainstaluje balíčky Pythonu zadané v souboru `conda_dependencies.yml` a spustí kontejner Docker. Potom zkopíruje místní kopii (nebo na ni podle konfigurace spuštění vytvoří odkaz) složky projektu a spustí skript `iris_sklearn.py`. Nakonec by se vám měl zobrazit naprosto stejný výsledek jako při použití prostředí **local**.

5. Teď vyzkoušíme prostředí Spark. Základní image Dockeru obsahuje předinstalovanou a nakonfigurovanou instanci prostředí Spark. Z toho důvodu v ní můžete spustit skript PySpark. Je to jednoduchý způsob, jak vyvíjet a testovat program v jazyce Spark, aniž byste museli ztrácet čas instalací a konfigurací prostředí Spark. 

   Otevřete soubor `iris_pyspark.py`. Tento skript načte datový soubor `iris.csv` a klasifikuje datovou sadu Iris pomocí algoritmu logistické regrese z knihovny Spark ML. Teď změňte prostředí pro spuštění na **docker-spark** a skript na **iris_pyspark.py** a znovu ho spusťte. Trvá to o něco déle, protože je potřeba vytvořit relaci Spark a spustit ji uvnitř kontejneru Docker. Můžete si taky všimnout, že standardní výstup se liší od standardního výstupu skriptu `iris_pyspark.py`.

6. Proveďte pár dalších spuštění a vyzkoušejte různé argumenty. 

7. Otevřete soubor `iris_pyspark.py` a podívejte se na jednoduchý logistický regresní model sestavený pomocí knihovny Spark ML. 

8. Vyzkoušejte si práci s panelem **Úlohy**, zobrazením seznamu historie spuštění a podrobným zobrazením vašich spuštění v různých prostředích pro spuštění.

## <a name="execute-scripts-in-the-azure-ml-cli-window"></a>Spouštění skriptů v okně Azure ML CLI

1. V aplikaci Azure Machine Learning Workbench otevřete kliknutím na nabídku **Soubor** a potom na položku **Otevřít příkazový řádek** okno příkazového řádku. Příkazový řádek se spustí ve složce projektu s příkazem `C:\Temp\myIris\>`.

   >[!Important]
   >Okno příkazového řádku (spuštěné z aplikace Workbench) musíte použít k provedení následujících kroků:

2. Pomocí příkazového řádku (CLI) se přihlaste k Azure. 

   Aplikace Workbench a CLI používají při ověřování prostředků Azure nezávislé mezipaměti přihlašovacích údajů. Tento krok je potřeba provést jenom jednou, než vyprší token v mezipaměti. Příkaz **az account list** vrátí seznam předplatných dostupných pro vaše přihlášení. Pokud jich je víc, použijte hodnotu ID z požadovaného předplatného a nastavte ji jako výchozí účet k použití pomocí příkazu **az set account -s**, u kterého zadáte hodnotu ID předplatného. Potom nastavení ověřte pomocí příkazu account show.

   ```azurecli
   REM login using aka.ms/devicelogin site.
   az login
   
   REM list all Azure subscriptions you have access to. 
   az account list -o table
   
   REM set the current Azure subscription to the one you want to use.
   az set account -s <subscriptionId>
   
   REM verify your current subscription is set correctly
   az account show
   ```

3. Jakmile proběhne ověření a nastavení aktuálního kontextu předplatného Azure, zadáním následujících příkazů do okna CLI nainstalujte balíček matplotlib a odešlete skript v jazyce Python jako experiment ke spuštění.

   ```azurecli
   REM You don't need to do this if you have installed matplotlib locally from the previous steps.
   pip install matplotlib
   
   REM Kick off an execution of the iris_sklearn.py file against local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Zkontrolujte výstup. Všimněte si, že je výstup a výsledek stejný jako v předchozích případech v tomto kurzu, kdy jste ke spuštění skriptu používali aplikaci Workbench. 

5. Pokud máte v počítači nainstalovaný Docker, spusťte stejný skript pomocí spouštěcího prostředí Docker.

   ```azurecli
   REM Execute iris_sklearn.py in local Docker container Python environment.
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM Execute iris_pyspark.py in local Docker container Spark environment.
   az ml experiment submit -c docker-spark .\iris_pyspark.py 0.1
   ```
6. V aplikaci Azure Machine Learning Workbench kliknutím na ikonu složky na levém panelu nástrojů zobrazte seznam souborů projektu a otevřete skript Python s názvem **run.py**. 

   Tento skript slouží k vytvoření smyčky s různými mírami regularizace a vícenásobnému spuštění experimentu s těmito mírami. Tento skript spustí úlohu `iris_sklearn.py` s mírou regularizace `10.0` (absurdně vysoké číslo), v následujícím spuštění tuto míru o polovinu sníží a tak dále, dokud nedosáhne hodnoty `0.005`. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Ke spuštění skriptu **run.py** z příkazového řádku spusťte následující příkazy:

   ```cmd
   REM Submit iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Když se skript `run.py` dokončí, uvidíte v zobrazení seznamu historie spuštění v aplikaci Azure Machine Learning Workbench graf.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Spuštění v kontejneru Docker ve vzdáleném počítači
Pokud chcete skript spustit v kontejneru Docker ve vzdáleném počítači s Linuxem, potřebujete přístup SSH (uživatelské jméno a heslo) do tohoto vzdáleného počítače. Ve vzdáleném počítači musí být nainstalovaný a spuštěný modul Docker. Nejjednodušší způsob, jak takový počítač s Linuxem získat, spočívá ve vytvoření [virtuálního počítače pro datovou vědu se systémem Ubuntu](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) v Azure. (Všimněte si, že není dostupná podpora virtuálních počítačů pro datovou vědu se systémem CentOS.) 

1. Po vytvoření virtuálního počítače můžete tento virtuální počítač připojit jako prostředí pro spuštění tím, že pomocí níže uvedeného příkazu vygenerujete dvojici souborů `.runconfig` a `.compute`. Nové prostředí si pojmenujeme `myvm`.
 
   ```azurecli
   REM create myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >Oblastí IP adresy může být také veřejně adresovatelný plně kvalifikovaný název domény, například `vm-name.southcentralus.cloudapp.azure.com`. Osvědčilo se přidat do virtuálního počítače pro datovou vědu plně kvalifikovaný název domény a používat ho místo IP adresy, protože z důvodu úspory nákladů můžete někdy chtít virtuální počítač vypnout. Kromě toho se může při příštím spuštění virtuálního počítače IP adresa změnit.

   Potom spusťte následující příkaz pro vytvoření image Dockeru ve virtuálním počítači, abyste ho připravili na spouštění skriptů.
   
   ```azurecli
   REM prepare the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >Můžete také změnit hodnotu položky `PrepareEnvironment` v souboru `myvm.runconfig` z výchozí hodnoty `false` na `true`. Tím automaticky připravíte kontejner Docker pro první spuštění.

2. Upravte vygenerovaný soubor `myvm.runconfig` ve složce `aml_config` a změňte rozhraní z výchozí hodnoty `PySpark` na `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Pokud necháte nastavení rámce na hodnotě PySpark, mělo by také fungovat správně. Toto nastavení je ale trochu neefektivní, pokud doopravdy nepotřebujete, aby váš skript Python spouštěla relace Spark.

3. V okně CLI zadejte stejný příkaz jako předtím, jenom tentokrát s cílem _myvm_:
   ```azurecli
   REM execute iris_sklearn.py in remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Příkaz se spustí stejně, jako když používáte prostředí `docker-python`, jenže tentokrát se spustí ve vzdáleném virtuálním počítači s Linuxem. V okně CLI se zobrazí stejné výstupní informace.

4. Teď si vyzkoušíme Spark v kontejneru. Otevřete Průzkumníka souborů (pokud znáte základní příkazy pro manipulaci se soubory, můžete to také udělat z okna CLI). Vytvořte kopii souboru `myvm.runconfig` a pojmenujte ji `myvm-spark.runconfig`. Upravte nový soubor tak, že změníte nastavení `Framework` z hodnoty `Python` na `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Neprovádějte žádné změny souboru `myvm.compute`. Pro provádění příkazu Spark se používá stejná image Dockeru ve stejném virtuálním počítači. V novém souboru `myvy-spark.runconfig` pole `target` odkazuje na stejný soubor `myvm.compute` prostřednictvím jeho názvu `myvm`.

5. Zadáním následujícího příkazu ho spusťte v instanci Spark ve vzdáleném kontejneru Docker:
   ```azureli
   REM execute iris_pyspark.py in Spark instance on remote Docker container
   az ml experiment submit -c myvm-spark .\iris_pyspark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Spuštění skriptu v clusteru služby HDInsight
Tento skript můžete také spustit ve skutečném clusteru Spark. 

1. Pokud máte přístup ke clusteru Spark pro Azure HDInsight, vygenerujte příkaz pro spuštění konfigurace HDI podle uvedeného příkladu. Jako parametry zadejte název clusteru HDInsight, vaše uživatelské jméno HDInsight a heslo. Použijte následující příkaz:

   ```azurecli
   REM create a compute target that points to a HDI cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepare the HDI cluster
   az ml experiment prepare -c myhdi
   ```

   Plně kvalifikovaný název domény hlavního uzlu clusteru je většinou `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >`username` je uživatelské jméno SSH clusteru. Výchozí hodnota je `sshuser`, pokud ji nezměníte při zřizování HDI. Není to `admin`, což je druhý uživatel vytvořený při zřizování za účelem povolení přístupu na web správce clusteru. 

2. Zadejte následující příkaz a skript se spustí v clusteru HDInsight:

   ```azurecli
   REM execute iris_pyspark on the HDI cluster
   az ml experiment submit -c myhdi .\iris_pyspark.py
   ```

   >[!NOTE]
   >Při spouštění ve vzdáleném clusteru HDI můžete také pomocí uživatelského účtu `admin` zobrazit na adrese `https://<cluster_name>.azurehdinsight.net/yarnui` podrobnosti provádění úlohy YARN.


## <a name="next-steps"></a>Další kroky
V této druhé části třídílné série kurzů jste se naučili, jak pomocí služeb Azure Machine Learning provádět tyto úlohy:
> [!div class="checklist"]
> * Práce v aplikaci Azure Machine Learning Workbench
> * Otevírání skriptů a kontrola kódu
> * Spouštění skriptů v místním prostředí
> * Kontrola historie spuštění
> * Spouštění skriptů v místním prostředí Dockeru
> * Spouštění skriptů v místním okně Azure CLI
> * Spouštění skriptů ve vzdáleném prostředí Dockeru
> * Spouštění skriptů v cloudovém prostředí HDInsight

Teď můžete přejít ke třetí části série kurzů. Vytvořili jste logistický regresní model, takže ho teď můžeme nasadit jako webovou službu v reálném čase.

> [!div class="nextstepaction"]
> [Nasazení modelu](tutorial-classifying-iris-part-3.md)
