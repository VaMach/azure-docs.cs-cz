---
title: "Připojení ke službě Azure SQL Database pomocí Node.js | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu Node.js, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: lbosq
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: a7b1a504c6ae7850e5f986895adcf4d92fd5adc1
ms.contentlocale: cs-cz
ms.lasthandoff: 06/15/2017


---
# Azure SQL Database: Použití Node.js k připojení a dotazování dat
<a id="azure-sql-database-use-nodejs-to-connect-and-query-data" class="xliff"></a>

Tento Rychlý start ukazuje použití jazyka [Node.js](https://nodejs.org/en/) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Windows, Ubuntu Linux a Mac.

## Požadavky
<a id="prerequisites" class="xliff"></a>

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

## Instalace Node.js
<a id="install-nodejs" class="xliff"></a> 

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Node.js a teprve začínáte pracovat se službou Azure SQL Database. Pokud s vývojem pomocí Node.js začínáte, přejděte na web [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Sestavení aplikace s použitím SQL Serveru), vyberte **Node.js** a pak váš operační systém.

### **Mac OS**
<a id="mac-os" class="xliff"></a>
Zadejte následující příkazy, abyste nainstalovali **brew**, snadno použitelného správce balíčků pro Mac OS X a **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

### **Linux (Ubuntu)**
<a id="linux-ubuntu" class="xliff"></a>
Zadejte následující příkazy, abyste nainstalovali **Node.js** a **npm** – správce balíčků pro Node.js.

```bash
sudo apt-get install -y nodejs npm
```

### **Windows**
<a id="windows" class="xliff"></a>
Přejděte na [stránky pro stažení Node.js](https://nodejs.org/en/download/) a vyberte požadovanou možnost Instalační služby systému Windows.


## Instalace ovladače Tedious SQL Server pro Node.js
<a id="install-the-tedious-sql-server-driver-for-nodejs" class="xliff"></a>
Doporučeným ovladačem pro Node.js je **[tedious](https://github.com/tediousjs/tedious)**. Tedious je open-source iniciativa, kterou Microsoft přispívá k aplikacím Node.js na jakékoli platformě. Pro účely tohoto kurzu budete potřebovat prázdný adresář pro kód a závislosti `npm`, které nainstalujeme.

Pro instalaci ovladače **tedious** spusťte ve vašem adresáři následující příkaz:

```cmd
npm install tedious
```

## Získání informací o připojení
<a id="get-connection-information" class="xliff"></a>

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na obrázku níže. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**. 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.
    
## Výběr dat
<a id="select-data" class="xliff"></a>

Použijte následující kód k dotazování databáze Azure SQL pro nejlepších 20 produktů podle kategorie. Nejprve importujte třídy připojení a žádosti ovladače z knihovny ovladačů tedious. Potom vytvořte objekt konfigurace a proměnné **uživatelské jméno**, **heslo**, **server** a **databáze** nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. Vytvořte objekt `Connection` pomocí zadaného objektu `config`. Potom definujte zpětné volání pro událost `connect` objektu `connection` pro spuštění funkce `queryDatabase()`.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}
var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        queryDatabase()
    }
});

function queryDatabase(){
    console.log('Reading rows from the Table...');

    // Read all rows from table
    request = new Request(
        "SELECT TOP 1 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    
    request.on('row', function(columns) {
        columns.forEach(function(column) {
            console.log("%s\t%s", column.metadata.colName, column.value);
        });
    });

    connection.execSql(request);
}
```

## Vložení data do databáze
<a id="insert-data-into-the-database" class="xliff"></a>
Použijte následující kód k vložení nového produktu do tabulky SalesLT.Product pomocí funkce `insertIntoDatabase()` a příkazu jazyka Transact-SQL [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Proměnné **uživatelské jméno**, **heslo**, **server** a **databáze** nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. 

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        insertIntoDatabase()
    }
});

function insertIntoDatabase(){
    console.log("Inserting a brand new product into database...");
    request = new Request(
        "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) inserted');
        }
    );
    connection.execSql(request);
}
```

## Aktualizace dat v databázi
<a id="update-data-in-the-database" class="xliff"></a>
Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí funkce `updateInDatabase()` a příkazu jazyka Transact-SQL [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Proměnné **uživatelské jméno**, **heslo**, **server** a **databáze** nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. Tato ukázka používá název produktu vložený v předchozím příkladu.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        updateInDatabase()
    }
});

function updateInDatabase(){
    console.log("Updating the price of the brand new product in database...");
    request = new Request(
        "UPDATE SalesLT.Product SET ListPrice = 50 WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) updated');
        }
    );
    connection.execSql(request);
}
```

## Odstranění dat z databáze
<a id="delete-data-from-the-database" class="xliff"></a>
K odstranění dat z databáze použijte následující kód. Proměnné **uživatelské jméno**, **heslo**, **server** a **databáze** nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT. Tentokrát použijte **příkaz DELETE** ve funkci `deleteFromDatabase()`. Tato ukázka také používá název produktu vložený v předchozím příkladu.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        deleteFromDatabase()
    }
});

function deleteFromDatabase(){
    console.log("Deleting the brand new product in database...");
    request = new Request(
        "DELETE FROM SalesLT.Product WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    connection.execSql(request);
}
```


## Další kroky
<a id="next-steps" class="xliff"></a>
- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladač Microsoft Node.js pro SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md)
- [Připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md)



