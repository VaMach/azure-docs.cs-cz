---
title: "Ukázka skriptu rozhraní příkazového řádku služby prostředků infrastruktury – seznam aplikací v clusteru s podporou"
description: "Služba Fabric ukázka skriptu rozhraní příkazového řádku – seznam aplikací, které jsou zřízené v clusteru Service Fabric."
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: 
ms.openlocfilehash: f764c36bcc4905224242cbdf1602d7482e52efac
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>Seznam aplikace běžící v clusteru Service Fabric

Tento ukázkový skript připojí ke clusteru Service Fabric a zobrazí seznam všech zřízené aplikací.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[main](../../../cli_scripts/service-fabric/list-application/list-application.sh "List provisioned applications from a cluster")]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
