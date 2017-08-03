---
title: "Připojení k Azure Database for PostgreSQL z Pythonu | Dokumentace Microsoftu"
description: "Obsahuje vzorek kódu Python, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/07/2017
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: f6ae1ef3855711a86333857f26400f29dfd7c54e
ms.contentlocale: cs-cz
ms.lasthandoff: 08/01/2017

---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a>Azure Database for PostgreSQL: Použití Pythonu k připojení a dotazování dat
Tento Rychlý start ukazuje použití jazyka [Python](https://python.org) pro připojení k Azure Database for PostgreSQL a následné použití příkazů jazyka SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Mac OS, Ubuntu Linux a Windows. Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí Pythonu a teprve začínáte pracovat se službou Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá jako výchozí bod prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](quickstart-create-server-database-azure-cli.md)

Budete také muset:
- Nainstalovat [Python](https://www.python.org/downloads/)
- Nainstalovat balíček [PIP](https://pip.pypa.io/en/stable/installing/) (pokud používáte binární soubory Pythonu 2 >=2.7.9 nebo Pythonu 3 >=3.4 stažené z webu [python.org](https://python.org), balíček pip už máte nainstalovaný, musíte ho ale upgradovat)

## <a name="install-the-python-connection-libraries-for-postgresql"></a>Instalace knihoven připojení Pythonu pro PostgreSQL
Nainstalujte balíček [psycopg2](http://initd.org/psycopg/docs/install.html) umožňující připojení a dotazování databáze. psycopg2 je [k dispozici v PyPI](https://pypi.python.org/pypi/psycopg2/) ve formě balíčků [wheel](http://pythonwheels.com/) pro nejběžnější platformy (Linux, OSX, Windows), takže můžete prostřednictvím instalace pip získat binární verzi modulu včetně všech závislostí:

```cmd
pip install psycopg2
```
Nezapomeňte použít aktuální verzi pip (můžete ji upgradovat pomocí příkazu podobného tomuto: `pip install -U pip`).

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce na levé straně na portálu Azure Portal klikněte na **Všechny prostředky** a vyhledejte právě vytvořený server **mypgserver-20170401**.
3. Klikněte na název serveru **mypgserver-20170401**.
4. Vyberte stránku **Přehled** serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/connect-python/1-connection-string.png)
5. Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.

## <a name="how-to-run-python-code"></a>Spuštění kódu Pythonu
- Pomocí oblíbeného textového editoru vytvořte nový soubor postgres.py a uložte ho do složky projektu. Zkopírujte vzorový kód uvedený níže, vložte ho do textového souboru a ten uložte. Pokud soubor ukládáte v operačním systému Windows, nezapomeňte vybrat kódování UTF-8. 
- Pokud chcete kód spustit, spusťte příkazový řádek nebo prostředí Bash. Změňte adresář na složku vašeho projektu, například `cd postgresql`. Potom zadejte příkaz python následovaný názvem souboru, například `python postgres.py`.

> [!NOTE]
> Od Pythonu verze 3 se při spouštění níže uvedených bloků kódu může zobrazit chyba `SyntaxError: Missing parentheses in call to 'print'`. Pokud k tomu dojde, nahraďte všechna volání příkazu `print "string"` za volání funkce použitím závorek, například `print("string")`.

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

