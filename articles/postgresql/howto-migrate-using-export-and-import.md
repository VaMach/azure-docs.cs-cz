---
title: "Migrace databáze pomocí importu a exportu v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje, jak extrahovat databázi PostgreSQL do souboru skriptu a importovat data do cílové databáze z tohoto souboru."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5e306d516d04789e4526bfd09bf99139b83573ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>PostgreSQL databázi migrujte pomocí exportu a importu
Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) extrahovat databázi PostgreSQL do souboru skriptu a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) pro import dat do cílové databáze z tohoto souboru.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) se pravidla brány firewall umožňující přístup a databáze v něm.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) nainstalovaný nástroj příkazového řádku
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nainstalovaný nástroj příkazového řádku

Postupujte podle těchto kroků pro export a import databázi PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Vytvoření souboru skriptu pomocí pg_dump, který obsahuje data, která mají být načten
K exportu existující PostgreSQL databáze místní nebo do virtuálního počítače do souboru skriptu sql, spusťte následující příkaz v vaše stávající prostředí:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Například, pokud máte místní server a databáze názvem **testdb** v ní
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postrgesql"></a>Umožňuje importovat data na cílové databázi Azure pro PostrgeSQL
Můžete použít psql příkazového řádku a -d, – dbname parametr pro import dat do databáze Azure pro PostrgeSQL jsme vytvořili a načtení dat ze souboru sql.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Tento příklad používá nástroj psql a souboru skriptu s názvem **testdb.sql** z předchozího kroku k importu dat do databáze **mypgsqldb** na cílovém serveru **mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Další kroky
- K migraci databázi PostgreSQL pomocí výpisu a obnovení, najdete v části [PostgreSQL databázi migrujte pomocí výpisu a obnovení](howto-migrate-using-dump-and-restore.md)