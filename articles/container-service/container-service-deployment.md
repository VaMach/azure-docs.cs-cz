---
title: "Nasazení clusteru Azure Container Service | Dokumentace Microsoftu"
description: "Cluster Azure Container Service můžete nasadit pomocí Portálu Azure, rozhraní příkazového řádku Azure CLI nebo prostředí PowerShell."
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
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: a7d957fd4be4c823077b1220dfb8ed91070a0e97
ms.openlocfilehash: d056b9489eba1f97e8fb87f231b03d104c4cab66


---
# <a name="deploy-an-azure-container-service-cluster"></a>Nasazení clusteru Azure Container Service
Azure Container Service umožňuje rychlé nasazení oblíbených open-source řešení pro clustering a orchestraci kontejnerů. Pomocí Azure Container Service můžete nasadit clustery DC/OS, Kubernetes a Docker Swarm s pomocí šablon Azure Resource Manageru nebo webu Azure Portal. Tyto clustery nasadíte pomocí sad škálování virtuálních počítačů Azure, a díky tomu bude u nich možné využívat nabídky Azure v oblasti sítí a úložišť. Pro přístup k Azure Container Service potřebujete předplatné Azure. Pokud žádné nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Tento dokument vás provede nasazením clusteru Azure Container Service pomocí [Portálu Azure](#creating-a-service-using-the-azure-portal), [rozhraní příkazového řádku (CLI) Azure](#creating-a-service-using-the-azure-cli) a [modulu Azure PowerShell](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Vytvoření služby pomocí Portálu Azure
Přihlaste se k webu Azure Portal, vyberte **Nový** a na Azure Marketplace vyhledejte **Azure Container Service**.

![Vytvoření nasazení 1](media/acs-portal1.png)  <br />

Vyberte **Azure Container Service** a klikněte na **Vytvořit**.

![Vytvoření nasazení 2](media/acs-portal2.png)  <br />

Zadejte následující informace:

* **Uživatelské jméno:** Toto je uživatelské jméno, které se bude pro účet používat na všech virtuálních počítačích a v sadách škálování virtuálních počítačů v clusteru Azure Container Service.
* **Předplatné:** Vyberte předplatné Azure.
* **Skupina prostředků:** Vyberte existující skupinu prostředků nebo vytvořte novou.
* **Umístění:** Vyberte oblast Azure pro nasazení Azure Container Service.
* **Veřejný klíč SSH:** Přidejte veřejný klíč, který se bude používat pro ověřování u virtuálních počítačů Azure Container Service. Je velmi důležité, aby tento klíč neobsahoval žádné konce řádků a zahrnoval předponu ssh-rsa a příponu 'username@domain'. Měl by vypadat nějak takto: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. Pokyny k vytváření klíčů SSH (Secure Shell) najdete v tématech [Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) a [Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/).

Až budete připravení pokračovat, klikněte na **OK**.

![Vytvoření nasazení 3](media/acs-portal3.png)  <br />

Vyberte typ orchestrace. Dostupné možnosti:

* **DC/OS:** Nasadí cluster DC/OS.
* **Swarm:** Nasadí cluster Docker Swarm.
* **Kubernetes**: Nasadí cluster Kubernetes.

Až budete připravení pokračovat, klikněte na **OK**.

![Vytvoření nasazení 4](media/acs-portal4-new.png)  <br />

Pokud je v rozevírací nabídce vybráno **Kubernetes**, budete muset zadat ID a tajný klíč klienta instančního objektu.
Další informace o tom, jak vytvořit instanční objekt, najdete na [této](https://github.com/Azure/acs-engine/blob/master/docs/serviceprincipal.md) stránce. 

![Vytvoření nasazení 4,5](media/acs-portal10.PNG)  <br />

Zadejte následující informace:

* **Počet hlavních serverů:** Počet hlavních serverů v clusteru. Pokud je vybráno „Kubernetes“, bude počet hlavních serverů nastaven na výchozí hodnotu 1.
* **Počet agentů**: U Docker Swarm a Kubernetes to bude počáteční počet agentů ve škálovací sadě agentů. U DC/OS to bude počáteční počet agentů v privátní sadě škálování. Kromě toho se vytvoří sada škálování obsahující předem určený počet agentů. Počet agentů v této veřejné sadě škálování závisí na tom, kolik hlavních serverů bylo vytvořeno v clusteru: jeden veřejný agent pro jeden hlavní server a dva veřejní agenti pro tři nebo pět hlavních serverů.
* **Velikost virtuálního počítače agenta:** Velikost těchto virtuálních počítačů.
* **Předpona DNS:** Celosvětově jedinečný název, který se bude používat jako předpona klíčových částí plně kvalifikované názvy domény pro službu.

Až budete připravení pokračovat, klikněte na **OK**.

![Vytvoření nasazení 5](media/acs-portal5.png)  <br />

Až se dokončí ověření služby, klikněte na **OK**.

![Vytvoření nasazení 6](media/acs-portal6.png)  <br />

Kliknutím na **Vytvořit** zahájíte proces nasazení.

![Vytvoření nasazení 7](media/acs-portal7.png)  <br />

Pokud se rozhodnete nasazení připnout na Azure Portal, uvidíte jeho stav.

![Vytvoření nasazení 8](media/acs-portal8.png)  <br />

Po dokončení nasazování bude cluster Azure Container Service připraven k použití.

## <a name="create-a-service-by-using-the-azure-cli"></a>Vytvoření služby pomocí rozhraní příkazového řádku Azure CLI
Pokud chcete vytvořit instanci Azure Container Service pomocí příkazového řádku, potřebujete předplatné Azure. Pokud žádné nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Také musíte mít [nainstalováno](../xplat-cli-install.md) a [nakonfigurováno](../xplat-cli-connect.md) rozhraní příkazového řádku Azure CLI.

Pokud chcete nasadit cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z následujících šablon z GitHubu. 

* [Šablona DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Šablona Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
* [Šablona Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

Dále se ujistěte, že rozhraní Azure CLI je připojeno k předplatnému Azure. Můžete to provést pomocí následujícího příkazu:

```bash
azure account show
```
Pokud není účet Azure vrácen, použijte následující příkaz, kterým přihlásíte rozhraní CLI k Azure:

```bash
azure login -u user@domain.com
```

Potom nakonfigurujte nástroje Azure CLI tak, aby používaly Azure Resource Manager:

```bash
azure config mode arm
```

Vytvořte skupinu prostředků Azure a cluster Container Service pomocí následujícího příkazu, kde:

* **RESOURCE_GROUP** je název skupiny prostředků, kterou chcete použít pro tuto službu.
* **LOCATION** je oblast Azure, kde se skupina prostředků a nasazení Azure Container Service vytvoří.
* **TEMPLATE_URI** je umístění souboru nasazení. Poznámka: Tato hodnota musí být soubor RAW, nikoli ukazatel na uživatelské rozhraní GitHub. Tuto adresu URL najdete tak, že vyberete soubor azuredeploy.json na webu GitHub a kliknete na tlačítko **Raw**.

> [!NOTE]
> Při spuštění tohoto příkazu vás prostředí vyzve k zadání hodnot parametrů nasazení.
> 
> 

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Zadání parametrů šablony
Tato verze příkazu vyžaduje, abyste parametry definovali interaktivně. Pokud chcete zadat parametry, jako je řetězec ve formátu JSON, můžete to udělat pomocí přepínače `-p`. Příklad:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Alternativně můžete poskytnout soubor parametrů ve formátu JSON pomocí přepínače `-e`:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Pokud chcete zobrazit příklad souboru parametrů s názvem `azuredeploy.parameters.json`, vyhledejte ho v šablonách Azure Container Service na webu GitHub.

## <a name="create-a-service-by-using-powershell"></a>Vytvoření služby pomocí prostředí PowerShell
Cluster Azure Container Service můžete také nasadit v prostředí PowerShell. Tento dokument je založen na verzi 1.0 [modulu Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).

Pokud chcete nasadit cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z následujících šablon. Všimněte si, že tyto dvě šablony jsou stejné s výjimkou orchestrátoru, který je vybrán jako výchozí.

* [Šablona DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Šablona Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
* [Šablona Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

Před vytvořením clusteru v rámci vašeho předplatného Azure ověřte, že daná relace prostředí PowerShell je přihlášena k Azure. Můžete to provést pomocí příkazu `Get-AzureRMSubscription`:

```powershell
Get-AzureRmSubscription
```

Pokud se potřebujete přihlásit k Azure, použijte příkaz `Login-AzureRMAccount`:

```powershell
Login-AzureRmAccount
```

Pokud nasazení provádíte do nové skupiny prostředků, je třeba nejdřív vytvořit skupinu prostředků. Pokud chcete vytvořit novou skupinu prostředků, použijte příkaz `New-AzureRmResourceGroup` a zadejte název skupiny prostředků a cílovou oblast:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Po vytvoření skupiny prostředků můžete vytvořit cluster pomocí následujícího příkazu. Pro parametr `-TemplateUri` bude zadán identifikátor URI požadované šablony. Při spuštění tohoto příkazu vás PowerShell vyzve k zadání hodnot parametrů nasazení.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Zadání parametrů šablony
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




<!--HONumber=Nov16_HO5-->


