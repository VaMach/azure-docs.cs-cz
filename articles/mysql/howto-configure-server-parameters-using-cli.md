---
title: "Konfigurovat parametry služby ve službě Azure Database pro databázi MySQL"
description: "Tento článek popisuje, jak nakonfigurovat parametry služby ve službě Azure Database pro databázi MySQL pomocí nástroje příkazového řádku Azure CLI."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 9caf6b164f2433ab3c1b701554f562211cc2de80
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure
Seznam, zobrazit a aktualizovat parametry konfigurace pro databázi Azure pro MySQL server pomocí rozhraní příkazového řádku Azure, nástroj příkazového řádku Azure. Část konfigurace modulu je vystaven na úrovni serveru a nelze jej změnit. 

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databázi Azure pro server databáze MySQL](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) nástroj příkazového řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Seznam parametrů konfigurace serveru pro databázi Azure pro server databáze MySQL
Chcete-li zobrazit seznam všech změn parametrů v serveru a jejich hodnoty, spusťte [seznamu konfigurací serveru mysql az](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) příkaz.

Můžete vytvořit seznam parametrů konfigurace serveru pro server **mydemoserver.mysql.database.azure.com** ve skupině prostředků **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Definice jednotlivých uvedených parametrů, najdete v části MySQL odkaz na [serveru systémové proměnné](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Zobrazit podrobnosti parametr konfigurace serveru
Chcete-li zobrazit podrobnosti o konkrétní konfiguračního parametru pro server, spusťte [az mysql serveru konfigurace zobrazit](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_show) příkaz.

Tento příklad zobrazuje podrobnosti o **pomalé\_dotazu\_protokolu** parametr konfigurace serveru pro server **mydemoserver.mysql.database.azure.com** ve skupině prostředků **myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Změnit hodnotu parametru konfigurace serveru
Můžete také upravit hodnotu parametru určité serveru konfigurace, která aktualizuje základní hodnotu konfigurace pro modul MySQL serveru. Chcete-li aktualizovat konfiguraci, použijte [az mysql serveru konfigurační sady](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) příkaz. 

Aktualizace **pomalé\_dotazu\_protokolu** parametr konfigurace serveru serveru **mydemoserver.mysql.database.azure.com** ve skupině prostředků  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Pokud chcete resetovat hodnota parametru konfigurace, vynechejte nepovinný `--value` platí výchozí hodnota parametru a služby. Pro výše uvedeném příkladu by vypadat podobně jako:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Tento kód resetuje **pomalé\_dotazu\_protokolu** konfigurace na výchozí hodnotu **OFF**. 

## <a name="next-steps"></a>Další postup
- Postup konfigurace [parametry serveru, na portálu Azure](howto-server-parameters.md)