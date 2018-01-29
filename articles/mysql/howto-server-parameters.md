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
ms.date: 01/25/2018
ms.openlocfilehash: 59eeed42356a276c259bd8da55890b7ada67d729
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Postup konfigurace serveru parametry v Azure Database pro databázi MySQL pomocí portálu Azure

Azure databáze pro databázi MySQL podporuje konfiguraci některé parametry serveru. Tento článek popisuje, jak nakonfigurovat tyto parametry pomocí portálu Azure. Ne všechny parametry serveru můžete upravit. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Přejděte na parametry serveru, na portálu Azure
1. Přihlaste se k portálu Azure a potom vyhledejte vaší databázi Azure pro server databáze MySQL.
2. V části **nastavení** klikněte na tlačítko **parametry serveru** chcete otevřít stránku parametry Server pro databázi Azure pro databázi MySQL.
3. Najděte všechna nastavení, které je potřeba upravit. Zkontrolujte **popis** sloupec pochopit účel a povolené hodnoty. 
4. Klikněte na tlačítko **Uložit** uložte provedené změny.

![Stránka parametry server portálu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Seznam parametrů konfigurovat server

Seznam parametrů podporovaných serveru stále roste. Na kartě Parametry server na portálu Azure a získat definici konfigurace parametry serveru, na základě požadavků vaší aplikace. 

## <a name="nonconfigurable-server-parameters"></a>Parametry Nonconfigurable serveru
InnoDB fondu vyrovnávací paměti a maximální počet připojení nejsou konfigurovatelné a vázanou na vaše [cenová úroveň](concepts-service-tiers.md). 

| **Cenová úroveň** | **Fond InnoDB vyrovnávací paměti (MB)** | **Maximální počet připojení** |
| :------------------------ | :-------- | :----------- |
| Basic 50 | 1024 | 50 | 
| Basic 100  | 2560 | 100 | 
| Standardní 100 | 2560 | 200 | 
| Standard 200 | 5120 | 400 | 
| Standardní 400 | 10240 | 800 | 
| Standard 800 | 20480 | 1600 |

Tyto další server parametry nejsou konfigurovatelné v systému:

|**Parameter**|**Pevná hodnota**|
| :------------------------ | :-------- |
|innodb_file_per_table v základní vrstvě|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512MB|

Další parametry serveru, které zde nejsou uvedeny jsou nastaveny na MySQL out-of-box výchozí hodnoty pro verze [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) a [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Další postup
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md).
