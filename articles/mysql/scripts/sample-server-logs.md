---
title: "Azure CLI: Stažení protokolů serveru ve službě Azure Database pro databázi MySQL"
description: "Tento ukázkový skript příkazového řádku Azure CLI ukazuje, jak povolit a stažení protokolů serveru Azure databáze pro server databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/11/2018
ms.openlocfilehash: b0d34009d189ab136dcb6f28fdccc49b6da9e108
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Povolení a stažení protokolů pomalé dotazu serveru Azure databáze MySQL serveru pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku umožňuje a stáhne protokoly pomalé dotazu služby jedné databáze Azure pro server databáze MySQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tato ukázka vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo. Nahraďte < NAZEV_SOUBORU_PROTOKOLU > v příkazech monitorování az vlastní název souboru protokolu serveru.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [server databáze mysql az vytvořit](/cli/azure/mysql/server#az_msql_server_create) | Vytvoří MySQL serveru, který je hostitelem databáze. |
| [seznam konfigurace serverů az mysql](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Seznam hodnot konfigurace pro server. |
| [AZ mysql serveru konfigurační sady](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Aktualizujte konfiguraci serveru. |
| [Seznam protokolů serveru az mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Seznam souborů protokolu pro server. |
| [stažení protokolů serveru az mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Stáhněte soubory protokolu. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další postup
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)
