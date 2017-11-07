---
title: "Koncepty serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Toto téma obsahuje důležité informace a pokyny pro práci s databází Azure pro servery PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 67cf6b133e8e869ee3a157d79d68602760d9137c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="azure-database-for-postgresql-servers"></a>Databáze Azure pro servery PostgreSQL
Tento článek obsahuje důležité informace a pokyny pro práci s databází Azure pro servery PostgreSQL.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Co je Azure databáze PostgreSQL serveru?
Databázi Azure pro PostgreSQL server je centrální administrativní bod pro více databází. Je stejné konstrukce serveru PostgreSQL, který může na světě místní být obeznámeni s. Konkrétně službu PostgreSQL je spravovaný, poskytuje záruku výkonu, zpřístupní přístup a funkce na úrovni serveru.

Databázi Azure pro PostgreSQL server:

- Je vytvořen v rámci předplatného Azure.
- Je nadřazený prostředek pro databáze.
- Poskytuje obor názvů pro databáze.
- Je kontejner s silné životnost sémantiku - odstranění serveru a odstraní databázích s omezením.
- Collocates prostředky v oblasti.
- Poskytuje koncového bodu připojení pro přístup k serveru a databází (. postgresql.database.azure.com).
- Poskytuje oboru pro zásady správy, které se vztahují k jeho databázím: přihlášení, brána firewall, uživatelů, rolí, konfigurace atd.
- Je k dispozici v několika verzích. Další informace najdete v tématu [verze databáze podporována PostgreSQL](concepts-supported-versions.md).
- Je rozšiřitelný uživatelé. Další informace najdete v tématu [PostgreSQL rozšíření](concepts-extensions.md).

V rámci Azure Database pro PostgreSQL server můžete vytvořit jeden nebo více databází. Můžete se rozhodnout vytvořit jednu databázi na server pro využití všech prostředků nebo vytvořit několik databází, které budou prostředky sdílet. Cenách je strukturovaných jednotlivých serverů, na základě konfigurace cenová úroveň, výpočetní jednotky a úložiště (GB). Další informace najdete v tématu [cenové úrovně](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Jak připojit a ověřit k databázi Azure pro PostgreSQL server?
Tyto prvky pomoct zajistit bezpečný přístup k vaší databázi.

|||
| :-- | :-- |
| **Ověřování a autorizace** | Azure databázi PostgreSQL serveru podporuje nativní PostgreSQL ověřování. Můžete se připojit a ověření serveru s přihlašovací jméno správce serveru. |
| **Protokol** | Služba podporuje protokol na základě zpráv používá PostgreSQL. |
| **TCP/IP** | Protokol je podporována přes TCP/IP a přes sockets Unix domény. |
| **Brána firewall** | Chrání vaše data, pravidlo brány firewall zabrání veškerý přístup k serveru a k jeho databázím, dokud nezadáte, které počítače mají oprávnění. V tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Jak lze spravovat server?
Databáze Azure pro servery PostgreSQL můžete spravovat pomocí portálu Azure nebo [rozhraní příkazového řádku Azure](/cli/azure/postgres).

## <a name="next-steps"></a>Další kroky
- Přehled služby najdete v tématu [databáze Azure pro přehled PostgreSQL](overview.md).
- Informace o konkrétní prostředek kvóty a omezení na základě vaší **vrstvy služby**, najdete v části [úrovních služeb](concepts-service-tiers.md).
- Informace o připojení ke službě najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md).
