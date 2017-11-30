---
title: "Přístup v protokolech serveru ve službě Azure Database pro databázi MySQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak získat přístup v protokolech serveru ve službě Azure Database pro databázi MySQL pomocí nástroje příkazového řádku Azure CLI."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 908f28d8bd3d0dcbd03636e69cd47b5c47f3cfde
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure
Můžete si stáhnout Azure databáze MySQL protokolů serveru pomocí rozhraní příkazového řádku Azure, nástroje příkazového řádku Azure.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze MySQL serveru Azure](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) nebo použijte prostředí cloudové služby Azure v prohlížeči.

## <a name="configure-logging-for-azure-database-for-mysql"></a>Konfigurace protokolování pro databázi Azure pro databázi MySQL
Můžete nastavit server pro přístup k protokolu pomalé dotazu MySQL.
1. Zapnutí protokolování nastavením **pomalé\_dotazu\_protokolu** parametr na hodnotu ON.
2. Nastavit další parametry, jako je **dlouho\_dotazu\_čas** a **protokolu\_pomalé\_správce\_příkazy**.

V tématu [postup konfigurace serveru parametry](howto-configure-server-parameters-using-cli.md) se dozvíte, jak nastavit hodnotu z těchto parametrů prostřednictvím rozhraní příkazového řádku Azure.

Například následující příkaz rozhraní příkazového řádku zapne protokol pomalé dotazu, nastaví čas dlouhého dotazu na 10 sekund a vypne protokolování příkaz pomalé správce. Nakonec se zobrazí seznam možností konfigurace pro zkontrolovali.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server myserver4demo --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server myserver4demo --value OFF
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Seznam protokolů pro databázi Azure pro server databáze MySQL
Chcete-li zobrazit seznam souborů k dispozici protokolu pro váš server, spusťte [seznamu protokolů serveru mysql az](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) příkaz.

Můžete seznam souborů protokolu pro server **myserver4demo.mysql.database.azure.com** ve skupině prostředků **myresourcegroup**a přesměrování na textový soubor s názvem **protokolu\_soubory \_seznam.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server myserver4demo > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Stažení protokolů ze serveru
[Az mysql – protokoly serveru stáhnout](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) příkazu lze stáhnout jednotlivých protokolových souborů pro váš server. 

Tento příklad stáhne konkrétním souboru protokolu pro server **myserver4demo.mysql.database.azure.com** ve skupině prostředků **myresourcegroup** na vašem místním prostředí.
```azurecli-interactive
az mysql server-logs download --name 20170414-myserver4demo-mysql.log --resource-group myresourcegroup --server myserver4demo
```

## <a name="next-steps"></a>Další kroky
- Další informace o [v protokolech serveru ve službě Azure Database pro databázi MySQL](concepts-server-logs.md)
