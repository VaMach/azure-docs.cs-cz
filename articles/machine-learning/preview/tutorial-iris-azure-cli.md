---
title: "Kurz článku funkce verze preview Azure Machine Learning - rozhraní příkazového řádku | Microsoft Docs"
description: "Tento kurz ukázce všechny kroky nutné pro dokončení Iris klasifikace začátku do konce z rozhraní příkazového řádku."
services: machine-learning
author: ahgyger
ms.author: ahgyger, ritbhat
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: 453c774c97b77dd7829a50fa5e5668d06f817a1d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Kurz: Klasifikace Iris pomocí rozhraní příkazového řádku
Služby Azure Machine Learning (Preview) představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

V tomto kurzu můžete naučit se používat nástroje rozhraní příkazového řádku (CLI) v Azure Machine Learning preview funkcí: 
> [!div class="checklist"]
> * Nastavit účet experimentování a vytvořit pracovní prostor
> * Vytvoření projektu
> * Odeslání na experiment více cílů výpočetní
> * Povýšení a registraci modulu trained model.
> * Nasazení webové služby skóre pro nová data

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
- Potřebujete přístup k předplatnému Azure a oprávnění pro vytváření prostředků v tomto předplatném. 
- Je potřeba nainstalovat aplikaci Azure Machine Learing Workbench pomocí následujících [instalace a vytvoření rychlý Start](quickstart-installation.md). 

  >[!NOTE]
  >Stačí nainstalovat nástroje Azure Machine Learning Workbench místně. Stačí postupujte podle kroků v části s názvem nainstalovat Azure Machine Learning Workbench, od vytvoření účtu a vytvořte nový projekt, který kroků bude provedeno pomocí příkazového řádku v tomto článku.
 
## <a name="getting-started"></a>Začínáme
Azure Machine Learning rozhraní příkazového řádku (CLI) umožňuje provádět všechny úkoly vyžadované pro pracovní postup začátku do konce dat vědecké účely. Nástroje příkazového řádku se můžete dostat následujícími způsoby:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Možnost 1. spuštění příkazového řádku Azure ML z Azure ML přihlášení dialogové okno sady nástrojů
Při prvním spuštění Azure ML Workbench a přihlaste se a již nemáte přístup k účtu experimentování, zobrazí se následující obrazovka:

![účet není nalezen](media/tutorial-iris-azure-cli/no_account_found.png)

Klikněte na **okno příkazového řádku** odkaz v dialogovém okně spusťte okno příkazového řádku.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Možnost 2. spuštění rozhraní příkazového řádku Azure ML z Azure ML Workbench aplikace
Pokud již máte přístup k účtu experimentování, vám umožní přihlásit úspěšně. A pak můžete otevřít okno příkazového řádku kliknutím na **soubor** --> **otevřete příkazový řádek příkaz** nabídky.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Možnost 3. Povolte Azure ML rozhraní příkazového řádku v libovolné okně příkazového řádku
Můžete také povolit příkazového řádku Azure ML v jakékoli okno příkazového řádku. Jednoduše spusťte okno příkazového řádku a zadejte následující příkazy:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Chcete-li změny trvalé, můžete použít `SETX` v systému Windows. U systému macOS, můžete použít `setenv`.

>[!TIP]
>Rozhraní příkazového řádku Azure můžete povolit v vaše oblíbené okno terminálu tak výše uvedených proměnných prostředí.

## <a name="step-1-log-in-to-azure"></a>Krok 1. Přihlaste se k Azure.
Prvním krokem je otevřete rozhraní příkazového řádku z AMLWorkbench aplikace (Soubor > otevřít příkazový řádek). Tím zajistíte používáme prostředí správné python a máme příkazy ML rozhraní příkazového řádku, která je k dispozici. 

Pak je potřeba nastavit kontext vpravo v vaše rozhraní příkazového řádku pro přístup a správu prostředků Azure.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Krok 2. Vytvořte nový účet Azure Machine Learning experimentování a pracovního prostoru
Začneme vytvořením nového účtu experimentování a nový pracovní prostor. V tématu [koncepty Azure Machine Learning](overview-general-concepts.md) další podrobnosti o účtech experimentování a pracovní prostory.

> [!NOTE]
> Účty experimentování vyžadují účet úložiště, který se používá k ukládání výstupy běží vaše experimentu. Název účtu úložiště musí být globálně jedinečný v Azure, protože je adresu url s ním spojená. Pokud nezadáte stávající účet úložiště, název účtu experimentování se používá k vytvoření nového účtu úložiště. Nezapomeňte použít jedinečný název nebo obdržíte chybu, jako _"účet úložiště s názvem \<storage_account_name > je už zabrané."_ Alternativně můžete použít `--storage` argumentu zadat existující účet úložiště.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Krok 2.a (volitelné) sdílet s kolegy pracovního prostoru
Zde jsme zjistit, jak sdílet přístup k pracovním prostoru s spolupracovník. Kroky pro sdílení přístup k účtu experimentování nebo do projektu by být stejné. Způsob získání ID prostředku Azure by potřeba aktualizovat.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com`ve výše uvedeném příkazu musí být platná identit Azure AD v adresáři, kam patří aktuální předplatné.

## <a name="step-3-create-a-new-project"></a>Krok 3. Vytvoření nového projektu
Naše dalším krokem je vytvoření nového projektu. Existuje několik způsobů, jak začít s nový projekt.

### <a name="create-a-new-blank-project"></a>Vytvořit nový prázdný projekt

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Vytvoření nového projektu pomocí výchozí šablony projektu
Můžete vytvořit nový projekt pomocí výchozí šablony.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Vytvoření nového projektu spojené s cloudem úložiště Git
Můžeme vytvořit nový projekt přidružené úložiště Git služby VSTS (Visual Studio Team Service). Pokaždé, když je odeslání experimentu, snímek složce celý projekt se zaměřuje na vzdálené úložiště Git. V tématu [úložiště pomocí Git s projektem Azure Machine Learning Workbench](using-git-ml-project.md) další podrobnosti.

> [!NOTE]
> Azure Machine Learning podporuje pouze vytvořené v služby VSTS prázdný úložiště Git.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Pokud se zobrazuje chybu "adresu url úložiště může být neplatná nebo uživatel nemusí mít přístup", můžete vytvořit token zabezpečení služby VSTS (v části _zabezpečení_, _přidat osobní přístupové tokeny_ nabídky) a použít `--vststoken`argument při vytváření projektu. 

### <a name="sample_create"></a>Vytvoření nového projektu z vzorku
V tomto příkladu vytvoříme nový projekt pomocí ukázkový projekt jako šablona.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Po vytvoření projektu, použijte `cd` příkazu zadejte adresář, do projektu.

## <a name="step-4-run-the-training-experiment"></a>Krok 4 spusťte experiment školení 
Níže uvedené kroky předpokládají, že byl projektu s ukázkou Iris (najdete v části [vytvoření nového projektu z online ukázky](#sample_create)).

### <a name="prepare-your-environment"></a>Příprava prostředí 
Pro ukázku Iris potřebujeme nainstalovat matplotlib.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Odeslání experimentu

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Iterace v experimentu s sestupném regulaci sazby
Pomocí některé kreativitu je jednoduchý dávat dohromady Python skript, který odešle experimenty s jinou regulaci sazby. (Možná muset upravit tak, aby odkazovaly na správné projektu cestu souboru.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Krok 5. Zobrazení historie spuštění
Následující příkaz zobrazí seznam všech předchozích spuštění provést. 

```azure-cli
$ az ml history list -o table
```
Spuštění výše uvedeném příkazu se zobrazí seznam všech spustí patřící do tohoto projektu. Uvidíte, že jsou příliš uvedené přesnost a regulaci míra metriky. Toto zpřístupnění usnadňují určit nejlepší spustit ze seznamu.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Krok 5.a zobrazení přílohy vytvořené dané spustit 
Chcete-li zobrazit přílohu spojené s danou spustit, jsme můžete použít příkaz informace z historie spouštění. Najít spustit id konkrétní spustit ze seznamu výše.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Chcete-li artefakty z spustit, můžete použít následující příkaz:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Krok 6. Zvýšení úrovně artefakty spustit 
Jeden z je spuštěn, které jsme provedli má lepší AUC, takže chceme ji použít k vytvoření vyhodnocování webové služby pro nasazení do produkčního prostředí. Chcete-li to provést, musíme nejprve zvýšit úroveň artefakty do prostředku.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Tím se vytvoří `assets` složky v adresáři projektu s `model.pkl.link` souboru. Tento odkaz soubor se používá k odkazování propagovaných asset.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Krok 7. Stáhnout soubory, které chcete být operationalized
Nyní potřebujeme stáhnout propagovaných modelu, takže jsme můžete použít k vytvoření naše předpovědi webové služby. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-setup-your-model-management-environment"></a>Krok 8. Nastavení prostředí model správy 
Vytvoříme prostředí pro nasazování webových služeb. V místním počítači pomocí Docker jsme můžete spustit webovou službu. Nebo ji nasadit do clusteru služby ACS pro vysoce škálované operace. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Krok 9. Vytvořit účet model správy 
Účet pro správu modelu je potřeba zavést a sledovat modely v produkčním prostředí. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Krok 10. Vytvoření webové služby
Poté vytvoříme webová služba, která vrátí předpovědi pomocí modelu, který jsme nasadili. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score.py -r python –n <web service name>
```

## <a name="step-10-run-the-web-service"></a>Krok 10. Spustit webovou službu
Pomocí id webové služby z výstupu v předchozím kroku, jsme volání webové služby a otestovat. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="deleting-all-the-resources"></a>Odstranění všechny prostředky 
Umožňuje tak, že odstraníte všechny prostředky, které jsme vytvořili, pokud chcete pokračovat v práci na jeho dokončení tohoto kurzu! 

Uděláte to tak, můžeme jednoduše odstranit skupinu prostředků, která uchovává všechny naše prostředky. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili funkce preview Azure Machine Learning, které chcete použít 
> [!div class="checklist"]
> * Nastavit účet experimentování, vytváření pracovního prostoru
> * Vytváření projektů
> * Odeslání experimentů k několika cílů výpočetní
> * Povýšení a registraci modulu trained model.
> * Vytvoření účtu správy modelu pro model správy
> * Vytvoření prostředí pro nasazení webové služby
> * Nasazení webové služby a skóre s nová data