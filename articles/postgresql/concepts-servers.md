---
title: "Koncepty serveru v databázi Azure pro PostgreSQL"
description: "Tento článek obsahuje důležité informace a pokyny pro konfiguraci a správu databáze Azure pro servery PostgreSQL."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 2db18b014606799bdf5707c4c19f363bbc323e5c
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
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
- Je k dispozici v několika verzích. Další informace najdete v tématu [podporované verze databáze PostgreSQL](concepts-supported-versions.md).
- Je rozšiřitelný uživatelé. Další informace najdete v tématu [PostgreSQL rozšíření](concepts-extensions.md).

V rámci Azure Database pro PostgreSQL server můžete vytvořit jeden nebo více databází. Můžete se rozhodnout vytvořit jednu databázi na server pro využití všech prostředků nebo vytvořit několik databází, které budou prostředky sdílet. Cenách je strukturovaných jednotlivých serverů, na základě konfigurace cenová úroveň, vCores a úložiště (GB). Další informace najdete v tématu [cenové úrovně](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Jak připojit a ověřit k databázi Azure pro PostgreSQL server?
Tyto prvky pomoct zajistit bezpečný přístup k vaší databázi:

|||
|:--|:--|
| **Ověřování a autorizace** | Azure databázi PostgreSQL serveru podporuje nativní PostgreSQL ověřování. Můžete se připojit a ověření serveru s přihlašovací jméno správce serveru. |
| **Protokol** | Služba podporuje protokol na základě zpráv používá PostgreSQL. |
| **TCP/IP** | Protokol je podporována přes TCP/IP a přes sockets Unix domény. |
| **Brána firewall** | Chrání vaše data, pravidlo brány firewall zabrání veškerý přístup k serveru a k jeho databázím, dokud nezadáte, které počítače mají oprávnění. V tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md). |

## <a name="how-do-i-manage-a-server"></a>Jak lze spravovat server?
Databáze Azure pro servery PostgreSQL můžete spravovat pomocí [portál Azure](https://portal.azure.com) nebo [rozhraní příkazového řádku Azure](/cli/azure/postgres).

## <a name="server-parameters"></a>Parametry serveru
Parametry serveru PostgreSQL zjistit konfiguraci serveru. V databázi Azure pro PostgreSQL seznam parametrů lze zobrazit a upravit pomocí portálu Azure nebo Azure CLI. 

Konfigurovat parametry v databáze Azure pro PostgreSQL jako spravované služby pro Postgres, jsou podmnožinou parametrů v místní instanci Postgres (Další informace o parametrech Postgres najdete v tématu [PostgreSQL dokumentace](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). Vaše databáze Azure pro PostgreSQL serveru je povolená s výchozí hodnoty pro každý parametr při vytváření. Parametry, které by vyžadovaly server restartovat nebo superuživatel přístup pro změny se projeví nelze konfigurovat uživatelem.


## <a name="next-steps"></a>Další postup
- Přehled služby najdete v tématu [databáze Azure pro přehled PostgreSQL](overview.md).
- Informace o konkrétní prostředek kvóty a omezení na základě vaší **vrstvy služby**, najdete v části [úrovních služeb](concepts-pricing-tiers.md).
- Informace o připojení ke službě najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md).
- Zobrazit a upravit parametry serveru prostřednictvím [portál Azure](howto-configure-server-parameters-using-portal.md) nebo [rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).
