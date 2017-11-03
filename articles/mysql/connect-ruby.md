---
title: "Připojení k Azure Database for MySQL pomocí Ruby | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete několik vzorových kódů Ruby, které můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 10f774262015cb19e158a687138b4618ce50063b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a>Azure Database for MySQL: Připojení a dotazování dat pomocí Ruby
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v [Ruby](https://www.ruby-lang.org) a gemu [mysql2](https://rubygems.org/gems/mysql2) z platforem Windows, Ubuntu Linux a Mac. Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. Toto téma předpokládá, že jste obeznámeni s vývojem pomocí Ruby a že jste novou práce s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Instalace Ruby
Instalace Ruby, Gem a knihovně MySQL2 ve vašem počítači. 

### <a name="windows"></a>Windows
1. Stáhněte a nainstalujte [Ruby](http://rubyinstaller.org/downloads/) verze 2.3.
2. Z nabídky Start spusťte nový příkazový řádek (cmd).
3. Změňte adresář na adresář Ruby verze 2.3. `cd c:\Ruby23-x64\bin`
4. Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.
5. Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.
6. Pomocí nástroje Gem sestavte modul Mysql2 pro Ruby spuštěním příkazu `gem install mysql2`.

### <a name="macos"></a>MacOS
1. Nainstalujte Ruby pomocí Homebrew spuštěním příkazu `brew install ruby`. Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/#homebrew) Ruby.
2. Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.
3. Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.
4. Pomocí nástroje Gem sestavte modul Mysql2 pro Ruby spuštěním příkazu `gem install mysql2`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Nainstalujte Ruby spuštěním příkazu `sudo apt-get install ruby-full`. Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/) Ruby.
2. Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.
3. Nainstalujte nejnovější aktualizace pro nástroj Gem spuštěním příkazu `sudo gem update --system`.
4. Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.
5. Nainstalujte gcc, make a další nástroje sestavení spuštěním příkazu `sudo apt-get install build-essential`.
6. Nainstalujte klientské knihovny MySQL pro vývojáře spuštěním příkazu `sudo apt-get install libmysqlclient-dev`.
7. Pomocí nástroje Gem sestavte modul mysql2 pro Ruby spuštěním příkazu `sudo gem install mysql2`.

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky**a poté vyhledejte serveru mají pokrčené (například **myserver4demo**).
3. Klikněte na název serveru **myserver4demo**.
4. Vyberte server, na **vlastnosti** stránky a poté si poznamenejte **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-ruby/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte na **přehled** stránky zobrazíte přihlašovací jméno správce serveru a v případě potřeby obnovení hesla.

## <a name="run-ruby-code"></a>Spuštění kódu Ruby 
1. Vložte kód Ruby z části níže do textových souborů a potom uložte soubory do složky projektu s .rb rozšíření souboru (například `C:\rubymysql\createtable.rb` nebo `/home/username/rubymysql/createtable.rb`).
2. Pokud chcete spustit kód, spustit příkazový řádek nebo prostředí Bash. Změňte adresář na složku vašeho projektu pomocí příkazu `cd rubymysql`.
3. Zadejte příkaz Ruby a název souboru, například `ruby createtable.rb` ke spuštění aplikace.
4. V operačním systému Windows Pokud Ruby aplikace není ve vaší proměnné prostředí path, musíte pro spuštění uzlu aplikace, jako například použijte úplnou cestu`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`

## <a name="connect-and-create-a-table"></a>Připojení a vytvoření tabulky
Použít následující kód k připojení a vytvořte tabulku s použitím **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** příkazů SQL pro přidání řádků do tabulky.

Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8). Potom několikrát volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů DROP, CREATE TABLE a INSERT INTO. Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Čtení dat
Použít následující kód k připojení a číst data pomocí **vyberte** příkaz jazyka SQL. 

Kód používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class.new() metoda pro připojení k databázi Azure pro databázi MySQL. Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů SELECT. Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Aktualizace dat
Použít následující kód k připojení a aktualizovat data pomocí **aktualizace** příkaz jazyka SQL.

Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8). Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů UPDATE. Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Odstranění dat
Použít následující kód k připojení a číst data pomocí **odstranit** příkaz jazyka SQL. 

Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8). Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů DELETE. Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
