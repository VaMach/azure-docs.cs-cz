---
title: "Připojit k databázi Azure pro databázi MySQL z databáze MySQL Workbench | Microsoft Docs"
description: "Tento rychlý Start obsahuje kroky k MySQL Workbench připojení a dotazování dat z Azure databáze pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 024db86b8760c8edb8347679eec6c68ceab3cd35
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-to-connect-and-query-data"></a>Azure databáze pro databázi MySQL: použití MySQL Workbench, aby se připojení a dotazování dat
Tento rychlý start předvádí, jak se připojit k databázi Azure pro databázi MySQL pomocí aplikace MySQL Workbench. 

## <a name="prerequisites"></a>Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>Nainstalujte MySQL Workbench
Stáhněte a nainstalujte MySQL Workbench na vašem počítače od [webu MySQL](https://dev.mysql.com/downloads/workbench/).

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

2. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky**a poté vyhledejte serveru, který jste vytvořili (například **myserver4demo**).

3. Klikněte na název serveru.

4. Vyberte server, na **vlastnosti** stránky a poté si poznamenejte **název serveru** a **přihlašovací jméno pro Server správce**.

 ![Databáze Azure pro název serveru MySQL](./media/connect-workbench/1-server-properties-name-login.png)
 
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte na **přehled** stránky zobrazíte přihlašovací jméno správce serveru a v případě potřeby obnovení hesla.

## <a name="connect-to-the-server-by-using-mysql-workbench"></a>Připojení k serveru pomocí MySQL Workbench 
Pro připojení k serveru databáze MySQL Azure pomocí grafického uživatelského rozhraní nástroje MySQL Workbench:

1.  Spusťte aplikaci MySQL Workbench ve vašem počítači. 

2.  V **nastavit připojení k nové** dialogové okno pole, zadejte následující informace **parametry** karty:

    ![nastavení nového připojení](./media/connect-workbench/2-setup-new-connection.png)

    | **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
    |---|---|---|
    |   Název připojení | Ukázkové připojení | Zadejte popisek pro toto připojení. |
    | Způsob připojení | Standard (TCP/IP) | Standard (TCP/IP) je dostačující. |
    | Název hostitele | *název serveru* | Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for MySQL. Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použijte plně kvalifikovaný název domény (\*.mysql.database.azure.com), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.  |
    | Port | 3306 | Při připojování ke službě Azure Database for MySQL vždy používejte port 3306. |
    | Uživatelské jméno |  *přihlašovací jméno správce serveru* | Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for MySQL. Uživatelské jméno v našem příkladu je myadmin@myserver4demo. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. Formát je *username@servername*.
    | Heslo | vaše heslo | Klikněte na tlačítko **úložiště v trezoru...**  tlačítko Uložit heslo. |

3.   Pokud chcete otestovat, jestli jsou všechny parametry správně nakonfigurované, klikněte na **Test připojení**. 

4.   Klikněte na tlačítko **OK** pro uložení připojení. 

5.   V seznamu z **MySQL připojení**, klikněte na dlaždici odpovídající vašeho serveru a potom počkejte připojení lze navázat.

        Otevře novou kartu SQL s prázdné editoru můžete zadat své dotazy.
    
        > [!NOTE]
        > Ve výchozím nastavení je zabezpečení připojení protokol SSL vyžaduje a vynucovat u vaší databázi Azure pro server databáze MySQL. Žádná další konfigurace s certifikáty protokolu SSL je obvykle potřeba MySQL Workbench, aby se připojení k serveru. Další informace o SSL najdete v tématu [připojení SSL konfigurace v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL](./howto-configure-ssl.md).  Pokud je nutné zakázat protokol SSL, najdete na portálu Azure a klikněte na stránce zabezpečení připojení zakázat přepínací tlačítko připojení SSL vynutit.

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a>Umožňuje vytvořit tabulku, vkládání dat, čtení dat, aktualizace dat, odstranit data
1. Zkopírujte a vložte ukázkový kód SQL do prázdné karty SQL pro ilustraci ukázková data.

    Tento kód vytvoří prázdnou databázi s názvem quickstartdb a poté vytvoří ukázkovou tabulku s názvem inventáře. Vloží některé řádky, potom načte řádky. Změny dat pomocí příkazu update a znovu načte řádky. Nakonec se odstraní řádku a potom znovu načte řádky.
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    Na snímku obrazovky vidíte příklad kódu SQL v SQL Workbench a výstup po jeho spuštění.
    
    ![Karta SQL Workbench MySQL spustit ukázkový kód SQL](media/connect-workbench/3-workbench-sql-tab.png)

2. Pokud chcete spustit ukázkový kód SQL, klikněte na ikonu zesvětlením bolt na panelu nástrojů **soubor SQL** kartě.
3. Všimněte si tři záložkách výsledky v **výsledek mřížky** části uprostřed stránky. 
4. Upozornění **výstup** seznam v dolní části stránky. Stav každého příkazu se zobrazí. 

Nyní jste připojení k databázi Azure pro databázi MySQL pomocí MySQL Workbench a zkontrolují data pomocí jazyka SQL.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
