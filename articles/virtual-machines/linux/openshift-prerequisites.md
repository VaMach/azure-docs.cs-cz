---
title: "OpenShift v Azure požadavky | Microsoft Docs"
description: "Požadavky na nasazení OpenShift v Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 0c90b8a6d17fa293b6708d942afd35e1333623cb
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="common-prerequisites-for-openshift-in-azure"></a>Běžné požadavky pro OpenShift v Azure

Při nasazování OpenShift v Azure, existuje několik běžných požadavky bez ohledu na to, jestli nasazujete OpenShift původ nebo OpenShift kontejneru platformy.

Instalace OpenShift se provádí prostřednictvím Ansible playbooks. Ansible používá SSH k připojení ke všem hostitelům, které budou součástí clusteru, aby bylo možné dokončit kroky instalace.
Při zahájení připojení SSH na vzdáleného hostitele, neexistuje žádný způsob, jak zadat přístupové heslo. Z tohoto důvodu privátní klíč nemůže mít přístupové heslo s ním spojená nebo nasazení se nezdaří.
Vzhledem k tomu, že všechny virtuální počítače jsou nasazeny pomocí šablony Resource Manageru, stejný veřejný klíč se používá pro přístup ke všem virtuálním počítačům. Musíme vložit do virtuálního počítače, který provádí také všechny playbooks odpovídající soukromý klíč.
Pokud to chcete provést bezpečně, použijeme k předání privátní klíč do virtuálního počítače Azure Key Vault.

Pokud je třeba pro trvalé úložiště pro kontejnery, jsou potřeba trvalé svazky (PV). Tyto systémy současné hodnoty musí být založenou na určitou formu trvalého úložiště. OpenShift podporuje Azure disky (VHD) pro tuto funkci, ale Azure musí být nejprve nakonfigurován jako zprostředkovatele cloudu. V tomto modelu bude OpenShift:

- Vytvoření virtuálního pevného disku objektu v účtu úložiště Azure
- Připojit virtuální pevný disk k virtuálnímu počítači a formátování svazku
- Připojte svazek k Pod

Pro tento postup musí OpenShift oprávnění k provedení předchozí úlohy v Azure. Jak toho docílit, je potřeba hlavní název služby. Služby hlavní (SP) je účet zabezpečení v Azure Active Directory, který má uděleno oprávnění k prostředkům.
Objekt služby musí mít přístup k účtům úložiště a virtuálních počítačů, které tvoří clusteru. Pokud všechny prostředky clusteru OpenShift se nasadí do jedné skupiny prostředků, můžete SP udělit oprávnění k této skupině zdrojů.

Tato příručka popisuje, jak vytvořit artefakty související s požadavky.

> [!div class="checklist"]
> * Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.
> * Vytvořte objekt služby pro použití poskytovatelem cloudu Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure. 
Přihlaste se k předplatnému Azure s [az přihlášení](/cli/azure/#login) příkazů a postupujte podle na obrazovce pokynů nebo klikněte na **vyzkoušet** používat cloudové prostředí.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Doporučuje se, že skupinu vyhrazené prostředků se používá pro hostování Key Vault - odděleně od skupinu prostředků, prostředků clusteru OpenShift se nasadí do. 

Následující příklad vytvoří skupinu prostředků s názvem *keyvaultrg* v *eastus* umístění.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
Vytvoření KeyVault pro ukládání klíčů SSH pro cluster s [vytvořit az keyvault](/cli/azure/keyvault#create) příkaz. Název trezoru klíč musí být globálně jedinečný.

Následující příklad vytvoří keyvault, s názvem *keyvault* v *keyvaultrg* skupinu prostředků.

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Vytvoření klíče SSH 
Klíč SSH je potřeba zabezpečit přístup k počátku OpenShift clusteru. Vytvoření dvojice klíč SSH pomocí `ssh-keygen` (v systému Linux nebo Mac.).
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Pár klíčů SSH, které vytvoříte nesmí mít přístupové heslo.

Další informace o klíče SSH v systému Windows [vytvoření SSH klíčů v systému Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Uložit privátní klíč SSH v trezoru klíčů
Nasazení OpenShift používá klíč SSH, které jste vytvořili pro zabezpečený přístup k hlavnímu serveru OpenShift. Pokud chcete povolit nasazení tak, aby bezpečně načítat klíč SSH, uložení klíče v Key Vault, pomocí následujícího příkazu:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu 
OpenShift komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například OpenShift. Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure. Pokud chcete zvýšit zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a přihlašovací údaje, které potřebuje OpenShift výstup.

Následující příklad vytvoří službu objektu zabezpečení a přiřadí ji k skupinu prostředků s názvem myResourceGroup oprávnění přispěvatele. Pokud používáte Windows, spouštění ```az group show --name myResourceGroup --query id``` samostatně a použít výstup ke kanálu možnost--obory.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Poznamenejte si vlastnost appId vrácená z příkazu.
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",            
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Nevytvářejte nezabezpečené heslo.  Postupujte podle pokynů v tématu [Pravidla a omezení pro hesla Azure AD](/azure/active-directory/active-directory-passwords-policy).

Další informace o objekty služby najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="next-steps"></a>Další kroky

Tento článek popsané v následujících tématech:
> [!div class="checklist"]
> * Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.
> * Vytvořte objekt služby pro použití poskytovatelem cloudu Azure.

Nyní přejděte nasazení clusteru OpenShift

- [Nasazení OpenShift původu](./openshift-origin.md)
- [Nasazení OpenShift kontejneru platformy](./openshift-container-platform.md)

