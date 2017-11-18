---
title: "Roaming a spolupráce v Azure strojového učení Workbench | Microsoft Docs"
description: "Příručka k řešení a seznam známých problémů"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 856348c07a198a8c53c6661441d5c49196ef3af5
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming a spolupráce v Azure Machine Learning Workbench
Tento dokument vás provede jak Azure Machine Learning Workbench vám mohou pomoci přenášet projekty mezi počítače, jakož i povolení spolupráce s ostatními členy týmu. 

Když vytvoříte projekt Azure Machine Learning s odkazem vzdáleného úložiště Git (úložiště), metadata projektu a snímky jsou uložené v cloudu. Odkaz cloudu umožňuje přístup k projektu, z jiného počítače (Roaming). Můžete také poskytnout přístup k vaší spolupracovníci, a tak umožňuje spolupráci. 

## <a name="prerequisites"></a>Požadavky
Nejdřív nainstalujte Azure Machine Learning Workbench s přístupem k účtu experimenty. Postupujte podle [Průvodce instalací](quickstart-installation.md) další podrobnosti.

Druhý, přístup k [Visual Studio Team System](https://www.visualstudio.com) a úložišti propojení projektu pro vytvoření. Podrobné informace o Git, podívejte se [pomocí úložiště Git s projektem Azure Machine Learning Workbench](using-git-ml-project.md) článku.

## <a name="create-a-new-azure-machine-learning-project"></a>Vytvořte nový projekt Azure Machine Learning
Spusťte Azure Machine Learning Workbench a vytvoření nového projektu (například _iris_). Vyplnění **adresu URL úložiště GIT Visualstudio.com** textové pole s platnou adresu URL úložiště Git služby VSTS. 
>[!IMPORTANT]
>Vytvoření projektu se nezdaří, pokud nemáte přístup pro čtení a zápis na úložiště Git a úložiště Git není prázdná, tj. už je hlavní větve.

Po vytvoření projektu se odešlete několik běží na všech skriptů v projektu. Tato akce provede stavu projektu do vzdáleného úložiště Git historie spouštění větev. 

Pokud máte instalaci Git ověřování, můžete také explicitně fungovat v hlavní větve, nebo vytvořte novou větev. 

Jako příklad: 
```
# check current repo status
$ git status

# stage all changes in the current repo
$ git add -A

# commit changes
$ git commit -m "my commit fixes this weird bug!"

# push to remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-azure-machine-learning-workbench-on-second-machine"></a>Otevřete Azure Machine Learning Workbench na druhý počítač
Jakmile úložiště Git služby VSTS je propojená s projektem, dostanete _iris_ projekt z libovolného počítače, kam jste nainstalovali Azure Machine Learning Workbench. 

Přístup k projektu iris na jiném počítači, musíte se přihlásit k aplikaci se stejnými pověřeními, použít při vytváření projektu. Kromě toho budete muset přejít na stejný účet experimentování a pracovního prostoru. _Iris_ projektu abecedně označené Další projekty v pracovním prostoru. 

### <a name="download-project-on-second-machine"></a>Stáhněte si projekt na druhý počítač
Po otevření pracovního prostoru na druhý počítač, na ikonu vedle _iris_ projektu se liší od ikony typické složky. Ikona stažení označuje, že obsah projektu je v cloudu a musí být stáhnout do aktuálního počítače. 

![Vytvoření projektu](./media/roaming-and-collaboration/downloadable-project.png)

Kliknutím na _iris_ projektu spustí stahování akce. Za malou chvíli, kdy stahování dokončí je připraven na druhý počítač přístup k projektu. 

V systému Windows je`C:\Users\<username>\Documents\AzureML`

V systému macOS je tady:`/home/<username>/Documents/AzureML`

V budoucí verzi plánujeme vylepšují funkce a umožní vám vybrat cílovou složku. 

>Poznámka: Pokud jste dojít na požadovanou složku v adresáři Azure ML, který má přesně stejný název jako projektu, se nezdaří stahování. V současné době budete muset přejmenovat existující složku Chcete-li tento problém obejít.


### <a name="work-on-the-downloaded-project"></a>Práce na staženého projektu 
Nově staženého projektu odráží projektu stavu od posledního spuštění v projektu. Snímek stavu projektu je automaticky provedeny do historie spouštění větve v úložišti Git služby VSTS pokaždé, když odešlete spustit. Používáme snímku spojené s nejnovější spuštění při vytvoření instance projekt na druhém počítači. 
 

## <a name="collaboration"></a>Spolupráce
Můžete spolupracovat s ostatními členy týmu v projektech propojené do úložiště Git služby VSTS. Můžete přiřadit oprávnění pro uživatele na účet experimentování, pracovní prostor a projekt. V tomto okamžiku můžete provést příkazy Azure Resource Manager pomocí rozhraní příkazového řádku Azure. Můžete také použít [portál Azure](https://portal.azure.com). V tématu [následující části](#portal).    

### <a name="using-command-line-to-add-users"></a>Pomocí příkazového řádku pro přidání uživatelů
Umožňuje použít příklad. Řekněme, Alice je vlastníkem tý e_Iris_ projektu a chce sdílet přístup s Bob. 

Alice klikne na **soubor** nabídky a vybere **příkazového řádku** položky nabídky ke spuštění příkazového řádku nakonfigurovat tak, aby _iris_ projektu. Alice je pak může rozhodnout, jaké úrovně přístupu fo chce poskytnout Bobovi spuštěním následujících příkazů.  

```azurecli
# Find ARM ID of the experimnetation account
az ml account experimentation show --query "id"

# Add Bob to the Experimentation Account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the Workspace by inheritance. And he can invite or remove others.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace ARM ID>
```

Po přiřazení role přímo nebo prostřednictvím dědičnosti, Bob vidí na projekt v seznamu Workbench projektu. Aplikace může být nutné restartování, chcete-li zobrazit projektu. Bob pak můžete stáhnout projektu, jak je popsáno v [Roaming části](#roaming) a spolupracovat s Alice. 

Historie spouštění pro všechny uživatele spolupráce na projektu se zaměřuje na stejné vzdáleného úložiště Git. Proto když Alice provede spuštění, Bob vidí spustit v části historie spouštění projektu v aplikaci Workbench. Bob můžete také obnovit projekt stavu všech spustit včetně spustí spustí Alice. 

Sdílení vzdáleného úložiště Git pro projekt umožňuje Alice a Bob také spolupracovat na hlavní větve. V případě potřeby můžou taky vytvořit osobní větve a používat Git-žádosti o přijetí změn a sloučí spolupracovat. 

### <a name="using-azure-portal-to-add-users"></a>Pomocí portálu Azure přidat uživatele
<a name="portal"></a>

Azure Machine Learning experimentování účty, pracovních prostorů a projekty jsou prostředky Azure Resource Manager. Můžete použít odkaz řízení přístupu v [portál Azure](https://portal.azure.com) přiřadit role. 

Najít prostředek, který Pokud chcete přidat uživatele zobrazíte ze všech zdrojů. Klikněte na řízení přístupu (IAM) odkaz v rámci stránky. Přidání uživatelů 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Ukázkový pracovní postup spolupráce
Pro ilustraci toku spolupráce, projděme příklad. Zaměstnanci contoso Alice a Bob chcete spolupracovat na projektu vědecké účely dat pomocí Azure ML Workbench. Svou identitu patřit do stejné klientovi Contoso Azure AD.

1. Alice nejprve vytvoří prázdný úložiště Git v projektu služby VSTS. Tento projekt služby VSTS by měl za provozu v předplatné Azure vytvořili v rámci klienta Contoso AAD. 

2. Alice pak vytvoří účet Azure ML experimentování, pracovní prostor a projektu Azure ML Workbench na svém počítači. Jana poskytuje adresu URL úložiště Git, při vytváření projektu.

3. Alice začne pracovat na projektu. Vytvoří některé skripty a provede několik spustí. Při každém spuštění se automaticky snímek složce celý projekt posune do historie spouštění větev z úložiště služby VSTS Git, vytvořené Workbench jako potvrzení.

4. Alice je nyní radostí s probíhající práce. Chce jeho změnu místní potvrdit _hlavní_ větev a doručí jej do úložiště Git služby VSTS _hlavní_ firemní pobočky. Uděláte to tak, že s projektem otevřeným se spustí v okně příkazového řádku Azure ML Workbench a vydá následující příkazy:
    
    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # stage all changes
    $ git add -A

    # commit changes with a comment
    $ git commit -m "this is a good milestone"

    # push the commit to the master branch of the remote Git repo in VSTS
    $ git push
    ```

5. Alice pak přidá Roberta do pracovního prostoru jako Přispěvatel. Bude moct provést z portálu Azure nebo pomocí `az role assignment` příkaz ilustrují výše. Jana také uděluje Bob přístup pro čtení nebo zápis do úložiště Git služby VSTS.

6. Bob teď nástroje Azure ML Workbench přihlásí na svém počítači. Si můžete zobrazit Alice prostoru sdílet s ním a projekt uvedené v části tohoto pracovního prostoru. 

7. Robert klikne na název projektu a projekt se stáhne do svého počítače.
    
    a. Spustí zaznamenané v historii spouštění klony snímku nejnovější jsou soubory staženého projektu. Nejsou poslední potvrzení na hlavní větve.
    
    b. Složka místní projektu je nastavena na _hlavní_ firemní pobočky s unstaged změny.

8. Bob teď můžete procházet spustí provedený Alice a obnovení snímku všech předchozích spuštění.

9. Bob chce získání nejnovějších změn nabídnutých podle Alice a začít pracovat na jinou firemní pobočky. Tak, aby mohl otevře okno příkazového řádku z Azure ML Workbench a spouští následující příkazy:

    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # get the latest commit in VSTS Git master branch and overwrite current files
    $ git pull --force

    # create a new local branch named "bob" so Bob's work is done on the "bob" branch
    $ git checkout -b bob
    ```

10. Bob teď upraví projektu a odeslání nové spuštění. Změny se provádějí v _bob_ firemní pobočky. A také budou zobrazeny Alici Boba spustí.

11. Robert je nyní připraven k nabízení své změny do vzdáleného úložiště Git. Aby se zabránilo konfliktu s _hlavní_ firemní pobočky, kde Alice pracuje, rozhodne se push svou práci do nové vzdálené větve také s názvem _bob_.

    ```sh
    # verify that the current branch is "bob" and it has unstaged changes
    $ git status
    
    # stage all changes
    $ git add -A

    # commit them with a comment
    $ git commit -m "I found a cool new trick."

    # create a new branch on the remote VSTS Git repo, and push changes
    $ git push origin bob
    ```

12. Bob můžete v jeho kód, pak říct Alice o nové nástrojů efektu a vytvoří žádost o přijetí změn na vzdálené úložiště Git z _bob_ větvení do _hlavní_ firemní pobočky. A Alice můžete pak je sloučit žádost o přijetí změn do _hlavní_ firemní pobočky.

## <a name="next-steps"></a>Další kroky
Další informace o Azure ML Workbench pomocí Git: [úložiště Git pomocí služby Azure Machine Learning Workbench projektu](using-git-ml-project.md)