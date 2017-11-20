---
title: "Instanční objekt pro cluster Azure Kubernetes | Dokumentace Microsoftu"
description: "Vytvoření a správa instančního objektu služby Azure Active Directory pro cluster Kubernetes v AKS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6c61d99f1d023ac643455faae10ef284f1f5bb14
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Instanční objekty se službou Azure Container Service (AKS)

Cluster AKS vyžaduje [instanční objekt služby Azure Active Directory](../active-directory/develop/active-directory-application-objects.md) pro interakci s rozhraními API Azure. Instanční objekt je potřeba k dynamické správě prostředků, jako jsou například [uživatelem definované trasy](../virtual-network/virtual-networks-udr-overview.md) a [vrstva 4 služby Azure Load Balancer](../load-balancer/load-balancer-overview.md).

Tento článek ukazuje různé možnosti nastavení instančního objektu pro cluster Kubernetes ve službě AKS.

## <a name="before-you-begin"></a>Než začnete

Podrobně popsané kroky v tomto dokumentu předpokládají, že jste vytvořili cluster AKS a navázali s ním připojení přes kubectl. Pokud potřebujete tyto položky, přečtěte si [Rychlý úvod ke službě AKS](./kubernetes-walkthrough.md).

Abyste mohli vytvořit instanční objekt služby Azure AD, musíte mít oprávnění k registraci aplikace v tenantu Azure AD a přiřazení aplikace k roli v předplatném. Pokud nemáte potřebná oprávnění, možná budete muset požádat správce služby Azure AD nebo předplatného o jejich přiřazení nebo vytvořit instanční objekt pro cluster Kubernetes předem.

Také musíte mít nainstalované a nakonfigurované rozhraní Azure CLI verze 2.0.21 nebo novější. Pokud chcete zjistit verzi, spusťte příkaz az --version. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-sp-with-aks-cluster"></a>Vytvoření instančního objektu s clusterem AKS

Při nasazování clusteru AKS pomocí příkazu `az aks create` máte možnost automaticky vygenerovat instanční objekt.

V následujícím příkladu se vytvoří cluster AKS, a protože není zadaný existující instanční objekt, vytvoří se pro cluster nový instanční objekt. Pro dokončení této operace musí mít váš účet příslušná práva k vytvoření instančního objektu.

```azurecli
az aks create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Použití existujícího instančního objektu

Pro použití s clusterem AKS je možné použít existující instanční objekt služby Azure AD nebo ho předem vytvořit. To je užitečné při nasazování clusteru z webu Azure Portal, kdy se vyžaduje zadání informací o instančním objektu.

Pokud chcete použít existující instanční objekt, musí splňovat následující požadavky:

- Obor: Předplatné použité k nasazení clusteru
- Role:Přispěvatel
- Tajný klíč klienta: Musí to být heslo

## <a name="pre-create-a-new-sp"></a>Vytvoření nového instančního objektu předem

Pokud chcete vytvořit instanční objekt pomocí Azure CLI, použijte příkaz [az ad sp create-for-rbac]().

```azurecli
id=$(az account show --query id --output tsv)
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$id"
```

Výstup je podobný tomuto. Poznamenejte si hodnoty `appId` a `password`. Tyto hodnoty se používají při vytváření clusteru AKS.

```
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Použití existujícího instančního objektu

Pokud chcete použít předem vytvořený instanční objekt, zadejte `appId` a `password` jako hodnoty argumentů příkazu `az aks create`.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-princal <appId> ----client-secret <password>
```

Pokud nasazujete cluster AKS z webu Azure Portal, zadejte tyto hodnoty do formuláře konfigurace clusteru AKS.

![Obrázek přechodu na aplikaci Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Další aspekty

Při práci s instančními objekty služeb Azure AD a AKS mějte na paměti následující informace.

* Instanční objekt pro Kubernetes je součástí konfigurace clusteru. K nasazení clusteru ale nepoužívejte identitu.
* Každý instanční objekt je přidružený k aplikaci Azure AD. Instanční objekt pro cluster Kubernetes může být přidružený k jakémukoli platnému názvu aplikace Azure AD (například `https://www.contoso.org/example`). Adresa URL aplikace nemusí být skutečný koncový bod.
* Při zadávání **ID klienta** instančního objektu můžete použít hodnotu `appId` (jak je ukázáno v tomto článku) nebo odpovídající `name` instančního objektu (například `https://www.contoso.org/example`).
* Na hlavním virtuálním počítači a virtuálních počítačích uzlů v clusteru Kubernetes jsou pověření instančního objektu uložená v souboru /etc/kubernetes/azure.json.
* Pokud použijete příkaz `az aks create` k automatickému vygenerování instančního objektu, zapíší se přihlašovací údaje instančního objektu do souboru ~/.azure/acsServicePrincipal.json na počítači, který jste ke spuštění příkazu použili.
* Pokud použijete příkaz `az aks create` k automatickému vygenerování instančního objektu, bude se tento instanční objekt moci ověřovat také pomocí služby [Azure Container Registry](../container-registry/container-registry-intro.md) vytvořené ve stejném předplatném.

## <a name="next-steps"></a>Další kroky

Další informace o instančních objektech služby Azure Active Directory najdete v dokumentaci k aplikacím Azure AD.

> [!div class="nextstepaction"]
> [Aplikační a instanční objekty](../active-directory/develop/active-directory-application-objects.md)
