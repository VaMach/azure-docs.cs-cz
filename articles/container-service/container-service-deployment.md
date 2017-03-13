---
title: "Nasazení clusteru kontejneru Docker v Azure | Dokumentace Microsoftu"
description: "Nasazení řešení Kubernetes, DC/OS nebo Docker Swarm ve službě Azure Container Service pomocí webu Azure Portal nebo šablony Resource Manageru"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, Mikroslužby, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs"
ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2464901d22bb91cbf396ef60f4bda6d979b578b7
ms.openlocfilehash: 003d975f57d63bcb95d6b0de9dcfaf8816fcdd6f
ms.lasthandoff: 03/02/2017

---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-portal"></a>Nasazení řešení hostování kontejnerů Dockeru pomocí webu Azure Portal



Azure Container Service umožňuje rychlé nasazení oblíbených open-source řešení pro clustering a orchestraci kontejnerů. Tento dokument vás provede nasazením clusteru Azure Container Service pomocí webu Azure Portal nebo šablony Azure Resource Manageru pro rychlý start. 

Cluster Azure Container Service můžete také nasadit pomocí webu [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) nebo rozhraní API služby Azure Container Service.

Související informace najdete v tématu [Úvod do služby Azure Container Service](container-service-intro.md).


## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure:** Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

* **Veřejný klíč SSH RSA:** Při nasazení pomocí portálu nebo některé ze šablon Azure pro rychlý start budete muset zadat veřejný klíč pro ověření s virtuálními počítači Azure Container Service. Pokud chcete vytvořit klíče SSH (Secure Shell) RSA, postupujte podle pokynů pro systémy [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **ID a tajný klíč klienta instančního objektu** (pouze Kubernetes): Další informace a pokyny k vytvoření instančního objektu služby Azure Active Directory najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).



## <a name="create-a-cluster-by-using-the-azure-portal"></a>Vytvoření clusteru pomocí webu Azure Portal
1. Přihlaste se k webu Azure Portal, vyberte **Nový** a na Azure Marketplace vyhledejte **Azure Container Service**.

    ![Azure Container Service na Marketplace](media/container-service-deployment/acs-portal1.png)  <br />

2. Klikněte na **Azure Container Service** a potom na **Vytvořit**.

3. V okně **Základy** zadejte následující informace:

    * **Orchestrátor:** Vyberte některý z orchestrátorů kontejneru pro nasazení clusteru.
        * **DC/OS:** Nasadí cluster DC/OS.
        * **Swarm:** Nasadí cluster Docker Swarm.
        * **Kubernetes**: Nasadí cluster Kubernetes.
    * **Předplatné:** Vyberte předplatné Azure.
    * **Skupina prostředků:** Zadejte název nové skupiny prostředků pro nasazení.
    * **Umístění:** Vyberte oblast Azure pro nasazení Azure Container Service. Informace o dostupnosti najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/).
    
    ![Základní nastavení](media/container-service-deployment/acs-portal3.png)  <br />
    
    Až budete připravení pokračovat, klikněte na **OK**.

4. V okně **Konfigurace hlavních uzlů** zadejte následující nastavení pro hlavní linuxový uzel nebo uzly v clusteru (některá nastavení se u jednotlivých orchestrátorů liší):

    * **Název DNS hlavního uzlu:** Předpona, která slouží k vytvoření jedinečného plně kvalifikovaného názvu domény pro hlavní uzel. Plně kvalifikovaný název domény hlavního uzlu je ve formátu *předpona*mgmt.*umístění*.cloudapp.azure.com.
    * **Uživatelské jméno:** Uživatelské jméno účtu na každém virtuálním počítači s Linuxem v clusteru.
    * **Veřejný klíč SSH RSA:** Přidejte veřejný klíč, který se použije k ověřování u virtuálních počítačů s Linuxem. Je důležité, aby tento klíč neobsahoval žádné konce řádků a zahrnoval předponu `ssh-rsa`. Přípona `username@domain` je volitelná. Klíč by měl vypadat nějak takto: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. 
    * **Instanční objekt:** Pokud jste vybrali orchestrátor Kubernetes, zadejte **ID klienta instančního objektu** (označuje se taky jako appId) a **Tajný kód klienta instančního objektu** (heslo) služby Azure Active Directory. Další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).
    * **Počet hlavních serverů:** Počet hlavních serverů v clusteru.
    * **Diagnostika virtuálních počítačů:** Pro některé orchestrátory můžete povolit diagnostiku virtuálních počítačů v hlavních uzlech.

    ![Konfigurace hlavních uzlů](media/container-service-deployment/acs-portal4.png)  <br />

    Až budete připravení pokračovat, klikněte na **OK**.

5. V okně **Konfigurace agentů** zadejte následující informace:

    * **Počet agentů:** U Docker Swarm a Kubernetes je tato hodnota počáteční počet agentů ve škálovací sadě agentů. U DC/OS se jedná o počáteční počet agentů v privátní škálovací sadě. Kromě toho se pro DC/OS vytvoří škálovací sada obsahující předem určený počet agentů. Počet agentů v této veřejné škálovací sadě závisí na počtu hlavních uzlů v clusteru: jeden veřejný agent pro jeden hlavní uzel a dva veřejní agenti pro tři nebo pět hlavních uzlů.
    * **Velikost virtuálního počítače agenta:** Velikost těchto virtuálních počítačů.
    * **Operační systém:** Toto nastavení je aktuálně dostupné, pouze pokud jste vybrali orchestrátor Kubernetes. Vyberte linuxovou distribuci nebo operační systém Windows Server, který poběží na agentech. Toto nastavení určuje, jestli v clusteru bude možné spouštět aplikace typu kontejner Linux nebo Windows. 

        > [!NOTE]
        > Podpora kontejnerů Windows je pro clustery Kubernetes ve verzi Preview. V clusterech DC/OS a Swarm jsou v současné době ve službě Azure Container Service podporováni pouze linuxoví agenti.

    * **Přihlašovací údaje agentů:** Pokud jste vybrali operační systém Windows, zadejte **Uživatelské jméno** a **Heslo** správce virtuálních počítačů agentů. 

    ![Konfigurace agentů](media/container-service-deployment/acs-portal5.png)  <br />

    Až budete připravení pokračovat, klikněte na **OK**.

6. Až se dokončí ověření služby, klikněte na **OK**.

    ![Ověření](media/container-service-deployment/acs-portal6.png)  <br />

7. Přečtěte si podmínky. Proces nasazení zahájíte kliknutím na **Vytvořit**.

    Pokud se rozhodnete nasazení připnout na Azure Portal, uvidíte jeho stav.

    ![Stav nasazení](media/container-service-deployment/acs-portal8.png)  <br />

Dokončení nasazení trvá několik minut. Potom bude cluster Azure Container Service připravený k použití.


## <a name="create-a-cluster-by-using-a-quickstart-template"></a>Vytvoření clusteru pomocí šablony pro rychlý start
Pro nasazení clusteru ve službě Azure Container Service jsou k dispozici šablony Azure pro rychlý start. Poskytované šablony pro rychlý začátek lze upravit tak, aby obsahovaly další nebo rozšířenou konfiguraci Azure. K vytvoření clusteru Azure Container Service pomocí šablony Azure pro rychlý start potřebujete předplatné Azure. Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

Postupujte podle těchto kroků a nasaďte cluster pomocí šablony a Azure CLI 2.0 (viz [pokyny k instalaci a nastavení](/cli/azure/install-az-cli2.md)).

> [!NOTE] 
> Pokud jste v systému Windows, můžete k nasazení šablony podobným způsobem použít Azure PowerShell. Postup najdete dál v této části. Můžete taky nasadit šablonu prostřednictvím [portálu](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo jiné metody.

1. Pokud chcete nasadit cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z šablon rychlého startu z GitHubu. Následuje dílčí seznam. Šablony DC/OS a Swarm jsou stejné s výjimkou orchestrátoru, který je vybrán jako výchozí.

    * [Šablona DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Šablona Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Šablona Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Přihlaste se k účtu Azure (`az login`) a ujistěte se, že rozhraní příkazového řádku Azure je připojeno k předplatnému Azure. Výchozí předplatné můžete zobrazit pomocí následujícího příkazu:

    ```azurecli
    az account show
    ```
    
    Pokud máte více než jedno předplatné a je třeba nastavit jiné výchozí předplatné, spusťte příkaz `az account set --subscription` a zadejte ID nebo název předplatného.

3. Doporučuje se použít pro nasazení novou skupinu prostředků. Pokud chcete vytvořit skupinu prostředků, použijte příkaz `az group create` a zadejte název skupiny prostředků a umístění: 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. Vytvořte soubor JSON obsahující požadované parametry šablony. Stáhněte soubor parametrů s názvem `azuredeploy.parameters.json`, který doprovází šablonu Azure Container Service `azuredeploy.json` v GitHubu. Zadejte požadované hodnoty parametrů pro váš cluster. 

    Pokud chcete například použít [šablonu DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), zadejte hodnoty parametrů pro `dnsNamePrefix` a `sshRSAPublicKey`. Přečtěte si popisy v souboru `azuredeploy.json` a možnosti pro ostatní parametry.  
 

5. Vytvořte cluster Container Service předáním souboru parametrů nasazení následujícímu příkazu, kde:

    * **RESOURCE_GROUP** je název skupiny prostředků, kterou jste vytvořili v předchozím kroku.
    * **DEPLOYMENT_NAME** (volitelný) je název, který jste přidělili nasazení.
    * **TEMPLATE_URI** je umístění souboru nasazení `azuredeploy.json`. Tento identifikátor URI musí být soubor RAW, nikoli ukazatel na uživatelské rozhraní GitHub. Tento identifikátor URI najdete tak, že vyberete soubor `azuredeploy.json` na webu GitHub a kliknete na tlačítko **Raw**.  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    Parametry můžete zadat taky jako řetězec formátu JSON na příkazovém řádku. Použijte příkaz podobný následujícímu:

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > Dokončení nasazení trvá několik minut.
    > 

### <a name="equivalent-powershell-commands"></a>Ekvivalentní příkazy PowerShellu
Šablonu clusteru Azure Container Service můžete také nasadit v PowerShellu. Tento dokument je založen na verzi 1.0 [modulu Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).

1. Pokud chcete nasadit cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z šablon rychlého startu z GitHubu. Následuje dílčí seznam. Všimněte si, že šablony DC/OS a Swarm jsou stejné s výjimkou orchestrátoru, který je vybrán jako výchozí.

    * [Šablona DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Šablona Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Šablona Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Před vytvořením clusteru v rámci vašeho předplatného Azure ověřte, že daná relace prostředí PowerShell je přihlášena k Azure. Můžete to provést pomocí příkazu `Get-AzureRMSubscription`:

    ```powershell
    Get-AzureRmSubscription
    ```

3. Pokud se potřebujete přihlásit k Azure, použijte příkaz `Login-AzureRMAccount`:

    ```powershell
    Login-AzureRmAccount
    ```

4. Doporučuje se použít pro nasazení novou skupinu prostředků. Pokud chcete vytvořit skupinu prostředků, použijte příkaz `New-AzureRmResourceGroup` a zadejte název skupiny prostředků a cílovou oblast:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. Po vytvoření skupiny prostředků můžete vytvořit cluster pomocí následujícího příkazu. Identifikátor URI požadované šablony se určuje pomocí parametru `-TemplateUri`. Při spuštění tohoto příkazu vás PowerShell vyzve k zadání hodnot parametrů nasazení.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a>Zadání parametrů šablony
Pokud PowerShell znáte, víte, že mezi parametry dostupnými pro rutinu můžete cyklicky přepínat zadáním znaménka minus (-) a následným stisknutím klávesy TAB. Stejně to funguje i při práci s parametry, které definujete v šabloně. Jakmile zadáte název šablony, rutina načte šablonu, provede analýzu parametrů a dynamicky přidá parametry šablony do příkazu. Díky tomu je zadání hodnot parametr šablony snadné. A pokud zapomenete zadat hodnotu požadovaného parametru, prostředí PowerShell vás vyzve k jejímu zadání.

Tady je celý příkaz včetně parametrů. Pro názvy prostředků zadejte vlastní hodnoty.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Další kroky
Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:

* [Připojení ke clusteru Azure Container Service](container-service-connect.md)
* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce se službou Azure Container Service a nástrojem Docker Swarm](container-service-docker-swarm.md)
* [Práce s Azure Container Service a Kubernetes](container-service-kubernetes-walkthrough.md)

