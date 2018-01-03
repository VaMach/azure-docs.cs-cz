---
title: "Online migrace do databáze Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje postup provedení online migrace extrahováním databázi PostgreSQL do souboru výpisu obnovení databáze PostgreSQL ze souboru archivu vytvořil finanční pg_dump v Azure databázi PostgreSQL a nastavení počáteční zatížení a souvislých synchronizace dat ze zdrojové databáze na cílovou databázi pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: cf17ad87b33ed6196d86cebaf6efe699a542bf39
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-postgresql"></a>Online migrace do databáze Azure pro PostgreSQL
Můžete migrovat existující databáze PostgreSQL databáze Azure pro PostgreSQL pomocí společnosti Attunity replikace pro Microsoft Migrations, společně sponzorovaný, společné nabídky od společnosti Attunity a společnosti Microsoft, která je k dispozici společně se službou Azure databáze migrace bez dalších poplatků zákazníkům společnosti Microsoft. Replikovat společnosti Attunity pro Microsoft Migrations taky umožňuje minimální výpadek při migraci databáze a zdrojové databáze je stále provozní během procesu migrace.

Společnosti Attunity replikace je nástroj replikace dat, která umožňuje synchronizaci dat mezi různé zdroje a cíle, šíření skriptu pro vytváření schémat a data související s každou databázové tabulky. Replikovat společnosti Attunity nešířily všechny artefakty (například SP, triggery, funkce, atd.) nebo převést, například kód PL/SQL hostované v artefaktům T-SQL.

> [!NOTE]
> I když společnosti Attunity replikovat podporuje širokou škálu scénářů migrace, společnosti Attunity replikace pro Microsoft Migrations se zaměřuje na podporu pro určitou dílčí sadu párů zdroj a cíl.

Přehled procesu pro provedení online migrace zahrnuje:

1. **Migraci schématu zdroje PostgreSQL** pro Azure databázi PostgreSQL pomocí [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) a [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) s parametrem - n.

2. **Nastavit počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze a cílová databáze** pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. To proto minimalizuje čas, který zdrojové databáze musí být nastavena jen pro čtení, zatímco připravujete přepínač aplikace na cílovou databázi PostgreSQL na Azure.

Další informace o společnosti Attunity replikace Microsoft Migrations nabídky zobrazte v následujících zdrojích informací:
 - Replikovat společnosti Attunity pro Microsoft Migrations [webové stránky](https://aka.ms/attunity-replicate).
 - [Stáhněte si](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) společnosti Attunity replikace pro Microsoft migrace.
 - Podrobné pokyny k používání společnosti Attunity při migraci z PostgreSQL do databáze Azure pro PostgreSQL, najdete v tématu [příručka k migraci databáze](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).