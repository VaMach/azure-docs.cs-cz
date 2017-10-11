---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup vytvoření a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 6f081416dd7d78f0153b3fda21a340a8c1a70c5f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure
Pravidla brány firewall na úrovni serveru umožňují správcům řídit přístup k databázi Azure pro PostgreSQL Server z konkrétní IP adresu nebo rozsah IP adres. Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit pravidla brány firewall ke správě serveru. Přehled Azure databáze pro PostgreSQL brány firewall, najdete v tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkaz nástroj řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurace pravidel brány firewall pro databázi Azure pro PostgreSQL
[Az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule) příkazy se používají ke konfiguraci pravidel brány firewall.

## <a name="list-firewall-rules"></a>Seznam pravidel brány firewall 
Pro zobrazení seznamu existující pravidla brány firewall serveru na serveru, spusťte [seznamu pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#list) příkaz.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
Výstup obsahuje pravidla, pokud existuje, ve výchozím nastavení ve formátu JSON. Můžete použít přepínač `--output table` pro přehlednějším tvaru tabulky jako výstup.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Vytvořte pravidlo brány firewall
Chcete-li vytvořit nové pravidlo brány firewall na serveru, spusťte [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz. 

Tento příklad umožňuje řadu všechny IP adresy pro přístup k serveru **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Povolit singulární IP adresa pro přístup, zadejte stejnou adresu jako počáteční IP a koncové IP adresy, jako v následujícím příkladu.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, text zprávy showserror výstup.

## <a name="update-firewall-rule"></a>Aktualizovat pravidla brány firewall 
Aktualizovat existující pravidlo brány firewall na serveru pomocí [aktualizace pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#update) příkaz. Zadejte název existující pravidla brány firewall jako vstup a spusťte IP adresy a koncové IP atributů k aktualizaci.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, text zprávy showserror výstup.
> [!NOTE]
> Pokud pravidlo brány firewall neexistuje, získá vytvořené pomocí příkazu update.

## <a name="show-firewall-rule-details"></a>Zobrazit podrobnosti o pravidlech brány firewall
Můžete také zobrazit existující brány firewall pravidla Podrobnosti serveru spuštěním [zobrazit pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#show) příkaz.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste zadali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, text zprávy showserror výstup.

## <a name="delete-firewall-rule"></a>Odstranit pravidlo brány firewall
K odvolání přístupu pro rozsah adres IP ze serveru, odstraňte existující pravidlo brány firewall tak, že spustíte [odstranit az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule#delete) příkaz. Zadejte název existující pravidlo brány firewall.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Po úspěšné neexistuje žádný výstup. Při selhání je vrácen text chybové zprávy.

## <a name="next-steps"></a>Další kroky
- Podobně můžete použít webový prohlížeč, aby [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure](howto-manage-firewall-using-portal.md)
- Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)
- Pomoc při připojování k databázi Azure pro PostgreSQL server najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
