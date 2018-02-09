---
title: "Návrh první databáze Azure Database for MySQL – Azure CLI | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat databázi a server Azure Database for MySQL pomocí Azure CLI 2.0 z příkazového řádku."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 5f323086ce66a504188c1834d20873a52a990311
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Návrh první databáze Azure Database for MySQL

Azure Database for MySQL je relační databázová služba v cloudu Microsoftu založená na databázovém stroji MySQL Community Edition. V tomto kurzu použijete Azure CLI (rozhraní příkazového řádku) a další nástroje k získání informací o těchto tématech:

> [!div class="checklist"]
> * Vytvoření Azure Database for MySQL
> * Konfigurace brány firewall serveru
> * Použití [nástroje pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze
> * Načtení ukázkových dat
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

Ke spuštění bloků kódu v tomto kurzu můžete použít Azure Cloud Shell v prohlížeči nebo si [nainstalujte Azure CLI 2.0]( /cli/azure/install-azure-cli) ve vašem počítači.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém tento prostředek existuje nebo ve kterém se fakturuje. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#az_account_set) určité ID předplatného.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte [skupinu prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.

Následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v umístění `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Vytvořte server Azure Database for MySQL pomocí příkazu az mysql server create. Server může spravovat více databází. Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.

Následující příklad vytvoří server Azure Database for MySQL, jehož umístěním je `westus` ve skupině prostředků `mycliresource` s názvem `mycliserver`. Server má správce s přihlašovacím jménem `myadmin` a heslem `Password01!`. Server je vytvořen s úrovní výkonu **Basic** a **50** výpočetními jednotkami sdílenými mezi všemi databázemi na serveru. Výpočetní jednotky a úložiště můžete škálovat nahoru nebo dolů podle potřeb aplikace.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurace pravidla brány firewall
Pomocí příkazu az mysql server firewall-rule create vytvořte pravidlo brány firewall na úrovni serveru pro Azure Database for MySQL. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je například nástroj pro příkazový řádek **mysql** nebo MySQL Workbench, aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure MySQL. 

Následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres. Tento příklad ukazuje celý možný rozsah IP adres.

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

## <a name="connect-to-the-server-using-mysql"></a>Připojení k serveru pomocí mysql
Pro navázání připojení k serveru Azure Database for MySQL použijte [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html). V tomto příkladu se použije příkaz:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Po připojení k serveru vytvořte prázdnou databázi.
```sql
mysql> CREATE DATABASE mysampledb;
```

Na příkazovém řádku spusťte následující příkaz pro přepnutí připojení na tuto nově vytvořenou databázi:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Vytvoření tabulek v databázi
Teď, když už víte, jak se připojit k databázi Azure Database for MySQL, dokončíte některé základní úlohy:

Nejdřív vytvoříte tabulku a načtete do ní data. Vytvořme tabulku, ve které jsou uložené informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Načtení dat do tabulek
Teď už máte tabulku a můžete do ní vložit data. V otevřeném okně příkazového řádku spusťte následující dotaz pro vložení několika řádků dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Tabulka, kterou jste vytvořili dříve, teď obsahuje dva řádky ukázkových dat.

## <a name="query-and-update-the-data-in-the-tables"></a>Dotazování na data a aktualizace dat v tabulkách
Spuštěním následujícího příkazu načtete informace z databázové tabulky.
```sql
SELECT * FROM inventory;
```

Data v tabulce můžete také aktualizovat.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načtení dat se řádek příslušným způsobem aktualizuje.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase
Představte si, že jste tuto tabulku omylem odstranili. Z této situace se nejde snadno zotavit. Azure Database for MySQL umožňuje vrátit se do libovolného bodu v čase během posledních až 35 dnů a obnovit tento bod v čase na nový server. Tento nový server můžete použít k obnovení odstraněných dat. Následující kroky obnoví ukázkový server do bodu před přidáním tabulky.

K obnovení budete potřebovat následující informace:

- Bod obnovení: Vyberte bod v čase, který nastal dřív, než došlo ke změně serveru. Musí být větší nebo rovný hodnotě Nejstarší záloha zdrojové databáze.
- Cílový server: Zadejte nový název serveru, na který chcete provést obnovení.
- Zdrojový server: Zadejte název serveru, ze kterého se má obnovení provést.
- Umístění: Nejde vyberte oblast, ve výchozím nastavení je stejná jako u zdrojového serveru.

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Obnovte server a proveďte [obnovení k určitému bodu v čase](./howto-restore-server-portal.md) před odstraněním tabulky. Obnovení serveru k jinému bodu v čase vytvoří duplicitní nový server k původnímu serveru v bodu v čase, který zadáte, a to za předpokladu, že spadá do doby uchování pro vaši [úroveň služeb](./concepts-service-tiers.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:
> [!div class="checklist"]
> * Vytvoření Azure Database for MySQL
> * Konfigurace brány firewall serveru
> * Použití [nástroje pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze
> * Načtení ukázkových dat
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Azure Database for MySQL – ukázky v Azure CLI](./sample-scripts-azure-cli.md)
