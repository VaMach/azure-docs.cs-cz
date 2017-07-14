---
title: "Připojení ke službě Azure SQL Database pomocí jazyka Ruby | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu Ruby, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: c5d09cf03c87c8da1d8588be62fea3f0cc3eec4f
ms.contentlocale: cs-cz
ms.lasthandoff: 06/22/2017

---

# Azure SQL Database: Použití Ruby k připojení a dotazování dat
<a id="azure-sql-database-use-ruby-to-connect-and-query-data" class="xliff"></a>

Tento Rychlý start ukazuje, jak použít technologii [Ruby](https://www.ruby-lang.org) k připojení k databázi SQL Azure a následně použít jazyk Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Mac OS a Ubuntu Linux.

## Požadavky
<a id="prerequisites" class="xliff"></a>

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

## Instalace Ruby a komunikačních knihoven databáze
<a id="install-ruby-and-database-communication-libraries" class="xliff"></a>

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Ruby a teprve začínáte pracovat se službou Azure SQL Database. Pokud s vývojem pomocí technologie Ruby začínáte, přejděte na web [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Sestavení aplikace s použitím SQL Serveru), vyberte **Ruby** a pak váš operační systém.

### **Mac OS**
<a id="mac-os" class="xliff"></a>
Otevřete terminál a přejděte do adresáře, kde plánujete vytvořit skript v jazyce Ruby. Zadejte následující příkazy, kterými nainstalujete**brew**, **FreeTDS** a **TinyTDS**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install FreeTDS
gem install tiny_tds
```

### **Linux (Ubuntu)**
<a id="linux-ubuntu" class="xliff"></a>
Otevřete terminál a přejděte do adresáře, kde plánujete vytvořit skript v jazyce Ruby. Zadejte následující příkazy, kterými nainstalujete **FreeTDS** a **TinyTDS**.

```bash
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.27.tar.gz
tar -xzf freetds-1.00.27.tar.gz
cd freetds-1.00.27
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
gem install tiny_tds
```

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
Použijte následující kód k zadání dotazu na Top 20 produktů podle kategorie pomocí funkce [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) s využitím příkazu [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) jazyka Transact-SQL. Funkce TinyTDS::Clinet přijme dotaz a vrátí sadu výsledků. V sadě výsledků dotazu budou provedeny iterace pomocí [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## Vložení dat
<a id="insert-data" class="xliff"></a>
Pomocí následujícího kódu vložte nový produkt do tabulky SalesLT.Product pomocí funkce [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) s příkazem jazyka Transact-SQL [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

Tento příklad ukazuje, jak bezpečně provést příkaz INSERT, předat parametry, které ochrání vaši aplikaci před ohrožením zabezpečení prostřednictvím [injektáže SQ](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx)L a načíst automaticky generovanou hodnotu [primárního klíče](https://docs.microsoft.com/sql/relational-databases/tables/primary-and-foreign-key-constraints).    
  
Pokud chcete používat TinyTDS s platformou Azure, doporučujeme provést několik příkazů `SET` a změnit tak způsob, jakým aktuální relace zpracovává konkrétní informace. Doporučené parametry `SET` jsou k dispozici ve vzorovém kódu. Například příkaz `SET ANSI_NULL_DFLT_ON` umožní, aby nově vytvořené sloupce obsahovaly hodnotu null, přestože možnost jejího použití není pro daný sloupec explicitně nastavená.  
  
Pokud chcete sladit formát vlastnosti [datetime](https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql) s Microsoft SQL Serverem, pomocí funkce [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) přetypujte její odpovídající formát.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def insert(name, productnumber, color, standardcost, listprice, sellstartdate)
    tsql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) 
        VALUES (N'#{name}', N'#{productnumber}',N'#{color}',N'#{standardcost}',N'#{listprice}',N'#{sellstartdate}')"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
insert('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')
```

## Aktualizace dat
<a id="update-data" class="xliff"></a>
Použijte následující kód k aktualizaci nového produktu, který jste přidali dříve, pomocí funkce [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) s příkazem jazyka Transact-SQL [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true
    
def update(name, listPrice, client)
    tsql = "UPDATE SalesLT.Product SET ListPrice = N'#{listPrice}' WHERE Name =N'#{name}'";
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
update('BrandNewProduct', 500, client)
```

## Odstranění dat
<a id="delete-data" class="xliff"></a>
Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí funkce [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) s příkazem jazyka Transact-SQL [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def delete(name, client)
    tsql = "DELETE FROM SalesLT.Product WHERE Name = N'#{name}'"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
delete('BrandNewProduct', client)
```

## Další kroky
<a id="next-steps" class="xliff"></a>
- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Úložiště GitHub pro TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Hlášení problémů / kladení dotazů](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ovladače Ruby pro SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)


