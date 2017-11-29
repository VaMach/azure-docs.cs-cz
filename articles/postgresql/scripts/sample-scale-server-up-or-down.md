---
title: "Rozhraní příkazového řádku Azure skriptu škálování Azure databázi PostgreSQL | Microsoft Docs"
description: "Azure CLI ukázka skriptu - škálování Azure databázi PostgreSQL serveru do úroveň různých výkonu po dotazování metriky."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 11/27/2017
ms.openlocfilehash: a5a24e9aeea193df28bd49d5c428a72b5ec75d1b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro server PostgreSQL na úroveň výkonu různých po dotazování metriky. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo. Nahraďte SubscriptionID použít v příkazech monitorování az s vlastní identifikátor předplatného.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#az_postgres_server_create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze. |
| [seznam metriky az monitorování](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Zobrazí seznam hodnota metriky pro prostředky. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
- Přečtěte si další informace o škálování: [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotky úložiště](../concepts-compute-unit-and-storage.md)
