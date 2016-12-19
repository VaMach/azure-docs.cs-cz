---
title: "Vytvoření registru kontejnerů pomocí Azure CLI | Dokumentace Microsoftu"
description: "Začínáme vytvářet a spravovat registry kontejnerů Azure pomocí Azure CLI 2.0 Preview"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: aa4b960ed75b5a4702317bf557b4588e7a54fa0e
ms.openlocfilehash: d806564056390ea2db74eebbbe89e1b20a350f33

---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Vytvoření registru kontejnerů pomocí Azure CLI
Pomocí příkazů na stránce [Azure CLI 2.0 Preview](https://github.com/Azure/azure-cli) můžete vytvořit registr kontejnerů a spravovat jeho nastavení z počítače se systémem Linux, Mac nebo Windows. Vytvořit a spravovat registry kontejnerů můžete také pomocí webu [Azure Portal](container-registry-get-started-portal.md) nebo programově pomocí rozhraní [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376) služby Container Registry.


* Související informace a koncepty najdete v tématu [Co je Azure Container Registry?](container-registry-intro.md)
* Nápovědu k příkazům rozhraní příkazového řádku služby Container Registry (příkazy `az acr`) získáte předáním parametru `-h` příslušnému příkazu.

> [!NOTE]
> Container Registry je aktuálně ve verzi Preview.
> 
> 

## <a name="prerequisites"></a>Požadavky
* **Azure CLI 2.0 Preview** – Chcete-li nainstalovat a začít používat Azure CLI 2.0 Preview, přečtěte si [pokyny k instalaci](https://github.com/Azure/azure-cli/blob/master/README.rst). Přihlaste se ke svému předplatnému Azure spuštěním příkazu `az login`.
* **Skupina prostředků** – Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků. Ujistěte se, že je skupina prostředků v umístění, kde je služba Container Registry [dostupná](https://azure.microsoft.com/regions/services/). Chcete-li vytvořit skupinu prostředků pomocí Azure CLI 2.0 Preview, podívejte se na [ukázky Azure CLI 2.0 Preview](https://github.com/Azure/azure-cli-samples/tree/master/arm). 
* **Účet úložiště** (volitelné) – Pro účely zálohování registru kontejnerů vytvořte standardní [účet úložiště](../storage/storage-introduction.md) Azure ve stejném umístění. Pokud při vytváření registru pomocí příkazu `az acr create` nezadáte účet úložiště, příkaz ho vytvoří za vás. Chcete-li vytvořit účet úložiště pomocí Azure CLI 2.0 Preview, podívejte se na [ukázky Azure CLI 2.0 Preview](https://github.com/Azure/azure-cli-samples/tree/master/storage).
* **Instanční objekt** (volitelné) – Pokud vytvoříte registr pomocí rozhraní příkazového řádku, ve výchozím nastavení nebude nastavený pro přístup. Podle potřeby můžete k registru přiřadit existující instanční objekt Azure Active Directory (nebo vytvořit a přiřadit nový) nebo povolit uživatelský účet s právy pro správu registru. Pokyny najdete v dalších částech tohoto článku. Další informace o přístupu k registru najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md). 

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
Spuštěním příkazu `az acr create` vytvořte registr kontejnerů. 

> [!TIP]
> Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice. V tomto příkladu je název registru `myRegistry`, ale nahraďte jej vlastním jedinečným názvem. 
> 
> 

Následující příkaz pomocí minimálního počtu parametrů vytvoří registr kontejnerů `myRegistry` ve skupině prostředků `myResourceGroup` v umístění Střed USA – jih:

```azurecli
az acr create -n myRegistry -g myResourceGroup -l southcentralus
```

* Parametry `--storage-account-name` a `-s` jsou volitelné. Pokud není zadaný,vytvoří se účet úložiště s náhodným názvem v zadané skupině prostředků.

Výstup je podobný tomuto:

![Výstup příkazu az acr create](./media/container-registry-get-started-azure-cli/acr_create.png)


Všimněte si zejména těchto hodnot:

* `id` – Identifikátor registru v rámci vašeho předplatného, který budete potřebovat, pokud budete chtít registru přiřadit instanční objekt. 
* `loginServer` – Plně kvalifikovaný název, který budete muset zadat pro [přihlášení k registru](container-registry-authentication.md). V tomto příkladu je název `myregistry-contoso.exp.azurecr.io` (malými písmeny).

## <a name="assign-a-service-principal"></a>Přiřazení instančního objektu
Pomocí příkazů Azure CLI 2.0 Preview přiřaďte registru instanční objekt Azure Active Directory. Instančnímu objektu v těchto příkladech je přiřazena role vlastníka, ale pokud chcete, můžete mu přiřadit [jiné role](../active-directory/role-based-access-control-configure.md).

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a>Vytvoření instančního objektu a přiřazení přístupu k registru
V následujícím příkazu se novému instančnímu objektu přiřadí přístup role vlastníka k identifikátoru registru předanému v parametru `--scopes`. V parametru `--password` zadejte silné heslo.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Přiřazení existujícího instančního objektu
Pokud již máte instanční objekt, kterému chcete přiřadit přístup role vlastníka k registru, spusťte podobný příkaz jako v následujícím příkladu. ID aplikace instančního objektu předáte pomocí parametru `--assignee`:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Správa přihlašovacích údajů správce
Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý. Následující příklady ukazují příkazy rozhraní příkazového řádku `az acr`, které slouží ke správě přihlašovacích údajů správce pro registr kontejnerů.

### <a name="obtain-admin-user-credentials"></a>Získání přihlašovacích údajů uživatele s právy pro správu
```azurecli
az acr credential show -n myRegistry
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Povolení uživatele s právy pro správu pro existující registr
```azurecli
az acr update -n myRegistry --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Zakázání uživatele s právy pro správu pro existující registr
```azurecli
az acr update -n myRegistry --admin-enabled false
```

## <a name="list-images-and-tags"></a>Výpis obrázků a značek
Pomocí příkazů rozhraní příkazového řádku `az acr` se můžete dotazovat na obrázky a značky v úložišti. 

> [!NOTE]
> Container Registry v současné době nepodporuje použití příkazu `docker search` k dotazování obrázků a značek.


### <a name="list-repositories"></a>Výpis úložišť
Následující příklad zobrazí seznam úložišť v registru ve formátu JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myRegistry -o json
```

### <a name="list-tags"></a>Výpis značek
Následující příklad zobrazí seznam značek na úložišti **samples/nginx** ve formátu JSON:

```azurecli
az acr repository show-tags -n myRegistry --repository samples/nginx -o json
```

## <a name="next-steps"></a>Další kroky
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)




<!--HONumber=Nov16_HO3-->


