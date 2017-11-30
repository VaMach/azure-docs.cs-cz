---
title: "Obnovení se nezdařilo okruh Azure ExpressRoute: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vám umožňuje obnovit okruh ExpressRoute, který je ve stavu selhání."
documentationcenter: na
services: expressroute
author: anzaman
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 0e017200193de3e4a02275cec3b09c32f1fa5c31
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="reset-a-failed-expressroute-circuit"></a>Obnovení se nezdařilo okruh ExpressRoute

Při operaci v okruhu ExpressRoute úspěšně nedokončí, okruh může přejít do stavu "selhání". Tento článek vám umožňuje obnovit selhání okruh Azure ExpressRoute.

## <a name="reset-a-circuit"></a>Resetování okruhu

1. Nainstalujte nejnovější verzi rutin PowerShellu pro Azure Resource Manager. Další informace najdete v článku [Instalace a konfigurace Azure PowerShellu](/powershell/azure/install-azurerm-ps).

2. Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

  ```powershell
  Login-AzureRmAccount
  ```
3. Pokud máte více předplatných Azure, zkontrolujte předplatná pro daný účet.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Určete předplatné, které chcete použít.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Spusťte následující příkazy resetovat okruh, který je v chybovém stavu:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Okruh by teď měly být v pořádku. Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud okruh je stále ve stavu selhání.

## <a name="next-steps"></a>Další kroky

Otevřete lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud pořád dochází k problémům.
