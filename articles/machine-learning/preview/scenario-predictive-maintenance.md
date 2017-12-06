---
title: "Skutečných scénáři prediktivní údržby | Microsoft Docs"
description: "Prediktivní údržby reálného světa scénář pomocí PySpark"
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 2687eb022bce0b71c217f0be611c8fabdfb66040
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="predictive-maintenance-real-world-scenario"></a>Prediktivní údržby scénářem z reálného prostředí.

Dopad výpadek neplánovanou zařízení může být škodlivé pro všechny firmy. Je důležité k proto zachovat pole zařízení spuštěné, aby mohl maximalizovat využití a výkonu a současně minimalizujete její nákladná, neplánované výpadky. Časná identifikaci problémů může pomoci přidělení prostředků omezené údržby v nákladově efektivní způsob, jak a vylepšení kvality a zadat řetězec procesy. 

V tomto scénáři používáme relativně [rozsáhlé datové](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) vás uživatele prostřednictvím hlavní kroky z přijímání dat funkci technikům, vytváření modelů a pak nakonec operationalization modelu a nasazení. Kód pro celý proces je napsána v PySpark a implementovaná pomocí Jupyter notebooks v rámci Azure ML Workbench. Nejlepší modelu se nakonec operationalized pomocí Azure Machine Learning Model správy pro použití v produkčním prostředí pro vytváření předpovědi selhání v reálném čase.   

## <a name="link-to-the-gallery-github-repository"></a>Propojit s úložišti GitHub Galerie

Toto je odkaz na veřejné úložiště GitHub: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Přehled případu použití

Hlavní problém firmy v odvětví asset náročné, jimž je významné náklady, které jsou přidruženy zpoždění mechanických problémy s. Většina podniků zajímá predikci při tyto problémy jsou vyvolány prevence proaktivně je předtím, než k nim dojde. Cílem je snížení nákladů snížením výpadky a možném zvětšení zabezpečení. Odkazovat [scénářem pro prediktivní údržby](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance) podrobné vysvětlení běžné použít případů, jakož i modelování přístupu, používají pro prediktivní údržby.

Tento scénář využívá nápady z playbook s cílem poskytnout kroky pro implementaci prediktivního modelu pro scénáře, která je založena na shrnutí více skutečných obchodních problémů. Tento příklad spojuje, že běžné datové prvky zjištěnými mezi mnoha prediktivní údržby případy použití.

Obchodního problému pro toto simulované dat je k předvídání problémy příčinou selhání komponent. Proto je obchodní otázku "*co je pravděpodobnost, že je počítač ocitne mimo provoz z důvodu selhání součásti*?" Tento problém je formátován jako více třídami klasifikace problému (více komponent na počítač) a algoritmu strojového učení se používá k vytvoření prediktivního modelu. Cvičení modelu na historická data shromážděná z počítačů. V tomto scénáři uživatel prochází různé kroky implementace model, v prostředí Azure Machine Learning Workbench.

## <a name="prerequisites"></a>Požadavky

* [Účet Azure](https://azure.microsoft.com/en-us/free/) (bezplatné zkušební verze jsou k dispozici).
* Nainstalovaná kopie produktu [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) následující [Průvodce instalací úvodní](./quickstart-installation.md) k instalaci programu a vytvořit pracovní prostor.
* Azure Machine Learning Operationalization vyžaduje prostředí pro místní nasazení a [modelu účet pro správu](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

V tomto příkladu můžete spustit na jakýkoli kontext výpočetní AML Workbench. Však doporučujeme používat ji s minimálně 16 GB paměti. V tomto scénáři byl vytvořen a otestovali na počítač s Windows 10 běží vzdálené standard DS4_V2 [datové vědy virtuálního počítače pro Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model operationalization bylo provedeno pomocí 0.1.0a22 verze nástroje Azure ML rozhraní příkazového řádku.

## <a name="create-a-new-workbench-project"></a>Vytvoření nového projektu Workbench

Vytvořte nový projekt v tomto příkladu jako šablona:
1.  Otevřete Azure Machine Learning Workbench
2.  Na **projekty** klikněte na tlačítko  **+**  přihlásit a vybrat **nový projekt**
3.  V **vytvořit nový projekt** podokně, vyplňte informace pro nový projekt
4.  V **šablony projektů vyhledávání** vyhledávacího pole zadejte "Prediktivní Údržba" a vyberte šablonu
5.  Klikněte na **Vytvořit**

## <a name="prepare-the-notebook-server-computation-target"></a>Příprava cílového výpočetní server poznámkového bloku

Ke spuštění na místním počítači, z nástroje Workbench AML `File` nabídce vyberte buď `Open Command Prompt` nebo `Open PowerShell CLI`. V rámci rozhraní příkazového řádku systému windows spusťte následující příkazy:

`az ml experiment prepare --target docker --run-configuration docker`

Doporučujeme spuštěna na virtuálním počítači vědecké účely Data pro Linux (Ubuntu). Jakmile je nakonfigurovaný DSVM, spusťte následující dva příkazy:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Pomocí docker bitových kopií připravené, otevřete server poznámkového bloku Jupyter, buď v rámci kartě poznámkových bloků AML Workbench nebo spuštění webového serveru, spusťte: `az ml notebook start`.

Poznámkové bloky, které jsou uložené v `Code` adresáře v rámci prostředí Jupyter. Jsme spouštění poznámkových bloků postupně, počínaje na prvním (`Code\1_data_ingestion.ipynb`) poznámkového bloku. Když otevřete každý Poznámkový blok, budete vyzváni k vyberte výpočetní jádra. Zvolte _Template [název_projektu] [Desired_Connection_Name] a klikněte na tlačítko Nastavit jádra.

## <a name="data-description"></a>Popis dat

[Simulated data](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) se skládá z pěti hodnot oddělených čárkami (.csv) soubory. Na následujících odkazech získat další podrobnější popis datových sad.

* [Počítače](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): funkce rozlišení každý počítač. Například stáří a modelu.
* [Chyba](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): V protokolu chyb obsahuje chyby pevné vyvolána při tento počítač je stále v provozu. Tyto chyby se nepovažují za selhání, když mohou být prediktivní budoucí selhání události. Chyba data a času jsou zaokrouhleny na nejbližší hodiny vzhledem k tomu, že telemetrická data jsou shromažďována hodinové tempem.
* [Údržby](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Protokol údržby obsahuje oba záznamy plánovaných a neplánovaných údržby. Plánované údržby odpovídá pravidelné kontroly součástí, neplánovanou údržbu může způsobit selhání mechanických nebo jiných snížení výkonu. Datum a čas údržby jsou zaokrouhleny na nejbližší hodiny vzhledem k tomu, že telemetrická data jsou shromažďována hodinové tempem.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): telemetrická data časové řady se skládá z měření svůj senzor napětí, otáčení, zatížení a vibrace shromážděných z každého počítače v reálném čase. Data jsou průměr za hodinu a uložené v protokolech telemetrie
* [Selhání](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): selhání odpovídají náhrady součásti v rámci protokolu údržby. Každý záznam obsahuje ID počítače, typ součásti a nahrazení datum a čas. Tyto záznamy se používají k vytvoření strojového učení štítků, které se pokouší o předpovědi modelu.

Najdete v článku [přijímání dat](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) scénář Poznámkový blok Jupyter v části kódu ke stažení sady nezpracovaných dat z úložiště Githubu a vytvořit PySpark datových sad u této analýzy.

## <a name="scenario-structure"></a>Struktura scénář
Obsah pro tento scénář je k dispozici na [úložiště GitHub](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

V úložišti, je [Readme](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) souboru, který popisuje procesy od přípravy data dokud vytváření několik modelů a nakonec nejlepší modely zprovozňování. Jsou k dispozici v čtyři Jupyter notebooks [kód](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) složku v rámci úložiště.   

Potom jsme popisují podrobné scénář pracovního postupu. Koncová scénář je napsána v PySpark a je rozdělená do čtyř poznámkových bloků:

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Tento poznámkový blok stáhne pět souborů vstupní CSV, nemá některé čištění předběžné dat a vizualizaci. Poznámkového bloku převede data na formát PySpark a ukládá výsledky v kontejneru objektů blob v Azure pro použití v úloze engineering funkce.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Pomocí vyčištěnou datovou sadu z předchozího kroku, funkce lag a agregované funkce jsou vytvořeny pro součást náhrady telemetrie senzory a počty chyb, informace o počítači jsou připojené k telemetrická data. Náhrady související selhání součásti jsou používány k vytváření popisků popisující, které součásti došlo k chybě. Data s popiskem funkce se uloží do objektu blob Azure pro model vytváření úloh.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Pomocí s popiskem funkce datovou sadu, poznámkového bloku modelování rozdělí data na datové sady train a vývojářů podél razítka data a času. Poznámkový blok je instalační program sady experimentu s `pyspark.ml.classification` modelů. Je vectorized Cvičná data a uživatele můžete experimentovat s buď `DecisionTreeClassifier` nebo `RandomForestClassifier`, manipulace s hyperparameters najít nejvýkonnější modelu. Výkon je určen vyhodnocení statistiky míry pro datovou sadu vývojářů. Ve statistikách přihlášeni zpět na obrazovku čas AML Workbench spustit pro sledování. Při každém spuštění se uloží poznámkového bloku výsledný model na místní disk s jádrem poznámkového bloku Jupyter. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Pomocí poslední modelu uložit na místní disk (jádro poznámkového bloku Jupyter), tento poznámkový blok sestavení součásti pro zprovozňování modelu do Azure webové služby. Úplné provozní prostředky jsou zkomprimovanou do `o16n.zip` soubor uložený v jiném kontejneru objektů blob v Azure. Soubor ZIP obsahuje:

* `service_schema.json`Soubor definice schématu pro nasazení. 
* `pdmscore.py`Funkce init() a run() vyžadované Azure webovou službou
* `pdmrfull.model`Adresář definice modelu.
    
 Poznámkového bloku testy funkce s definicí modelu před zabalení operationalization prostředky pro nasazení. Pokyny pro nasazení jsou zahrnuty na konci poznámkového bloku.

## <a name="conclusion"></a>Závěr

Tento scénář nabízí čtečky přehled o tom, jak sestavit řešení prediktivní údržby koncová pomocí PySpark prostředí Poznámkový blok Jupyter v Azure ML Workbench. Tento scénář také provede čtečky na tom, jak nejlepší modelu můžete snadno operationalized a nasadit pomocí prostředí Azure Machine Learning Model správy pro použití v produkčním prostředí pro vytváření předpovědi selhání v reálném čase. Čtečka pak můžete upravit odpovídající části scénáři a zahrotit potřebám své firmy.  

## <a name="references"></a>Odkazy

Tento případ použití dříve vyvinula ve více platformách:

* [Šablona řešení prediktivní údržby](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Průvodce modelováním prediktivní údržby](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Prediktivní údržby modelování příručce, která je pomocí služby SQL Server R](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Prediktivní údržby modelování Poznámkový blok průvodce Python](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Prediktivní údržby pomocí PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

# <a name="next-steps"></a>Další kroky

Nejsou k dispozici v rámci nástroje workbench Azure Machine Learning, které ukazují další funkce produktu mnoho dalších ukázkové scénáře. 