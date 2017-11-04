---
title: "Navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.openlocfilehash: d753772adeb9064f391f1e3846de654bdb60facf
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure 
V tomto kurzu budete používat rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje pro další postup:
> [!div class="checklist"]
> * Vytvoření serveru Azure Database for PostgreSQL
> * Konfigurace brány firewall serveru
> * Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj k vytvoření databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

Můžete použít prostředí cloudové služby Azure v prohlížeči nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) ve vašem počítači ke spuštění příkazů v tomto kurzu.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém tento prostředek existuje nebo ve kterém se fakturuje. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.
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

Následující příklad vytvoří na serveru s názvem `mypgserver-20170401` ve vaší skupině prostředků `myresourcegroup` s přihlašovací jméno správce serveru `mylogin`. Název serveru se mapuje na název DNS, a proto musí být v rámci Azure globálně jedinečný. Nahraďte položku `<server_admin_password>` vlastní hodnotou.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Zde zadané jméno správce serveru a heslo se vyžadují k přihlášení na server a jeho databáze dále v tomto rychlém startu. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) vytvořte pravidlo brány firewall na úrovni serveru Azure PostgreSQL. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je třeba [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/), aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure PostgreSQL. 

Abyste se mohli připojit z vaší sítě, můžete nastavit pravidlo brány firewall, které pokrývá rozsah IP adres. Následující příklad používá [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) k vytvoření pravidla brány firewall `AllowAllIps` , který umožňuje připojení z libovolné IP adresy. Chcete-li otevřít všechny IP adresy, použijte jako počáteční IP adresu 0.0.0.0 a jako koncovou adresu 255.255.255.255.

Pokud chcete omezit přístup k serveru Azure PostgreSQL pouze do sítě, můžete nastavit pravidlo brány firewall se vztahují pouze na rozsah IP adres vaší podnikové síti.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Server Azure PostgreSQL komunikuje přes port 5432. Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený. Pořádejte oddělení IT, aby otevřeli port 5432 pro připojení k vašemu serveru Azure SQL Database.
>

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

## <a name="connect-to-azure-database-for-postgresql-database-using-psql"></a>Připojit k databázi Azure pro databázi PostgreSQL pomocí psql
Pokud má klientský počítač PostgreSQL nainstalován, můžete použít místní instanci [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), nebo v konzole Azure Cloud pro připojení k serveru Azure PostgreSQL. Teď pro připojení k serveru Azure Database for PostgreSQL použijeme nástroj příkazového řádku psql.

1. Spusťte následující příkaz psql pro připojení k databázi Azure pro databázi PostgreSQL:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Třeba tento příkaz provádí pomocí přihlašovacích údajů pro přístup připojení k výchozí databázi s názvem **postgres** na serveru PostgreSQL **mypgserver-20170401.postgres.database.azure.com**. Zadejte heslo `<server_admin_password>`, které jste zvolili při zobrazení výzvy k zadání hesla.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Jakmile se připojíte k serveru, vytvořte prázdnou databázi příkazového řádku:
```sql
CREATE DATABASE mypgsqldb;
```

3.  Na příkazovém řádku spusťte následující příkaz, který přepne připojení na nově vytvořenou databázi **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-the-database"></a>Vytváření tabulek v databázi
Teď, když víte, jak se připojit k databázi Azure pro PostgreSQL, jsme projít jak provést některé základní úlohy.

Jsme nejprve vytvořit tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která sleduje informace o inventáři:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Nyní jsou zobrazeny nově vytvořené tabulky seznam tabulek zadáním:
```sql
\dt
```

## <a name="load-data-into-the-table"></a>Načtení dat do tabulky
Teď, když máme tabulku, jsme do něj vložte některá data. V okně Otevřít příkazového řádku spusťte následující dotaz vložit některých řádků dat:
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nyní jste přidali dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.

## <a name="query-and-update-the-data-in-the-tables"></a>Dotaz a aktualizovat data v tabulkách
Spusťte následující dotaz pro načtení informací z tabulky inventáře: 
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat data v tabulce inventáře:
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načítání dat, můžete zjistit aktualizovanými hodnotami:
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase
Představte si, že jste omylem odstranili tabulku. Toto je něco, které nelze snadno obnovit z. Azure databázi PostgreSQL můžete přejít zpět k žádné bodu v čase (až 7 dní v Basic) a 35 dní ve verzi Standard a obnovit tento bod v čase na nový server. Tento nový server můžete obnovit odstraněná data. 

Ukázkový server bod před přidáním tabulky obnoví následující příkaz:
```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

`az postgres server restore` Příkaz vyžaduje následující parametry:
| Nastavení | Navrhovaná hodnota | Popis  |
| --- | --- | --- |
| --skupiny prostředků |  myResourceGroup |  Skupinu prostředků, ve kterém má zdrojový server existuje.  |
| – Název | Obnovit mypgserver | Název nového serveru, který je vytvořen pomocí příkazu restore. |
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte v daném okamžiku k obnovení. Toto datum a čas musí být v období uchovávání záloh zdrojového serveru. Použití datum a čas ve formátu ISO 8601. Například můžete použít vlastní místním časovém pásmu, jako například `2017-04-13T05:59:00-08:00`, nebo použijte formát času UTC zulština `2017-04-13T13:59:00Z`. |
| --zdrojového serveru | mypgserver 20170401 | Název nebo ID obnovení ze zdrojového serveru. |

Obnovení serveru v daném okamžiku se vytvoří nový server, zkopírovat jako původní server od bodu v čase, který zadáte. Umístění a cenovou úroveň hodnoty pro obnovené serveru jsou stejné jako na zdrojovém serveru.

Příkaz je synchronní a vrátí po obnovení serveru. Po dokončení obnovení vyhledejte nový server, který byl vytvořen. Ověřte, že data byla obnovena podle očekávání.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak používat rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje na:
> [!div class="checklist"]
> * Vytvoření serveru Azure Database for PostgreSQL
> * Konfigurace brány firewall serveru
> * Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj k vytvoření databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

Dále se naučíte, jak pomocí portálu Azure do podobných úloh, přečtěte si v tomto kurzu: [navrhnout první databáze Azure pro PostgreSQL pomocí portálu Azure](tutorial-design-database-using-azure-portal.md)
