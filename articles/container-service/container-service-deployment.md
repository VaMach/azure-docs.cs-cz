---
title: "Nasazení clusteru kontejneru Docker v Azure | Dokumentace Microsoftu"
description: "Cluster Azure Container Service můžete nasadit pomocí webu Azure Portal nebo šablony Azure Resource Manageru."
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
ms.date: 02/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: b9be92498f9daf1d2f964cc689bacb2358b237be


---
# <a name="deploy-an-azure-container-service-cluster"></a>Nasazení clusteru Azure Container Service



Azure Container Service umožňuje rychlé nasazení oblíbených open-source řešení pro clustering a orchestraci kontejnerů. Tento dokument vás provede nasazením clusteru Azure Container Service pomocí webu Azure Portal nebo šablony Azure Resource Manageru pro rychlý start. 

Cluster Azure Container Service můžete také nasadit pomocí webu [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) nebo rozhraní API služby Azure Container Service.



## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure:** Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

* **Veřejný klíč SSH RSA:** Při nasazení pomocí portálu nebo některé ze šablon Azure pro rychlý start budete muset zadat veřejný klíč pro ověření s virtuálními počítači Azure Container Service. Pokud chcete vytvořit klíče SSH (Secure Shell) RSA, postupujte podle pokynů pro systémy [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **ID a tajný klíč klienta instančního objektu** (pouze Kubernetes): Další informace a pokyny k vytvoření instančního objektu najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).



## <a name="create-a-cluster-by-using-the-azure-portal"></a>Vytvoření clusteru pomocí webu Azure Portal
1. Přihlaste se k webu Azure Portal, vyberte **Nový** a na Azure Marketplace vyhledejte **Azure Container Service**.

    ![Azure Container Service na Marketplace](media/container-service-deployment/acs-portal1.png)  <br />

2. Vyberte **Azure Container Service** a klikněte na **Vytvořit**.

    ![Vytvoření služby kontejneru](media/container-service-deployment/acs-portal2.png)  <br />

3. Zadejte následující informace:

    * **Uživatelské jméno:** Uživatelské jméno pro účet na všech virtuálních počítačích a ve škálovacích sadách virtuálních počítačů v clusteru Azure Container Service.
    * **Předplatné:** Vyberte předplatné Azure.
    * **Skupina prostředků:** Vyberte existující skupinu prostředků nebo vytvořte novou. Doporučuje se použít pro každé nasazení novou skupinu prostředků.
    * **Umístění:** Vyberte oblast Azure pro nasazení Azure Container Service.
    * **Veřejný klíč SSH RSA:** Přidejte veřejný klíč, který se bude používat pro ověřování u virtuálních počítačů Azure Container Service. Je důležité, aby tento klíč neobsahoval žádné konce řádků a zahrnoval předponu `ssh-rsa`. Přípona `username@domain` je volitelná. Klíč by měl vypadat nějak takto: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. 

4. Až budete připravení pokračovat, klikněte na **OK**.

    ![Základní nastavení](media/container-service-deployment/acs-portal3.png)  <br />

5. V okně **Konfigurace rozhraní** vyberte **Konfigurace orchestrátoru**. Mezi možnosti patří:

  * **DC/OS:** Nasadí cluster DC/OS.
  * **Swarm:** Nasadí cluster Docker Swarm.
  * **Kubernetes**: Nasadí cluster Kubernetes.


6. Až budete připravení pokračovat, klikněte na **OK**.

    ![Volba orchestrátoru](media/container-service-deployment/acs-portal4-new.png)  <br />

7. Pokud je v rozevírací nabídce vybráno **Kubernetes**, budete muset zadat ID (také pod názvem appId) a tajný kód klienta instančního objektu (heslo). Další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).

    ![Zadání instančního objektu pro Kubernetes](media/container-service-deployment/acs-portal10.png)  <br />

7. V okně nastavení **Azure Container Service** zadejte následující informace:

    * **Počet hlavních serverů:** Počet hlavních serverů v clusteru.
    * **Počet agentů:** U Docker Swarm a Kubernetes je tato hodnota počáteční počet agentů ve škálovací sadě agentů. U DC/OS se jedná o počáteční počet agentů v privátní škálovací sadě. Kromě toho se pro DC/OS vytvoří škálovací sada obsahující předem určený počet agentů. Počet agentů v této veřejné škálovací sadě závisí na tom, kolik hlavních serverů bylo vytvořeno v clusteru: jeden veřejný agent pro jeden hlavní server a dva veřejní agenti pro tři nebo pět hlavních serverů.
    * **Velikost virtuálního počítače agenta:** Velikost těchto virtuálních počítačů.
    * **Předpona DNS:** Celosvětově jedinečný název, který se bude používat jako předpona klíčových částí plně kvalifikované názvy domény pro službu.
    * **Diagnostika virtuálních počítačů:** Pro některé zvolené orchestrátory můžete povolit diagnostiku virtuálních počítačů.

8. Až budete připravení pokračovat, klikněte na **OK**.

    ![Nastavení služby Container Service](media/container-service-deployment/acs-portal5.png)  <br />

9. Až se dokončí ověření služby, klikněte na **OK**.

    ![Ověření](media/container-service-deployment/acs-portal6.png)  <br />

10. Přečtěte si podmínky. Kliknutím na **Koupit** zahájíte proces nasazení.

    ![Koupit](media/container-service-deployment/acs-portal7.png)  <br />

    Pokud se rozhodnete nasazení připnout na Azure Portal, uvidíte jeho stav.

    ![Stav nasazení](media/container-service-deployment/acs-portal8.png)  <br />

Dokončení nasazení trvá několik minut. Potom bude cluster Azure Container Service připravený k použití.



## <a name="create-a-cluster-by-using-a-quickstart-template"></a>Vytvoření clusteru pomocí šablony pro rychlý start
Pro nasazení clusteru ve službě Azure Container Service jsou k dispozici šablony Azure pro rychlý start. Poskytované šablony pro rychlý začátek lze upravit tak, aby obsahovaly další nebo rozšířenou konfiguraci Azure. K vytvoření clusteru Azure Container Service pomocí šablony Azure pro rychlý start potřebujete předplatné Azure. Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

Postupujte podle těchto kroků a nasaďte cluster pomocí šablony a Azure CLI 2.0 (viz [pokyny k instalaci a nastavení](/cli/azure/install-az-cli2.md)).

> [!NOTE] 
> Pokud jste v systému Windows, můžete k nasazení šablony podobným způsobem použít Azure PowerShell. Postup najdete dál v této části. Můžete taky nasadit šablonu prostřednictvím [portálu](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo jiné metody.

1. Pokud chcete nasadit cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z šablon rychlého startu z GitHubu. Následuje dílčí seznam. Všimněte si, že šablony DC/OS a Swarm jsou stejné s výjimkou orchestrátoru, který je vybrán jako výchozí.

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

5. Po vytvoření skupiny prostředků můžete vytvořit cluster pomocí následujícího příkazu. Pro parametr `-TemplateUri` bude zadán identifikátor URI požadované šablony. Při spuštění tohoto příkazu vás PowerShell vyzve k zadání hodnot parametrů nasazení.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a>Zadání parametrů šablony
Pokud PowerShell znáte, víte, že mezi parametry dostupnými pro rutinu můžete cyklicky přepínat zadáním znaménka minus (-) a následným stisknutím klávesy TAB. Stejně to funguje i při práci s parametry, které definujete v šabloně. Jakmile zadáte název šablony, rutina načte šablonu, provede analýzu parametrů a dynamicky přidá parametry šablony do příkazu. Díky tomu je zadání hodnot parametrů šablony velmi snadné. A pokud zapomenete zadat hodnotu požadovaného parametru, prostředí PowerShell vás vyzve k jejímu zadání.

Níže je celý příkaz i s vloženými parametry. Pro názvy prostředků můžete zadat vlastní hodnoty.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Další kroky
Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:

* [Připojení ke clusteru Azure Container Service](container-service-connect.md)
* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce se službou Azure Container Service a nástrojem Docker Swarm](container-service-docker-swarm.md)
* [Práce s Azure Container Service a Kubernetes](container-service-kubernetes-walkthrough.md)



<!--HONumber=Feb17_HO4-->


