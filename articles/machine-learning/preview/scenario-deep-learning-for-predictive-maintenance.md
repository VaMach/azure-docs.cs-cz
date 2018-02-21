---
title: "Hloubkové learning pro prediktivní údržby reálných scénářů - Azure | Microsoft Docs"
description: "Zjistěte, jak replikovat kurz na hloubkové learning pro prediktivní údržby pomocí Azure Machine Learning Workbench."
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Přímý učení pro prediktivní údržby scénářem z reálného prostředí.

Hloubkové learning je jednou z nejčastěji používané trendy ve strojovém učení se s aplikací na mnoha oblastech, včetně:
- standardizovaných aut a robotics
- rozpoznávání řeči a bitové kopie
- finanční prognózy.

Také označuje jako hluboké Neuronové sítě (DNN), tyto metody jsou INSPIROVANÉ jednotlivých neurons v rámci mozku (biologické neuronové sítě).

Dopad výpadek neplánovanou zařízení může být škodlivé pro všechny firmy. Je důležité zachovat pole zařízení s maximalizovat využití a výkonu a minimalizovat prostoje nákladná, neplánované. Časná identifikaci problémů může pomoci přidělení prostředků omezené údržby v nákladově efektivní způsob, jak a vylepšení kvality a zadat řetězec procesy. 

Strategie prediktivní údržby (odp) používá k určení stavu zařízení ho preventivně provádění údržby, aby se zabránilo výkon nežádoucí počítače machine learning metody. V PM data, shromažďují průběžně monitorovat stav počítače, je analýzy najít vzorů, které mohou být použity k předpovědi selhání. [Dlouhé paměti krátké termín (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sítě jsou atraktivní pro toto nastavení vzhledem k tomu, že jsou navrženy dozvědět se od pořadí dat.

## <a name="link-to-the-gallery-github-repository"></a>Propojit s úložišti GitHub Galerie

Toto je odkaz na veřejné úložiště GitHub pro problém sestavy a příspěvky: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Přehled případu použití

Tento kurz používá jako příklad simulované letadla modul spustit selhání události k předvedení prediktivní údržby modelování procesu. Tento scénář je popsán v [prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3)

Hlavní předpokladů v toto nastavení je modul progresivně degradovaný přes celé jeho životnosti. Snížení lze zjistit v modulu senzor měření. PM pokusí modelu vztah mezi změny v tyto hodnoty čidel a historických selhání. Model lze poté odhadnout, kdy a modul může selhat v budoucnu na základě aktuálního stavu senzor měření.

Tento scénář se vytvoří síť LSTM k předvídání zbývající dobu životnosti (RUL) z letecké motory pomocí historických senzor hodnot. Pomocí [Keras](https://keras.io/) s [Tensorflow](https://www.tensorflow.org/) hloubkové učení framework jako modul výpočetní scénář soupravy LSTM s jednu sadu moduly a testovací síti na sadu neviditelný modul.

## <a name="prerequisites"></a>Požadavky
- [Účet Azure](https://azure.microsoft.com/free/) (bezplatné zkušební verze jsou k dispozici).
- Azure Machine Learning Workbench, s vytvořen pracovní prostor.
- Pro model operationalization: Azure Machine Learning Operationalization, s prostředím místní nasazení nastavení a [účet Azure Machine Learning Model správy](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Vytvoření nového projektu Workbench

Vytvořte nový projekt v tomto příkladu jako šablona:

1. Otevřete strojového učení Workbench.
2. Na **projekty** vyberte  **+** a potom vyberte **nový projekt**.
3. V **vytvořit nový projekt** podokně, zadejte informace pro nový projekt.
4. V **šablony projektů vyhledávání** vyhledávacího pole zadejte "Prediktivní Údržba", vyberte **hloubkové Learning pro scénáři prediktivní údržby** šablony.
5. Klikněte **vytvořit** tlačítko

## <a name="prepare-the-notebook-server-computation-target"></a>Příprava cílového výpočetní server poznámkového bloku

Ke spuštění na místním počítači, z nástroje Workbench AML `File` nabídce vyberte buď `Open Command Prompt` nebo `Open PowerShell CLI`. Rozhraní příkazového řádku získáte přístup k vaší služby Azure pomocí `az` příkazy. První, přihlášení k účtu Azure pomocí příkazu:

```
az login
``` 

Tento příkaz poskytuje ověřovací klíč pro použití s `https:\\aka.ms\devicelogin` adresy URL. Rozhraní příkazového řádku počká, až operace přihlášení prostřednictvím zařízení vrátí, poskytuje některé informace o připojení. Další, pokud máte místní [docker](https://www.docker.com/get-docker) instalaci, připravit místní výpočetní prostředí pomocí následujících příkazů:

```
az ml experiment prepare --target docker --run-configuration docker
```

Je vhodnější ke spuštění na [datové vědy virtuálního počítače pro Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) pro požadavky na paměť a disku. Po nakonfigurování DSVM Příprava prostředí vzdálené docker s následující dva příkazy:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Po připojení ke vzdálené docker kontejneru se připravit DSVM docker výpočetní prostředí pomocí: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Pomocí docker výpočetní prostředí připravené, otevřete server poznámkového bloku Jupyter, buď v rámci kartě poznámkových bloků AML Workbench nebo spuštění webového serveru s: 
```
az ml notebook start
```

Příklad poznámkových bloků, které jsou uložené v `Code` adresáře. Poznámkových bloků se nastavit tak, aby spouští sekvenčně, spouštění v prvním (`Code\1_data_ingestion.ipynb`) poznámkového bloku. Když otevřete každý Poznámkový blok, zobrazí se výzva k vyberte výpočetní jádra. Vyberte `[Project_Name]_Template [Connection_Name]` jádra pro spuštění ve dříve nakonfigurované DSVM.

## <a name="data-description"></a>Popis dat

Šablona používá tři datových sad jako vstupy, v souborech **PM_train.txt**, **PM_test.txt**, a **PM_truth.txt**. 

-  **Cvičení data**: letadla modul data spustit selhání. Data train (PM_train.txt) se skládá z několika, multivariační časové řady s *cyklus* jako časovou jednotku. Obsahuje 21 odečty snímačů pro každý cyklus. 

    - Každý časové řady se generují z jiný modul stejného typu. Každý motor začíná různých stupňů počáteční opotřebení a některé jedinečné výrobní variace. Tyto informace Neznámý uživateli. 

    - V těchto datech simulované modul se předpokládá, že na začátku každé časové řady pracovat normálně. Začne snižovat v určitém okamžiku během řadu provozní cykly. Snížení hodnoty a zvětšování v rozsahem. 

    - Když je dosaženo předdefinované prahovou hodnotu, modul považovány za nebezpečné pro další operace. Posledního cyklu každého časové řady je bod selhání tohoto modulu.

-   **Testovacích dat**: letadla modul provozních dat bez zaznamenaných událostí selhání. Testovací data (PM_test.txt) mají stejné schéma dat, jako jsou Cvičná data. Jediným rozdílem je, že data neindikuje, když dojde k selhání (poslední časové období nemá *není* představují bodem selhání). Není znám, kolik více cyklů tento modul může poslední předtím, než se nezdaří.

- **Data pravdivosti**: true zbývající informace cyklů pro každý modul v testovacích datech. Data pravdivosti základů poskytuje počet cyklů zbývající práce pro moduly v testovacích datech.

## <a name="scenario-structure"></a>Struktura scénář

Pracovní postup scénář je rozdělené do tří kroky, každý spustit v poznámkových bloků Jupyter. Každý poznámkového bloku vytváří artefaktů dat, které jsou nastavené jako trvalé místně pro použití v následujících poznámkových bloků: 

### <a name="task-1-data-ingestion-and-preparation"></a>Úloha 1: Přijímání dat a příprava

Blok Jupyter přijímání dat v `Code/1_data_ingestion_and_preparation.ipnyb` načte tři vstupních datových sad do formát rámce Pandas data. Pak připraví data pro modelování a nemá některé vizualizace předběžné data. Datové sady se ukládají místní do kontextu výpočetní pro použití v poznámkovém bloku vytváření modelu.

### <a name="task-2-model-building-and-evaluation"></a>Úloha 2: Vytváření modelů a vyhodnocení

Blok Jupyter sestavení modelu v `Code/2_model_building_and_evaluation.ipnyb` čte train a testovací sady dat z disku a vytvoří síť LSTM trénovací datové sady. Výkon modelu se měří v testovací sadě. Výsledný model je serializované a uložená v kontextu místního výpočetní pro použití v úloze operationalization.

### <a name="task-3-operationalization"></a>Úloha 3: Operationalization

Operationalization Poznámkový blok Jupyter v `Code/3_operationalization.ipnyb` používá uložené model vytvářet funkce a schéma pro volání modelu v Azure hostovaná webové služby. Testy funkce poznámkového bloku a pak komprimaci prostředky do `LSTM_o16n.zip` souboru, který je načten do vašeho kontejneru úložiště Azure pro nasazení.

`LSTM_o16n.zip` Soubor nasazení obsahuje následující artefakty:

- `webservices_conda.yaml` definuje balíčky python, které jsou nutná k provozování LSTM modelu na cíl nasazení.  
- `service_schema.json` definuje schéma dat očekávanou LSTM modelu.     
- `lstmscore.py` Definuje funkcích, které běží cíl nasazení skóre nová data.    
- `modellstm.json` Definuje LSTM architekturu. Funkce lstmscore.py přečíst architektuře a váhu k chybě při inicializaci modelu.
- `modellstm.h5` definuje váhu modelu.
- `test_service.py` Test skript, který volá nasazení koncový bod s testovací datových záznamů. 
- `PM_test_files.pkl` `test_service.py` Skript přečte modul historických dat z `PM_test_files.pkl` souboru a odešle webová služba dostatek cyklů pro LSTM vrátit pravděpodobnost selhání modulu.

Poznámkového bloku testy funkce pomocí definice modelu předtím, než ho balíčky operationalization prostředky pro nasazení. Pokyny k instalaci a testování webovou službu jsou zahrnuty na konci `Code/3_operationalization.ipnyb` poznámkového bloku.

## <a name="conclusion"></a>Závěr

Tento kurz používá jednoduchý scénář pomocí hodnoty čidel předpovědi. Pokročilejší scénáře prediktivní údržby, jako je třeba [prediktivní údržby modelování R Poznámkový blok průvodce](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), můžete použít víc zdrojů dat, jako je například zaznamenává historické údržby, protokoly chyb a počítač funkce. Dalších zdrojů dat může vyžadovat různých způsobů pro použití s hloubkovým učení.


## <a name="references"></a>Odkazy

Další příklady případu použití prediktivní údržby nejsou k dispozici v mnoha různých platforem:

* [Šablona řešení prediktivní údržby](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Průvodce modelováním prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Prediktivní údržby modelování příručce, která je pomocí služby SQL Server R](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Prediktivní údržby modelování Poznámkový blok průvodce Python](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Prediktivní údržby pomocí PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Scénářem z reálného prostředí prediktivní údržby](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Další postup

Nejsou k dispozici v rámci nástroje workbench Azure Machine Learning, které ukazují další funkce produktu mnoho dalších ukázkové scénáře. 