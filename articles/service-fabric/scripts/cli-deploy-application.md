---
title: "Ukázka nasazení skriptu rozhraní příkazového řádku Azure Service Fabric"
description: "Nasazení aplikace do clusteru služby Azure Service Fabric pomocí rozhraní příkazového řádku Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 2bbf689820a92cfa01b26fdbacb4526ade8956ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Nasazení aplikace do clusteru Service Fabric

Tento ukázkový skript zkopíruje balíček aplikace do úložiště bitové kopie clusteru, zaregistruje typ aplikace v clusteru a vytvoří instanci aplikace z typu aplikace. V tuto chvíli se také vytvořit žádné výchozí služby.

V případě potřeby nainstalujte [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Až budete hotoví, [odebrat](cli-remove-application.md) skriptu lze aplikaci odebrat. Odebrat skript odstraní instanci aplikace, zrušení registrace typu aplikace a balíček aplikace z úložiště bitových kopií.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
