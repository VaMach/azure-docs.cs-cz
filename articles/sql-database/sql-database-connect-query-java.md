---
title: "Připojení ke službě Azure SQL Database pomocí Javy | Dokumentace Microsoftu"
description: "Obsahuje vzorový kód v Javě, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/23/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 0df7bd56d3aac462a86e31b5512e48371918bbf2
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017


---
<a id="azure-sql-database-use-java-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: Použití Javy k připojení a dotazování dat

Tento Rychlý start ukazuje použití jazyka [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Mac OS, Ubuntu Linux a Windows.

<a id="prerequisites" class="xliff"></a>

## Požadavky

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

<a id="install-java-software" class="xliff"></a>

## Instalace softwaru Java

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Javy a teprve začínáte pracovat se službou Azure SQL Database. Pokud s vývojem pomocí rozhraní Javy začínáte, přejděte na web [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Sestavení aplikace s použitím SQL Serveru), vyberte jazyk **Java** a potom váš operační systém.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě. Zadáním následujících příkazů nainstalujte **brew** a **Maven**: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě. Zadáním následujících příkazů nainstalujte **Maven**:

```bash
sudo apt-get install maven
```

<a id="windows" class="xliff"></a>

### **Windows**
Nainstalujte [Maven](https://maven.apache.org/download.cgi) pomocí oficiální instalační služby. Maven použijte jako pomoc při správě závislostí, sestavování, testování a spouštění vašeho projektu v Javě. 

<a id="get-connection-information" class="xliff"></a>

## Získání informací o připojení

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na obrázku níže. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**. 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud zapomenete přihlašovací informace pro server, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby obnovili heslo.
5. Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.

6. Zkontrolujte úplný připojovací řetězec **JDBC**.

    ![Připojovací řetězec JDBC](./media/sql-database-connect-query-jdbc/jdbc-connection-string.png)   

<a id="create-maven-project" class="xliff"></a>

### **Vytvoření projektu v Mavenu**
Na terminálu vytvořte nový projekt v Mavenu. 
```bash
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

K závislostem v souboru ***pom.xml*** přidejte **ovladač Microsoft JDBC pro SQL Server**. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.0.jre8</version>
</dependency>
```

<a id="select-data" class="xliff"></a>

## Výběr dat

Použijte následující kód k zadání dotazu na Top 20 produktů podle kategorie pomocí třídy [Connection](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection) s využitím příkazu [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) jazyka Transact-SQL. Nahraďte parametry hostName (název hostitele), dbName (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření databáze s ukázkovými daty AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                try (Statement statement = connection.createStatement();
                    ResultSet resultSet = statement.executeQuery(selectSql)) {

                        // Print results from select statement
                        System.out.println("Top 20 categories:");
                        while (resultSet.next())
                        {
                            System.out.println(resultSet.getString(1) + " "
                                + resultSet.getString(2));
                        }
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="insert-data" class="xliff"></a>

## Vložení dat

Použijte následující kód k vložení nového produktu do tabulky SalesLT.Product pomocí [připravených příkazů](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) s příkazem [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) jazyka Transact-SQL. Nahraďte parametry hostName (název hostitele), dbName (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření databáze s ukázkovými daty AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, " 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                try (PreparedStatement prep = connection.prepareStatement(insertSql)) {
                        prep.setString(1, "BrandNewProduct");
                        prep.setInt(2, 200989);
                        prep.setString(3, "Blue");
                        prep.setDouble(4, 75);
                        prep.setDouble(5, 80);
                        prep.setTimestamp(6, sqlTimeStamp);

                        int count = prep.executeUpdate();
                        System.out.println("Inserted: " + count + " row(s)");
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
<a id="update-data" class="xliff"></a>

## Aktualizace dat

Použijte následující kód k aktualizaci nového produktu, který jste přidali dříve, pomocí [připravených příkazů](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) s příkazem [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) jazyka Transact-SQL. Doje k aktualizaci dat v databázi Azure SQL. Nahraďte parametry hostName (název hostitele), dbName (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření databáze s ukázkovými daty AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Update data example:");
                System.out.println("=========================================");

                // Prepared statement to update data
                String updateSql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(updateSql)) {
                        prep.setString(1, "500");
                        prep.setString(2, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Updated: " + count + " row(s)")
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



<a id="delete-data" class="xliff"></a>

## Odstranění dat

Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí [připravených příkazů](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) s příkazem [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) jazyka Transact-SQL. Nahraďte parametry hostName (název hostitele), dbName (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření databáze s ukázkovými daty AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Delete data example:");
                System.out.println("=========================================");

                // Prepared statement to delete data
                String deleteSql = "DELETE SalesLT.Product WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(deleteSql)) {
                        prep.setString(1, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Deleted: " + count + " row(s)");
                }
        }       
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="next-steps" class="xliff"></a>

## Další kroky
- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladač Microsoft JDBC pro SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Hlášení problémů/kladení dotazů](https://github.com/microsoft/mssql-jdbc/issues)


