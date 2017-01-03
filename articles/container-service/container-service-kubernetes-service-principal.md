---
title: "Instanční objekt pro cluster Azure Kubernetes | Dokumentace Microsoftu"
description: "Vytvoření a správa instančního objektu služby Azure Active Directory v clusteru Azure Container Service přes Kubernetes"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 24e12e4606a5ec4fabf7046fe9847123033bb70a
ms.openlocfilehash: 073a9e66ac68643b27ecdd44a4ecac3ad79ec218


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>O instančním objektu služby Azure Active Directory pro cluster Kubernetes v Azure Container Service



Kubernetes vyžaduje v Azure Container Service [instanční objekt služby Azure Active Directory](../active-directory/active-directory-application-objects.md) jako účet služby pro interakci s rozhraními API Azure. Instanční objekt je potřeba k dynamické správě prostředků, jako jsou například [uživatelem definované trasy](../virtual-network/virtual-networks-udr-overview.md) a vrstva 4 služby [Azure Load Balancer](../load-balancer/load-balancer-overview.md).

Tento článek popisuje různé možnosti specifikace instančního objektu pro cluster Kubernetes. Pokud jste například nainstalovali a nastavili [Azure CLI 2.0 (Preview)](https://docs.microsoft.com/cli/azure/install-az-cli2), můžete spustit příkaz [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) a vytvořit současně cluster Kubernetes i instanční objekt.

> [!NOTE]
> Podpora pro Kubernetes je v Azure Container Service momentálně ve verzi preview.


## <a name="requirements-for-the-service-principal"></a>Požadavky pro instanční objekt

Toto jsou požadavky pro instanční objekt služby Azure Active Directory v clusteru Kubernetes v Azure Container Service. 

* **Obor** – Předplatné Azure, do kterého se cluster nasazuje.

* **Role** - **Přispěvatel**

* **Tajný kód klienta** – Musí to být heslo. V současné době nemůžete použít instanční objekt nastavený pro ověření certifikátu.

> [!NOTE]
> Každý instanční objekt je přidružený k aplikaci Azure Active Directory. Instanční objekt pro cluster Kubernetes se dá přidružit jakémukoli platnému názvu aplikace Azure Active Directory.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Možnosti instančního objektu pro cluster Kubernetes

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>Možnost 1: Předání ID klienta instančního objektu a tajného kódu klienta

Při vytváření clusteru Kubernetes zadejte **ID klienta** (pro ID aplikace často označované jako `appId`) a **tajný kód klienta** (`password`) existujícího instančního objektu jako parametry. Pokud používáte existující instanční objekt, ujistěte se, že splňuje požadavky uvedené v předchozí části. Pokud instanční objekt potřebujete vytvořit, přečtěte si téma [Vytvoření instančního objektu](#create-a-service-principal-in-azure-active-directory) dále v tomto článku.

Tyto parametry můžete při [nasazování clusteru Kubernetes](./container-service-deployment.md) určit pomocí portálu, rozhraní příkazového řádku Azure (CLI) nebo prostředí Azure PowerShell.

>[!TIP] 
>Při zadávání **ID klienta** se ujistěte, že používáte `appId` instančního objektu, a nikoli `ObjectId` instančního objektu.
>

Následující příklad ukazuje jeden ze způsobů předání parametrů pomocí [rozhraní příkazového řádku Azure](../xplat-cli-install.md) v [režimu Resource Manageru](../xplat-cli-connect.md). Tento příklad používá [šablonu Kubernetes pro rychlý start](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. Soubor s parametry šablony azuredeploy.parameters.json [si stáhněte](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) z GitHubu.

2. Instanční objekt specifikujte zadáním hodnot pro `servicePrincipalClientId` a `servicePrincipalClientSecret` v souboru. (Pro `dnsNamePrefix` a `sshRSAPublicKey` musíte zadat také vlastní hodnoty. V druhém případě se jedná o veřejný klíč SSH pro přístup ke clusteru.) Uložte soubor.

    ![Předání parametrů instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Spusťte následující příkaz a parametr `-e` použijte k nastavení cesty k souboru azuredeploy.parameters.json. Tento příkaz nasadí cluster v existující skupině prostředků s názvem `myResourceGroup`.

    ```CLI
    azure group deployment create -n myClusterName -g myResourceGroup --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" -e azuredeploy.parameters.json
    ```


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20-preview"></a>Možnost 2: Vygenerování instančního objektu při vytváření clusteru pomocí Azure CLI 2.0 Preview

Pokud jste si nainstalovali a nastavili [Azure CLI 2.0 (Preview)](https://docs.microsoft.com/cli/azure/install-az-cli2), můžete spustit příkaz [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) pro [vytvoření clusteru](./container-service-create-acs-cluster-cli.md).

Stejně jako u ostatních možností vytvoření clusteru Kubernetes můžete při spuštění příkazu `az acs create` určit parametry pro existující instanční objekt. Pokud ale tyto parametry vynecháte, Azure Container Service vytvoří instanční objekt automaticky. Tato akce se provede transparentně během nasazení. 

Následující příkaz vytvoří cluster Kubernetes a vygeneruje klíče SSH a pověření instančního objektu:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Vytvoření instančního objektu v Azure Active Directory

Pokud chcete vytvořit instanční objekt v Azure Active Directory pro použití v clusteru Kubernetes, nabízí vám Azure několik metod. 

Příkazy v následujícím příkladu vám ukážou, jak to můžete udělat pomocí [Azure CLI 2.0 (Preview)](https://docs.microsoft.com/cli/azure/install-az-cli2). Instanční objekt můžete případně vytvořit pomocí [rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md), [prostředí Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md) nebo [klasického portálu](../azure-resource-manager/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Nezapomeňte si zkontrolovat požadavky pro instanční objekt uvedené výše v tomto článku.
>

```console
az login

az account set --subscription="mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Vrátí se výsledek podobný tomuto (zobrazuje se zde zrevidovaně):

![Vytvoření instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Zvýrazní se **ID klienta** (`appId`) a **tajný kód klienta** (`password`), které se použijí jako parametry instančního objektu pro nasazení clusteru.


Instanční objekt potvrďte otevřením nového prostředí a spuštěním následujících příkazů s nahrazením `appId`, `password` a `tenant`:

```console 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Další aspekty


* Při určování **ID klienta** instančního objektu můžete použít hodnotu `appId` (jak je uvedeno v tomto článku) nebo odpovídající `name` instančního objektu (například `https://www.contoso.org/example`).

* Pokud použijete příkaz `az acs create` k automatickému vygenerování instančního objektu, zapíší se pověření instančního objektu do souboru ~/.azure/acsServicePrincipal.json na počítači, který jste ke spuštění příkazu použili.

* Na hlavním virtuálním počítači a virtuálních počítačích uzlů v clusteru Kubernetes jsou pověření instančního objektu uložená v souboru /etc/kubernetes/azure.json.

## <a name="next-steps"></a>Další kroky

* [Začněte používat Kubernetes](container-service-kubernetes-walkthrough.md) v clusteru služby kontejneru.



<!--HONumber=Jan17_HO1-->


