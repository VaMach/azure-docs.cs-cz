---
title: "Nastavení clusteru Azure Service Fabric | Dokumentace Microsoftu"
description: "Rychlé zprovoznění – vytvoření clusteru Service Fabric s Windows nebo Linuxem v Azure."
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
ms.date: 08/24/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ecf9554554c8b7acbd8b8f5aa9122ce1678c6502
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---

# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Vytvoření vašeho prvního clusteru Service Fabric v Azure
[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Tento Rychlý start vám pomůže během několika minut vytvořit cluster s pěti uzly běžící ve Windows nebo Linuxu, a to prostřednictvím [Azure PowerShellu](https://msdn.microsoft.com/library/dn135248) nebo webu [Azure Portal](http://portal.azure.com).  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.


## <a name="use-the-azure-portal"></a>Použití webu Azure Portal

Přihlaste se k webu Azure Portal na adrese [http://portal.azure.com](http://portal.azure.com).

### <a name="create-the-cluster"></a>Vytvoření clusteru

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.
2. V okně **Nový** vyberte **Compute** a potom v okně **Compute** vyberte **Cluster Service Fabric**.
3. Pro Service Fabric vyplňte formulář **Základy**. V poli **Operační systém** vyberte verzi systému Windows nebo Linux, na které chcete spouštět uzly clusteru. Uživatelské jméno a heslo, které tady zadáte, se používá k přihlášení k virtuálnímu počítači. V části **Skupina prostředků** vytvořte novou. Skupina prostředků je logický kontejner, ve kterém se vytváří a hromadně spravují prostředky Azure. Jakmile budete hotovi, klikněte na **OK**.

    ![Výstup po instalaci clusteru][cluster-setup-basics]

4. Vyplňte formulář **Konfigurace clusteru**.  Jako **Počet typů uzlu** zadejte 1.

5. Vyberte **Typ uzlu 1 (Primární)** a vyplňte formulář **Konfigurace typu uzlu**.  Zadejte název typu uzlu a nastavte [Úroveň odolnosti](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) na Bronzová.  Vyberte velikost virtuálního počítače.

    Typy uzlů definují velikost virtuálních počítačů, jejich počet, vlastní koncové body a další nastavení pro virtuální počítače daného typu. Každý definovaný typ uzlu je nastavený jako samostatná škálovací sada virtuálních počítačů, která se používá k nasazení a správě virtuálních počítačů jako sady. U každého typu uzlu je možné nezávisle vertikálně navýšit nebo snížit kapacitu. Mají různé sady otevřených portů a můžou mít různé metriky kapacity.  První (nebo primární) typ uzlu je ten, kde jsou hostované systémové služby Service Fabric. Musí mít pět nebo víc virtuálních počítačů.

    Důležitým krokem každého produkčního nasazení je [plánování kapacity](service-fabric-cluster-capacity.md).  V tomto rychlém zprovoznění ale nespouštíme aplikace, takže jako velikost virtuálního počítače vyberte *DS1_v2 Standard*.  Jako [Úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) vyberte Stříbrná a počáteční kapacitu škálovací sady virtuálních počítačů nastavte na hodnotu 5.  

    Vlastní koncové body otevřou porty v nástroji Azure Load Balancer, takže se můžete připojit k aplikacím běžícím v clusteru.  Zadejte 80, 8172. Otevřou se porty 80 a 8172.

    Nezaškrtávejte políčko **Konfigurovat rozšířená nastavení**, které se používá pro přizpůsobení koncových bodů správy TCP/HTTP, rozsahů portů aplikací, [omezení umístění](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) a [vlastnosti kapacity](service-fabric-cluster-resource-manager-metrics.md).    

    Vyberte **OK**.

6. Ve formuláři **Konfigurace clusteru** nastavte **Diagnostika** na **Zapnuto**.  Pro toto rychlé zprovoznění nemusíte zadávat žádné vlastnosti [nastavení prostředků infrastruktury](service-fabric-cluster-fabric-settings.md).  V poli **Verze prostředků infrastruktury** vyberte režim **automatického** upgradu, takže Microsoft automaticky aktualizuje verzi kódu prostředků infrastruktury v clusteru.  Pokud chcete [zvolit podporovanou verzi](service-fabric-cluster-upgrade.md) pro upgrade, nastavte režim na **Ruční**. 

    ![Konfigurace typu uzlu][node-type-config]

    Vyberte **OK**.

7. Vyplňte formulář **Zabezpečení**.  Pro toto rychlé zprovoznění vyberte **Nezabezpečené**.  Pro úlohy v produkčním prostředí ale důrazně doporučujeme vytvořit zabezpečený cluster vzhledem k tomu, že k nezabezpečenému clusteru se může anonymně připojit kdokoli a provádět operace správy.  

    Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty. Další informace o použití certifikátů ve službě Service Fabric najdete v tématu věnovaném [scénářům zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md).  Pokud chcete povolit ověřování uživatelů pomocí Azure Active Directory nebo nastavit certifikáty pro zabezpečení aplikací, [vytvořte cluster pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md).

    Vyberte **OK**.

8. Zkontrolujte souhrnné informace.  Pokud chcete stáhnout šablonu Resource Manageru vytvořenou na základě nastavení, která jste zadali, vyberte **Stáhnout šablonu a parametry**.  Vyberte **Vytvořit**. Cluster se vytvoří.

    Průběh vytváření můžete sledovat v oznámeních. (Klikněte na ikonu zvonku u stavového řádku v pravém horním rohu obrazovky.) Pokud jste při vytváření clusteru klikli na **Připnout na Úvodní panel**, na tabuli **Start** bude připnuté **Nasazování clusteru Service Fabric**.

### <a name="connect-to-the-cluster-using-powershell"></a>Připojení ke clusteru pomocí prostředí PowerShell
Připojte se pomocí PowerShellu a ověřte, že cluster je spuštěný.  Modul PowerShell ServiceFabric se instaluje spolu se sadou [Service Fabric SDK](service-fabric-get-started.md).  Rutina [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) vytvoří připojení ke clusteru.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Další příklady připojení ke clusteru najdete v článku [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md). Po připojení ke clusteru zobrazte pomocí rutiny [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) seznam uzlů v clusteru a stavové informace pro každý uzel. Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.

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
Cluster Service Fabric se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Proto je ke kompletnímu odstranění clusteru Service Fabric potřeba odstranit taky prostředky, které ho tvoří. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků. Další způsoby odstranění clusteru nebo některých (ale ne všech) prostředků ve skupině prostředků najdete v tématu [Odstranění clusteru](service-fabric-cluster-delete.md)

Odstranění skupiny prostředků na webu Azure Portal:
1. Přejděte ke clusteru Service Fabric, který chcete odstranit.
2. Na stránce základů clusteru klikněte na název **skupiny prostředků**.
3. Na stránce **Základy skupiny prostředků** klikněte na **Odstranit skupinu prostředků**, postupujte podle pokynů na této stránce a dokončete odstranění skupiny prostředků.
    ![Odstranění skupiny prostředků][cluster-delete]


## <a name="use-azure-powershell-to-deploy-a-secure-windows-cluster"></a>Nasazení zabezpečeného clusteru s Windows pomocí Azure PowerShellu
1. Stáhněte si na počítač [modul Azure PowerShell verze 4.0 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

2. Otevřete okno Windows PowerShellu a spusťte následující příkaz. 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    Zobrazený výstup by měl vypadat přibližně takto:

    ![ps-list][ps-list]

3. Přihlaste se k Azure a vyberte předplatné, pro které chcete vytvořit cluster.

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. Nyní vytvořte zabezpečený cluster spuštěním následujícího příkazu. Nezapomeňte upravit parametry. 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also the name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    Dokončení příkazu může trvat 10–30 minut a pak by se měl zobrazit výstup podobný následujícímu. Výstup obsahuje informace o certifikátu, službě KeyVault, do které se nahrál, a místní složce, do které se certifikát zkopíroval. 

    ![ps-out][ps-out]

5. Celý výstup zkopírujte a uložte do textového souboru, protože se k němu budeme odkazovat. Z výstupu si poznamenejte následující informace. 

    - **CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx
    - **CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10
    - **ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080
    - **ClientConnectionEndpointPort** : 19000

### <a name="install-the-certificate-on-your-local-machine"></a>Instalace certifikátu na místním počítači
  
Aby bylo možné připojení ke clusteru, je potřeba nainstalovat certifikát do osobního úložiště (Moje) aktuálního uživatele. 

Spusťte následující příkaz PowerShellu:

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\the name of the cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.

### <a name="connect-to-a-secure-cluster"></a>Připojení k zabezpečenému clusteru 

Spuštěním následujícího příkazu PowerShellu se připojte k zabezpečenému clusteru. Podrobnosti o certifikátu se musí shodovat s certifikátem použitým k nastavení clusteru. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


Následující příklad ukazuje vyplněné parametry: 

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

Remove-AzureRmResourceGroup -Name $RGname -Force

```
## <a name="use-azure-cli-to-deploy-a-secure-linux-cluster"></a>Nasazení zabezpečeného clusteru s Linuxem pomocí Azure CLI

1. Nainstalujte na svém počítači [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).
2. Přihlaste se k Azure a vyberte předplatné, ve kterém chcete cluster vytvořit.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Spuštěním příkazu [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) vytvořte zabezpečený cluster.

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
    # the domain that you use to access the Service Fabric cluster.  The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Připojení ke clusteru
Spuštěním následujícího příkazu rozhraní příkazového řádku se připojte ke clusteru pomocí certifikátu.  Pokud k ověřování používáte klientský certifikát, podrobnosti o certifikátu musí odpovídat certifikátu nasazenému do uzlů clusteru.  Pro certifikát podepsaný svým držitelem použijte možnost `--no-verify`.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Spuštěním následujícího příkazu zkontrolujte, že jste připojeni a cluster je v pořádku.

```azurecli
az sf cluster health
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
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG

