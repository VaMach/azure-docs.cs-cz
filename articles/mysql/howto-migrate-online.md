---
title: "Minimální dobou výpadku migrace do Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak provést minimální dobou výpadku migraci databáze MySQL do Azure databáze pro databázi MySQL a jak nastavit počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi pomocí společnosti Attunity replikace pro Microsoft Migrace."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Minimální dobou výpadku migrace do Azure Database pro databázi MySQL
Můžete migrovat existující databázi MySQL do databáze Azure pro databázi MySQL pomocí společnosti Attunity replikace pro Microsoft Migrations, společně sponzorovaný, společné nabídky od společnosti Attunity a společnosti Microsoft, která je k dispozici společně se službou Azure databáze migrace na Ne Další náklady na zákazníků společnosti Microsoft. Replikovat společnosti Attunity pro Microsoft Migrations taky umožňuje minimální výpadek při migraci databáze a zdrojové databáze je stále provozní během procesu migrace.

Společnosti Attunity replikace je nástroj replikace dat, která umožňuje synchronizaci dat mezi různé zdroje a cíle, šíření skriptu pro vytváření schémat a data související s každou databázové tabulky. Replikovat společnosti Attunity nešířily všechny artefakty (například SP, triggery, funkce, atd.) nebo převést, například kód PL/SQL hostované v artefaktům T-SQL.

> [!NOTE]
> I když společnosti Attunity replikovat podporuje širokou škálu scénářů migrace, společnosti Attunity replikace pro Microsoft Migrations se zaměřuje na podporu pro určitou dílčí sadu párů zdroj a cíl.

Přehled procesu k provedení migrace minimální dobou výpadku zahrnuje:

1. **Migrace schématu zdroje MySQL** k databázi Azure pro databázi MySQL pomocí [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Nastavení počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi** pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. Díky tomu minimalizuje čas, který zdrojové databáze musí být nastavena jen pro čtení během přípravy k přepínač aplikace na cílovou databázi MySQL na Azure.

Další informace o společnosti Attunity replikace Microsoft Migrations nabídky zobrazte v následujících zdrojích informací:
 - Replikovat společnosti Attunity pro Microsoft Migrations [webové stránky](https://aka.ms/attunity-replicate).
 - [Stáhněte si](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) společnosti Attunity replikace pro Microsoft migrace.
 - Replikovat společnosti Attunity [komunity](https://microsoft.attunity.com/), s úvodní příručku, kurzy a podporu.
 - Podrobné pokyny k používání společnosti Attunity k migraci z databáze MySQL do Azure Database pro databázi MySQL, najdete v tématu [příručka k migraci databáze](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).