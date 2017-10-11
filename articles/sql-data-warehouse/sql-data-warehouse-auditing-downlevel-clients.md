---
title: "Podpora klientů nižší úrovně SQL Data Warehouse data auditování | Microsoft Docs"
description: "Další informace o podpoře klientů nižší úrovně SQL Data Warehouse pro auditování dat"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>SQL Data Warehouse – podpora klientů nižší úrovně pro auditování a dynamické maskování dat
[Auditování](sql-data-warehouse-auditing-overview.md) funguje s klienty SQL, které podporují přesměrování TDS.

Jakýkoli klient, který implementuje TDS 7.4 také podporuje přesměrování. Výjimky zahrnují JDBC 4.0, ve kterém funkci přesměrování není plně podporována a Tedious pro Node.JS, ve které přesměrování nebyla implementována.

Pro "Klienty nižší úrovně" tj. by měl být které podporu TDS 7.3 a pod - server plně kvalifikovaný název domény pro připojení řetězec verze upraven:

Původní server plně kvalifikovaný název domény v připojovacím řetězci: <*název serveru*>. database.windows.net

Upravené plně kvalifikovaný název domény serveru v připojovacím řetězci: <*název serveru*> .database. **zabezpečené**. windows.net

Částečný seznam "Klienty nižší úrovně" zahrnuje:

* Rozhraní .NET 4.0 a níže,
* ODBC 10.0 a níže.
* JDBC (při JDBC podporuje TDS 7.4, TDS přesměrování funkce není podporována plně)
* Zdlouhavé (pro platformu Node.JS)

**Poznámka:** výše uvedeném serveru úpravy plně kvalifikovaného názvu domény může být užitečné také pro použití zásad auditování na úrovni SQL serveru bez potřebu konfiguraci krok v každé databázi (dočasné zmírnění).     

