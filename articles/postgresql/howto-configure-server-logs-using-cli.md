---
title: "Konfigurace a přístup k protokolům serveru pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup konfigurace a přístup v protokolech serveru v Azure databázi PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 26f8e12c493904f722cad5191ee053feff20f7fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure
Můžete si stáhnout PostgreSQL protokoly chyb serveru pomocí rozhraní příkazového řádku (Azure CLI). Přístup k protokoly transakcí však není podporován. 

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Azure databázi PostgreSQL serveru.](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) nástroj příkazového řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurace protokolování pro databázi Azure pro PostgreSQL
Můžete nastavit server pro přístup k dotazu protokoly a protokoly chyb. Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.
1. Povolení protokolování
2. Aktualizace protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz Povolit protokolování dotazu
3. Doba uchování aktualizace

Další informace najdete v tématu [přizpůsobení parametry konfigurace serveru](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Seznam protokolů pro databázi Azure pro PostgreSQL server
Chcete-li zobrazit seznam souborů k dispozici protokolu pro váš server, spusťte [az postgres protokoly serveru seznamu](/cli/azure/postgres/server-logs#list) příkaz.

Můžete seznam souborů protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**a přesměrování na textový soubor s názvem **protokolu\_soubory\_seznam.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Stažení protokolů místně ze serveru
[Az postgres – protokoly serveru stáhnout](/cli/azure/postgres/server-logs#download) příkazu lze stáhnout jednotlivých protokolových souborů pro váš server. 

Tento příklad stáhne konkrétním souboru protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup** na vašem místním prostředí.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Další kroky
- Další informace o protokolech serveru najdete v tématu [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)
