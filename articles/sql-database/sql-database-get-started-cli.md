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
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: cli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f1e07d232328c35a43497c5a0ed6661a4277423d
ms.lasthandoff: 03/17/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Vytvoření izolované databáze SQL Azure pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento průvodce podrobně uvádí, jak pomocí Azure CLI nasadit databázi SQL Azure ve [skupině prostředků Azure](../azure-resource-manager/resource-group-overview.md) na [logický server Azure SQL Database](sql-database-features.md).

Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované. Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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

Vytvořte [pravidlo brány firewall na úrovni serveru služby Azure SQL Database](sql-database-firewall-configure.md) pomocí příkazu [az sql server firewall create](/cli/azure/sql/server/firewall#create). Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je SQL Server Management Studio nebo nástroj SQLCMD, připojení k databázi SQL přes bránu firewall služby SQL Database. Následující příklad vytvoří pravidlo brány firewall pro předdefinovaný rozsah adres, který je v tomto příkladu tvořen celým možným rozsahem IP adres. Nahraďte tyto předdefinované hodnoty hodnotami pro váš rozsah IP adres nebo externích IP adres. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server"></a>Vytvoření databáze na serveru

Vytvořte na serveru databázi s [úrovní výkonu S0](sql-database-service-tiers.md) pomocí příkazu [az sql db create](/cli/azure/sql/db#create). Následující příklad vytvoří prázdnou databázi `mySampleDatabase`. Podle potřeby tuto předdefinovanou hodnotu nahraďte.

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --service-objective S0
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Rychlé starty **Připojení pomocí** v této kolekci a kurzy v kolekci kurzů staví na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujícího příkazu odstraňte všechny prostředky vytvořené tímto rychlým startem.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

- Informace o připojení a dotazování pomocí aplikace SQL Server Management Studio najdete v tématu [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md).
- Informace o připojení pomocí sady Visual Studio najdete v tématu [Připojení a dotazování pomocí sady Visual Studio](sql-database-connect-query.md).
- Technický přehled služby SQL Database najdete v tématu [O službě SQL Database](sql-database-technical-overview.md).

