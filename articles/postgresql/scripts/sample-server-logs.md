---
title: "Azure CLI skriptu: stažení serveru v protokolech v Azure databázi PostgreSQL"
description: "Tento ukázkový skript příkazového řádku Azure CLI ukazuje, jak povolit a stažení protokolů serveru Azure databáze pro PostgreSQL server."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Povolení a stažení serveru pomalé dotazu protokolů databáze služby Azure pro PostgreSQL server pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku umožňuje a stáhne protokoly pomalé dotazu služby jedné databáze Azure pro PostgreSQL server.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tato ukázka vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo. Nahraďte < NAZEV_SOUBORU_PROTOKOLU > v příkazech monitorování az vlastní název souboru protokolu serveru.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [AZ postgres server vytvořit](/cli/azure/postgres/server#az_msql_server_create) | Vytvoří PostgreSQL serveru, který je hostitelem databáze. |
| [seznam konfigurace serverů postgres az](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Seznam hodnot konfigurace pro server. |
| [AZ postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Aktualizujte konfiguraci serveru. |
| [Seznam protokolů serveru postgres az](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Seznam souborů protokolu pro server. |
| [stažení protokolů serveru postgres az](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Stáhněte soubory protokolu. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další postup
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)
- [Konfigurace a v protokolech serveru přístup k portálu Azure](../howto-configure-server-logs-in-portal.md)
