---
title: "Navrhnout první databáze Azure pro databázi MySQL. rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat Azure databáze MySQL server a databáze pomocí příkazového řádku Azure CLI 2.0."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: f17f2cab39b42341886ed86e1c08569ca8f5eff0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Navrhnout první databáze Azure pro databázi MySQL

Služba relační databáze v Microsoft cloudu podle databázový stroj MySQL Community Edition je Azure databáze pro databázi MySQL. V tomto kurzu budete používat rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje pro další postup:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru
> * Použití [nástroj příkazového řádku mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

Můžete použít prostředí cloudové služby Azure v prohlížeči nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) ve vašem počítači ke spuštění bloky kódu v tomto kurzu.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém tento prostředek existuje nebo ve kterém se fakturuje. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#az_account_set) určité ID předplatného.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#az_group_create) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.

Následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v umístění `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Vytvoření databáze Azure pro server databáze MySQL se serverem mysql az vytvořit příkaz. Server může spravovat více databází. Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.

Následující příklad vytvoří server Azure Database for MySQL, jehož umístěním je `westus` ve skupině prostředků `mycliresource` s názvem `mycliserver`. Server má správce s přihlašovacím jménem `myadmin` a heslem `Password01!`. Server je vytvořen s úrovní výkonu **Basic** a **50** výpočetními jednotkami sdílenými mezi všemi databázemi na serveru. Výpočetní jednotky a úložiště můžete škálovat nahoru nebo dolů podle potřeb aplikace.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurace pravidla brány firewall
Vytvoření databáze Azure pro pravidlo brány firewall na úrovni serveru MySQL s az mysql serveru pravidlo brány firewall-vytvoření příkazu. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například **mysql** nástroj příkazového řádku nebo MySQL Workbench, aby se připojení k serveru přes bránu firewall služby Azure MySQL. 

Následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres. Tento příklad ukazuje celý možné rozsah IP adres.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Získání informací o připojení

Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.
```azurecli-interactive
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

## <a name="connect-to-the-server-using-mysql"></a>Připojit k serveru pomocí mysql
Použití [nástroj příkazového řádku mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k navázání připojení k vaší databázi Azure pro server databáze MySQL. V tomto příkladu je příkaz:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Po připojení k serveru vytvořte prázdnou databázi.
```sql
mysql> CREATE DATABASE mysampledb;
```

Do příkazového řádku spusťte následující příkaz k přepínači připojení k této nově vytvořené databáze:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Vytváření tabulek v databázi
Teď, když víte, jak se připojit k databázi Azure pro databázi MySQL, proveďte některé základní úlohy:

Nejprve vytvořte tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která ukládá informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Načtení dat do tabulky
Teď, když máte tabulku, vložte některá data do ní. V okně Otevřít příkazového řádku spusťte následující dotaz vložit některé řádky dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nyní máte dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.

## <a name="query-and-update-the-data-in-the-tables"></a>Dotaz a aktualizovat data v tabulkách
Spustíte následující dotaz pro načtení informací z tabulky databáze.
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat data v tabulkách.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Načtení dat získá příslušným způsobem aktualizuje řádek.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase
Představte si, že jste omylem odstranili této tabulky. Toto je něco, které nelze snadno obnovit z. Databáze pro databázi MySQL Azure umožňuje vrátit do libovolného bodu v čase v poslední až 35 dnů a obnovit tento bod v čase na nový server. Tento nový server můžete obnovit odstraněná data. Ukázka serveru bod před přidáním tabulky obnovit následující kroky.

Pro obnovení potřebujete následující informace:

- Bod obnovení: Vyberte bodu v čase, k níž dojde před server byl změněn. Musí být větší než nebo rovna hodnotě pro nejstarší zálohy zdrojové databáze.
- Cílový server: Zadejte nový název serveru, kterou chcete obnovit
- Zdrojový server: Zadejte název serveru, kterou chcete obnovit z
- Umístění: Nelze vyberte oblast, ve výchozím nastavení je stejné jako na zdrojovém serveru

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

K obnovení serveru a [obnovení v daném okamžiku](./howto-restore-server-portal.md) předtím, než tabulka byla odstraněna. Obnovení serveru do jiného bodu v čase vytvoří duplicitní nový server jako původní server od bodu v čase, které zadáte, za předpokladu, že je v rámci dobu uchování vašeho [vrstvy služby](./concepts-service-tiers.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli na:
> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru
> * Použití [nástroj příkazového řádku mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Azure databáze pro databázi MySQL – ukázky rozhraní příkazového řádku Azure](./sample-scripts-azure-cli.md)
