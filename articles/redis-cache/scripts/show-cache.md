---
title: "Ukázka skriptu Azure CLI - získáte podrobnosti Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - získáte podrobnosti Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 21db3c9d02c275ec8827062de5135631ed900da8
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="get-details-of-an-azure-redis-cache"></a>Podrobnosti o Azure Redis Cache

V tomto scénáři zjistěte, jak načíst podrobnosti o instanci služby Azure Redis Cache, včetně jeho stav zřizování.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Redis Cache")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k načtení podrobností o instanci služby Azure Redis Cache. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az redis](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Načtěte podrobnosti o instanci služby Azure Redis Cache. |


## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).