---
title: "Azure CLI skriptu - změna konfigurace serveru | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku jsou uvedeny všechny konfigurace k dispozici server a aktualizuje hodnotu innodb_lock_wait_timeout."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/03/2017
ms.openlocfilehash: 9a94f257e5cd3534127e8594ddee3c5f837876df
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Konfigurace seznamu a aktualizaci databáze služby Azure pro MySQL server pomocí rozhraní příkazového řádku Azure
Tento ukázkový skript rozhraní příkazového řádku obsahuje seznam všech dostupných parametrů, jakož i jejich povolených hodnot pro databázi Azure pro server databáze MySQL a nastaví *innodb_lock_wait_timeout* na hodnotu, která je jiné než výchozí nastavení.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript
Tento ukázkový skript změňte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| **Příkaz** | **Poznámky k** |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [server databáze mysql az vytvořit](/cli/azure/mysql/server#az_msql_server_create) | Vytvoří MySQL serveru, který je hostitelem databáze. |
| [seznam konfigurace serverů az mysql](/cli/azure/mysql/server/configuration#az_msql_server_configuration_list) | Zobrazí seznam konfiguraci databáze MySQL serveru služby Azure. |
| [AZ mysql serveru konfigurační sady](/cli/azure/mysql/server/configuration#az_msql_server_configuration_set) | Aktualizujte konfiguraci databáze MySQL serveru služby Azure. |
| [AZ mysql serveru konfigurace zobrazení](/cli/azure/mysql/server/configuration#az_msql_server_configuration_show) | Zobrazit konfigurační databáze služby Azure pro server databáze MySQL. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).
- Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)
- Další informace o parametry serveru, najdete v části [jak pro konfiguraci serveru parametry ve službě Azure Database pro databázi MySQL](../howto-server-parameters.md).
