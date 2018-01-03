---
title: "Online migrace do Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak provést online migraci databáze MySQL do Azure Database pro databázi MySQL a jak nastavit počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze na cílovou databázi pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: e3a30510e1b3070bc320faa6071cf546a2024e2f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-mysql"></a>Online migrace do Azure Database pro databázi MySQL
Můžete migrovat existující databázi MySQL do databáze Azure pro databázi MySQL pomocí společnosti Attunity replikace pro Microsoft Migrations, společně sponzorovaný, společné nabídky od společnosti Attunity a společnosti Microsoft, která je k dispozici společně se službou Azure databáze migrace na Ne Další náklady na zákazníků společnosti Microsoft. Replikovat společnosti Attunity pro Microsoft Migrations taky umožňuje minimální výpadek při migraci databáze a zdrojové databáze je stále provozní během procesu migrace.

Společnosti Attunity replikace je nástroj replikace dat, která umožňuje synchronizaci dat mezi různé zdroje a cíle, šíření skriptu pro vytváření schémat a data související s každou databázové tabulky. Replikovat společnosti Attunity nešířily všechny artefakty (například SP, triggery, funkce, atd.) nebo převést, například kód PL/SQL hostované v artefaktům T-SQL.

> [!NOTE]
> I když společnosti Attunity replikovat podporuje širokou škálu scénářů migrace, společnosti Attunity replikace pro Microsoft Migrations se zaměřuje na podporu pro určitou dílčí sadu párů zdroj a cíl.

Přehled procesu pro provedení online migrace zahrnuje:

1. **Migraci schématu zdroje MySQL** pro databázi MySQL pomocí Azure [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Nastavit počáteční zatížení a synchronizaci průběžné dat ze zdrojové databáze a cílová databáze** pomocí společnosti Attunity replikace pro Migrations společnosti Microsoft. To proto minimalizuje čas, který zdrojové databáze musí být nastavena jen pro čtení, zatímco připravujete přepínač aplikace na cílovou databázi MySQL na Azure.

Další informace o společnosti Attunity replikace Microsoft Migrations nabídky zobrazte v následujících zdrojích informací:
 - Replikovat společnosti Attunity pro Microsoft Migrations [webové stránky](https://aka.ms/attunity-replicate).
 - [Stáhněte si](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) společnosti Attunity replikace pro Microsoft migrace.
 - Podrobné pokyny k používání společnosti Attunity k migraci z databáze MySQL do Azure Database pro databázi MySQL, najdete v tématu [příručka k migraci databáze](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).