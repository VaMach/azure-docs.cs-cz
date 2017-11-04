---
title: "Implementovat řešení zeměpisné polohy Azure SQL Database | Microsoft Docs"
description: "Další informace ke konfiguraci Azure SQL Database a aplikace pro převzetí služeb při selhání na replikované databáze a testovací převzetí služeb při selhání."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 910be8ff5f9a882c7bb8ae875b8bf5fc74d1fb9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="implement-a-geo-distributed-database"></a>Implementace geograficky distribuovaná databáze

V tomto kurzu konfigurace Azure SQL database a aplikace pro převzetí služeb při selhání do vzdáleného oblasti a poté otestujte plánu převzetí služeb při selhání. Získáte informace o těchto tématech: 

> [!div class="checklist"]
> * Vytvoření databáze uživatelů a udělit mu oprávnění
> * Nastavit pravidlo brány firewall na úrovni databáze
> * Vytvoření [geografická replikace převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)
> * Vytvoření a kompilace aplikace Java k dotazování databáze Azure SQL
> * Provedení postupu zotavení po havárii

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.


## <a name="prerequisites"></a>Požadavky

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

- Nainstalován nejnovější [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Nainstalovat Azure SQL database. Tento kurz používá ukázkové databáze AdventureWorksLT s názvem **mySampleDatabase** z jednoho z těchto rychlé spuštění:

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- Našli metodu pro spuštění skriptů SQL na databázi, můžete použít jednu z následujících nástrojů dotazu:
   - V editoru dotazů [portál Azure](https://portal.azure.com). Další informace o používání editoru dotazů na portálu Azure najdete v tématu [připojit a zadávat dotazy pomocí editoru dotazů](sql-database-get-started-portal.md#query-the-sql-database).
   - Nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), což je integrované prostředí pro správu jakékoliv infrastruktury, SQL, z SQL serveru do databáze SQL pro Microsoft Windows.
   - Nejnovější verzi [Visual Studio Code](https://code.visualstudio.com/docs), což je editor grafické kódu pro Linux, systému macOS, a systém Windows, který podporuje rozšíření, včetně [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server, Azure SQL Database a SQL Data Warehouse. Další informace o použití tohoto nástroje s Azure SQL Database, najdete v části [připojit a zadávat dotazy s VS Code](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Vytvoření databáze uživatelů a udělit oprávnění

Připojení k vaší databázi a vytvořte uživatelské účty pomocí jedné z následujících nástrojů dotazu:

- Editor dotazů na portálu Azure
- SQL Server Management Studio
- Visual Studio Code

Tyto uživatelské účty automaticky replikovat do sekundárního serveru (a sesynchronizovávat). Pokud chcete použít SQL Server Management Studio nebo Visual Studio Code, musíte nakonfigurovat pravidlo brány firewall, pokud se připojujete z klienta na adresu IP, pro kterou jste zatím nenakonfigurovali bránou firewall. Podrobné pokyny najdete v tématu [vytvoření pravidla brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- V okně dotazu spustíte následující dotaz, který vytvoříte dva uživatelské účty ve vaší databázi. Tento skript uděluje **db_owner** oprávnění k **app_admin** účet a uděluje **vyberte** a **aktualizace** oprávnění k **app_user** účtu. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Vytvoření brány firewall na úrovni databáze

Vytvoření [pravidlo brány firewall na úrovni databáze](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) pro vaši databázi SQL. Toto pravidlo brány firewall na úrovni databáze automaticky replikuje na sekundární server, který vytvoříte v tomto kurzu. Pro zjednodušení (v tomto kurzu) použijte veřejnou IP adresu počítače, na kterém provádíte kroky v tomto kurzu. Adresa IP použitá pro pravidlo brány firewall na úrovni serveru pro váš aktuální počítač, zjistíte v [vytvoření brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- V okně otevřeného dotazu nahraďte tento dotaz, nahraďte IP adresy na příslušné IP adresy pro vaše prostředí předchozího dotazu.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Vytvořit skupinu aktivní geografickou replikací automatické převzetí služeb při selhání 

Pomocí Azure PowerShell, vytvořte [aktivní geografickou replikací automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) mezi existující server Azure SQL a nový prázdný server Azure SQL v oblasti Azure a poté přidejte ukázkové databáze ke skupině převzetí služeb při selhání.

> [!IMPORTANT]
> Tyto rutiny vyžadují Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Naplnění proměnných pro skripty prostředí PowerShell pomocí hodnot pro existující server a ukázkovou databázi a zadejte globálně jedinečná hodnota pro název skupiny pro převzetí služeb při selhání.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Vytvoření prázdného zálohování serveru ve vaší oblasti převzetí služeb při selhání.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Vytvořte skupinu převzetí služeb při selhání mezi dvěma servery.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Přidejte databázi ke skupině převzetí služeb při selhání.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Instalace softwaru Java

Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Javy a teprve začínáte pracovat se službou Azure SQL Database. 

### <a name="mac-os"></a>**Mac OS**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě. Zadáním následujících příkazů nainstalujte **brew** a **Maven**: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **systému MacOS**a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě. Zadáním následujících příkazů nainstalujte **Maven**:

```bash
sudo apt-get install maven
```

Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **Ubuntu**a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v kroku 1.4, 1.2 a 1.3.

### <a name="windows"></a>**Windows**
Nainstalujte [Maven](https://maven.apache.org/download.cgi) pomocí oficiální instalační služby. Používání Maven k správě závislosti, sestavení, otestovat a spustit projekt Java. Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte Windows a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.

## <a name="create-sqldbsample-project"></a>Vytvoření projektu SqlDbSample

1. V konzole příkazu (například Bash) vytvořte projekt Maven. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Typ **Y** a klikněte na tlačítko **Enter**.
3. Změňte adresáře na nově vytvořený projekt.

   ```bash
   cd SqlDbSamples
   ```

4. Ve složce projektu pomocí vašeho oblíbeného editoru otevřete soubor pom.xml. 

5. Přidáte ovladač JDBC Microsoft pro systém SQL Server závislost na projekt Maven otevřením svém oblíbeném textovém editoru a kopírování a vkládání následující řádky do souboru pom.xml. Nepřepisovat existující hodnoty naplněnými v souboru. Závislost JDBC musí vložení v rámci větší (část) pro "závislosti".   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Zadejte verzi jazyka Java kompilace projektu před přidáním v následující části "vlastnosti" do souboru pom.xml po v části "závislosti". 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Přidejte následující části "vytvoření" po v části "vlastnosti" pro podporu manifestu souborů v JAR do souboru pom.xml.       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. Soubor pom.xml uložte a zavřete.
9. Otevřete soubor App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) a nahraďte jeho obsah s tímto obsahem. Název skupiny pro převzetí služeb při selhání nahraďte název pro skupinu pro převzetí služeb při selhání. Pokud jste změnili hodnoty pro název databáze, uživatele nebo heslo, změna také tyto hodnoty.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. Soubor App.java uložte a zavřete.

## <a name="compile-and-run-the-sqldbsample-project"></a>Zkompilování a spuštění projektu SqlDbSample

1. V konzole pro příkaz spusťte následující příkaz.

   ```bash
   mvn package
   ```
2. Po dokončení, spusťte následující příkaz ke spuštění aplikace (spuštění o jednu hodinu Pokud zastavíte ručně):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Provedení postupu zotavení po havárii

1. Volání ruční převzetí služeb při selhání skupiny převzetí služeb při selhání. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Pozorovat výsledky aplikace během převzetí služeb při selhání. Některé vloží nezdaří, při obnovení mezipaměti DNS.     

3. Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Navrácení služeb po obnovení.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Pozorovat výsledky aplikace během navrácení služeb po obnovení. Některé vloží nezdaří, při obnovení mezipaměti DNS.     

6. Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [aktivní geografickou replikaci a převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md).
