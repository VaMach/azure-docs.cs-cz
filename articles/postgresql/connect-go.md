---
title: "Připojení k Azure Database for PostgreSQL pomocí jazyka Go | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete ukázku programovacího jazyka Go, kterou můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 06/29/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: f3e7e57993b5253a895640854672da431c00854b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017

---

<a id="azure-database-for-postgresql-use-go-language-to-connect-and-query-data" class="xliff"></a>

# Azure Database for PostgreSQL: Použití jazyka Go k připojení a dotazování dat
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for PostgreSQL pomocí kódu napsaného v jazyce [Go](https://golang.org/). Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. V tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí jazyka Go, ale teprve začínáte pracovat se službou Azure Database for PostgreSQL.

<a id="prerequisites" class="xliff"></a>

## Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

<a id="install-go-and-pq" class="xliff"></a>

## Instalace jazyka Go a ovladače pq
- Stáhněte a nainstalujte jazyk Go podle [pokynů k instalaci](https://golang.org/doc/install) odpovídajících vaší platformě.
- Vytvořte složku pro váš projekt, například C:\Postgresql\. Pomocí příkazového řádku změňte adresář na složku projektu, například příkazem `cd C:\Postgres\`.
- Stáhněte [ovladač Pure Go Postgres (pq)](https://github.com/lib/pq) do složky projektu zadáním příkazu `go get github.com/lib/pq`, když jste ve stejném adresáři.

<a id="build-and-run-go-code" class="xliff"></a>

## Sestavení a spuštění kódu jazyka Go 
- Uložte kód do textového souboru s příponou *.go a soubor uložte do složky projektu, například `C:\postgres\read.go`.
- Pokud chcete kód spustit, změňte adresář na složku projektu příkazem `cd postgres` a pak zadejte příkaz `go run read.go`, který aplikaci zkompiluje a spustí.
- Pokud z kódu chcete sestavit nativní aplikaci, spusťte příkaz `go build read.go` a pak aplikaci spusťte pomocí souboru read.exe.

<a id="get-connection-information" class="xliff"></a>

## Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **mypgserver-20170401**.
3. Klikněte na název serveru **mypgserver-20170401**.
4. Vyberte stránku **Přehled** serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-go/1-connection-string.png)
5. Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled** a zobrazte přihlašovací jméno správce serveru. V případě potřeby obnovte heslo.

<a id="connect-and-create-a-table" class="xliff"></a>

## Připojení a vytvoření tabulky
Pomocí následujícího kódu se připojte a vytvořte tabulku s využitím příkazu **CREATE TABLE** jazyka SQL, po kterém následují příkazy **INSERT INTO** jazyka SQL, které do tabulky přidají řádky.

Kód importuje tři balíčky: [balíček sql](https://golang.org/pkg/database/sql/), [balíček pq](http://godoc.org/github.com/lib/pq) jako ovladač pro komunikaci se serverem Postgres a [balíček fmt](https://golang.org/pkg/fmt/) pro tisk vstupů a výstupů na příkazovém řádku.

Kód volá metodu [sql.Open()](http://godoc.org/github.com/lib/pq#Open) pro připojení ke službě Azure Database for PostgreSQL a pomocí metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) zkontroluje stav připojení. Po celou dobu se používá [popisovač databáze](https://golang.org/pkg/database/sql/#DB), který uchovává fond připojení pro databázový server. Kód několikrát volá metodu [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) pro spuštění několika příkazů jazyka SQL. Pokaždé vlastní metoda checkError() zkontroluje, jestli nedošlo k chybě, a pokud k nějaké dojde, nouzově kód ukončí.


Nahraďte parametry `HOST`, `DATABASE`, `USER` a `PASSWORD` vlastními hodnotami. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

<a id="read-data" class="xliff"></a>

## Čtení dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL. 

Kód importuje tři balíčky: [balíček sql](https://golang.org/pkg/database/sql/), [balíček pq](http://godoc.org/github.com/lib/pq) jako ovladač pro komunikaci se serverem Postgres a [balíček fmt](https://golang.org/pkg/fmt/) pro tisk vstupů a výstupů na příkazovém řádku.

Kód volá metodu [sql.Open()](http://godoc.org/github.com/lib/pq#Open) pro připojení ke službě Azure Database for PostgreSQL a pomocí metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) zkontroluje stav připojení. Po celou dobu se používá [popisovač databáze](https://golang.org/pkg/database/sql/#DB), který uchovává fond připojení pro databázový server. Dotaz SELECT se spustí zavoláním metody [db.Query()](https://golang.org/pkg/database/sql/#DB.Query) a výsledné řádky se uloží do proměnné typu [rows](https://golang.org/pkg/database/sql/#Rows). Kód čte hodnoty dat sloupců na aktuálním řádku pomocí metody [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) a ve smyčce prochází řádky pomocí iterátoru [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next), dokud nějaké řádky existují. Hodnoty sloupců každého řádku se vytisknou na výstup konzoly. Pokaždé vlastní metoda checkError() zkontroluje, jestli nedošlo k chybě, a pokud k nějaké dojde, nouzově kód ukončí.

Nahraďte parametry `HOST`, `DATABASE`, `USER` a `PASSWORD` vlastními hodnotami. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

<a id="update-data" class="xliff"></a>

## Aktualizace dat
Pomocí následujícího kódu se připojte a aktualizujte data s využitím příkazu **UPDATE** jazyka SQL.

Kód importuje tři balíčky: [balíček sql](https://golang.org/pkg/database/sql/), [balíček pq](http://godoc.org/github.com/lib/pq) jako ovladač pro komunikaci se serverem Postgres a [balíček fmt](https://golang.org/pkg/fmt/) pro tisk vstupů a výstupů na příkazovém řádku.

Kód volá metodu [sql.Open()](http://godoc.org/github.com/lib/pq#Open) pro připojení ke službě Azure Database for PostgreSQL a pomocí metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) zkontroluje stav připojení. Po celou dobu se používá [popisovač databáze](https://golang.org/pkg/database/sql/#DB), který uchovává fond připojení pro databázový server. Kód volá metodu [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) pro spuštění příkazu jazyka SQL, který aktualizuje tabulku. Vlastní metoda checkError() zkontroluje, jestli nedošlo k chybě, a pokud k nějaké dojde, nouzově kód ukončí.

Nahraďte parametry `HOST`, `DATABASE`, `USER` a `PASSWORD` vlastními hodnotami. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

<a id="delete-data" class="xliff"></a>

## Odstranění dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL. 

Kód importuje tři balíčky: [balíček sql](https://golang.org/pkg/database/sql/), [balíček pq](http://godoc.org/github.com/lib/pq) jako ovladač pro komunikaci se serverem Postgres a [balíček fmt](https://golang.org/pkg/fmt/) pro tisk vstupů a výstupů na příkazovém řádku.

Kód volá metodu [sql.Open()](http://godoc.org/github.com/lib/pq#Open) pro připojení ke službě Azure Database for PostgreSQL a pomocí metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) zkontroluje stav připojení. Po celou dobu se používá [popisovač databáze](https://golang.org/pkg/database/sql/#DB), který uchovává fond připojení pro databázový server. Kód volá metodu [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) pro spuštění příkazu jazyka SQL, který aktualizuje tabulku. Vlastní metoda checkError() zkontroluje, jestli nedošlo k chybě, a pokud k nějaké dojde, nouzově kód ukončí.

Nahraďte parametry `HOST`, `DATABASE`, `USER` a `PASSWORD` vlastními hodnotami. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

<a id="next-steps" class="xliff"></a>

## Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)

