---
title: "Připojení ke službě Azure SQL Database pomocí Pythonu | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu Python, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 99195b43a1577f978562864bac5fa12cdeb95d63
ms.contentlocale: cs-cz
ms.lasthandoff: 06/17/2017

---
# Azure SQL Database: Použití Pythonu k připojení a dotazování dat
<a id="azure-sql-database-use-python-to-connect-and-query-data" class="xliff"></a>

 Tento Rychlý start ukazuje použití jazyka [Python](https://python.org) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Mac OS, Ubuntu Linux a Windows.

## Požadavky
<a id="prerequisites" class="xliff"></a>

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

## Instalace Pythonu a komunikačních knihoven databáze
<a id="install-the-python-and-database-communication-libraries" class="xliff"></a>

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Pythonu a teprve začínáte pracovat se službou Azure SQL Database. Pokud s vývojem pomocí jazyka Python začínáte, přejděte na web [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) (Sestavení aplikace s použitím SQL Serveru), vyberte **Python** a pak váš operační systém.

### **Mac OS**
<a id="mac-os" class="xliff"></a>
Otevřete terminál a přejděte do adresáře, kde plánujete vytvořit skript v jazyce Python. Zadejte následující příkazy, kterými nainstalujete **brew**, **Microsoft ODBC Driver for Mac** a **pyodbc**. Modul pyodbc používá ovladač Microsoft ODBC Driver v systému Linux k připojování k databázím SQL.

``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql
sudo pip install pyodbc==3.1.1
```

### **Linux (Ubuntu)**
<a id="linux-ubuntu" class="xliff"></a>
Otevřete terminál a přejděte do adresáře, kde plánujete vytvořit skript v jazyce Python. Zadejte následující příkazy, kterými nainstalujete **Microsoft ODBC Driver for Linux** a **pyodbc**. Modul pyodbc používá ovladač Microsoft ODBC Driver v systému Linux k připojování k databázím SQL.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev
sudo pip install pyodbc==3.1.1
```

### **Windows**
<a id="windows" class="xliff"></a>
Nainstalujte [Microsoft ODBC Driver 13.1](https://www.microsoft.com/download/details.aspx?id=53339) (pokud se zobrazí výzva, upgradujte ovladač). Modul pyodbc používá ovladač Microsoft ODBC Driver v systému Linux k připojování k databázím SQL. 

Pak nainstalujte modul **pyodbc** pomocí funkce pip.

```cmd
pip install pyodbc==3.1.1
```

Pokyny k tomu, abyste mohli použít funkci pip, najdete [tady](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

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

Použijte následující kód k zadání dotazu na Top 20 produktů podle kategorie pomocí funkce [pyodbc.connect](https://github.com/mkleehammer/pyodbc/wiki) s využitím příkazu [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) jazyka Transact-SQL. Funkce [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) slouží k načítání sady výsledků dotazu z databáze SQL. Tato funkce přijme dotaz a vrátí sadu výsledků, u které můžete provést iteraci pomocí funkce **cursor.fetchone()**. Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print str(row[0]) + " " + str(row[1])
    row = cursor.fetchone()
```

## Vložení dat
<a id="insert-data" class="xliff"></a>
Použijte následující kód k vložení nového produktu do tabulky SalesLT.Product pomocí funkce [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) a příkazu jazyka Transact-SQL [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

## Aktualizace dat
<a id="update-data" class="xliff"></a>
Použijte následující kód k aktualizaci nového produktu, který jste přidali dříve, pomocí funkce [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) a příkazu jazyka Transact-SQL [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```

## Odstranění dat
<a id="delete-data" class="xliff"></a>
Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí funkce [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) a příkazu jazyka Transact-SQL [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Parametry serveru, databáze, uživatelského jména a hesla nahraďte hodnotami, které jste zadali při vytváření databáze pomocí ukázkových dat AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

## Další kroky
<a id="next-steps" class="xliff"></a>

- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladače Microsoft Python pro SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/?v=17.23h)


