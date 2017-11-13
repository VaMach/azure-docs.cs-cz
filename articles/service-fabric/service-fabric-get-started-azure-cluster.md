---
title: "Nastavení clusteru Azure Service Fabric | Dokumentace Microsoftu"
description: "Toto rychlé zprovoznění vám pomůže vytvořit cluster Service Fabric s Windows nebo Linuxem v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: ryanwi
ms.openlocfilehash: caf76bb739fa92982c511c8e3e6c6aaf2bf9d6c1
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Vytvoření vašeho prvního clusteru Service Fabric v Azure
[Cluster Azure Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Toto rychlé zprovoznění vám pomůže během několika minut vytvořit cluster s pěti uzly běžící ve Windows nebo Linuxu, a to prostřednictvím [Azure PowerShellu](https://msdn.microsoft.com/library/dn135248) nebo webu [Azure Portal](http://portal.azure.com). Pro tuto úlohu můžete použít také Azure CLI.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.


## <a name="use-the-azure-portal"></a>Použití webu Azure Portal

Přihlaste se k webu Azure Portal na adrese [http://portal.azure.com](http://portal.azure.com).

### <a name="create-the-cluster"></a>Vytvoření clusteru

1. V levém horním rohu webu Azure Portal vyberte **Nový**.
2. Vyhledejte **Service Fabric** a v navrácených výsledcích vyberte **Cluster Service Fabric**. Potom vyberte **Vytvořit**.
3. Pro Service Fabric vyplňte formulář **Základy**. V poli **Operační systém** vyberte verzi systému Windows nebo Linux, na které chcete spouštět uzly clusteru. Uživatelské jméno a heslo, které tady zadáte, se používá k přihlášení k virtuálnímu počítači. V části **Skupina prostředků** vytvořte novou. Skupina prostředků je logický kontejner, ve kterém se vytváří a hromadně spravují prostředky Azure. Jakmile budete hotovi, vyberte **OK**.

    ![Snímek obrazovky s výstupem po instalaci clusteru][cluster-setup-basics]

4. Vyplňte formulář **Konfigurace clusteru**. Jako **Počet typů uzlů** zadejte **1**.

5. Vyberte **Typ uzlu 1 (Primární)** a vyplňte formulář **Konfigurace typu uzlu**. Zadejte název typu uzlu a nastavte [Úroveň odolnosti](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) na **Bronzová**. Potom vyberte velikost virtuálního počítače.

    Typy uzlů definují velikost virtuálních počítačů, jejich počet, vlastní koncové body a další nastavení pro virtuální počítače daného typu. Každý definovaný typ uzlu je nastavený jako samostatná škálovací sada virtuálních počítačů, která se používá k nasazení a správě virtuálních počítačů jako sady. U každého typu uzlu je možné nezávisle vertikálně navýšit nebo snížit kapacitu. Mají různé sady otevřených portů a různé metriky kapacity. První (nebo primární) typ uzlu je ten, kde jsou hostované systémové služby Service Fabric. Tento typ uzlu musí mít pět nebo více virtuálních počítačů.

    Důležitým krokem každého produkčního nasazení je [plánování kapacity](service-fabric-cluster-capacity.md). V tomto rychlém zprovoznění ale nespouštíme aplikace, takže jako velikost virtuálního počítače vyberte *DS1_v2 Standard*. Jako [Úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) vyberte **Stříbrná** a jako počáteční kapacitu škálovací sady virtuálních počítačů zadejte hodnotu 5.  

    Vlastní koncové body otevřou porty ve službě Azure Load Balancer, abyste se mohli připojit k aplikacím běžícím v clusteru.  Zadejte **80, 8172**. Otevřou se porty 80 a 8172.

    Nezaškrtávejte políčko **Konfigurovat rozšířená nastavení**, které se používá pro přizpůsobení koncových bodů správy TCP/HTTP, rozsahů portů aplikací, [omezení umístění](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) a [vlastností kapacity](service-fabric-cluster-resource-manager-metrics.md).    
    
    ![Snímek obrazovky s konfigurací typu uzlu][node-type-config]

    Vyberte **OK**.

6. Ve formuláři **Konfigurace clusteru** nastavte **Diagnostika** na **Zapnuto**. Pro toto rychlé zprovoznění nemusíte zadávat žádné vlastnosti [nastavení prostředků infrastruktury](service-fabric-cluster-fabric-settings.md).  V poli **Verze prostředků infrastruktury** vyberte režim **automatického** upgradu, takže Microsoft automaticky aktualizuje verzi kódu prostředků infrastruktury v clusteru.  Pokud chcete [zvolit podporovanou verzi](service-fabric-cluster-upgrade.md) pro upgrade, nastavte režim na **Ruční**.     

    Vyberte **OK**.

7. Vyplňte formulář **Zabezpečení**. Pro toto rychlé zprovoznění vyberte **Nezabezpečené**. Nezapomeňte, že pro produkční úlohy byste obecně měli vytvořit zabezpečený cluster. K nezabezpečenému clusteru se kdokoli může anonymně připojit a provádět operace správy.  

   Service Fabric k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací používá certifikáty. Další informace najdete v tématu [Scénáře zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md). Pokud chcete povolit ověřování uživatelů pomocí Azure Active Directory nebo nastavit certifikáty pro zabezpečení aplikací, přečtěte si téma [Vytvoření clusteru pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md).

    Vyberte **OK**.

8. Zkontrolujte souhrnné informace. Pokud chcete stáhnout šablonu Azure Resource Manageru vytvořenou na základě nastavení, která jste zadali, vyberte **Stáhnout šablonu a parametry**. Vyberte **Vytvořit**. Cluster se vytvoří.

    Průběh vytváření můžete sledovat v oznámeních. (Vyberte ikonu zvonku u stavového řádku v pravém horním rohu obrazovky.) Pokud jste při vytváření clusteru vybrali **Připnout na Úvodní panel**, na tabuli **Start** bude připnuté **Nasazování clusteru Service Fabric**.

### <a name="connect-to-the-cluster-by-using-powershell"></a>Připojení ke clusteru pomocí PowerShellu
Připojte se ke clusteru pomocí PowerShellu a ověřte, že je spuštěný. Modul PowerShellu pro Service Fabric se nainstaluje spolu se sadou [Service Fabric SDK](service-fabric-get-started.md). Rutina [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) vytvoří připojení ke clusteru.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Další příklady připojení ke clusteru najdete v článku [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md). Po připojení ke clusteru zobrazte pomocí rutiny [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) seznam uzlů v clusteru a informace o stavu jednotlivých uzlů. Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a>Odebrání clusteru
Cluster Service Fabric se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Pokud chcete cluster Service Fabric úplně odstranit, musíte odstranit také všechny prostředky, které ho tvoří. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků. Další způsoby odstranění clusteru nebo některých (ale ne všech) prostředků ve skupině prostředků najdete v tématu [Odstranění clusteru](service-fabric-cluster-delete.md).

Odstranění skupiny prostředků na webu Azure Portal:
1. Přejděte ke clusteru Service Fabric, který chcete odstranit.
2. Na stránce základů clusteru vyberte název **skupiny prostředků**.
3. Na stránce **Základy skupiny prostředků** vyberte **Odstranit skupinu prostředků**. Potom postupujte podle pokynů na této stránce a dokončete odstranění skupiny prostředků.
    ![Snímek obrazovky se stránkou Základy skupiny prostředků se zvýrazněnou možností Odstranit skupinu prostředků][cluster-delete]


## <a name="use-azure-powershell"></a>Použití Azure Powershell
Dalším způsobem, jak vytvořit cluster, je použít PowerShell. Zde je uveden postup:

1. Stáhněte si na počítač [modul Azure PowerShell verze 4.0 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

2. Spusťte rutinu [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster) a vytvořte cluster Service Fabric s pěti uzly zabezpečený pomocí certifikátu X.509. Příkaz vytvoří certifikát podepsaný svým držitelem a nahraje ho do nového trezoru klíčů. Certifikát se také zkopíruje do místního adresáře. Nastavením parametru *-OS* zvolte verzi Windows nebo Linuxu, která se používá na uzlech clusteru. Podle potřeby upravte parametry. 

    ```powershell
    #Provide the subscription Id
    $subscriptionId = 'yourSubscriptionId'

    # Certificate variables.
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $certfolder="c:\mycertificates\"

    # Variables for VM admin.
    $adminuser="vmadmin"
    $adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

    # Variables for common values
    $clusterloc="SouthCentralUS"
    $clustername = "mysfcluster"
    $groupname="mysfclustergroup"       
    $vmsku = "Standard_D2_v2"
    $vaultname = "mykeyvault"
    $subname="$clustername.$clusterloc.cloudapp.azure.com"

    # Set the number of cluster nodes. Possible values: 1, 3-99
    $clustersize=5 

    # Set the context to the subscription ID where the cluster will be created
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Create the Service Fabric cluster.
    New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
    -ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
    -CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
    -OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
    ```

    Dokončení příkazu může trvat 10–30 minut. Výstup obsahuje informace o certifikátu, trezoru klíčů, do kterého se nahrál, a místní složce, do které se certifikát zkopíroval.     

3. Celý výstup zkopírujte a uložte do textového souboru (vrátíme se k němu později). Z výstupu si poznamenejte následující informace: 

    - CertificateSavedLocalPath
    - CertificateThumbprint
    - ManagementEndpoint
    - ClientConnectionEndpointPort

### <a name="install-the-certificate-on-your-local-machine"></a>Instalace certifikátu na místním počítači
  
Abyste se mohli připojit ke clusteru, nainstalujte certifikát do osobního úložiště (Moje) aktuálního uživatele. 

Spusťte následující příkazy:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\<certificatename>.pfx `
        -Password $certpwd
```

Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.

### <a name="connect-to-a-secure-cluster"></a>Připojení k zabezpečenému clusteru 

Spusťte rutinu [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) a připojte se k zabezpečenému clusteru. Podrobnosti o certifikátu se musí shodovat s certifikátem použitým k nastavení clusteru. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <ManagementEndpoint>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <CertificateThumbprint> `
          -FindType FindByThumbprint -FindValue <CertificateThumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

Následující příklad ukazuje ukázkové parametry: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Spuštěním následujícího příkazu zkontrolujte, že jste připojeni a cluster je v pořádku.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="remove-the-cluster"></a>Odebrání clusteru
Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků. 

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```
## <a name="use-azure-cli"></a>Použití Azure CLI
Dalším způsobem, jak vytvořit cluster, je použít rozhraní příkazového řádku. Zde je uveden postup:

1. Nainstalujte na svém počítači [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).
2. Přihlaste se k Azure a vyberte předplatné, ve kterém chcete cluster vytvořit.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Spusťte příkaz [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) a vytvořte cluster Service Fabric s pěti uzly zabezpečený pomocí certifikátu X.509. Příkaz vytvoří certifikát podepsaný svým držitelem a nahraje ho do nového trezoru klíčů. Certifikát se také zkopíruje do místního adresáře. Nastavením parametru *-os* zvolte verzi Windows nebo Linuxu, která se používá na uzlech clusteru. Podle potřeby upravte parametry.

    ```azurecli
    #!/bin/bash

    # Variables
    ResourceGroupName="aztestclustergroup" 
    ClusterName="aztestcluster" 
    Location="southcentralus" 
    Password="q6D7nN%6ck@6" 
    Subject="aztestcluster.southcentralus.cloudapp.azure.com" 
    VaultName="aztestkeyvault" 
    VaultGroupName="testvaultgroup"
    VmPassword="Mypa$$word!321"
    VmUserName="sfadminuser"

    # Create resource groups
    az group create --name $ResourceGroupName --location $Location 
    az group create --name $VaultGroupName --location $Location

    # Create secure five node Linux cluster. Creates a key vault in a resource group
    # and creates a certficate in the key vault. The certificate's subject name must match 
    # the domain that you use to access the Service Fabric cluster. The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Připojení ke clusteru
Spuštěním následujícího příkazu rozhraní příkazového řádku se připojte ke clusteru s použitím certifikátu.  Pokud k ověřování používáte klientský certifikát, ujistěte se, že podrobnosti o certifikátu odpovídají certifikátu nasazenému do uzlů clusteru. Pro certifikát podepsaný svým držitelem použijte možnost `--no-verify`.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Spuštěním následujícího příkazu zkontrolujte, že jste připojeni a cluster je v pořádku.

```azurecli
az sf cluster health
```

### <a name="connect-to-the-nodes-directly"></a>Přímé připojení k uzlům 

Pro připojení k uzlům v clusteru s Linuxem můžete použít příkaz SSH (Secure Shell). Zadejte číslo portu od 3389 výše. Například pro cluster s pěti uzly vytvořený dříve by příkazy vypadaly následovně:
```bash
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3389
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3390
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3391
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3392
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3393
```

### <a name="remove-the-cluster"></a>Odebrání clusteru
Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků. 

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Další kroky
Teď, když jste nastavili vývojový cluster, zkuste provést následující kroky:
* [Vizualizace clusteru pomocí nástroje Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Odebrání clusteru](service-fabric-cluster-delete.md) 
* [Nasazení aplikací s použitím prostředí PowerShell](service-fabric-deploy-remove-applications.md)
* [Nasazení aplikací s použitím rozhraní příkazového řádku](service-fabric-application-lifecycle-sfctl.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
