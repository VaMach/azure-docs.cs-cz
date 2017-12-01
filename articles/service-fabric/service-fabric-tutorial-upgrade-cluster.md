---
title: Upgrade modulu runtime Azure Service Fabric | Microsoft Docs
description: "Zjistěte, jak pomocí prostředí PowerShell k upgradu runtime clusteru hostovaných v Azure Service Fabric."
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
ms.date: 11/28/2017
ms.author: adegeo
ms.openlocfilehash: 5057a741a60bf9632f15dbc47236791e06f5bf6b
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="upgrade-the-runtime-of-a-service-fabric-cluster"></a>Upgrade modul runtime cluster Service Fabric

V tomto kurzu je součástí tři řady a ukazuje, jak upgradovat modulu runtime Service Fabric v clusteru Azure Service Fabric. Tato část kurz je napsán pro clustery infrastruktury služby spuštěné v Azure a nevztahuje na samostatné Service Fabric clustery.

> [!WARNING]
> Tato část kurzu vyžaduje rozhraní PowerShell. Pomocí nástrojů příkazového řádku Azure ještě není dostupná podpora pro upgrade clusteru runtime. Alternativně lze cluster upgradovat na portálu. Další informace najdete v tématu [Upgrade clusteru služby Azure Service Fabric](service-fabric-cluster-upgrade.md).

Pokud váš cluster je již spuštěna nejnovější modulu runtime Service Fabric, není potřeba tento krok. Však tento článek slouží k instalaci žádné podporované runtime v clusteru Azure Service Fabric.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přečtěte si verze clusteru
> * Nastavit verzi clusteru

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure pomocí šablony
> * [Škálování clusteru příchozí nebo odchozí](service-fabric-tutorial-scale-cluster.md)
> * Upgrade clusteru modul runtime
> * [Nasazení správy rozhraní API pomocí Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) nebo [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure
- Pokud nasadíte clusteru se systémem Windows, nastavení vývojového prostředí systému Windows. Nainstalujte [Visual Studio 2017](http://www.visualstudio.com) a **Azure development**, **ASP.NET a webové vývoj**, a **vývoj pro různé platformy .NET Core**úlohy.  Potom nastavit [vývojové prostředí .NET](service-fabric-get-started.md).
- Pokud nasadíte Linux cluster, nastavit vývojové prostředí Java na [Linux](service-fabric-get-started-linux.md) nebo [systému MacOS](service-fabric-get-started-mac.md).  Nainstalujte [služby Fabric rozhraní příkazového řádku](service-fabric-cli.md). 

### <a name="sign-in-to-azure"></a>Přihlášení k Azure
Přihlaste se ke svému účtu Azure vyberte předplatné, před spuštěním příkazů Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Získat verzi modulu runtime

Po připojení k Azure, vybrané předplatné obsahující cluster Service Fabric, můžete získat verzi modulu runtime clusteru.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Nebo právě získat seznam všech clusterech ve vašem předplatném s následujícími službami:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Poznámka: **ClusterCodeVersion** hodnotu. Tato hodnota se použije v další části.

## <a name="upgrade-the-runtime"></a>Upgrade modulu runtime

Použijte hodnotu **ClusterCodeVersion** z předchozí části s `Get-ServiceFabricRuntimeUpgradeVersion` rutiny zjistit, jaké verze jsou k dispozici upgrade. Tato rutina může spustit pouze z počítače připojeného k Internetu. Například, pokud jste chtěli najdete v části jaké verze modulu runtime, může k upgradu z verze `5.7.198.9494`, použijte následující příkaz:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Seznam verzí můžete zjistit, Azure Service Fabric clusteru pro upgrade na novější modulu runtime. Například pokud verze `6.0.219.9494` je k dispozici k upgradu, použijte následující příkaz pro upgrade clusteru.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Modul runtime upgrade clusteru může trvat dlouhou dobu pro dokončení. Prostředí PowerShell je blokována při upgradu. Chcete-li zkontrolovat stav upgradu můžete použít jiná relace prostředí PowerShell.

Stav upgradu můžete sledovat pomocí buď prostředí PowerShell nebo `sfctl` rozhraní příkazového řádku.

Nejprve připojte ke clusteru s certifikátem SSL vytvořené v první části tohoto kurzu. Použití `Connect-ServiceFabricCluster` rutiny nebo `sfctl cluster upgrade-status`.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
                             -KeepAliveIntervalInSec 10 `
                             -X509Credential -ServerCertThumbprint $thumbprint `
                             -FindType FindByThumbprint -FindValue $thumbprint `
                             -StoreLocation CurrentUser -StoreName My
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Pak pomocí `Get-ServiceFabricClusterUpgrade` nebo `sfctl cluster upgrade-status` k zobrazení stavu. Zobrazí se něco podobného jako následující výsledek.

```powershell
Get-ServiceFabricClusterUpgrade

TargetCodeVersion                          : 6.0.219.9494
TargetConfigVersion                        : 3
StartTimestampUtc                          : 11/28/2017 3:09:48 AM
UpgradeState                               : RollingForwardPending
UpgradeDuration                            : 00:09:00
CurrentUpgradeDomainDuration               : 00:09:00
NextUpgradeDomain                          : 1
UpgradeDomainsStatus                       : { "0" = "Completed";
                                             "1" = "Pending";
                                             "2" = "Pending";
                                             "3" = "Pending";
                                             "4" = "Pending" }
UpgradeKind                                : Rolling
RollingUpgradeMode                         : Monitored
FailureAction                              : Rollback
ForceRestart                               : False
UpgradeReplicaSetCheckTimeout              : 37201.09:59:01
HealthCheckWaitDuration                    : 00:05:00
HealthCheckStableDuration                  : 00:05:00
HealthCheckRetryTimeout                    : 00:45:00
UpgradeDomainTimeout                       : 02:00:00
UpgradeTimeout                             : 12:00:00
ConsiderWarningAsError                     : False
MaxPercentUnhealthyApplications            : 0
MaxPercentUnhealthyNodes                   : 100
ApplicationTypeHealthPolicyMap             : {}
EnableDeltaHealthEvaluation                : True
MaxPercentDeltaUnhealthyNodes              : 0
MaxPercentUpgradeDomainDeltaUnhealthyNodes : 0
ApplicationHealthPolicyMap                 : {}
```

```azurecli
sfctl cluster upgrade-status

{
  "codeVersion": "6.0.219.9494",
  "configVersion": "3",

... item cut to save space ...

  },
  "upgradeDomains": [
    {
      "name": "0",
      "state": "Completed"
    },
    {
      "name": "1",
      "state": "Pending"
    },
    {
      "name": "2",
      "state": "Pending"
    },
    {
      "name": "3",
      "state": "Pending"
    },
    {
      "name": "4",
      "state": "Pending"
    }
  ],
  "upgradeDurationInMilliseconds": "PT1H2M4.63889S",
  "upgradeState": "RollingForwardPending"
}
```

## <a name="conclusion"></a>Závěr
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Získá verzi modulu runtime clusteru
> * Upgrade clusteru runtime
> * Monitorování upgradu

V dalším kroku přechodu na následující kurzu se dozvíte, jak nasadit API Management se cluster Service Fabric.
> [!div class="nextstepaction"]
> [Nasazení správy rozhraní API pomocí Service Fabric](service-fabric-tutorial-deploy-api-management.md)
