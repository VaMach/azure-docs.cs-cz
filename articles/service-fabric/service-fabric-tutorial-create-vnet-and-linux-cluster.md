---
title: "Vytvořit cluster Linux Service Fabric v Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit cluster Linux Service Fabric do existující virtuální síť Azure pomocí rozhraní příkazového řádku Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: 33a3474ed91194efbaf2ef96957ad268f43a717e
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Nasazení clusteru Service Fabric Linux do virtuální sítě Azure
V tomto kurzu je součástí, jednu z řady. Se dozvíte, jak nasadit cluster Linux Service Fabric do existující virtuální sítě Azure (VNET) a dílčí net pomocí rozhraní příkazového řádku Azure. Jakmile budete hotovi, máte cluster se systémem, kterou můžete nasadit aplikace do cloudu. Vytvoření clusteru se systémem Windows pomocí prostředí PowerShell naleznete v tématu [vytvoření clusteru s podporou zabezpečení systému Windows na Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí rozhraní příkazového řádku Azure
> * Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí rozhraní příkazového řádku Azure
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojte se ke clusteru pomocí Service Fabric rozhraní příkazového řádku
> * Odebrat cluster

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Vytvoření clusteru s podporou zabezpečení v Azure
> * [Nasazení správy rozhraní API pomocí Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [služby Fabric rozhraní příkazového řádku](service-fabric-cli.md)
- Nainstalujte [rozhraní příkazového řádku Azure 2.0](/cli/azure/install-azure-cli)

Následujících postupů vytvořte cluster s pěti uzly Service Fabric. Chcete-li vypočítat náklady způsobené spuštění clusteru Service Fabric v Azure pomocí [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Přihlaste se do Azure a vybrat své předplatné
Tento průvodce pomocí rozhraní příkazového řádku Azure. Když spustíte novou relaci, přihlaste se k účtu Azure a vybrat své předplatné před spuštěním příkazů Azure.
 
Spusťte následující skript pro přihlášení k účtu Azure vyberte své předplatné:

```azurecli
az login
az account set --subscription <guid>
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořit novou skupinu prostředků pro vaše nasazení a dejte mu název a umístění.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
az group create --name $ResourceGroupName --location $Location
```

## <a name="deploy-the-network-topology"></a>Nasazení síťové topologie
Potom si nastavte topologie sítě, do které bude nasazen API Management a cluster Service Fabric. [Network.json] [ network-arm] šablony Resource Manageru nastaven tak, aby vytvořit virtuální síť (VNET) a také zabezpečení skupinu podsítě a sítě (NSG) pro Service Fabric a na podsíť a skupina NSG pro správu rozhraní API . Další informace o virtuální sítě, podsítě a skupin Nsg [zde](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy podsítí a skupin Nsg, které nasazujete Service Fabric a API Management.  API Management je nasazena v [následující kurzu](service-fabric-tutorial-deploy-api-management.md). Tato příručka hodnoty parametru není nutné změnit. Tyto hodnoty použít šablony služby Správce prostředků infrastruktury.  Pokud hodnoty jsou tady změnit, musíte je změnit v jiných šablonách Resource Manager používá v tomto kurzu a [nasazení API Management kurzu](service-fabric-tutorial-deploy-api-management.md). 

Stáhněte si následující soubor šablony a parametry Resource Manager:
- [Network.JSON][network-arm]
- [Network.Parameters.JSON][network-parameters-arm]

Pomocí následujícího skriptu pro nasazení Resource Manager soubory šablony a parametr pro nastavení sítě:

```azurecli
az group deployment create \
    --name VnetDeployment \
    --resource-group $ResourceGroupName \
    --template-file network.json \
    --parameters @network.parameters.json
```
<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Nasazení clusteru Service Fabric
Po dokončení síťové prostředky nasazení, dalším krokem je k nasazení clusteru Service Fabric v podsíti virtuální sítě a NSG určené pro cluster Service Fabric. Nasazení clusteru s podporou na existující virtuální síť a podsíť (dříve nasazené v tomto článku) vyžaduje šablony Resource Manageru.  Další informace najdete v tématu [vytvořit cluster pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Pro tento kurz řady šablona je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku.  

Stáhněte si následující soubor šablony a parametry Resource Manager:
- [linuxcluster.JSON][cluster-arm]
- [linuxcluster.Parameters.JSON][cluster-parameters-arm]

Tuto šablonu použijte k vytvoření clusteru s podporou zabezpečení.  Certifikát clusteru je certifikát X.509, který používá k zabezpečení komunikace mezi uzly a ověření clusteru koncových bodů správy klient pro správu.  Certifikát clusteru také poskytuje protokolem SSL pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS. Azure Key Vault se používá ke správě certifikátů pro clusterů Service Fabric v Azure.  Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, který je zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů. 

Můžete použít certifikát od certifikační autority (CA) jako cluster certifikát nebo pro účely testování, vytvořit certifikát podepsaný svým držitelem. Musí být certifikát clusteru:

- obsahovat privátní klíč.
- vytvořit pro výměnu klíčů, což je exportovat do souboru Personal Information Exchange (.pfx).
- máte název subjektu, který odpovídá domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání se vyžaduje k zajištění SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat certifikát SSL od certifikační autority (CA) pro. cloudapp.azure.com domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

Vyplňte tyto prázdné parametry v *linuxcluster.parameters.json* soubor pro vaše nasazení:

|Parametr|Hodnota|
|---|---|
|adminPassword|Heslo #1234|
|adminUserName|vmadmin|
|Název clusteru|mysfcluster|

Ponechte **certificateThumbprint**, **certificateUrlValue**, a **sourceVaultValue** parametry prázdné vytvořit certifikát podepsaný svým držitelem.  Pokud chcete použít stávající certifikát předtím nahrála do trezoru klíčů, zadejte tyto hodnoty parametrů.

Tento skript používá [vytvoření clusteru sf az](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) příkaz a šablony pro nasazení do nového clusteru v Azure. Rutina také vytvoří nového trezoru klíčů v Azure, přidá nový certifikát podepsaný svým držitelem do trezoru klíčů a stáhne soubor certifikátu místně. Existující certifikát nebo klíče trezoru můžete zadat pomocí dalších parametrů [vytvoření clusteru sf az](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) příkaz.

```azurecli
Password="q6D7nN%6ck@6"
Subject="mysfcluster.southcentralus.cloudapp.azure.com"
VaultName="linuxclusterkeyvault"
az group create --name $ResourceGroupName --location $Location

az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-output-folder . --certificate-password $Password --certificate-subject-name $Subject \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file linuxcluster.json --parameter-file linuxcluster.parameters.json

```

## <a name="connect-to-the-secure-cluster"></a>Připojte se ke zabezpečení clusteru
Připojte se ke clusteru pomocí rozhraní příkazového řádku služby Fabric `sfctl cluster select` příkaz pomocí vašeho klíče.  Všimněte si, použít pouze **– ne ověřte** možnost pro certifikát podepsaný svým držitelem.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Zkontrolujte, zda jste připojeni, a je v pořádku clusteru pomocí `sfctl cluster health` příkaz.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Další články v této série kurzu použijte clusteru, který jste právě vytvořili. Pokud si nejste okamžitě přesunutí k další článek, můžete odstranit cluster účtovány poplatky. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.

Přihlaste se k Azure a vybrat ID předplatného, pro který chcete odebrat cluster.  Můžete najít svoje ID předplatného přihlášením k [portál Azure](http://portal.azure.com). Odstranit skupinu prostředků a všechny prostředky clusteru pomocí [odstranění skupiny az](/cli/azure/group?view=azure-cli-latest#az_group_delete) příkaz.

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí rozhraní příkazového řádku Azure
> * Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí rozhraní příkazového řádku Azure
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojte se ke clusteru pomocí Service Fabric rozhraní příkazového řádku
> * Odebrat cluster

V dalším kroku přechodu na následující kurzu se dozvíte, jak škálování clusteru.
> [!div class="nextstepaction"]
> [Škálování clusteru](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.parameters.json
