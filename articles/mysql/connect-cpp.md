---
title: "Připojení k Azure Database for MySQL z jazyka C++ | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód jazyka C++, který můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: C++
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 92620c8081b1f0f5c96cc3ae09465b3526e74042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>Azure Database for MySQL: Připojení a dotazování dat pomocí konektoru Connector/C++
Tento rychlý start předvádí, jak se připojit k databázi Azure pro databázi MySQL pomocí aplikace C++. Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. Toto téma předpokládá, že jste obeznámeni s vývojem pomocí C++ a že jste novou práce s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky vytvořené jako výchozí bod v některém z následujících příručkách:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Nainstalovat [.NET Framework](https://www.microsoft.com/net/download)
- Nainstalovat [Visual Studio](https://www.visualstudio.com/downloads/)
- Nainstalovat [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/) 
- Nainstalovat [Boost](http://www.boost.org/)

## <a name="install-visual-studio-and-net"></a>Instalace sady Visual Studio a .NET
Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí rozhraní .NET.

### <a name="windows"></a>**Windows**
- Nainstalujte Visual Studio Community 2017, což je úplné vybrané, rozšiřitelný, volné IDE pro vytváření moderní aplikace pro Android, iOS, Windows, jakož i webové a databázové aplikace a cloudových služeb. Můžete nainstalovat buď úplné rozhraní .NET Framework, nebo jenom .NET Core: fragmenty kódu v rychlé spuštění fungovat se. Pokud již máte v počítači nainstalovanou sadu Visual Studio, přeskočte následující dva kroky.
   1. Stáhněte si [instalační program sady Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
   2. Spusťte instalační program a podle zobrazených pokynů instalaci dokončete.

### <a name="configure-visual-studio"></a>**Konfigurace sady Visual Studio**
1. V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Linker > Obecné > Další adresáře knihoven přidejte adresář lib\opt (tj.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) konektoru C++.
2. Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > Obecné > Další adresáře souborů k zahrnutí:
   - Přidat zahrnují / directory c ++ konektoru (tj.: C:\Program Files (x86) \MySQL\MySQL konektor C++ 1.1.9\include\).
   - Přidání knihovny nárůst kořenový adresář (tj.: C:\boost_1_64_0\).
3. Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > linkeru > vstup > Další závislosti, do textového pole, přidejte mysqlcppconn.lib.
4. Zkopírujte mysqlcppconn.dll ze složky knihovny konektor C++ v kroku 3 stejném adresáři jako spustitelný soubor aplikace nebo přidejte do proměnné prostředí, vaše aplikace k dispozici.

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky**a vyhledejte serveru, který jste vytvořili (například **myserver4demo**).
3. Klikněte na název serveru.
4. Vyberte server, na **vlastnosti** stránky a poté si poznamenejte **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Název serveru Azure Database for MySQL](./media/connect-cpp/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte na **přehled** stránky zobrazíte přihlašovací jméno správce serveru a v případě potřeby obnovení hesla.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použít následující kód k připojení a načíst data pomocí **CREATE TABLE** a **INSERT INTO** příkazů SQL. Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect(). Potom kód použije metody createStatement() a execute() pro spuštění příkazů databáze. 

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a>Čtení dat

Použít následující kód k připojení a číst data pomocí **vyberte** příkaz jazyka SQL. Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect(). Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů select. V dalším kroku kód používá next() posunut na záznamy ve výsledcích. Nakonec kód používá getInt() a funkci getString() analyzovat hodnoty v záznamu.

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Aktualizace dat
Použít následující kód k připojení a číst data pomocí **aktualizace** příkaz jazyka SQL. Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect(). Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů update. 

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Odstranění dat
Použít následující kód k připojení a číst data pomocí **odstranit** příkaz jazyka SQL. Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect(). Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů delete.

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace databáze MySQL do služby Azure Database for MySQL pomocí výpisu a obnovení.](concepts-migrate-dump-restore.md)
