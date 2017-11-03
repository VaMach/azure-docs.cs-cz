---
title: "Postup konfigurace serveru parametry v databáze Azure pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat parametry serveru MySQL v Azure Database pro databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/10/2017
ms.openlocfilehash: 06c7f9f6bd49ebfaf03b04cb6e30b963593bfb35
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Postup konfigurace serveru parametry v Azure Database pro databázi MySQL pomocí portálu Azure

Azure databáze pro databázi MySQL podporuje konfiguraci některé parametry serveru. Toto téma popisuje, jak nakonfigurovat tyto parametry pomocí portálu Azure. Ne všechny parametry serveru můžete upravit. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Přejděte na parametry serveru, na portálu Azure
1. Přihlaste se k portálu Azure a potom vyhledejte vaší databázi Azure pro server databáze MySQL.
2. V části **nastavení** klikněte na tlačítko **parametry serveru** chcete otevřít stránku parametry Server pro databázi Azure pro databázi MySQL.
3. Najděte všechna nastavení, které je potřeba upravit. Zkontrolujte **popis** sloupec pochopit účel a povolené hodnoty. 
4. Klikněte na tlačítko **Uložit** uložte provedené změny.

![Okno Parametry server portálu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Seznam parametrů konfigurovat server

Seznam parametrů podporovaných serveru stále roste. Na kartě Parametry server na portálu Azure a získat definici konfigurace parametry serveru, na základě požadavků vaší aplikace. 

## <a name="nonconfigurable-server-parameters"></a>Parametry Nonconfigurable serveru

Následující parametry nejsou konfigurovatelné a vázané na vaše [cenová úroveň](concepts-service-tiers.md). 

| **Cenová úroveň** | **Fond InnoDB vyrovnávací paměti (MB)** | **Maximální počet připojení** |
| :------------------------ | :-------- | :----------- |
| Základní 50 | 1024 | 50 | 
| Základní 100  | 2560 | 100 | 
| Standardní 100 | 2560 | 200 | 
| Standardní 200 | 5120 | 400 | 
| Standardní 400 | 10240 | 800 | 
| Standardní 800 | 20480 | 1600 |

Výchozí hodnoty parametru další server pro verzi [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) a [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md).
