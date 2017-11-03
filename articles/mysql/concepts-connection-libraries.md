---
title: "Knihovny připojení pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Tento článek obsahuje seznam všech knihovny a ovladače, klienta programy můžete používat při připojování k databázi Azure pro databázi MySQL."
services: mysql
author: mswutao
ms.author: wutao
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.openlocfilehash: 759fa290cff94b04e29edd818b985b11267caab7
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Knihovny připojení pro databázi Azure pro databázi MySQL
Tento článek obsahuje seznam všech knihovny a ovladače, klienta programy můžete používat při připojování k databázi Azure pro databázi MySQL.

## <a name="client-interfaces"></a>Rozhraní klienta
MySQL nabízí připojení standardní databáze ovladačů pro použití s aplikacemi a nástroje, které jsou kompatibilní se standardy rozhraní ODBC a JDBC MySQL. Všechny systémy, které pracuje s ODBC nebo JDBC můžete použít MySQL.

| **Jazyk** | **Platforma** | **Další zdroje** | **Stáhnout** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [Nativní ovladače MySQL pro jazyk PHP - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Stáhnout](http://php.net/downloads.php) |
| ODBC | Platformy systému Windows, Mac OS X, Linux a Unix | [Příručka vývojáře MySQL konektoru nebo ODBC](https://dev.mysql.com/doc/connector-odbc/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [Příručka vývojáře MySQL Connector/Net](https://dev.mysql.com/doc/connector-net/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Nezávislé na platformě | [Příručka vývojáře konektor/J MySQL 5.1](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares nebo uzel mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Stáhnout](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [Příručka vývojáře konektoru nebo Python MySQL](https://dev.mysql.com/doc/connector-python/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [Příručka vývojáře MySQL konektor/C++](https://dev.mysql.com/doc/connector-cpp/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [Příručka vývojáře MySQL konektor/C](https://dev.mysql.com/doc/connector-c/en/) | [Stáhnout](https://dev.mysql.com/downloads/connector/c/)
| Perl | Platformy systému Windows, Mac OS X, Linux a Unix | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Stáhnout](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Další kroky
Přečtěte si tyto elementy QuickStart o tom, jak připojit k a dotazovat Azure Database pro databázi MySQL pomocí vámi zvolený jazyk:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [přejděte](./connect-go.md)

