---
title: "Minimální dobou výpadku migrace do databáze Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak provést migraci minimální dobou výpadku extrahujte databázi PostgreSQL do souboru výpisu, obnovení databáze PostgreSQL ze souboru archivu vytvořené pg_dump v databázi Azure pro PostgreSQL a nastavení počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429828515411986dc7b3586360cb9923a5bb20de
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Minimální dobou výpadku migrace do databáze Azure pro PostgreSQL
Existující databáze PostgreSQL můžete migrovat do databáze Azure pro PostgreSQL pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. Společnosti Attunity replikace je společné nabídky od společnosti Attunity a společnosti Microsoft. Společně se službou migrace databáze Azure je zahrnuta bez dalších poplatků zákazníkům společnosti Microsoft. 

Replikovat společnosti Attunity pomáhá minimalizovat prostoje během migrace databáze, a udržuje provozní v celém procesu zdrojové databáze.

Společnosti Attunity replikace je nástroj replikace dat, která umožňuje synchronizaci dat mezi různé zdroje a cíle. Se rozšíří schéma vytváření skriptu a data související s každou databázové tabulky. Replikovat společnosti Attunity nešířily všechny artefakty (například SP, triggery, funkce a tak dále) nebo převést, například, kód PL/SQL, který je hostován v artefaktům T-SQL.

> [!NOTE]
> I když společnosti Attunity replikovat podporuje širokou škálu scénářů migrace, se zaměřuje na podporu pro určitou dílčí sadu párů zdroj a cíl.

Přehled procesu k provedení migrace minimální dobou výpadku zahrnuje:

* **Migraci schématu zdroje PostgreSQL** k databázi Azure pro databázi PostgreSQL pomocí [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) příkazu s parametrem - n a potom pomocí [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) příkaz.

* **Nastavení počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi** pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. Díky tomu minimalizuje čas, který zdrojové databáze musí být nastavena jen pro čtení během přípravy k přepínač aplikace na cílovou databázi PostgreSQL na Azure.

Další informace o společnosti Attunity replikace pro Microsoft Migrations nabídky najdete v následujících zdrojích informací:
 - Přejděte na [společnosti Attunity replikace pro Microsoft Migrations](https://aka.ms/attunity-replicate) webovou stránku.
 - Stáhněte si [společnosti Attunity replikace pro Microsoft migrace](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Přejděte na [společnosti Attunity replikovat komunity](https://microsoft.attunity.com/) úvodní příručku, kurzy a podporu.
 - Podrobné pokyny k používání společnosti Attunity replikovat při migraci z PostgreSQL do databáze Azure pro PostgreSQL najdete v tématu [příručka k migraci databáze](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).