---
title: "Hloubkové learning pro prediktivní údržby reálných scénářů - Azure | Microsoft Docs"
description: "Zjistěte, jak replikovat kurz na hloubkové learning pro prediktivní údržby pomocí Azure Machine Learning Workbench."
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 6019437763f82fa14b8677b6c7ec7f0c6938fda7
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Hloubkové learning pro scénářích reálného světa prediktivní údržby

Hloubkové learning je jednou z nejčastěji používané trendy v machine learning. Hloubkové učení se používá v mnoha pole a aplikace, včetně standardizovaných aut, rozpoznávání řeči a bitové kopie, robotics až po finanční. Hloubkové learning je sada algoritmy, které je INSPIROVANÉ tvar mozku (biologické neuronové sítě) a strojové učení. Kognitivní vědců obvykle naleznete hloubkové learning jako umělý neuronové sítě (ANNs).

Prediktivní údržby je také oblíbených. V prediktivní údržby jsou navrženy mnoho různých technik sloužící k určení stavu zařízení a k předpovědi by se měla provést údržbu. Některá běžná použití služby prediktivní údržby jsou předpovědi selhání, selhání diagnostiky (provést analýzu hlavní příčiny), detekce chyb, selhání typ klasifikace a doporučení zmírnění nebo údržby akcí po selhání.

Ve scénářích prediktivní údržby data jsou shromažďována v čase sledovat stav zařízení. Cílem je najít vzorů, které můžou pomoct předpovědi a nakonec zabránit chybám. Pomocí [dlouho paměti krátké termín (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sítěmi je přímým učení metod, které jsou obzvláště přitažlivými v prediktivní údržby. LSTM sítě jsou dobře při učení se z pořadí. Časové řady údaje slouží k podívejte se nahoru na delší období rozpoznat vzory selhání.

V tomto kurzu jsme sestavení síť LSTM pro sadu dat a scénáře, které jsou popsány v [prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3). Používáme sítí k předvídání zbývající dobu životnosti letecké motory. Šablona používá hodnoty čidel simulované letadla k předvídání, když se v budoucnu nezdaří leteckého motoru. Pomocí této předpovědi, údržby můžou být plánované předem, aby se zabránilo selhání.

Tento kurz používá [Keras](https://keras.io/) hloubkové učení knihovny a kognitivní nástrojů Microsoft [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) jako back-end.

Veřejné úložiště GitHub, který má ukázky v tomto kurzu je na [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Přehled případu použití

Tento kurz používá jako příklad simulované letadla modul spustit selhání události k předvedení prediktivní údržby modelování procesu. 

Implicitní předpokládá modelování dat popsané v tomto poli je, že asset má své činnosti vzor snížení výkonu. Vzor se odrazí v měření senzor assetu. Prověřením hodnoty čidel assetu časem algoritmu strojového učení další vztah mezi hodnoty čidel, změny v hodnoty čidel a historických selhání. Tento vztah se používá k předvídání selhání v budoucnu. 

Doporučujeme prozkoumat formát dat a dokončit všechny tři kroky šablony předtím, než je ukázková data nahradit vlastní data.

## <a name="prerequisites"></a>Požadavky

- [Účet Azure](https://azure.microsoft.com/free/) (bezplatné zkušební verze jsou k dispozici).
- Azure Machine Learning Workbench, s vytvořen pracovní prostor.
- Pro model operationalization: Azure Machine Learning Operationalization, s prostředím místní nasazení nastavení a [účet Azure Machine Learning Model správy](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview).

## <a name="create-a-new-workbench-project"></a>Vytvoření nového projektu Workbench

Vytvořte nový projekt v tomto příkladu jako šablona:

1. Otevřete strojového učení Workbench.
2. Na **projekty** vyberte  **+** a potom vyberte **nový projekt**.
3. V **vytvořit nový projekt** podokně, zadejte informace pro nový projekt.
4. V **šablony projektů vyhledávání** pole pro vyhledávání, zadejte **prediktivní údržby**a potom vyberte šablonu.
5. Vyberte **Vytvořit**.

## <a name="prepare-the-notebook-server-computation-target"></a>Příprava cílového výpočetní server poznámkového bloku

V místním počítači, na nástroje Machine Learning Workbench **soubor** nabídce vyberte buď **spusťte příkazový řádek** nebo **otevřete PowerShell**. V okně příkazového řádku možnosti, který zvolíte spusťte následující příkazy:

`az ml experiment prepare --target docker --run-configuration docker`

Doporučujeme serverem poznámkového bloku na standardní DS4_V2 [datové vědy virtuálního počítače (DSVM) pro Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Po DSVM nastaveno, spusťte následující příkazy k přípravě imagí Dockeru:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Pomocí Docker bitových kopií připravené otevřete server poznámkového bloku Jupyter. Otevřete Poznámkový blok Jupyter serveru, buď přejděte do nástroje Machine Learning Workbench **poznámkových bloků** kartě nebo spuštění webového serveru:`az ml notebook start`

Poznámkové bloky jsou uložené v adresáři kódu v prostředí Jupyter. Spusťte tyto poznámkových bloků postupně jako číslované, počínaje Code\1_data_ingestion_and_preparation.ipynb.

Vyberte jádra odpovídající vaší _Template [název_projektu] [connection_name], a potom vyberte **nastavit jádra**.

## <a name="data-description"></a>Popis dat

Šablona používá tři datových sad jako vstupy, v souborech PM_train.txt, PM_test.txt a PM_truth.txt.

-  **Cvičení data**: letadla modul data spustit selhání. Data train (PM_train.txt) se skládá z několika, multivariační časové řady s *cyklus* jako časovou jednotku. Obsahuje 21 odečty snímačů pro každý cyklus. 

    Každý časové řady může předpokládá, že se generují z jiný modul stejného typu. Každý motor se předpokládá, že spuštění různých stupňů počáteční opotřebení a výrobní variace. Tyto informace Neznámý uživateli. 

    V těchto datech simulované modul se předpokládá, že na začátku každé časové řady pracovat normálně. Začne snižovat v určitém okamžiku během řadu provozní cykly. Snížení hodnoty a zvětšování v rozsahem. 

    Když je dosaženo předdefinované prahovou hodnotu, modul považovány za nebezpečné pro další operace. V každé časové řady posledního cyklu lze považovat za bod selhání odpovídající stroje.

-   **Testovacích dat**: letadla modul provozních dat bez zaznamenaných událostí selhání. Testovací data (PM_test.txt) mají stejné schéma dat, jako jsou Cvičná data. Jediným rozdílem je, že data neindikuje, když dojde k selhání (poslední časové období nemá *není* představují bodem selhání). Není znám, kolik více cyklů tento modul může poslední předtím, než se nezdaří.

- **Data pravdivosti**: true zbývající informace cyklů pro každý modul v testovacích datech. Data pravdivosti základů poskytuje počet cyklů zbývající práce pro moduly v testovacích datech.

## <a name="scenario-structure"></a>Struktura scénář

Obsah pro tento scénář je k dispozici v [úložiště GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance). 

Soubor [readme] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md) v úložišti, popisuje procesy, od přípravy data a k vytváření a zprovozňování modelu. Tři Jupyter notebooks jsou k dispozici ve složce [kód] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) v úložišti. 

V dalším kroku jsme popisují podrobné scénář pracovního postupu.

### <a name="task-1-data-ingestion-and-preparation"></a>Úloha 1: Přijímání dat a příprava

Blok Jupyter přijímání dat v Code/1_data_ingestion_and_preparation.ipnyb načte tři vstupních datových sad do Pandas dataframe formátu. Potom ho připraví data pro modelování a nemá některé vizualizace dat předběžné. Data se potom převede do formátu PySpark a uložené v kontejner úložiště objektů Blob v Azure ve vašem předplatném Azure pro použití v dalším úkolem modelování.

### <a name="task-2-model-building-and-evaluation"></a>Úloha 2: Vytváření modelů a vyhodnocení

Poznámkovým blokem Jupyter sestavení modelu v Code/2_model_building_and_evaluation.ipnyb čtení PySpark trénování a testování sad dat z úložiště objektů Blob. Pak je síť LSTM vytvořené s nástroji sady dat školení. Výkon modelu se měří v testovací sadě. Výsledný model je serializované a uložená v kontextu místního výpočetní pro použití v úloze operationalization.

### <a name="task-3-operationalization"></a>Úloha 3: Operationalization

Operationalization Poznámkový blok Jupyter v Code/3_operationalization.ipnyb používá uložené model vytvářet funkce a schéma, které jsou požadovány pro volání modelu v Azure hostovaná webové služby. Poznámkového bloku testuje funkce a potom Zpráva prolétne (zkomprimuje) operationalization prostředky do zip souboru.

Soubor ZIP obsahuje tyto soubory:

- **modellstm.JSON**: soubor definice schématu pro nasazení. 
- **lstmscore.PY**: **init()** a **run()** funkce, které jsou vyžadované Azure webovou službou.
- **lstm.model**: adresář definice modelu.

Poznámkového bloku testy funkce pomocí definice modelu předtím, než ho balíčky operationalization prostředky pro nasazení. Pokyny pro nasazení jsou zahrnuty na konci poznámkového bloku.


## <a name="conclusion"></a>Závěr

Tento kurz používá jednoduchého scénáře, ve které pouze jeden datový zdroj (hodnoty čidel) se používá k zajištění předpovědi. V další pokročilé scénáře prediktivní údržby, jako je třeba [prediktivní údržby modelování R Poznámkový blok průvodce](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), velký počet zdrojů dat se dají používat. Ostatní zdroje dat může obsahovat záznamy Historie údržby, protokoly chyb a počítač a operátor funkce. Dalších zdrojů dat může vyžadovat různé typy ošetření, který se má použít v sítích hloubkové učení. Je také důležité pro optimalizaci modely pro správné parametry, jako je pro velikost okna. 

Můžete upravit odpovídající části tohoto scénáře a pokuste se použít jiný problém scénářů, například těm, které jsou popsané v [prediktivní údržby modelování průvodce](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1), který zahrnuje několik dalších datových zdrojů.


## <a name="references"></a>Odkazy

- [Šablona řešení prediktivní údržby](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Průvodce modelováním prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Prediktivní údržby modelování Poznámkový blok průvodce Python](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [Prediktivní údržby pomocí PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

