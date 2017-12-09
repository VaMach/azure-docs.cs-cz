---
title: "Přímý učení pro scénářem z reálného prostředí prediktivní údržby - Azure | Microsoft Docs"
description: "Tento dokument popisuje, jak replikovat hloubkové Learning kurzu prediktivní údržby pomocí Azure Machine Learning Workbench"
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 8997e38a81ed848c9e052ab08566ffc99560ed86
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Přímý učení pro scénářem z reálného prostředí prediktivní údržby

Hloubkové learning je jednou z nejčastěji používané trendy v strojového učení místa v současné době a existuje mnoho pole a aplikace kde vystupoval, jako je například standardizovaných aut, rozpoznávání řeči a bitové kopie, robotics a finanční. Hloubkové learning je sada algoritmů, které je INSPIROVANÉ obrazec naše mozku (biologické neuronové sítě) a machine learningu a kognitivní vědců obvykle na ni odkazuje jako umělý Neuronové sítě (ANN).

Prediktivní údržby je také velmi oblíbených oblasti, kde jsou navrženy mnoho různých technik sloužící k určení stavu zařízení, aby bylo možné předpovědět při provádění údržby musí být. Prediktivní Údržba zahrnuje celou řadu témata, včetně mimo jiné: selhání předpovědi, selhání diagnostiky (Analýza hlavní příčiny), detekce chyb, selhání typ klasifikace a doporučení zmírnění nebo údržby akcí po došlo k chybě.

Ve scénářích prediktivní údržby data jsou shromažďována v čase sledovat stav zařízení s posledním cílem vyhledávání vzorů k předvídání selhání. Mezi hloubkového učení metody [dlouho paměti krátké termín (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sítě jsou obzvláště přitažlivými k doméně prediktivní údržby, vzhledem k tomu, že jsou velmi dobře při učení se z pořadí. Tuto skutečnost slouží k jejich aplikací pomocí časových řad dat tím, že ji možné do minulosti pro delší časové období pro zjišťování selhání struktur.

V tomto kurzu jsme sestavení síť LSTM pro datovou sadu a scénář popsaný v [prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) k předvídání zbývající dobu životnosti letecké motory. Šablona v souhrnu, používá hodnoty čidel simulované letadla k předpovědi leteckého motoru selže v budoucnu, tak, aby předem může být plánované údržby.

Tento kurz používá [keras](https://keras.io/) hloubkové learning knihovna Microsoft kognitivní Toolkit [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) jako back-end.

## <a name="link-to-the-gallery-github-repository"></a>Propojit s úložišti GitHub Galerie 

Toto je odkaz na veřejné úložiště GitHub: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)

## <a name="use-case-overview"></a>Přehled případu použití

Tento kurz používá jako příklad simulované letadla modul spustit selhání události k předvedení prediktivní údržby modelování procesu. Implicitní předpokládá modelování dat, jak provést níže je, že asset zájmu má své činnosti vzoru snížení se odrazí v měření senzor assetu. Prověřením hodnoty čidel assetu časem algoritmu strojového učení další vztah mezi hodnoty čidel a změny v hodnoty čidel na historické selhání, aby bylo možné v budoucnu předpovědi selhání. Doporučujeme zkoumání formát dat a projít všechny tři kroky šablony před výměnou dat vlastními.

## <a name="prerequisites"></a>Požadavky

- [Účet Azure](https://azure.microsoft.com/free/) (bezplatné zkušební verze jsou k dispozici).
- Nainstalované kopie Azure Machine Learning Workbench s vytvořen pracovní prostor.
- Pro model operationalization: Azure Machine Learning Operationalization s instalaci prostředí z místního nasazení a [modelu účet pro správu](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Vytvoření nového projektu workbench

Vytvořte nový projekt v tomto příkladu jako šablona:

1. Otevřete Azure Machine Learning Workbench
2. Na stránce projekty, klikněte + přihlásit a vybrat nový projekt
3. V podokně vytvořit nový projekt zadejte informace pro nový projekt
4. Do vyhledávacího pole šablony projektů vyhledávání zadejte "Prediktivní Údržba" a vyberte šablonu
5. Kliknutí na Vytvořit

## <a name="prepare-the-notebook-server-computation-target"></a>Příprava cílového výpočetní server poznámkového bloku

Ke spuštění na místním počítači, z nástroje Workbench AML `File` nabídce vyberte buď `Open Command Prompt` nebo `Open PowerShell` rozhraní příkazového řádku. V rámci rozhraní příkazového řádku systému windows spusťte následující příkazy:

`az ml experiment prepare --target docker --run-configuration docker`

 Doporučujeme systémem DS4_V2 standard [datové vědy virtuálního počítače pro Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Jakmile je nakonfigurovaný DSVM, budete muset spustit následující dva příkazy:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

S imagí dockeru _připravený_, otevřete server poznámkového bloku jupyter, buď v rámci *AML Workbench* poznámkových bloků kartě nebo spuštění webového serveru, spusťte:`az ml notebook start`

- Poznámkové bloky, které jsou uložené v `Code` adresář nalézt v prostředí Jupyter. Spustíme tyto poznámkových bloků postupně jako číslované, spouštění na (`Code\1_data_ingestionand_and_preparation.ipynb`).

- Vyberte jádra odpovídající vaší _Template [název_projektu] [Desired_Connection_Name] a klikněte na tlačítko Nastavit jádra

## <a name="data-description"></a>Popis dat

Šablona má tři datové sady jako vstupy: "PM_train.txt", "PM_test.txt" a "PM_truth.txt"
1. Cvičení dat: je dat spustit selhání motoru letadla. Data train ("PM_train.txt") se skládá z několika multivariační časové řady s "cyklus" jako časovou jednotku, společně s 21 odečty snímačů pro každý cyklus. Dá se předpokládat každé časové řady jako generován z jiný modul stejného typu. Každý motor se předpokládá, že má začít s různým stupněm počáteční opotřebení a výrobní variace a tyto informace Neznámý uživateli. V těchto datech simulované modul se předpokládá, že na začátku každé časové řady pracovat normálně. Začne snižovat v určitém okamžiku během řadu provozní cykly. Snížení hodnoty a zvětšování v rozsahem. Když je dosaženo předdefinované prahovou hodnotu, modul považuje unsafe pro další operace. Jinými slovy posledního cyklu v každé časové řady lze považovat za bod selhání odpovídající stroje.

2. Testovacích dat: je leteckého motoru provozní údaje bez zaznamenaných událostí selhání. Testovací data ("PM_test.txt") mají stejné schéma dat, jako jsou Cvičná data. Jediným rozdílem je, že data neindikuje, když dojde k selhání (jinými slovy, poslední časové období nepředstavuje bodem selhání). Není znám, kolik více cyklů tento modul může poslední předtím, než se nezdaří.

3. Data pravdivosti: obsahuje informace true zbývající cyklů pro každý modul v testovacích datech. Data pravdivosti základů poskytuje počet cyklů zbývající práce pro moduly v testovacích datech.

## <a name="scenario-structure"></a>Struktura scénář

Obsah pro tento scénář je k dispozici v [úložišti GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

V úložišti, je soubor na [Readme] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md), který popisuje procesy od přípravy data dokud vytváření a zprovozňování model. Tři Jupyter notebooks jsou k dispozici ve složce [kód] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) v rámci úložiště. 

Potom jsme popisují podrobné scénář pracovního postupu:

### <a name="task-1-data-ingestion--preparation"></a>Úloha 1: Přijímání dat & přípravy

Blok Jupyter přijímání dat v `Code/1_data_ingestion_and_preparation.ipnyb` zatížení tří vstupní datové sady do `Pandas` dataframe formát, jsou data připravena pro modelování část a nemá některé vizualizace dat předběžné. Data se potom převede na `PySpark` formátování a uložené v kontejner úložiště objektů Blob v Azure na vaše předplatné pro použití v dalším úkolem modelování.

### <a name="task-2-model-building--evaluation"></a>Úloha 2: Vytváření modelů & vyhodnocení

Blok Jupyter sestavení modelu v `Code/2_model_building_and_evaluation.ipnyb` , který čte `PySpark` trénování a testování sad dat z úložiště objektů blob. Pak je síť LSTM vytvořené s nástroji školení datových sad. Výkon modelu se měří v testovací sadě. Výsledný model je serializované a uložená v kontextu místního výpočetní pro použití v úloze operationalization.

### <a name="task-3-operationalization"></a>Úloha 3: Operationalization

Operationalization Poznámkový blok Jupyter v `Code/3_operationalization.ipnyb` , trvá uložené modelu a sestavení požadované funkce a schéma pro volání modelu v Azure hostovaná webové služby. Poznámkového bloku testuje funkce a Zpráva prolétne operationalization prostředky do souboru zip, který je také uložen v kontejnerech úložiště objektů Blob Azure.
Soubor ZIP obsahuje:

- `service_schema.json`Soubor definice schématu pro nasazení. 
- `lstmscore.py`Funkce init() a run() vyžadované Azure webovou službou
- `lstmfull.model`Adresář definice modelu.

Poznámkového bloku testy funkce s definicí modelu před zabalení operationalization prostředky pro nasazení. Pokyny pro nasazení jsou zahrnuty na konci poznámkového bloku.


## <a name="conclusion"></a>Závěr

V tomto kurzu slouží jako vodítko pro začátečníky chtějí použít hloubkové učení v doméně prediktivní údržby v rámci prostředí Poznámkový blok Jupyter v *Azure Machine Learning Workbench*. Tento kurz používá jednoduchého scénáře, kde se používá jenom jeden zdroj dat (hodnoty čidel) k zajištění předpovědi. V další pokročilé scénáře prediktivní údržby, jako [prediktivní údržby modelování průvodce](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-R-Notebook-1), existuje mnoho dalších zdrojů dat (tj historických údržby záznamy, protokoly chyb, počítač a operátor funkce atd.) které může vyžadovat různé typy ošetření mají být použity v hloubkového učení sítě. Vzhledem k tomu, že prediktivní údržby není Typická domény pro přímý learning, je její aplikací otevřete oblast research.

## <a name="references"></a>Odkazy

- [Šablona řešení prediktivní údržby](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Průvodce modelováním prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1)
- [Prediktivní údržby modelování Poznámkový blok průvodce Python](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-Python-Notebook-1)
- [Prediktivní údržby pomocí PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="future-directions-and-improvements"></a>Budoucí pokynů a vylepšení

Tento kurz se zaměřuje na základní informace o použití hloubkové učení v prediktivní údržby; Mnoho problémů prediktivní údržby obvykle zahrnuje celou řadu zdrojů dat, které je třeba brát v úvahu při použití hloubkové učení v této doméně. Kromě toho je důležité pro optimalizaci modely pro správné parametry, jako je například velikost okna. Čtečka můžete upravit příslušné části v tomto scénáři a pokuste se použít jiný scénář, například těm, které jsou popsané v [prediktivní údržby modelování průvodce](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1) kde se podílejí několik dalších datových zdrojů.
