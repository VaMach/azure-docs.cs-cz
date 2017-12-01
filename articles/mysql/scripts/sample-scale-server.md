---
title: "Ukázek Azure CLI se škálovat databázi Azure pro server databáze MySQL | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku škáluje Azure databáze pro server databáze MySQL na úroveň výkonu různých po dotazování metriky."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/02/2017
ms.openlocfilehash: 517208b76757655990f82f50677267320eb1393c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku škáluje jedné databáze Azure pro server databáze MySQL na úroveň výkonu různých po dotazování metriky.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo. Nahraďte ID předplatného použít v příkazech monitorování az s vlastní id předplatného.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění skriptu ukázku, můžete odebrat skupinu prostředků a všechny prostředky, které jsou s ním spojená spuštěním následujícího příkazu:[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [server databáze mysql az vytvořit](/cli/azure/mysql/server#az_mysql_server_create) | Vytvoří MySQL serveru, který je hostitelem databáze. |
| [seznam metriky az monitorování](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Zobrazí seznam hodnota metriky pro prostředky. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)
- Další informace o škálování najdete v tématu [úrovně služeb](../concepts-service-tiers.md) a [výpočetní jednotky a jednotek úložiště](../concepts-compute-unit-and-storage.md).
