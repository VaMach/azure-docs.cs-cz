---
title: "Úložiště Git pomocí Azure Machine Learning Workbench projekt | Microsoft Docs"
description: "Tento článek vysvětlují, jak používat úložiště Git ve spojení s projektu Azure Machine Learning Workbench."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: fe2a302a32f1b9ec474416704c6cb613cd384a0e
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Pomocí projektu Azure Machine Learning Workbench úložiště Git
Tento dokument obsahuje informace o tom, jak Azure Machine Learning Workbench používá Git k poskytování správy verzí a zkontrolujte reprodukovatelnosti v experimentu vědecké účely data. K dispozici jsou také pokyny o tom, jak přidružit projektu cloudové úložiště Git.

## <a name="introduction"></a>Úvod
Azure Machine Learning Workbench je navržen s integrace Gitu od základů nahoru. Při vytváření nového projektu, složce projektu je automaticky "inicializován pro Git" jako místní úložiště Git (úložiště). Mezitím se také vytvoří druhý skrytá místní úložiště Git s větev s názvem _AzureMLHistory / < project_GUID >_ ke sledování změn složce projektu pro každé spuštění. 

Přidružení projektu Azure ML úložiště Git, hostovaným v rámci projektu Visual Studio Team Service (VSTS), umožňuje automatické verzí místně i vzdáleně. Toto přidružení umožňuje každý, kdo má přístup k Vzdálené úložiště stáhnout nejnovější zdrojový kód do jiného počítače (roaming).  

> [!NOTE]
> Služby VSTS má svou vlastní seznam řízení přístupu, která je nezávislá služba Azure Machine Learning experimenty. Uživatelský přístup se mezi úložiště Git a aplikace Azure ML prostoru nebo v projektu může lišit a může třeba spravovat. Takže pokud chcete sdílet projektu Azure ML s seskupení včetně úrovně přístupu kódu, kromě právě sdílet pracovním prostoru, budete muset explicitně udělit mu správný přístup k úložišti služby VSTS Git. 

S Gitem, je také možné explicitně správě verzí pomocí _hlavní_ větev nebo pomocí vytváření ostatní větve zůstanou v úložišti. Použijete na místní úložiště Git, a můžete také posouvat do vzdáleného úložiště Git Pokud zřízené.

Tento diagram znázorňuje vztah mezi úložišti Git služby VSTS a projektu Azure ML:

![AML Git](media/using-git-ml-project/aml_git.png)

Abyste mohli začít používat vzdálené úložiště Git, postupujte podle těchto pokynů základní.

> [!NOTE]
> Azure Machine Learning v současné době podporuje pouze úložiště Git v účtech služby VSTS. Podpora pro úložiště Git obecné (například Githubu a atd.) je plánované v budoucnu.

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>Krok 1. Vytvoření účtu Azure ML experimentování
Není-li již, vytvoření účtu Azure ML experimentování a nainstalujte aplikaci Azure ML Workbench. Další podrobnosti najdete v tématu [instalovat a vytvořte rychlý Start](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Krok 2. Vytvoření týmového projektu, nebo použijte existující týmového projektu
Z [portál Azure](https://portal.azure.com/), vytvořte novou **týmového projektu**.
1. Klikněte na**+**
2. Vyhledejte **"Team projektu"**
3. Zadejte požadované informace.
    - Název: Název týmu.
    - Správa verzí: **Git**
    - Předplatné: Jednu s účtem Azure Machine Learning experimenty.
    - Umístění: V ideálním případě zůstanou v oblasti, která je blízko vašich prostředků Azure Machine Learning experimentu.
4. Klikněte na možnost **Vytvořit**. 

![Vytvoření týmového projektu z portálu Azure](media/using-git-ml-project/create_vsts_team.png)

Ujistěte se, že se že přihlásíte pomocí stejného účtu Azure Active Directory (AAD), který používáte pro přístup k Azure Machine Learning Workbench. Jinak hodnota systém nemůže přistupovat k pomocí svoje přihlašovací údaje AAD Pokud použijete příkazového řádku k vytvoření projektu Azure ML a zadat osobní přístupový token pro přístup k úložišti Git. Další informace o to později.

Po vytvoření týmového projektu, jste připravení přejít k dalšímu kroku.

Přejděte přímo na týmový projekt právě vytvořili, adresa URL je `https://<team_project_name>.visualstudio.com`.

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Krok 3. Vytvořte nový projekt Azure ML s vzdáleného úložiště Git
Spusťte Azure ML Workbench a vytvoření nového projektu. Vyplnění textového pole úložiště Git s adresou URL úložiště Git služby VSTS, kterou můžete získat z kroku 2. Obvykle vypadá takto:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Vytvoření projektu Azure ML s úložiště Git](media/using-git-ml-project/create_project_with_git_rep.png)

Můžete také vytvořit projekt pomocí nástroje příkazového řádku. Máte možnost zadat osobní přístupový token. Azure ML můžete použít tento token pro přístup k úložišti Git vaším jménem, aniž byste museli spoléhat na vaše přihlašovací údaje AAD:

```
# create a new project with a Git repo and personal access token.
$ az ml project create -a <experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <VSTS personal access token>
```
> [!IMPORTANT]
> Pokud si zvolíte Šablona prázdného projektu, je OK Pokud úložiště Git zvolíte už má _hlavní_ firemní pobočky. Jednoduše provede klonování Azure ML _hlavní_ větev místně a přidejte `aml_config` složky a další metadata soubory do složky místní projektu projektu. Ale pokud si zvolíte jiné šablony projektu, nesmí mít již vašeho úložiště Git _hlavní_ větve, nebo se zobrazí chyba. Alternativou je použít `az ml project create` nástroj pro příkazový řádek pro vytvoření projektu a poskytnete `--force` přepínače. To odstraní soubory na původní hlavní větve a nahradíte je nové soubory v šabloně, který zvolíte.

Teď se vytvoří nový projekt Azure ML s integrací služby Vzdálené úložiště Git, povolené a jste připraveni.... Složce projektu je vždy inicializován pro Git jako místní úložiště Git. A Gitu _vzdáleného_ je nastavené na vzdálené úložiště Git služby VSTS, takže potvrzení možné převést do vzdáleného úložiště Git.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Krok 3a. Přidružit úložiště Git služby VSTS existujícího projektu Azure ML
Volitelně můžete také vytvoření projektu Azure ML bez úložiště Git služby VSTS a právě spoléhají na místní úložiště Git pro snímky historie spouštění. A je možné přidružit úložiště Git služby VSTS později tento existující projekt Azure ML pomocí následujícího příkazu:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

> [!NOTE] 
> Můžete provádět jenom operace aktualizace úložišti na projekt Azure ML, který nemá úložiště Git, s ním spojená. A jakmile je spojen úložiště Git, nelze ji odebrat.

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Krok 4. Zachycení snímku projektu v úložiště Git
Teď můžete spustit několik skript se spustí v projektu, zkontrolujte některé změny nevede je spuštěn. Můžete to udělat buď z plochy aplikace, nebo pomocí rozhraní příkazového řádku `az ml experiment submit` příkaz. Další podrobnosti, můžete postupovat podle [klasifikace Iris kurzu](tutorial-classifying-iris-part-1.md). Při každém spuštění, je-li všechny změny provedené v všechny soubory ve složce projektu snímek složce celý projekt je potvrzené a vložena do vzdáleného úložiště Git v části s názvem větev `AzureMLHistory/<Project_GUID>`. Můžete zobrazit tak, že přejde na adresu URL úložiště Git služby VSTS větve a potvrzení a najít tuto větev. 

> [!NOTE] 
> Snímek se potvrdí pouze před spuštěním skriptu. Aktuálně data Příprava aplikace spuštění nebo spuštění Poznámkový blok buněk neaktivuje snímku.

![Historie spouštění větve](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Je nejvhodnější Pokud není provozujete v pobočce historie sami pomocí příkazy Gitu. Díky tomu může zřeteli do historie spouštění. Použít hlavní větve nebo místo toho vytvořte ostatní větve zůstanou pro operace Git.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Krok 5. Obnovit předchozí snímek projektu 
Chcete-li obnovit složce celý projekt do stavu předchozího historie spouštění projektu snímku stavu, z Azure ML Workbench:
1. Klikněte na **spustí** v aktivitě panelu (ikona pohotovostní hodinu).
2. Z **seznamu spustit** zobrazení, klikněte na spustit, kterou chcete obnovit.
3. Z **spustit podrobností** zobrazení, klikněte na **obnovení**.

![Historie spouštění větve](media/using-git-ml-project/restore_project.png)

Alternativně můžete použít následující příkaz z okna příkazového řádku Azure ML Workbench.

```azurecli
# discover the run I want to restore snapshot from:
$ az ml history list -o table

# restore the snapshot from a particular run
$ az ml project restore --run-id <run_id>
```

Spuštěním tohoto příkazu jsme bude snímek pořízený v případě, že konkrétní spustit byla spuštěna přepište složce celý projekt. Ale váš projekt probíhal v aktuální větve. To znamená, že se **, ztratíte všechny změny** v aktuální složce projektu. Proto prosím buďte velmi opatrní při spuštění tohoto příkazu. Můžete chtít Git uložte provedené změny do aktuální větve před provedením výše uvedené operace. Před další podrobnosti najdete v.

## <a name="step-6-use-the-master-branch"></a>Krok 6. Použít hlavní větve
Jeden způsob, jak zabránit omylem došlo ke ztrátě aktuálního stavu projektu, je potvrzení projektu do hlavní větve (nebo všechny větve, které jste sami vytvořili) úložiště Git. Git můžete použít přímo z příkazového řádku (nebo vaše dalších oblíbených Git klienta nástroje) k provozu na hlavní větve. Například:

```
# check status to make sure you are on the master branch (or branch of your choice)
$ git status

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'these are my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Nyní můžete bezpečně obnovit projekt starší snímku následující krok 5, a zároveň budete vědět, že vždy se můžete vrátit zpět k potvrzení jste právě probíhají hlavní větve.

## <a name="authentication"></a>Authentication
Pokud se právě využívají funkce historie spouštění v Azure ML pořizování snímků projektu a obnovení databází, nemusíte starat o ověřování úložiště Git. Se stará vrstvě služby experimenty.

Ale pokud používáte vlastní Git nástroje pro správu verzí, musíte řádně zpracovávat ověřování proti vzdáleného úložiště Git na služby VSTS. Vzdálené úložiště Git se v Azure ML, přidá do místního úložiště jako vzdálené pomocí protokolu HTTPS úložiště Git. To znamená, pokud vydáte Git příkazy ke vzdálenému (jako je například push nebo pull), že budete muset zadat uživatelské jméno a heslo nebo osobní přístupový token. Postupujte podle [tyto pokyny](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) vytvořit osobní přístupový token v úložišti služby VSTS Git.

## <a name="next-steps"></a>Další kroky
Další informace o použití procesu Team dat vědecké účely uspořádání strukturu projektu, najdete v článku [struktury projekt se TDSP](how-to-use-tdsp-in-azure-ml.md)
