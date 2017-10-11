---
title: "Nasazení do Azure OpenShift počátek | Microsoft Docs"
description: "Naučte se nasadit OpenShift původ na virtuálních počítačích Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: e03da05625e440eab29ccc28a2343d3433fc7607
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-openshift-origin-to-azure-virtual-machines"></a>Nasazení OpenShift původ na virtuálních počítačích Azure 

[Původ OpenShift](https://www.openshift.org/) je kontejner platforma s otevřeným zdrojem založený na [Kubernetes](https://kubernetes.io/). Zjednodušuje proces nasazení, škálování a provozování víceklientským aplikacím. 

Tato příručka popisuje postup nasazení OpenShift původ na virtuálních počítačích Azure pomocí rozhraní příkazového řádku Azure a šablon Azure Resource Manageru. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.
> * Nasazení clusteru OpenShift na virtuálních počítačích Azure. 
> * Instalace a konfigurace [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) ke správě clusteru.
> * Přizpůsobte OpenShift nasazení.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento úvodní vyžaduje Azure CLI verze 2.0.8 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-to-azure"></a>Přihlaste se k Azure. 
Přihlaste se k předplatnému Azure s [az přihlášení](/cli/azure/#login) příkazů a postupujte podle na obrazovce pokynů nebo klikněte na **vyzkoušet** používat cloudové prostředí.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Vytvoření trezoru klíčů
Vytvoření KeyVault pro ukládání klíčů SSH pro cluster s [vytvořit az keyvault](/cli/azure/keyvault#create) příkaz.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Vytvoření klíče SSH 
Klíč SSH je potřeba zabezpečit přístup k počátku OpenShift clusteru. Vytvoření dvojice klíč SSH pomocí `ssh-keygen` příkaz. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Pár klíčů SSH, které vytvoříte nesmí mít přístupové heslo.

Další informace o klíče SSH v systému Windows [vytvoření SSH klíčů v systému Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Uložit privátní klíč SSH v Key Vault
Nasazení OpenShift používá klíč SSH, které jste vytvořili pro zabezpečený přístup k hlavnímu serveru OpenShift. Pokud chcete povolit nasazení tak, aby bezpečně načítat klíč SSH, uložení klíče v Key Vault, pomocí následujícího příkazu.

# <a name="enabled-for-template-deployment"></a>Povolit pro nasazení šablony
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu 
OpenShift komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například OpenShift. Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure. Pokud chcete zvýšit zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstupní přihlašovací údaje, které potřebuje OpenShift:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Poznamenejte si vlastnost appId vrácená z příkazu.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Nevytvářejte nezabezpečené heslo.  Postupujte podle pokynů v tématu [Pravidla a omezení pro hesla Azure AD](/azure/active-directory/active-directory-passwords-policy).

Další informace o objekty služby najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-the-openshift-origin-template"></a>Nasazení šablony OpenShift původu
Nasaďte další OpenShift počátek pomocí šablony Azure Resource Manager. 

> [!NOTE] 
> Tento příkaz vyžaduje az rozhraní příkazového řádku 2.0.8 nebo novější. Můžete ověřit az CLI verze se `az --version` příkaz. K aktualizaci verze rozhraní příkazového řádku, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

Použití `appId` hodnotu z objektu služby, který jste dříve vytvořili pro `aadClientId` parametr.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
Nasazení může trvat až 20 minut. Adresa URL konzoly OpenShift a název DNS je hlavní server OpenShift vytiskne na terminálu po dokončení nasazení.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-to-the-openshift-cluster"></a>Připojte se ke clusteru OpenShift
Po dokončení nasazení připojit ke konzole OpenShift pomocí prohlížeče `OpenShift Console Uri`. Alternativně můžete připojit k hlavnímu OpenShift pomocí následujícího příkazu.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, OpenShift clusteru a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento kurz, zjištěné postup:
> [!div class="checklist"]
> * Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.
> * Nasazení clusteru OpenShift na virtuálních počítačích Azure. 
> * Instalace a konfigurace [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) ke správě clusteru.

Nyní je nasazený tento OpenShift původní cluster. Zjistěte, jak nasadit svoji první aplikaci a používat nástroje OpenShift můžete výukové OpenShift. V tématu [Začínáme s OpenShift původu](https://docs.openshift.org/latest/getting_started/index.html) začít pracovat. 
