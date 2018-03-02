---
title: "Připojení SSL pro databázi Azure pro databázi MySQL"
description: "Informace pro konfiguraci Azure databáze MySQL a přidružené aplikace odpovídajícím způsobem používat připojení SSL"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: f59d5eab9772515a3c59f887a48d597d27bab135
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Připojení SSL ve službě Azure Database pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databázový server pro klientské aplikace pomocí Secure Sockets Layer (SSL). Díky vynucování připojení SSL mezi databázovým serverem a klientskými aplikacemi se šifruje datový proud mezi serverem a vaší aplikací, což pomáhá chránit před napadením útočníky, kteří se vydávají za prostředníky.

## <a name="default-settings"></a>Výchozí nastavení
Ve výchozím nastavení měla by se nakonfigurovat službu databáze vyžadovat připojení SSL při připojování k MySQL.  Doporučujeme, aby se zabránilo zakázání možností protokolu SSL, kdykoli je to možné. 

Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím portálu Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL. 

Připojovací řetězce pro různých programovacích jazyků se zobrazí na portálu Azure. Tyto připojovací řetězce zahrnout požadované parametry protokolu SSL pro připojení k vaší databázi. Na portálu Azure vyberte svůj server. V části **nastavení** záhlaví, vyberte **připojovací řetězce**. Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

Zjistěte, jak povolit nebo zakázat připojení SSL při vývoji aplikace, najdete v tématu [postup konfigurace protokolu SSL](howto-configure-ssl.md). 

## <a name="next-steps"></a>Další postup
[Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
