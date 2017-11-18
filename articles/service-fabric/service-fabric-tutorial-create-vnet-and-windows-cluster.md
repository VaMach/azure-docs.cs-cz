---
title: "Vytvoření clusteru s podporou Windows Service Fabric v Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit cluster Windows Service Fabric do existující virtuální síť Azure pomocí prostředí PowerShell."
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
ms.date: 11/02/2017
ms.author: ryanwi
ms.openlocfilehash: fb32ef2881bdc1e88bb3f54446163c0feac5da9b
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Nasazení clusteru služby Windows Fabric do virtuální sítě Azure
V tomto kurzu je součástí, jednu z řady. Se dozvíte, jak nasadit cluster Service Fabric se systémem Windows do existující virtuální sítě Azure (VNET) a dílčí net pomocí prostředí PowerShell. Jakmile budete hotovi, máte cluster se systémem, kterou můžete nasadit aplikace do cloudu.  Pokud chcete vytvořit cluster Linux pomocí příkazového řádku Azure CLI, najdete v části [vytvoření clusteru s podporou zabezpečení Linux na platformě Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí prostředí PowerShell
> * Vytvoření trezoru klíčů a nahrání certifikátu
> * Vytvoření zabezpečení clusteru Service Fabric v prostředí Azure PowerShell
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrat cluster

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Vytvoření clusteru s podporou zabezpečení v Azure
> * [Škálování clusteru příchozí nebo odchozí](/service-fabric-tutorial-scale-cluster.md)
> * [Nasazení správy rozhraní API pomocí Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [modul Service Fabric SDK a prostředí PowerShell](service-fabric-get-started.md)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Následujících postupů vytvořte cluster s pěti uzly Service Fabric. Chcete-li vypočítat náklady způsobené spuštění clusteru Service Fabric v Azure pomocí [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="introduction"></a>Úvod
V tomto kurzu nasadí cluster pěti uzlů v jednom uzlu typu do virtuální sítě v Azure.

[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Clustery můžete škálovat tisíce počítačů. Počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzel. Každý uzel je přiřazen název uzlu (řetězec). Uzly mají charakteristiky jako vlastnosti umístění.

Typ uzlu definuje velikost, počet a vlastnosti pro sadu virtuálních počítačů v clusteru. Každý typ definované uzlu je nastavený jako [škálovací sadu virtuálních počítačů](/azure/virtual-machine-scale-sets/), Azure výpočetní prostředky, které použijete k nasazení a správě kolekci jako sada virtuálních počítačů. Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky. Typy uzlů se používá k definování rolí pro sadu uzlů clusteru, například "front-end" nebo "back-end".  Cluster může mít více než jeden typ uzlu, ale typ primární uzel musí mít aspoň pět virtuálních počítačů pro produkční clusterů (nebo minimálně tři virtuální počítače pro testovacích clusterů).  [Service Fabric systémových služeb](service-fabric-technical-overview.md#system-services) umísťují na uzly typu primárního uzlu.

## <a name="cluster-capacity-planning"></a>Plánování kapacity clusteru
V tomto kurzu nasadí cluster pěti uzlů v jednom uzlu typu.  Jakékoli nasazení clusteru výroby plánování kapacity je důležitý krok. Tady jsou některé věci vzít v úvahu v rámci tohoto procesu.

- Počet uzlu typy váš cluster potřebuje 
- Vlastnosti každého typu uzlu (například velikost, primární, internetové a počet virtuálních počítačů)
- Spolehlivost a odolnost vlastnosti clusteru

Další informace najdete v tématu [informace o plánování kapacity clusteru](service-fabric-cluster-capacity.md).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Přihlaste se do Azure a vybrat své předplatné
Tato příručka používá prostředí Azure PowerShell. Když spustíte novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před spuštěním příkazů Azure.
 
Spusťte následující skript pro přihlášení k účtu Azure vyberte své předplatné:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořit novou skupinu prostředků pro vaše nasazení a dejte mu název a umístění.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc
```

## <a name="deploy-the-network-topology"></a>Nasazení síťové topologie
Potom si nastavte topologie sítě, do které bude nasazen API Management a cluster Service Fabric. [Network.json] [ network-arm] šablony Resource Manageru nastaven tak, aby vytvořit virtuální síť (VNET) a také zabezpečení skupinu podsítě a sítě (NSG) pro Service Fabric a na podsíť a skupina NSG pro správu rozhraní API . API Management se nasazuje později v tomto kurzu. Další informace o virtuální sítě, podsítě a skupin Nsg [zde](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy podsítí a skupin Nsg, které nasazujete Service Fabric a API Management.  API Management je nasazena v [následující kurzu](service-fabric-tutorial-deploy-api-management.md). Tato příručka hodnoty parametru není nutné změnit. Tyto hodnoty použít šablony služby Správce prostředků infrastruktury.  Pokud hodnoty jsou tady změnit, musíte je změnit v jiných šablonách Resource Manager používá v tomto kurzu a [nasazení API Management kurzu](service-fabric-tutorial-deploy-api-management.md). 

Stáhněte si následující soubor šablony a parametry Resource Manager:
- [Network.JSON][network-arm]
- [Network.Parameters.JSON][network-parameters-arm]

Použijte následující příkaz prostředí PowerShell pro nasazení Resource Manager soubory šablony a parametr pro nastavení sítě:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile C:\winclustertutorial\network.json -TemplateParameterFile C:\winclustertutorial\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Nasazení clusteru Service Fabric
Po dokončení síťové prostředky nasazení, dalším krokem je k nasazení clusteru Service Fabric v podsíti virtuální sítě a NSG určené pro cluster Service Fabric. Nasazení clusteru s podporou na existující virtuální síť a podsíť (dříve nasazené v tomto článku) vyžaduje šablony Resource Manageru.  Pro tento kurz řady šablona je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku.  

Stáhněte si následující soubor šablony a parametry Resource Manager:
- [cluster.JSON][cluster-arm]
- [cluster.Parameters.JSON][cluster-parameters-arm]

Tuto šablonu použijte k vytvoření clusteru s podporou zabezpečení.  Certifikát clusteru je certifikát X.509, který používá k zabezpečení komunikace mezi uzly a ověření clusteru koncových bodů správy klient pro správu.  Certifikát clusteru také poskytuje protokolem SSL pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS. Azure Key Vault se používá ke správě certifikátů pro clusterů Service Fabric v Azure.  Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, který je zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů. 

Můžete použít certifikát od certifikační autority (CA) jako cluster certifikát nebo pro účely testování, vytvořit certifikát podepsaný svým držitelem. Musí být certifikát clusteru:

- obsahovat privátní klíč.
- vytvořit pro výměnu klíčů, což je exportovat do souboru Personal Information Exchange (.pfx).
- máte název subjektu, který odpovídá domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání se vyžaduje k zajištění SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat certifikát SSL od certifikační autority (CA) pro. cloudapp.azure.com domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

Vyplňte tyto prázdné parametry v *cluster.parameters.json* soubor pro vaše nasazení:

|Parametr|Hodnota|
|---|---|
|adminPassword|Heslo #1234|
|adminUserName|vmadmin|
|Název clusteru|mysfcluster|
|location|southcentralus|

Ponechte *certificateThumbprint*, *certificateUrlValue*, a *sourceVaultValue* parametry prázdné vytvořit certifikát podepsaný svým držitelem.  Pokud chcete použít stávající certifikát předtím nahrála do trezoru klíčů, zadejte tyto hodnoty parametrů.

Tento skript používá [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) rutiny a šablony pro nasazení do nového clusteru v Azure. Rutina také vytvoří nového trezoru klíčů v Azure, přidá nový certifikát podepsaný svým držitelem do trezoru klíčů a stáhne soubor certifikátu místně. Existující certifikát nebo klíče trezoru můžete zadat pomocí dalších parametrů [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) rutiny.

```powershell
# Variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster"
$vaultname = "clusterkeyvault111"
$vaultgroupname="clusterkeyvaultgroup111"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile 'C:\winclustertutorial\cluster.json' `
-ParameterFile 'C:\winclustertutorial\cluster.parameters.json' -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname
```

## <a name="connect-to-the-secure-cluster"></a>Připojte se ke zabezpečení clusteru
Připojte ke clusteru pomocí Service Fabric prostředí PowerShell modul nainstalovaný pomocí Service Fabric SDK.  Nejprve nainstalujte certifikát do úložiště osobních (My) aktuálního uživatele ve vašem počítači.  Spusťte následující příkaz prostředí PowerShell:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.

**Service Fabric** modulu PowerShell poskytuje mnoho rutiny pro správu clusterů Service Fabric, aplikace a služby.  Použití [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) pro připojení k zabezpečení clusteru. Podrobnosti koncový bod připojení a kryptografický otisk certifikátu se nacházejí ve výstupu z předchozího kroku.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Zkontrolujte, zda jste připojeni, a je v pořádku clusteru pomocí [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Další články v této série kurzu použijte clusteru, který jste právě vytvořili. Pokud si nejste okamžitě přesunutí k další článek, můžete odstranit cluster a trezoru klíčů účtovány poplatky. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.

Odstranit skupinu prostředků a všechny prostředky clusteru pomocí [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  Také Odstraňte skupinu prostředků obsahující trezoru klíčů.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí prostředí PowerShell
> * Vytvoření trezoru klíčů a nahrání certifikátu
> * Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí prostředí PowerShell
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrat cluster

V dalším kroku přechodu na následující kurzu se dozvíte, jak škálování clusteru.
> [!div class="nextstepaction"]
> [Škálování clusteru](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
