---
title: "Azure CLI: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server SQL Database, pravidlo brány firewall na úrovni serveru a databáze pomocí Azure CLI."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/04/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 24a99c20dc015b15de980e8323f2d88a39d318dd
ms.lasthandoff: 04/12/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Vytvoření izolované databáze SQL Azure pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento průvodce podrobně uvádí, jak pomocí Azure CLI nasadit databázi SQL Azure ve [skupině prostředků Azure](../azure-resource-manager/resource-group-overview.md) na [logický server Azure SQL Database](sql-database-features.md).

K dokončení tohoto rychlého startu se ujistěte, že máte nainstalovanou nejnovější verzi [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Vytvoření logického serveru

Vytvořte [logický server Azure SQL Database](sql-database-features.md) pomocí příkazu [az sql server create](/cli/azure/sql/server#create). Logický server obsahuje soubor databází spravovaných jako skupina. Následující příklad vytvoří ve skupině prostředků náhodně pojmenovaný server s přihlašovacím jménem správce `ServerAdmin` a heslem `ChangeYourAdminPassword1`. Podle potřeby tyto předdefinované hodnoty nahraďte.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurace pravidla brány firewall serveru

Vytvořte [pravidlo brány firewall na úrovni serveru služby Azure SQL Database](sql-database-firewall-configure.md) pomocí příkazu [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create). Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je SQL Server Management Studio nebo nástroj SQLCMD, připojení k databázi SQL přes bránu firewall služby SQL Database. V následujícím příkladu je brána firewall otevřená pouze pro ostatní prostředky Azure. Pokud chcete povolit externí připojení, změňte IP adresu na příslušnou adresu pro vaše prostředí. Chcete-li otevřít všechny IP adresy, použijte jako počáteční IP adresu 0.0.0.0 a jako koncovou adresu 255.255.255.255.  

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Vytvoření databáze s ukázkovými daty na serveru

Vytvořte na serveru databázi s [úrovní výkonu S0](sql-database-service-tiers.md) pomocí příkazu [az sql db create](/cli/azure/sql/db#create). Následující příklad vytvoří databázi s názvem `mySampleDatabase` a načte do této databáze ukázková data AdventureWorksLT. Nahraďte podle potřeby tyto předdefinované hodnoty (další rychlé starty v této kolekci jsou postavené na tomto rychlém startu).

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujícího příkazu odstraňte všechny prostředky vytvořené tímto rychlým startem.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

- Informace o připojení a dotazování pomocí aplikace SQL Server Management Studio najdete v tématu [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md).
- Informace o připojení a dotazování pomocí Visual Studio Code najdete v tématu [Připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md).
- Informace o připojení a dotazování pomocí .NET najdete v tématu [Připojení a dotazování pomocí .NET](sql-database-connect-query-dotnet.md).
- Informace o připojení a dotazování pomocí PHP najdete v tématu [Připojení a dotazování pomocí PHP](sql-database-connect-query-php.md).
- Informace o připojení a dotazování pomocí Node.js najdete v tématu [Připojení a dotazování pomocí Node.js](sql-database-connect-query-nodejs.md).
- Informace o připojení a dotazování pomocí Javy najdete v tématu [Připojení a dotazování pomocí Javy](sql-database-connect-query-java.md).
- Informace o připojení a dotazování pomocí Pythonu najdete v tématu [Připojení a dotazování pomocí Pythonu](sql-database-connect-query-python.md).
- Informace o připojení a dotazování pomocí Ruby najdete v tématu [Připojení a dotazování pomocí Ruby](sql-database-connect-query-ruby.md).

