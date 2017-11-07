---
title: "Vytvoření Azure Database for PostgreSQL pomocí rozhraní CLI Azure | Dokumentace Microsoftu"
description: "Průvodce rychlým zahájením pro vytváření a Správa databáze Azure pro PostgreSQL serveru pomocí rozhraní příkazového řádku Azure (rozhraní příkazového řádku)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 11/03/2017
ms.openlocfilehash: a47e0c98593f92af6988795779700dc641f3011c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Vytvoření Azure Database for PostgreSQL pomocí rozhraní CLI Azure
Azure Database for PostgreSQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze PostgreSQL v cloudu. Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. V tomto rychlém startu se dozvíte, jak vytvořit server Azure Database for PostgreSQL ve [skupině prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí rozhraní CLI Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto článku vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud používáte rozhraní příkazového řádku místně, budete muset přihlásit k účtu pomocí [az přihlášení](/cli/azure/authenticate-azure-cli?view=interactive-log-in) příkaz.
```azurecli-interactive
az login
```

Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém má být fakturován prostředku. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v umístění `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Vytvořte [server Azure Database for PostgreSQL](overview.md) pomocí příkazu [az postgres server create](/cli/azure/postgres/server#create). Server obsahuje soubor databází spravovaných jako skupina. 

Následující příklad vytvoří ve skupině prostředků `myresourcegroup` server s názvem `mypgserver-20170401` a přihlašovacím jménem správce serveru `mylogin`. Název serveru se mapuje na název DNS, a proto musí být v rámci Azure globálně jedinečný. Nahraďte položku `<server_admin_password>` vlastní hodnotou.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Zde zadané jméno správce serveru a heslo se vyžadují k přihlášení na server a jeho databáze dále v tomto rychlém startu. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) vytvořte pravidlo brány firewall na úrovni serveru Azure PostgreSQL. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je třeba [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/), aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure PostgreSQL. 

Pokud se chcete připojovat ze své sítě, můžete nastavit pravidlo brány firewall, které pokrývá rozsah IP adres. Následující příklad vytváří pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) pravidlo brány firewall `AllowAllIps` pro rozsah IP adres. Chcete-li otevřít všechny IP adresy, použijte jako počáteční IP adresu 0.0.0.0 a jako koncovou adresu 255.255.255.255.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Server Azure PostgreSQL komunikuje přes port 5432. Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený. Pořádejte oddělení IT, aby otevřeli port 5432 pro připojení k vašemu serveru Azure SQL Database.

## <a name="get-the-connection-information"></a>Získání informací o připojení

Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Výsledek je ve formátu JSON. Poznamenejte si položky **administratorLogin** a **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-postgresql-database-using-psql"></a>Připojení k databázi PostgreSQL pomocí nástroje psql

Pokud má klientský počítač nainstalovaný systém PostgreSQL, můžete se připojit k serveru Azure PostgreSQL pomocí místní instance nástroje [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html). Teď se pomocí nástroje příkazového řádku psql připojíme k serveru Azure PostgreSQL.

1. Spusťte následující příkaz psql pro připojení k serveru Azure Database for PostgreSQL:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Třeba tento příkaz provádí pomocí přihlašovacích údajů pro přístup připojení k výchozí databázi s názvem **postgres** na serveru PostgreSQL **mypgserver-20170401.postgres.database.azure.com**. Zadejte heslo `<server_admin_password>`, které jste zvolili při zobrazení výzvy k zadání hesla.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Po připojení k serveru vytvořte na příkazovém řádku prázdnou databázi.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Na příkazovém řádku spusťte následující příkaz, který přepne připojení na nově vytvořenou databázi **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Připojení k databázi PostgreSQL pomocí aplikace pgAdmin

Připojení k serveru Azure PostgreSQL pomocí grafického uživatelského rozhraní aplikace _pgAdmin_
1.  Na klientském počítači spusťte aplikaci _pgAdmin_. _pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.
2.  V nabídce **Rychlé odkazy** zvolte **Přidat nový server**.
3.  V dialogovém okně **Vytvořit – server** na kartě **Obecné** zadejte jedinečný popisný název serveru. Může to být třeba **Azure PostgreSQL Server**.
 ![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  V dialogovém okně **Vytvořit – server** přejděte na kartu **Připojení**:
    - Do pole **Název/adresa hostitele** zadejte plně kvalifikovaný název serveru (třeba **mypgserver-20170401.postgres.database.azure.com**). 
    - Do pole **Port** zadejte port 5432. 
    - Zadejte **přihlášení správce serveru (user@mypgserver)**, které jste získali dříve v tomto rychlém startu, a heslo, které jste zadali při vytváření serveru, do polí **Uživatelské jméno** a **Heslo**.
    - U položky **Režim SSL** vyberte možnost **Vyžadovat**. Ve výchozím nastavení se všechny servery Azure PostgreSQL vytvářejí se zapnutým vynucováním SSL. Pokud chcete vynucování SSL vypnout, přečtěte si podrobnosti v tématu [Vynucování SSL](./concepts-ssl-connection-security.md).

    ![pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Klikněte na **Uložit**.
6.  V levém podokně prohlížeče rozbalte **Skupiny serverů**. Vyberte svůj server **Azure PostgreSQL Server**.
7.  Vyberte **Server**, ke kterému jste se připojili, a potom vyberte jeho **Databáze**. 
8.  Pravým tlačítkem myši klikněte na **Databáze** a vytvořte databázi.
9.  Vyberte název databáze **mypgsqldb** a jako vlastníka zadejte přihlašovací jméno správce serveru **mylogin**.
10. Kliknutím na **Uložit** vytvořte prázdnou databázi.
11. V okně **Prohlížeč** rozbalte skupinu **Server**. Rozbalte server, který jste vytvořili. Uvidíte pod ním databázi **mypgsqldb**.
 ![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků

Všechny prostředky, které jste v rychlém startu vytvořili, můžete vyčistit odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud budete chtít pokračovat v práci s následné – elementy QuickStart, neprovádí vyčištění prostředky vytvořené v tento rychlý start. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky, které jste v tomto rychlém startu vytvořili na webu Azure Portal.

```azurecli-interactive
az group delete --name myresourcegroup
```

Pokud chcete právě odstranit jeden server nově vytvořený, můžete spustit [odstranění serveru postgres az](/cli/azure/postgres/server#delete) příkaz.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
