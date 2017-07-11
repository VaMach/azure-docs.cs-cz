---
title: "Připojení k Azure Database for MySQL z PHP | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód PHP, který můžete použít k připojení a dotazování dat z databáze Azure Database for MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 06/26/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: e3ac0e1813022d1b3544fc2c784719d6c98a0cf3
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017

---

<a id="azure-database-for-mysql-use-php-to-connect-and-query-data" class="xliff"></a>

# Azure Database for MySQL: Připojení a dotazování dat pomocí PHP
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v [PHP](http://php.net/manual/intro-whatis.php). Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. V tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí PHP, ale teprve začínáte pracovat se službou Azure Database for MySQL.

<a id="prerequisites" class="xliff"></a>

## Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

<a id="install-php" class="xliff"></a>

## Instalace PHP
Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.

<a id="macos" class="xliff"></a>

### MacOS
- Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).
- Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.macosx.php).

<a id="linux-ubuntu" class="xliff"></a>

### Linux (Ubuntu)
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php).
- Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.unix.php).

<a id="windows" class="xliff"></a>

### Windows
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).
- Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.windows.php).

<a id="get-connection-information" class="xliff"></a>

## Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **myserver4demo**.
3. Klikněte na název serveru.
4. Vyberte stránku **Vlastnosti** serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Název serveru Azure Database for MySQL](./media/connect-php/1_server-properties-name-login.png)
5. Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.

<a id="connect-and-create-a-table" class="xliff"></a>

## Připojení a vytvoření tabulky
Pomocí následujícího kódu se připojte a vytvořte tabulku s využitím příkazu **CREATE TABLE** jazyka SQL. 

Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP. Kód volá metody [mysqli_init](http://php.net/manual/mysqli.init.php) a [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) pro připojení k MySQL. Potom volá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) pro spuštění dotazu. Potom volá metodu [mysqli_close](http://php.net/manual/mysqli.close.php) pro ukončení připojení.

Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="insert-data" class="xliff"></a>

## Vložení dat
Pomocí následujícího kódu se připojte a vložte data s využitím příkazu **SELECT** jazyka SQL.

Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP. Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Insert a potom vytvoří vazbu parametrů pro každou vloženou hodnotu sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

<a id="read-data" class="xliff"></a>

## Čtení dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.  Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP. Kód používá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) k provedení dotazu sql query a metodu [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) k načtení výsledných řádků.

Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="update-data" class="xliff"></a>

## Aktualizace dat
Pomocí následujícího kódu se připojte a aktualizujte data s využitím příkazu **UPDATE** jazyka SQL.

Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP. Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Update a potom vytvoří vazbu parametrů pro každou aktualizovanou hodnotu sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


<a id="delete-data" class="xliff"></a>

## Odstranění dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL. 

Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP. Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Delete a potom vytvoří vazbu parametrů pro klauzuli Where v tomto příkazu pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="next-steps" class="xliff"></a>

## Další kroky
> [!div class="nextstepaction"]
> [Vytvoření webové aplikace PHP a MySQL v Azure](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)

