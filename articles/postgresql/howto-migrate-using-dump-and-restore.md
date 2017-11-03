---
title: "Výpis stavu a obnovení v Azure databázi pro PostgreSQL | Microsoft Docs"
description: "Popisuje, jak extrahovat databázi PostgreSQL do souboru výpisu a obnovit databázi PostgreSQL ze souboru archivu vytvořené pg_dump v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 190373c4980b67e16b58700e4b7e65658545c615
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>PostgreSQL databázi migrujte pomocí výpisu a obnovení
Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) extrahovat databázi PostgreSQL do souboru výpisu a [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) ze souboru archivu vytvořené pg_dump obnovit databázi PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) se pravidla brány firewall umožňující přístup a databáze v něm.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) a [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) nainstalované nástroje příkazového řádku

Postupujte podle těchto kroků dump a obnovit databázi PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Vytvoření souboru výpisu pomocí pg_dump, který obsahuje data, která mají být načten
Chcete-li zálohovat stávající PostgreSQL databáze místní nebo ve virtuálním počítači, spusťte následující příkaz:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Například, pokud máte místní server a databáze názvem **testdb** v ní
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Obnovení dat do cílové databáze Azure pro PostrgeSQL pomocí pg_restore
Po vytvoření cílové databázi, můžete použít příkaz pg_restore a -d, parametr dbname – Chcete-li obnovit data do cílové databáze ze souboru s výpisem stavu.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
V tomto příkladu, obnovte data ze souboru s výpisem **testdb.dump** do databáze **mypgsqldb** na cílovém serveru **mypgserver 20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Další kroky
- K migraci databázi PostgreSQL pomocí exportu a importu, najdete v části [PostgreSQL databázi migrujte pomocí exportu a importu](howto-migrate-using-export-and-import.md)