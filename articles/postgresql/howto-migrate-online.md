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
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Minimální dobou výpadku migrace do databáze Azure pro PostgreSQL
Můžete migrovat existující databáze PostgreSQL databáze Azure pro PostgreSQL pomocí společnosti Attunity replikace pro Microsoft Migrations, společně sponzorovaný, společné nabídky od společnosti Attunity a společnosti Microsoft, která je k dispozici společně se službou Azure databáze migrace bez dalších poplatků zákazníkům společnosti Microsoft. Replikovat společnosti Attunity pro Microsoft Migrations taky umožňuje minimální výpadek při migraci databáze a zdrojové databáze je stále provozní během procesu migrace.

Společnosti Attunity replikace je nástroj replikace dat, která umožňuje synchronizaci dat mezi různé zdroje a cíle, šíření skriptu pro vytváření schémat a data související s každou databázové tabulky. Replikovat společnosti Attunity nešířily všechny artefakty (například SP, triggery, funkce, atd.) nebo převést, například kód PL/SQL hostované v artefaktům T-SQL.

> [!NOTE]
> I když společnosti Attunity replikovat podporuje širokou škálu scénářů migrace, společnosti Attunity replikace pro Microsoft Migrations se zaměřuje na podporu pro určitou dílčí sadu párů zdroj a cíl.

Přehled procesu k provedení migrace minimální dobou výpadku zahrnuje:

1. **Migraci schématu zdroje PostgreSQL** k databázi Azure pro databázi PostgreSQL pomocí [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) příkazu s parametrem - n a potom pomocí [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) příkaz.

2. **Nastavení počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi** pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. Díky tomu minimalizuje čas, který zdrojové databáze musí být nastavena jen pro čtení během přípravy k přepínač aplikace na cílovou databázi PostgreSQL na Azure.

Další informace o společnosti Attunity replikace Microsoft Migrations nabídky zobrazte v následujících zdrojích informací:
 - Replikovat společnosti Attunity pro Microsoft Migrations [webové stránky](https://aka.ms/attunity-replicate).
 - [Stáhněte si](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) společnosti Attunity replikace pro Microsoft migrace.
 - Replikovat společnosti Attunity [komunity](https://microsoft.attunity.com/), s úvodní příručku, kurzy a podporu.
 - Podrobné pokyny k používání společnosti Attunity při migraci z PostgreSQL do databáze Azure pro PostgreSQL, najdete v tématu [příručka k migraci databáze](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).