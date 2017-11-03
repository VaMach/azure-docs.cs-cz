---
title: "Úložiště Git pomocí Azure Machine Learning Workbench projekt | Microsoft Docs"
description: "Tento článek vysvětlují, jak používat úložiště Git ve spojení s projektu Azure Machine Learning Workbench."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: hning86
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 59b07c9834904e01256b75344ba2e6892e56438c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Pomocí projektu Azure Machine Learning Workbench úložiště Git
Tento dokument obsahuje informace o tom, jak Azure Machine Learning Workbench používá Git zajistit reprodukovatelnosti v experimentu vědecké účely data. K dispozici jsou také pokyny o tom, jak přidružit projektu cloudové úložiště Git.

## <a name="introduction"></a>Úvod
Azure Machine Learning Workbench je navržen s integrace Gitu od základů nahoru. Při vytváření nového projektu, složce projektu se automaticky "inicializován pro Git" jako místní úložiště Git (úložiště) při druhý skrytá místní úložiště Git se také vytvoří s větev s názvem _AzureMLHistory / < project_GUID >_ na udržování přehledu o změny složky projektu pro každé spuštění. 

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


> [!TIP]
> Zajistěte, aby pro přihlášení k účtu Azure Active Directory (AAD) používá pro přístup k Azure Machine Learning Workbench. Jinak hodnota nového týmového projektu mohou koncoví up pod nesprávné ID klienta a nemusí být Azure Machine Learning. V takovém případě je třeba použít rozhraní příkazového řádku a zadejte token služby VSTS.

Po vytvoření týmového projektu, jste připravení přejít k dalšímu kroku.

Přejděte přímo na týmový projekt právě vytvořili, adresa URL je `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> Azure Machine Learning v současné době podporuje pouze prázdné úložiště Git se žádné hlavní větve. Z rozhraní příkazového řádku, můžete použít argument--force nejprve odstranit hlavní větve. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Krok 3. Vytvořte nový projekt Azure ML s vzdáleného úložiště Git
Spusťte Azure ML Workbench a vytvoření nového projektu. Vyplnění textového pole úložiště Git s adresou URL úložiště Git služby VSTS, kterou můžete získat z kroku 2. Obvykle vypadá takto: http://<vsts_account_name>.visualstudio.com/_git/<project_name>

![Vytvoření projektu Azure ML s úložiště Git](media/using-git-ml-project/create_project_with_git_rep.png)

Teď se vytvoří nový projekt Azure ML s integrací služby Vzdálené úložiště Git, povolené a jste připraveni.... Složce projektu je vždy inicializován pro Git jako místní úložiště Git. A Gitu _vzdáleného_ je nastavené na vzdálené úložiště Git služby VSTS, takže potvrzení možné převést do vzdáleného úložiště Git.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Krok 3.a přidružení existujícího projektu Azure ML s úložišti Git služby VSTS
Volitelně můžete také vytvoření projektu Azure ML bez úložiště Git služby VSTS a právě spoléhají na místní úložiště Git pro snímky historie spouštění. A je možné přidružit úložiště Git služby VSTS později tento existující projekt Azure ML pomocí následujícího příkazu:

```azurecli
# make sure you are in the project path so CLI has context of your current project
az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Krok 4. Zachycení snímku projektu v úložiště Git
Teď můžete spustit několik běží v projektu, zkontrolujte některé změny nevede je spuštěn. Můžete to udělat buď z plochy aplikace, nebo pomocí rozhraní příkazového řádku `az ml experiment submit` příkaz. Další podrobnosti, můžete postupovat podle [klasifikace Iris kurzu](tutorial-classifying-iris-part-1.md). Při každém spuštění, je-li všechny změny provedené v všechny soubory ve složce projektu snímek složce celý projekt je potvrzené a vložena do vzdáleného úložiště Git. Větve a potvrzení můžete zobrazit tak, že přejde na adresu URL úložiště Git služby VSTS.

![Historie spouštění větve](media/using-git-ml-project/run_history_branch.png)

## <a name="step-5-restore-a-previous-project-snapshot"></a>Krok 5. Obnovit předchozí snímek projektu 
K obnovení složce celý projekt do stavu předchozího historie spouštění projektu snímku stavu, z AML Workbench.
1. Klikněte na **spustí** v aktivitě panelu (ikona pohotovostní hodinu).
2. Z **seznamu spustit** zobrazení, klikněte na spustit, kterou chcete obnovit.
3. Z **spustit podrobností** zobrazení, klikněte na **obnovení**.

![Historie spouštění větve](media/using-git-ml-project/restore_project.png)

Alternativně můžete použít následující příkaz z okna příkazového řádku Azure ML Workbench.

```azurecli
# discover the run I want to restore snapshot from:
az ml history list -o table

# restore the snapshot from a particular run
az ml project restore --run-id <run_id>
```

Spuštěním tohoto příkazu jsme bude snímek pořízený v případě, že konkrétní spustit byla spuštěna přepište složce celý projekt. To znamená, že se **, ztratíte všechny změny** v aktuální složce projektu. Proto prosím buďte velmi opatrní při spuštění tohoto příkazu.

## <a name="step-6-use-the-master-branch"></a>Krok 6. Použít hlavní větve
Jedním ze způsobů, abyste se vyhnuli ztrátě omylem aktuální stav projektu, je potvrzení projektu do hlavní větve úložiště Git. Git můžete použít přímo z příkazového řádku (nebo vaše dalších oblíbených Git klienta nástroje) k provozu na hlavní větve. Například:

```
# make sure you are on the master branch
git checkout master

# stage all changes
git add -A

# commit all changes locally on the master branch
git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
git push origin master
```

Nyní můžete bezpečně obnovit projekt starší snímku následující krok 5, a zároveň budete vědět, že vždy se můžete vrátit zpět k potvrzení jste právě probíhají hlavní větve.

## <a name="authentication"></a>Authentication
Pokud se právě využívají funkce historie spouštění v Azure ML pořizování snímků projektu a obnovení databází, nemusíte starat o ověřování úložiště Git. Se stará vrstvě služby experimenty.

Ale pokud používáte vlastní Git nástroje pro správu verzí, musíte řádně zpracovávat ověřování proti vzdáleného úložiště Git na služby VSTS. To znamená musíte nastavit ověřování s úložiště Git v místním počítači, než můžete použít příkazy Gitu proti tohoto vzdáleného úložiště Git. 

Nejjednodušší způsob, jak-li to provést, je vytvořit dvojici klíčů SSH a nahrajte na server veřejnou část klíče na nastavení zabezpečení úložiště Git.

### <a name="generate-ssh-key"></a>Generovat klíče SSH 
První umožňuje vygenerovat pár klíčů SSH ve vašem počítači.

#### <a name="on-windows"></a>V systému Windows:
Spusťte Git grafického uživatelského rozhraní aplikace na ploše ve Windows a v části _pomoci_ nabídky, klikněte na _zobrazit klíč SSH_.

![SSH klíč](media/using-git-ml-project/git_gui.png)

SSH zkopírujte do schránky.

#### <a name="on-macos"></a>V systému macOS:
Rychlé kroky příkazové prostředí:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Další kroky podrobnosti naleznete na [v tomto článku Githubu](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Nahrání veřejný klíč do úložiště Git
Přejděte na vaše domovská stránka v sadě Visual Studio účet: https://<vsts_account_name>.visualstudio.com a přihlásit, klikněte na zabezpečení v části miniatury.

Pak přidejte veřejný klíč SSH, vložte veřejný klíč SSH, který můžete získat z předchozího kroku a pojmenujte ho. Můžete přidat více klíčů sem.

Nyní můžete vydávat příkazy Gitu místně proti úložišti odebrat pomocí žádné další explicitní je vyžadováno ověření.

### <a name="read-more"></a>Další informace
Postupujte podle těchto dvou články (buď přístup může fungovat) Další podrobnosti o tom, jak povolit místní ověřování pro vzdálené úložiště Git v služby VSTS.
- [Použít ověření pomocí klíče SSH](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate)
- [Pomocí Správce přihlašovacích údajů Git](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers)


## <a name="next-steps"></a>Další kroky
Další informace o použití procesu Team dat vědecké účely uspořádání strukturu projektu, najdete v článku [struktury projekt se TDSP](how-to-use-tdsp-in-azure-ml.md)
