---
title: "Azure CLI skriptu: Změna konfigurace serveru"
description: "Tento ukázkový skript rozhraní příkazového řádku jsou uvedeny všechny možnosti konfigurace serveru k dispozici a aktualizuje hodnotu jednu z možností."
services: postgres
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 334322557c83d5576c9a11def6bb0dc0e3ad1fec
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Konfigurace seznamu a aktualizaci databáze služby Azure pro PostgreSQL server pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku obsahuje seznam všech dostupných parametrů, jakož i jejich povolených hodnot pro databázi Azure pro PostgreSQL server a nastaví *log_retention_days* na hodnotu, která je jiné než výchozí nastavení.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#az_postgres_server_create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze. |
| [seznam konfigurace serverů postgres az](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Zobrazí seznam konfigurace Azure databáze pro PostgreSQL server. |
| [AZ postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Aktualizujte konfiguraci databáze Azure pro PostgreSQL server. |
| [AZ postgres serveru konfigurace zobrazení](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) | Zobrazit konfigurační databáze služby Azure pro PostgreSQL server. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další postup
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
- Další informace o parametry serveru, najdete v části [jak chcete nakonfigurovat parametry serveru, na portálu Azure](../howto-configure-server-parameters-using-portal.md).
