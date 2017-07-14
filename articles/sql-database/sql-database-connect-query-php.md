---
title: "Připojení ke službě Azure SQL Database pomocí jazyka PHP | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu PHP, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 5b90c9b49448c6bc225f9d6ba7227782b81fc5ed
ms.contentlocale: cs-cz
ms.lasthandoff: 06/15/2017


---
# Azure SQL Database: Použití jazyka PHP k připojení a dotazování dat
<a id="azure-sql-database-use-php-to-connect-and-query-data" class="xliff"></a>

Tento Rychlý start ukazuje použití jazyka [PHP](http://php.net/manual/en/intro-whatis.php) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Mac OS, Ubuntu Linux a Windows.

## Požadavky
<a id="prerequisites" class="xliff"></a>

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

## Instalace jazyka PHP a databázového komunikačního softwaru
<a id="install-php-and-database-communications-software" class="xliff"></a>

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí jazyka PHP a teprve začínáte pracovat se službou Azure SQL Database. Pokud s vývojem pomocí jazyka PHP začínáte, přejděte na web [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Sestavení aplikace s použitím SQL Serveru), vyberte **PHP** a pak váš operační systém.

### **Mac OS**
<a id="mac-os" class="xliff"></a>
Otevřete terminál a zadejte následující příkazy, abyste nainstalovali **brew**, **ovladač Microsoft ODBC pro Mac** a **ovladače Microsoft PHP pro SQL Server**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql 
pecl install sqlsrv-4.1.7preview
pecl install pdo_sqlsrv-4.1.7preview
```

### **Linux (Ubuntu)**
<a id="linux-ubuntu" class="xliff"></a>
Zadejte následující příkazy, abyste nainstalovali **ovladač Microsoft ODBC pro Linux** a **ovladače Microsoft PHP pro SQL Server**.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev gcc g++ php-dev
sudo pecl install sqlsrv pdo_sqlsrv
sudo echo "extension= pdo_sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
sudo echo "extension= sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
```

### **Windows**
<a id="windows" class="xliff"></a>
- Instalace jazyka PHP 7.1.1 (x64) [z instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx?lang=) 
- Nainstalujte [ovladač Microsoft ODBC 13.1](https://www.microsoft.com/download/details.aspx?id=53339). 
- Stáhněte bezpečné knihovny DLL bez vlákna pro [ovladač Microsoft PHP pro SQL Server](https://pecl.php.net/package/sqlsrv/4.1.6.1/windows) a umístěte binární soubory do složky PHP\v7.x\ext.
- Potom upravte soubor php.ini (C:\Program Files\PHP\v7.1\php.ini) tak, že přidáte odkaz na knihovnu DLL. Například:
      
      extension=php_sqlsrv.dll
      extension=php_pdo_sqlsrv.dll

V tuto chvíli byste měli mít knihovny DLL zaregistrované u PHP.

## Získání informací o připojení
<a id="get-connection-information" class="xliff"></a>

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na obrázku níže. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**.  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud zapomenete přihlašovací informace pro server, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.     
    
## Výběr dat
<a id="select-data" class="xliff"></a>
Použijte následující kód k zadání dotazu na nejlepších 20 produktů podle kategorie pomocí funkce [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) s využitím příkazu [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) jazyka Transact-SQL. Funkce sqlsrv_query slouží k načítání sady výsledků dotazu z databáze SQL. Tato funkce přijme dotaz a vrátí sadu výsledků, u které můžete provést iteraci pomocí funkce [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
     FROM [SalesLT].[ProductCategory] pc
     JOIN [SalesLT].[Product] p
     ON pc.productcategoryid = p.productcategoryid";
$getResults= sqlsrv_query($conn, $tsql);
echo ("Reading data from table" . PHP_EOL);
if ($getResults == FALSE)
    echo (sqlsrv_errors());
while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
}
sqlsrv_free_stmt($getResults);
?>
```


## Vložení dat
<a id="insert-data" class="xliff"></a>
Použijte následující kód k vložení nového produktu do tabulky SalesLT.Product pomocí funkce [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) a příkazu jazyka Transact-SQL [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";
$params = array("BrandNewProduct", "200989", "Blue", 75, 80, "7/1/2016");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) inserted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

## Aktualizace dat
<a id="update-data" class="xliff"></a>
Použijte následující kód k aktualizaci dat v databázi Azure SQL pomocí funkce [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) a příkazu jazyka Transact-SQL [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "UPDATE SalesLT.Product SET ListPrice =? WHERE Name = ?";
$params = array(50,"BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) updated" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

## Odstranění dat
<a id="delete-data" class="xliff"></a>
Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí funkce [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) a příkazu jazyka Transact-SQL [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "DELETE FROM SalesLT.Product WHERE Name = ?";
$params = array("BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) deleted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
```

## Další kroky
<a id="next-steps" class="xliff"></a>
- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladače Microsoft PHP pro SQL Server](https://github.com/Microsoft/msphpsql/)
- [Hlášení problémů / kladení dotazů](https://github.com/Microsoft/msphpsql/issues)


