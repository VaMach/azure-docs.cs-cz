---
title: "Připojení k Azure Database for PostgreSQL pomocí Ruby | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód Ruby, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 06/30/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: a04188aed40a46e8cb64dfeb1230fb9b3c6ab679
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017

---

<a id="azure-database-for-postgresql-use-ruby-to-connect-and-query-data" class="xliff"></a>

# Azure Database for PostgreSQL: Použití Ruby k připojení a dotazování dat
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for PostgreSQL pomocí aplikace v [Ruby](https://www.ruby-lang.org). Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. V tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí Ruby, ale teprve začínáte pracovat se službou Azure Database for PostgreSQL.

<a id="prerequisites" class="xliff"></a>

## Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

<a id="install-ruby" class="xliff"></a>

## Instalace Ruby
Nainstalujte Ruby na vlastní počítač. 

<a id="windows" class="xliff"></a>

### Windows
- Stáhněte a nainstalujte nejnovější verzi [Ruby](http://rubyinstaller.org/downloads/).
- Na poslední obrazovce instalačního programu MSI zaškrtněte políčko s textem „Run 'ridk install' to install MSYS2 and development toolchain“ (Spustit ridk install pro instalaci MSYS2 a vývojářské sady nástrojů). Potom kliknutím na **Finish** (Dokončit) spusťte další instalační program.
- Spustí se instalační program RubyInstaller2 pro Windows. Zadejte 2 pro instalaci aktualizace úložiště MSYS2. Po dokončení a navrácení na příkazový řádek instalace zavřete příkazové okno.
- Z nabídky Start spusťte nový příkazový řádek (cmd).
- Příkazem `ruby -v` otestujte instalaci Ruby a zobrazte nainstalovanou verzi.
- Příkazem `gem -v` otestujte instalaci nástroje Gem a zobrazte nainstalovanou verzi.
- Pomocí nástroje Gem sestavte modul PostgreSQL pro Ruby spuštěním příkazu `gem install pg`.

<a id="macos" class="xliff"></a>

### MacOS
- Nainstalujte Ruby pomocí Homebrew spuštěním příkazu `brew install ruby`. Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/#homebrew) Ruby.
- Příkazem `ruby -v` otestujte instalaci Ruby a zobrazte nainstalovanou verzi.
- Příkazem `gem -v` otestujte instalaci nástroje Gem a zobrazte nainstalovanou verzi.
- Pomocí nástroje Gem sestavte modul PostgreSQL pro Ruby spuštěním příkazu `gem install pg`.

<a id="linux-ubuntu" class="xliff"></a>

### Linux (Ubuntu)
- Nainstalujte Ruby spuštěním příkazu `sudo apt-get install ruby-full`. Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/) Ruby.
- Příkazem `ruby -v` otestujte instalaci Ruby a zobrazte nainstalovanou verzi.
- Nainstalujte nejnovější aktualizace pro nástroj Gem spuštěním příkazu `sudo gem update --system`.
- Příkazem `gem -v` otestujte instalaci nástroje Gem a zobrazte nainstalovanou verzi.
- Nainstalujte gcc, make a další nástroje sestavení spuštěním příkazu `sudo apt-get install build-essential`.
- Nainstalujte knihovny PostgreSQL spuštěním příkazu `sudo apt-get install libpq-dev`.
- Pomocí nástroje Gem sestavte modul pg pro Ruby spuštěním příkazu `sudo gem install pg`.

<a id="run-ruby-code" class="xliff"></a>

## Spuštění kódu Ruby 
- Uložte kód do textového souboru a soubor uložte s příponou souboru .rb do složky projektu, například `C:\rubypostgres\read.rb` nebo `/home/username/rubypostgres/read.rb`.
- Pokud chcete kód spustit, spusťte příkazový řádek nebo prostředí Bash. Pokud chcete aplikaci spustit, změňte adresář na složku projektu příkazem `cd rubypostgres` a pak zadejte příkaz `ruby read.rb`.

<a id="get-connection-information" class="xliff"></a>

## Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **mypgserver-20170401**.
3. Klikněte na název serveru **mypgserver-20170401**.
4. Vyberte stránku **Přehled** serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-ruby/1-connection-string.png)
5. Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled** a zobrazte přihlašovací jméno správce serveru. V případě potřeby obnovte heslo.

<a id="connect-and-create-a-table" class="xliff"></a>

## Připojení a vytvoření tabulky
Pomocí následujícího kódu se připojte a vytvořte tabulku s využitím příkazu **CREATE TABLE** jazyka SQL, po kterém následují příkazy **INSERT INTO** jazyka SQL, které do tabulky přidají řádky.

Kód pro připojení ke službě Azure Database for PostgreSQL používá objekt [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) s konstruktorem [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize). Potom volá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) pro spuštění příkazů DROP, CREATE TABLE a INSERT INTO. Kód kontroluje chyby pomocí třídy [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error). Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `user` a `password` vlastními hodnotami. 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="read-data" class="xliff"></a>

## Čtení dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL. 

Kód pro připojení ke službě Azure Database for PostgreSQL používá objekt [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) s konstruktorem [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize). Potom volá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) pro spuštění příkazu SELECT a výsledky uloží do sady výsledků dotazu. Nad kolekcí sady výsledků dotazu se iteruje pomocí smyčky `resultSet.each do` a hodnoty aktuálního řádku se ukládají do proměnné `row`. Kód kontroluje chyby pomocí třídy [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error). Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `user` a `password` vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="update-data" class="xliff"></a>

## Aktualizace dat
Pomocí následujícího kódu se připojte a aktualizujte data s využitím příkazu **UPDATE** jazyka SQL.

Kód pro připojení ke službě Azure Database for PostgreSQL používá objekt [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) s konstruktorem [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize). Potom volá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) pro spuštění příkazu UPDATE. Kód kontroluje chyby pomocí třídy [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error). Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `user` a `password` vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


<a id="delete-data" class="xliff"></a>

## Odstranění dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL. 

Kód pro připojení ke službě Azure Database for PostgreSQL používá objekt [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) s konstruktorem [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize). Potom volá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) pro spuštění příkazu UPDATE. Kód kontroluje chyby pomocí třídy [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error). Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) pro ukončení připojení.

Nahraďte řetězce `host`, `database`, `user` a `password` vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="next-steps" class="xliff"></a>

## Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)

