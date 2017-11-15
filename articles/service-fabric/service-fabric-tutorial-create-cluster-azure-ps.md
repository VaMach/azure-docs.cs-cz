---
title: "Vytvořit cluster Service Fabric v Azure | Microsoft Docs"
description: "Informace o vytvoření clusteru s podporou Windows nebo Linux Service Fabric v Azure pomocí prostředí PowerShell"
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
ms.date: 10/03/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 217b9f2f0dfed5b095e1bac1c8146abf4753fadc
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-secure-cluster-in-azure-by-using-powershell"></a>Vytvoření clusteru s podporou zabezpečení v Azure pomocí prostředí PowerShell
Tento článek je první ze série kurzů, které ukazují, jak přesunout aplikace .NET do cloudu pomocí Azure Service Fabric clustery a kontejnerů. V následujících krocích se dozvíte, jak vytvořit cluster Service Fabric (Windows nebo Linux), který běží v Azure. Jakmile budete hotovi, máte zabezpečené clusteru, který běží v cloudu, do které můžete nasazovat aplikace.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Nainstalujte [Service Fabric SDK](service-fabric-get-started.md).
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). (V případě potřeby [nainstalovat Azure PowerShell](/powershell/azure/overview) nebo [aktualizovat na novější verzi](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0#update-azps).)


## <a name="create-a-service-fabric-cluster"></a>Vytvořit cluster Service Fabric

Tento skript vytvoří jeden uzel, náhled Service Fabric clusteru. Certifikát podepsaný svým držitelem zabezpečuje clusteru. Skript vytvoří certifikát společně s clusteru a pak umístí certifikát v trezoru klíčů. Nelze škálování clusterů s jedním uzlem nad rámec jednoho virtuálního počítače a clustery preview nelze upgradovat na novější verze.

Chcete-li vypočítat náklady na způsobené spuštění clusteru Service Fabric v Azure, použijte [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/).
Další informace o tom, jak vytvořit clustery se Service Fabric najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Otevřete konzolu prostředí PowerShell, přihlaste se k Azure a vyberte předplatné, které chcete nasadit v clusteru:

   ```PowerShell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

## <a name="cluster-parameters"></a>Parametry clusteru

   Tento skript používá tyto parametry a koncepty. Přizpůsobte parametry podle svých požadavků.

   | Parametr       | Popis | Navrhovaná hodnota |
   | --------------- | ----------- | --------------- |
   | Umístění | Oblast Azure, kde jste nasadili cluster. | Například *westeurope*, *eastasia*, nebo *eastus* |
   | Name (Název)     | Název clusteru, který chcete vytvořit. Název musí mít 4 až 23 znaků a může mít pouze malá písmena, číslice a pomlčky. | Například *honzuv sfpreviewcluster* |
   | Název skupiny prostředků   | Název skupiny prostředků, ve kterém chcete vytvořit cluster. | Například *myresourcegroup* |
   | VmSku  | Virtuální počítač SKU pro uzly. | Jakýkoli platný virtuální počítač SKU |
   | Operační systém  | Virtuální počítač operační systém určený k použití pro uzly. | Jakýkoli platný virtuální počítač operačního systému |
   | keyVaultName | Název nového trezoru klíčů pro přidružení ke clusteru. | Například *mykeyvault* |
   | ClusterSize | Počet virtuálních počítačů v clusteru (může být *1* nebo *3-99*).| Zadejte jenom jeden virtuální počítač pro cluster s podporou preview |
   | CertificateSubjectName | Název subjektu certifikátu, který se má vytvořit. | Následuje formát:  *<name>* . *<location>* . cloudapp.azure.com |

### <a name="default-parameter-values"></a>Výchozí hodnoty parametrů
**Virtuální počítač**: nepovinné nastavení. Pokud je nezadáte, uživatelské jméno správce výchozí *vmadmin* a prostředí PowerShell vás vyzve k zadání hesla pro virtuální počítač před vytvořením clusteru.

**Porty**: výchozí porty 80 a 8081. Můžete zadat další porty následující pokyny pro [porty v prostředí clusterů Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/create-load-balancer-rule).

**Diagnostika**: ve výchozím nastavení povolené.

**Služba DNS**: ve výchozím nastavení není povoleno.

**Reverse proxy**: ve výchozím nastavení není povoleno.

## <a name="create-the-cluster-with-your-parameters"></a>Vytvoření clusteru s parametry

Až se rozhodnete na parametry, které podle svých požadavků, spusťte následující příkaz pro generování zabezpečení clusteru Service Fabric a svůj certifikát.

Můžete upravit tento skript zahrnout další parametry. Další informace o parametry pro vytvoření clusteru najdete v tématu [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster.md) rutiny.

>[!NOTE]
>Před spuštěním tohoto příkazu musíte vytvořit složku, kam můžete ukládat certifikát clusteru.

```PowerShell

# Set the certificate variables. This creates and encrypts a password that Service Fabric will use.
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force

# You must create the folder where you want to store the certificate on your machine before you start this step.
$certfolder="c:\mycertificates\"

# Set the variables for common values. Change the values to fit your needs.
$clusterloc="WestUS"
$clustername = "mysfcluster"
$groupname="mysfclustergroup"       
$vmsku = "Standard_D2_v2"
$vaultname = "mykeyvault"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. The possible values are 1 and 3-99.
$clustersize=1

# Create the Service Fabric cluster and its self-signed certificate. The OS you specify here lets you use this cluster with any applications that are also using containers.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
```

Proces vytvoření může trvat několik minut. Po dokončení konfigurace výstupy informace o clusteru vytvoří v Azure. Také zkopíruje certifikát clusteru k adresáři - CertificateOutputFolder na cestu, kterou jste zadali pro tento parametr.

Poznamenejte si **ManagementEndpoint** adresu URL pro váš cluster, což může být například následující adresu URL: https://mycluster.westeurope.cloudapp.azure.com:19080.

## <a name="import-the-certificate"></a>Import certifikátu

Když je úspěšně vytvořen cluster, spusťte následující příkaz k zajištění, že můžete použít certifikát podepsaný svým držitelem:

```PowerShell

# To connect to the cluster, install the certificate into the Personal (My) store of the current user on your computer.
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
-FilePath C:\mycertificates\mysfclustergroup20170531104310.pfx `
-Password $certpwd
```

Tento příkaz nainstaluje certifikát na aktuálního uživatele, počítače. Je nutné tento certifikát pro přístup k Service Fabric Explorer a zobrazení stavu služby clusteru.


## <a name="view-your-cluster-optional"></a>Zobrazení vašeho clusteru (volitelné)

Až budete mít cluster a na importovaný certifikát, můžete připojit ke clusteru a sledovat jeho stav. Připojení přes Service Fabric Explorer nebo PowerShell několika způsoby.

### <a name="service-fabric-explorer"></a>Service Fabric Explorer
Můžete zobrazit stav clusteru pomocí Service Fabric Exploreru. Chcete-li tak učinit, procházejte k **ManagementEndpoint** adresa URL pro váš cluster a pak vyberte certifikát uložený na vašem počítači.

>[!NOTE]
>Když otevřete Service Fabric Exploreru, zobrazí chyba certifikátu, jak používáte certifikát podepsaný svým držitelem. V Edge, budete muset kliknout na tlačítko **podrobnosti**a klikněte **přejděte na webovou stránku** odkaz. V prohlížeči Chrome, budete muset kliknout na tlačítko **Upřesnit**a klikněte **pokračovat** odkaz.

### <a name="powershell"></a>PowerShell

Modul Service Fabric prostředí PowerShell poskytuje mnoho rutin pro správu clusterů Service Fabric, aplikace a služby. Použití [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) pro připojení k zabezpečení clusteru. Podrobnosti koncový bod připojení a kryptografický otisk certifikátu najdete ve výstupu z předchozího kroku.

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
-KeepAliveIntervalInSec 10 `
-X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-StoreLocation CurrentUser -StoreName My
```

Můžete také zkontrolovat, že jste připojení a že clusteru je v pořádku pomocí [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.

```PowerShell
Get-ServiceFabricClusterHealth
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak vytvořit cluster Service Fabric zabezpečení v Azure pomocí prostředí PowerShell.

V dalším kroku přechodu na následující kurzu se dozvíte, jak nasadit existující aplikaci:
> [!div class="nextstepaction"]
> [Nasazení aplikace .NET existující pomocí Docker Compose](service-fabric-host-app-in-a-container.md)

 
