---
title: "Azure Portal: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server služby SQL Database, pravidlo brány firewall na úrovni serveru a databáze na webu Azure Portal. Také se naučíte dotazovat databázi SQL Azure pomocí webu Azure Portal."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 1c087f3ecbdd6956b50a8ef9b7e81340f83ff0cf
ms.lasthandoff: 03/21/2017


---
# <a name="create-and-query-a-single-azure-sql-database-in-the-azure-portal"></a>Vytvoření a dotazování izolované databáze SQL Azure na webu Azure Portal

Databáze SQL Azure je možné vytvářet na webu Azure Portal. Tato metoda poskytuje uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet a konfigurovat databáze SQL Azure a všechny související prostředky Azure.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k [portálu Azure](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL

Databáze SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](sql-database-service-tiers.md). Databáze se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) a na [logickém serveru Azure SQL Database](sql-database-features.md). 

Postupujte podle následujících kroků a vytvořte databázi SQL obsahující ukázková data Adventure Works LT. 

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **SQL Database**.

3. Ve formuláři služby SQL Database vyplňte následující údaje: 
   - Název databáze: Zadejte název databáze.
   - Předplatné: Vyberte předplatné.
   - Skupina prostředků: Vyberte novou nebo existující.
   - Zdroj: Vyberte **Ukázka (AdventureWorksLT)**.
   - Server: Vytvořte nový server (název **Server** musí být globálně jedinečný).
   - Elastický fond: Pro tento rychlý start vyberte **Nyní ne**.
   - Cenová úroveň: Vyberte **20 DTU** a **250** GB úložiště.
   - Kolace: Při importu ukázkové databáze tuto hodnotu nelze změnit. 
   - Připnout na řídicí panel: Zaškrtněte toto políčko.

      ![vytvoření databáze](./media/sql-database-get-started/create-database-s1.png)

4. Po dokončení klikněte na **Vytvořit**. Zřizování trvá několik minut.
5. Po dokončení nasazování databáze SQL vyberte **Databáze SQL** na řídicím panelu nebo výběrem možnosti **Databáze SQL** z nabídky vlevo a na stránce **Databáze SQL** klikněte na novou databázi. Otevře se stránka s přehledem pro vaši databázi, na které se zobrazí plně kvalifikovaný název serveru (například **mynewserver20170313.database.windows.net**) a možnosti pro další konfiguraci.

      ![nová databáze sql](./media/sql-database-get-started/new-database-s1-overview.png) 

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Služba SQL Database vytvoří bránu firewall, která znemožní externím aplikacím a nástrojům připojení k vašemu serveru a databázi. Postupujte podle těchto kroků a vytvořte [pravidlo brány firewall na úrovni serveru služby SQL Database](sql-database-firewall-configure.md) pro vaši IP adresu, které umožní externí připojení přes bránu firewall služby SQL Database. 

1. Na panelu nástrojů pro vaši databázi klikněte na **Nastavit bránu firewall serveru**. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

      ![pravidlo brány firewall serveru](./media/sql-database-get-started/server-firewall-rule.png) 

2. Na panelu nástrojů klikněte na **Přidat IP adresu klienta** a pak klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu.

3. Kliknutím na **OK** a pak na **X** zavřete stránku Nastavení brány firewall.

Nyní se můžete připojit k databázi a jejímu serveru pomocí aplikace SQL Server Management Studio nebo jiného nástroje podle vašeho výběru.

## <a name="query-the-sql-database"></a>Dotazování databáze SQL

Podle těchto kroků můžete dotazovat databázi pomocí editoru dotazů na webu Azure Portal. 

1. Na stránce služby SQL Database pro vaši databázi klikněte na panelu nástrojů na **Nástroje**. Otevře se stránka **Editor dotazů** ve verzi Preview.

     ![nabídka nástroje](./media/sql-database-get-started/tools-menu.png) 

2. Klikněte na **Editor dotazů (Preview)**, zaškrtněte políčko **Podmínky verze Preview** a pak klikněte na **OK**. Otevře se editor dotazů.

3. Klikněte na **Přihlásit**, na vyzvání vyberte **Ověřování SQL Serveru** a pak zadejte přihlašovací jméno a heslo správce serveru.
4. Přihlaste se kliknutím na **OK**.

5. Po ověření zadejte do okna dotazu libovolný dotaz, například následující:

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klikněte na **Spustit** a pak zkontrolujte výsledky dotazu v podokně **Výsledky**.

    ![výsledky editoru dotazů](./media/sql-database-get-started/query-editor-results.png)

7. Kliknutím na **X** zavřete stránku Editor dotazů.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Rychlé starty **Připojení pomocí** v této kolekci a kurzy v kolekci kurzů staví na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.

1. Na webu Azure Portal v nabídce vlevo klikněte na **Skupiny prostředků** a pak na **myResourceGroup**. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte **myResourceGroup** a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

- Informace o připojení a dotazování pomocí aplikace SQL Server Management Studio najdete v tématu [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md).
- Informace o připojení pomocí sady Visual Studio najdete v tématu [Připojení a dotazování pomocí sady Visual Studio](sql-database-connect-query.md).
- Technický přehled služby SQL Database najdete v tématu [O službě SQL Database](sql-database-technical-overview.md).

