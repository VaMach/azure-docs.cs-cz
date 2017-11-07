---
title: "Připojení k Azure Database for PostgreSQL z Pythonu | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód Pythonu, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc, devcenter
ms.devlang: python
ms.topic: quickstart
ms.date: 11/03/2017
ms.openlocfilehash: ee310f10b27418c1dcd73755643120121f611f06
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a>Azure Database for PostgreSQL: Použití Pythonu k připojení a dotazování dat
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for PostgreSQL pomocí [Pythonu](https://python.org). Předvádí také použití příkazů jazyka SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem macOS, Ubuntu Linux a Windows. Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí Pythonu a teprve začínáte pracovat se službou Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá jako výchozí bod prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](quickstart-create-server-database-azure-cli.md)

Budete také muset:
- Nainstalovat [Python](https://www.python.org/downloads/)
- Nainstalovat balíček [pip](https://pip.pypa.io/en/stable/installing/) (pokud pracujete s binárními soubory Pythonu 2 >= 2.7.9 nebo Pythonu 3 >= 3.4 stažené z webu [python.org](https://python.org), balíček pip už máte nainstalovaný).

## <a name="install-the-python-connection-libraries-for-postgresql"></a>Instalace knihoven připojení Pythonu pro PostgreSQL
Nainstalujte balíček [psycopg2](http://initd.org/psycopg/docs/install.html) umožňující připojení a dotazování databáze. Balíček psycopg2 je [dostupný na webu PyPI](https://pypi.python.org/pypi/psycopg2/) ve formě balíčků [wheel](http://pythonwheels.com/) pro většinu běžných platforem (Linux, OSX, Windows). Pomocí příkazu pip install získejte binární verzi modulu včetně všech závislostí.

1. Na vlastním počítači spusťte rozhraní příkazového řádku:
    - V Linuxu spusťte prostředí Bash.
    - V systému macOS spusťte Terminál.
    - Ve Windows spusťte Příkazový řádek z nabídky Start.
2. Ujistěte se, že používáte nejaktuálnější verzi pip, spuštěním příkazu jako:
    ```cmd
    pip install -U pip
    ```

3. Spuštěním následujícího příkazu nainstalujte balíček psycopg2:
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte **mypgserver 20170401** (server, které jste vytvořili).
3. Klikněte na název serveru **mypgserver-20170401**.
4. Vyberte stránku **Přehled** serveru a potom si poznamenejte **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-python/1-connection-string.png)
5. Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.

## <a name="how-to-run-python-code"></a>Spuštění kódu Pythonu
Tento článek obsahuje celkem čtyři ukázky kódu, z nichž každý vykonává specifickou funkci. Následující pokyny uvádějí, jak vytvořit textový soubor, vložit do něj blok kódu a pak ho uložit, abyste ho mohli spustit později. Nezapomeňte vytvořit čtyři samostatné soubory – pro každý blok kódu jeden.

- Pomocí oblíbeného textového editoru vytvořte nový soubor.
- Zkopírujte a vložte do textového souboru jeden ze vzorových kódů v následujících částech. Nahraďte parametry **host** (hostitel), **dbname** (název databáze), **user** (uživatel) a **password** (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.
- Uložte soubor s příponou .py (třeba postgres.py) do složky vašeho projektu. Pokud používáte v systému Windows, je nutné vybrat kódování UTF-8 při ukládání souboru. 
- Spusťte prostředí shell příkazového řádku, Terminálové nebo Bash a potom změňte adresář na složky projektu, například `cd postgres`.
-  Pokud chcete spustit kód, zadejte příkaz Python následovaný názvem souboru, například `Python postgres.py`.

> [!NOTE]
> Od Python verze 3 se může zobrazit chyba `SyntaxError: Missing parentheses in call to 'print'` při spuštění následující bloky kódu: Pokud k tomu dojde, nahraďte každé volání příkazu `print "string"` s použitím závorky, jako například volání funkce `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Pomocí následujícího kódu se připojte a načtěte data s využitím funkce [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) a příkazu **INSERT** jazyka SQL. Funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) se používá k provedení dotazu SQL na databázi PostgreSQL. Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

Po úspěšném spuštění kódu se zobrazí následující výstup:

![Výstup příkazového řádku](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Čtení dat
Pomocí následujícího kódu přečtěte data vložená pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **SELECT** jazyka SQL. Tato funkce přijme dotaz a vrátí sadu výsledků, u které můžete provést iteraci pomocí funkce [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall). Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Aktualizace dat
Pomocí následujícího kódu aktualizujte řádek inventáře, který jste předtím vložili pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **UPDATE** jazyka SQL. Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Odstranění dat
Pomocí následujícího kódu odstraňte položku inventáře, kterou jste předtím vložili pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **DELETE** jazyka SQL. Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
