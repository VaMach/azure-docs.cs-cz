---
title: "Použití Node.js k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Node.js vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: d2765d4606bf6f926def15788ab23efd2a000ad4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a>Použití Node.js k dotazování databáze SQL Azure

Tento rychlý úvodní kurz ukazuje použití [Node.js](https://nodejs.org/en/) k vytvoření programu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:

- Databázi SQL Azure. Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- [Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.
- Máte nainstalované Node.js a související software pro váš operační systém.
    - **MacOS:** Nainstalujte Homebrew a Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD. Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
    - **Ubuntu:** Nainstalujte Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD. Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
    - **Windows:** Nainstalujte Chocolatey a Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD. Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na následujícím obrázku. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**. 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.

> [!IMPORTANT]
> Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz. Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="create-a-nodejs-project"></a>Vytvoření projektu Node.js

Otevřete příkazový řádek a vytvořte složku *sqltest*. Přejděte do složky, kterou jste vytvořili, a spusťte následující příkaz:

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a>Vložení kódu pro dotazování databáze SQL

1. Ve vývojovém prostředí nebo oblíbeném textovém editoru vytvořte nový soubor **sqltest.js**.

2. Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
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

## <a name="run-the-code"></a>Spuštění kódu

1. V příkazovém řádku spusťte následující příkazy:

   ```js
   node sqltest.js
   ```

2. Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.

## <a name="next-steps"></a>Další kroky

- Informace o [ovladači Microsoft Node.js pro SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- Informace o [připojení k databázi SQL Azure a jejím dotazování pomocí .NET Core](sql-database-connect-query-dotnet-core.md) v systému Windows, Linux nebo macOS  
- Informace o tom, [jak začít s .NET Core v systému Windows, Linux nebo macOS pomocí příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli)
- Informace o [návrhu první databáze SQL Azure pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [návrhu první databáze SQL Azure pomocí .NET](sql-database-design-first-database-csharp.md)
- Informace o [připojení a dotazování pomocí aplikace SSMS](sql-database-connect-query-ssms.md)
- Informace o [připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md)


