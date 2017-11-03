---
title: "Azure Service Fabric typů uzlů a virtuálního počítače sady škálování | Microsoft Docs"
description: "Zjistěte, jak nastaví uzlu Azure Service Fabric, které se týkají typů škálování virtuálních počítačů, a jak se vzdáleně připojit ke stupnici nastavit instanci nebo uzlu clusteru."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 2bd3053d645d9acd4850fddf7f27237ff954e8c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric typů uzlů a virtuálního počítače sady škálování
Sady škálování virtuálního počítače jsou prostředek výpočtů Azure. Můžete nasazovat a spravovat kolekci jako sada virtuálních počítačů sady škálování. Nastavte samostatnou škálování nastavit pro každý typ uzlu, který definujete v clusteru služby Azure Service Fabric. Můžete nezávisle škálovat každý typ uzlu nahoru nebo dolů, mají různé sady porty otevřít a použít jiný kapacity metriky.

Následující obrázek znázorňuje clusteru, který má dva typy uzel s názvem front-endové a back-end. Každý typ uzlu s pěti uzly.

![Cluster, který má dva typy uzlů][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Mapování instancí sady škálování virtuálního počítače pro uzly
Jak je vidět na předchozím obrázku, instancí sady škálování spuštění na instanci 0 a poté zvýšit 1. Číslování se projeví v názvech uzlu. Například uzel BackEnd_0 je 0 instance škálovací sady back-end. Tato sada konkrétní škálování má pět instancí s názvem BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4.

Při změně měřítka si sadu škálování, je vytvořena nová instance. Název instance nové sady škálování je obvykle že měřítka nastavit název plus číslo další instance. V našem příkladu je BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Měřítko mapy typy uzlů nastavte nástroje pro vyrovnávání zatížení a škálování sady
Pokud nasadíte cluster v portálu Azure nebo použít šablony Azure Resource Manageru ukázkový, jsou uvedeny všechny prostředky ve skupině prostředků. Můžete zobrazit nástroje pro vyrovnávání zatížení pro každý typ uzel nebo sada škálování. Název služby Vyrovnávání zatížení používá následující formát: **LB -&lt;název typu uzlu&gt;**. Příkladem je LB-sfcluster4doc-0, jak je znázorněno na následujícím obrázku:

![Zdroje][Resources]
## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Vzdálené připojení k instanci sady škálování virtuálního počítače nebo uzlu clusteru
Nastavte samostatný škálování nastavit pro každý typ uzlu, který jste definovali v clusteru. Typy uzlů můžete nezávisle škálovat nahoru nebo dolů. Také můžete použít různé identifikátory SKU virtuálních počítačů. Na rozdíl od jedné instance virtuálních počítačů instancí sady škálování nemají své vlastní virtuální IP adresy. To může být náročné, když hledáte adresu IP a port, který můžete použít vzdálené připojení ke konkrétní instanci.

Pokud chcete vyhledat adresu IP a port, který můžete použít vzdálené připojení ke konkrétní instanci, proveďte následující kroky.

**Krok 1**: najít virtuální IP adresy pro typ uzlu získáním příchozích pravidel NAT pro protokol RDP (Remote Desktop).

Nejdřív získat hodnoty příchozích pravidel NAT, které byly definované jako součást definice prostředků pro `Microsoft.Network/loadBalancers`.

Na portálu Azure, na stránce nástroje pro vyrovnávání zatížení vyberte **nastavení** > **pravidla příchozí NAT**. To vám dává IP adresu a port, který můžete použít se vzdáleně připojit k první měřítka nastavit instanci. 

![Nástroj pro vyrovnávání zatížení][LBBlade]

Na následujícím obrázku, IP adresa a port jsou **104.42.106.156** a **3389**.

![Pravidla NAT][NATRules]

**Krok 2**: najít port, který můžete použít se vzdáleně připojit k instanci sady konkrétní škálování nebo uzel.

Škálování nastavit instancí mapy do uzlů. Pomocí informací o sadě škálování určete přesnou portu, který chcete použít.

Porty jsou přiděleny ve vzestupném pořadí, který odpovídá instance sady škálování. Starší například typ uzlu front-endu následující tabulka uvádí porty pro jednotlivé instance pět uzlů. Použít stejná mapování k vaší instanci sady škálování.

| **Instance sady škálování virtuálního počítače** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

**Krok 3**: vzdáleně připojit k instanci sady konkrétní škálování.

Následující obrázek ukazuje, připojte se k instanci sady škálování FrontEnd_1 pomocí připojení ke vzdálené ploše:

![Připojení ke vzdálené ploše][RDP]

## <a name="change-the-rdp-port-range-values"></a>Změna hodnoty rozsahu portu protokolu RDP

### <a name="before-cluster-deployment"></a>Před nasazením clusteru
Při nastavování clusteru pomocí šablony Resource Manageru, zadejte rozsah v `inboundNatPools`.

Přejděte do definice prostředků pro `Microsoft.Network/loadBalancers`. Vyhledejte popis `inboundNatPools`.  Nahraďte `frontendPortRangeStart` a `frontendPortRangeEnd` hodnoty.

![inboundNatPools hodnoty][InboundNatPools]

### <a name="after-cluster-deployment"></a>Po nasazení clusteru
Změna hodnoty rozsahu portu RDP po nasazení clusteru je složitější. Aby se zajistilo, že nemáte recyklovat virtuálních počítačů, můžete nastavit nové hodnoty pomocí prostředí Azure PowerShell. 

> [!NOTE]
> Ujistěte se, že máte v počítači nainstalovaný Azure PowerShell 1.0 nebo novější. Pokud nemáte Azure Powershell 1.0 nebo novější verze, doporučujeme, postupujte podle kroků popsaných v [postup instalace a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)

1. Přihlaste se k účtu Azure. Pokud tyto příkazy prostředí PowerShell nezdaří, zkontrolujte, správně nainstalován prostředí PowerShell.

    ```
    Login-AzureRmAccount
    ```

2. Získání podrobností o nástroj pro vyrovnávání zatížení a zobrazit hodnoty pro popis `inboundNatPools`, spusťte následující kód:

    ```
    Get-AzureRmResource -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
    ```

3. Nastavit `frontendPortRangeEnd` a `frontendPortRangeStart` na hodnoty, které chcete.

    ```
    $PropertiesObject = @{
        #Property = value;
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name> -ApiVersion <use the API version that is returned> -Force
    ```

## <a name="change-the-rdp-user-name-and-password-for-nodes"></a>Změňte uživatelské jméno protokolu RDP a heslo pro uzly

Chcete-li změnit heslo pro všechny uzly určitý typ uzlu, proveďte následující kroky. Tyto změny se projeví na všech uzlech aktuálních a budoucích v sadě škálování.

1. Spusťte PowerShell jako správce. 
2. Přihlaste se a vybrat své předplatné pro relaci, spusťte následující příkazy. Změna `SUBSCRIPTIONID` parametr ID vašeho předplatného. 

    ```powershell
    Login-AzureRmAccount
    Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
    ```

3. Spusťte následující skript. Použití příslušných `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME`, a `PASSWORD` hodnoty. `USERNAME` a `PASSWORD` hodnoty jsou nová pověření pro použití v budoucnosti relace RDP. 

    ```powershell
    $nodeTypeName = 'NODETYPENAME'
    $resourceGroup = 'RESOURCEGROUP'
    $publicConfig = @{'UserName' = 'USERNAME'}
    $privateConfig = @{'Password' = 'PASSWORD'}
    $extName = 'VMAccessAgent'
    $publisher = 'Microsoft.Compute'
    $node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
    $node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

    Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
    ```

## <a name="next-steps"></a>Další kroky
* Najdete v článku [Přehled funkce "Nasazení odkudkoli" a porovnání s Azure spravované clustery](service-fabric-deploy-anywhere.md).
* Další informace o [clusteru zabezpečení](service-fabric-cluster-security.md).
* Další informace o [Service Fabric SDK a Začínáme](service-fabric-get-started.md).

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
