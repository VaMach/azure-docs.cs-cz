---
title: "Použití Ruby k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Ruby vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 07/14/2017
ms.author: carlrab
ms.openlocfilehash: b7617d8f4cfe09052204182b40f6e5f7d46087b8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a>Použití Ruby k dotazování na službu Azure SQL Database

Tento rychlý úvodní kurz ukazuje použití [Ruby](https://www.ruby-lang.org) k vytvoření programu pro připojení ke službě Azure SQL Database a použití příkazů jazyka Transact-SQL k dotazování dat.

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:

- Databázi SQL Azure. Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- [Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.
- Máte nainstalované Ruby a související software pro váš operační systém.
    - **MacOS:** Nainstalujte Homebrew, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS. Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu:** Nainstalujte požadavky pro Ruby, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS. Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi zkontrolujte plně kvalifikovaný název serveru. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**, jak je znázorněno na následujícím obrázku:

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.

> [!IMPORTANT]
> Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz. Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-to-query-sql-database"></a>Vložení kódu pro dotazování databáze SQL

1. V oblíbeném textovém editoru vytvořte nový soubor **sqltest.rb**.

2. Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.

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

## <a name="run-the-code"></a>Spuštění kódu

1. V příkazovém řádku spusťte následující příkazy:

   ```bash
   ruby sqltest.rb
   ```

2. Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.


## <a name="next-steps"></a>Další kroky
- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Úložiště GitHub pro TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Hlášení problémů nebo kladení dotazů ohledně TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ovladače Ruby pro SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
