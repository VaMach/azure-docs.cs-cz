---
title: "Rychlý start: Vytvoření serveru Azure Database for MySQL – rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Tento rychlý start popisuje, jak použít rozhraní příkazového řádku Azure k vytvoření serveru Azure Database for MySQL ve skupině prostředků Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.workload: 
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 574299dd64120d75a1a36cb2ded0fdd269292570
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Vytvoření serveru Azure Database for MySQL pomocí Azure CLI
Tento rychlý start popisuje, jak za pět minut vytvořit pomocí Azure CLI server Azure Database for MySQL ve skupině prostředků Azure. Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.

K dokončení tohoto rychlého startu se ujistěte, že máte nainstalovanou nejnovější verzi [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```
Postupujte podle pokynů příkazového řádku pro otevření adresy https://aka.ms/devicelog v prohlížeči a pak zadejte kód vygenerovaný v **příkazovém řádku**.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte [skupinu prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.

Následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v umístění `westus`.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Vytvořte server Azure Database for MySQL pomocí příkazu **az mysql server create**. Server může spravovat více databází. Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.

Následující příklad vytvoří server Azure Database for MySQL, jehož umístěním je `westus` ve skupině prostředků `mycliresource` s názvem `mycliserver`. Server má správce s přihlašovacím jménem `myadmin` a heslem `Password01!`. Server je vytvořen s úrovní výkonu **Basic** a **50** výpočetními jednotkami sdílenými mezi všemi databázemi na serveru. Výpočetní jednotky a úložiště můžete škálovat nahoru nebo dolů podle potřeb aplikace.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver--location westus --user myadmin --password Password01! --performance-tier Basic --compute-units 50
```

![Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./media/quickstart-create-mysql-server-database-using-azure-cli/3_az-mysq-server-create.png)

## <a name="configure-firewall-rule"></a>Konfigurace pravidla brány firewall
Vytvořte pravidlo brány firewall na úrovni serveru pro Azure Database for MySQL pomocí příkazu **az mysql server firewall-rule create**. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je například nástroj příkazového řádku **mysql.exe** nebo MySQL Workbench, aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure MySQL. 

Následující příklad vytvoří pravidlo brány firewall pro předdefinovaný rozsah adres, který je v tomto příkladu tvořen celým možným rozsahem IP adres.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurace nastavení SSL
Ve výchozím nastavení se připojení SSL mezi serverem a klientskými aplikacemi vynucuje.  Zajišťuje se tak zabezpečení dat „v pohybu“ prostřednictvím šifrování datového streamu v internetu.  Abychom tento rychlý start zjednodušili, zakážeme připojení SSL u vašeho serveru.  Pro provozní servery toto chování nedoporučujeme.  Další podrobnosti najdete v tématu [Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k Azure Database for MySQL](./howto-configure-ssl.md).

Následující příklad zakazuje vynucování SSL na serveru MySQL.
 
 ```azurecli
 az mysql server update --resource-group mycliresource --name mycliserver -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Získání informací o připojení

Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.

```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Výsledek je ve formátu JSON. Poznamenejte si **fullyQualifiedDomainName** a **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Připojení k serveru pomocí nástroje příkazového řádku mysql.exe
Pokud se k serveru chcete připojit pomocí nástroje příkazového řádku **mysql.exe**, ujistěte se, že na svém počítači máte instalaci MySQL.  MySQL si můžete stáhnout [tady](https://dev.mysql.com/downloads/).

Otevřete příkazový řádek a zadejte následující příkaz: 

1. Připojení k serveru pomocí nástroje příkazového řádku **mysql**:
```dos
 mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

2. Zobrazení stavu serveru:
```dos
 mysql> status
```
Pokud vše půjde dobře, měl by být výstup nástroje příkazového řádku následující:

```dos
C:\Users\v-chenyh>mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mycliserver.database.windows.net via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Další příkazy najdete v [Referenční příručce k MySQL 5.6 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Připojení k serveru pomocí nástroje grafického uživatelského rozhraní MySQL Workbench
1.    Na klientském počítači spusťte aplikaci MySQL Workbench. MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).

2.    V dialogovém okně pro **nastavení nového připojení** zadejte na kartě **Parametry** následující informace:

| **Parametry** | **Popis** |
|----------------|-----------------|
|    *Název připojení* | zadejte název pro toto připojení (libovolný) |
| *Způsob připojení* | zvolte Standardní (TCP/IP) |
| *Název hostitele* | mycliserver.database.windows.net (NÁZEV SERVERU, který jste si dříve poznamenali) |
| *Port* | 3306 |
| *Uživatelské jméno* | myadmin@mycliserver(PŘIHLAŠOVACÍ JMÉNO SPRÁVCE SERVERU, které jste si dříve poznamenali) |
| *Heslo* | heslo účtu správce můžete uložit do trezoru |

![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

3.    Pokud chcete otestovat, jestli jsou všechny parametry správně nakonfigurované, klikněte na **Test připojení**.

4.    Teď můžete kliknout na právě vytvořené připojení a úspěšně se připojit k serveru.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud tyto prostředky nepotřebujete pro další rychlý start nebo kurz, můžete je následujícím způsobem odstranit: 

```azurecli
az group delete --name mycliresource
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Návrh databáze MySQL pomocí rozhraní příkazového řádku Azure](./tutorial-design-database-using-cli.md)

