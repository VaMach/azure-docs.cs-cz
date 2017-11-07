---
title: "Škálování clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Naučte se rychle škálování clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2017
ms.author: adegeo
ms.openlocfilehash: b8a9204b9eece396fbc30eacc8912ba9e655b963
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="scale-a-service-fabric-cluster"></a>Škálování clusteru Service Fabric

V tomto kurzu je součástí tři řady a ukazuje, jak můžete škálovat vaše existující cluster v a. Po dokončení, budete vědět, jak škálování clusteru a jak vyčistit všechny prostředky, zbývá.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přečtěte si počet uzlů clusteru
> * Přidat uzly clusteru (škálování)
> * Odebrání uzlů clusteru (měřítka v)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) nebo [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure
- Pokud nasadíte clusteru se systémem Windows, nastavení vývojového prostředí systému Windows. Nainstalujte [Visual Studio 2017](http://www.visualstudio.com) a **Azure development**, **ASP.NET a webové vývoj**, a **vývoj pro různé platformy .NET Core**úlohy.  Potom nastavit [vývojové prostředí .NET](service-fabric-get-started.md).
- Pokud nasadíte Linux cluster, nastavit vývojové prostředí Java na [Linux](service-fabric-get-started-linux.md) nebo [systému MacOS](service-fabric-get-started-mac.md).  Nainstalujte [služby Fabric rozhraní příkazového řádku](service-fabric-cli.md). 

## <a name="sign-in-to-azure"></a>Přihlášení k Azure
Přihlaste se ke svému účtu Azure vyberte předplatné, před spuštěním příkazů Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Úspěšné dokončení této části kurzu, budete muset připojit ke clusteru Service Fabric a sady škálování virtuálního počítače (který je hostitelem clusteru). Sady škálování virtuálního počítače je prostředků Azure, který je hostitelem Service Fabric v Azure.

Jakmile se připojíte ke clusteru, můžete ho dotazovat informace. Další informace o jaké uzly clusteru je vědoma, můžete použít clusteru. Připojování ke clusteru následující kód používá stejný certifikát, který byl vytvořen v první části Tato řada. Nezapomeňte nastavit `$endpoint` a `$thumbprint` proměnné, které chcete svoje hodnoty.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Teď, když jste připojení, můžete příkaz získat stav každého uzlu v clusteru. Pro prostředí PowerShell, použijte `Get-ServiceFabricClusterHealth` příkaz a pro **sfctl** použít s příkaz.

## <a name="scale-out"></a>Horizontální navýšení kapacity

Škálovat, přidáte do sady škálování více instancí virtuálního počítače. Tyto instance budou uzly, které používá Service Fabric. Service Fabric vědět, kdy byly sadou škálování má více instancí přidán (škálování) a odpovědí automaticky. Následující kód získá škálování nastavit podle názvu a zvýší **kapacity** měřítka nastavit o 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Tento kód nastaví kapacitu 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Škálování v

Nastavení velikosti v je stejný jako škálování, s výjimkou použití nižší **kapacity** hodnotu. Při změně měřítka v sadě škálování, je třeba odebrat instancí virtuálního počítače ze sady škálování. Za normálních okolností Service Fabric neví, co se stalo a domnívá, že se ztratilo uzlu. Service Fabric hlásí stav není v pořádku pro cluster. Pokud chcete zabránit této špatný stav, musí informovat service fabric očekáváte, že uzel zmizí.

### <a name="remove-the-service-fabric-node"></a>Odebrat uzel service fabric

> [!NOTE]
> Tato část se vztahuje pouze na *bronzová* odolnost vrstvy. Další informace o odolnosti najdete v tématu [plánování kapacity clusteru Service Fabric][durability].

Při změně měřítka ve škálovací sadě virtuálních počítačů, sad (ve většině případů) škálování odebere instanci virtuálního počítače, který byl naposledy vytvořen. Proto musíte k nalezení shody, poslední vytvořit uzel topologie fabric service. Tento poslední uzel můžete najít biggest kontrolou `NodeInstanceId` hodnotu vlastnosti na uzlech fabric service. Příklady kódu níže řazení podle uzlu instance a vrátí podrobnosti o instanci s největší hodnotou id. 

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

```azurecli
`sfctl node list --query "sort_by(items[*], &instanceId)[-1]"`
```

Service fabric cluster je potřeba vědět, že tento uzel se odeberou. Existují tři kroky, které je třeba provést:

1. Zakážete uzlu, takže už je replikace pro data.  
Prostředí PowerShell:`Disable-ServiceFabricNode`  
sfcli:`sfctl node disable`

2. Zastavení uzlu tak, aby řádně ukončení modulu runtime service fabric a aplikace obdrží žádost o ukončení.  
Prostředí PowerShell:`Start-ServiceFabricNodeTransition -Stop`  
sfcli:`sfctl node transition --node-transition-type Stop`

2. Odebrání uzlu z clusteru.  
Prostředí PowerShell:`Remove-ServiceFabricNodeState`  
sfcli:`sfctl node remove-state`

Jakmile tyto tři kroky se používají k uzlu, může být odebrán ze sady škálování. Pokud používáte vrstvy jakékoli odolnost kromě [bronzová][durability], tyto kroky se provádějí v případě měřítka nastavit instanci je odebrán.

Následující blok kódu získá poslední vytvořený uzel, zakáže, zastaví a odstraní uzel z clusteru.

```powershell
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300
    
    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }
    
    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

V **sfctl** kódu níže, tento příkaz slouží k získání **název uzlu** a **id instance uzlu** hodnoty uzlu poslední vytvořit:`sfctl node list --query "sort_by(items[*], &instanceId)[-1].[instanceId,name]"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Použijte následující **sfctl** dotazy a zkontrolujte stav každého kroku
>
> **Zkontrolujte stav deaktivace**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].nodeDeactivationInfo"`
>
> **Zkontrolujte stav stop**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].isStopped"`
>


### <a name="scale-in-the-scale-set"></a>Škálování v sadě škálování

Teď, když uzel fabric service byl odebrán z clusteru, je možné rozšířit škálovací sadu virtuálních počítačů v. V následujícím příkladu je 1 snížit kapacitu sada škálování.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Tento kód nastaví kapacitu 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Přečtěte si počet uzlů clusteru
> * Přidat uzly clusteru (škálování)
> * Odebrání uzlů clusteru (měřítka v)


V dalším kroku přechodu na následující kurzu se dozvíte, jak nasadit aplikace a používat službu API management.
> [!div class="nextstepaction"]
> [Nasadit službu API Management](service-fabric-tutorial-deploy-api-management.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
