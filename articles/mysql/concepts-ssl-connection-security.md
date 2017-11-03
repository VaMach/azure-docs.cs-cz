---
title: "Připojení SSL pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Informace pro konfiguraci Azure databáze MySQL a přidružené aplikace odpovídajícím způsobem používat připojení SSL"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Připojení SSL ve službě Azure Database pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databázový server pro klientské aplikace pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.

## <a name="default-settings"></a>Výchozí nastavení
Ve výchozím nastavení měla by se nakonfigurovat službu databáze vyžadovat připojení SSL při připojování k MySQL.  Je doporučeno, nezakazujte možností protokolu SSL, kdykoli je to možné. 

Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím portálu Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL. 

Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" v rámci vašeho serveru na portálu Azure, zahrnout požadované parametry pro běžné jazyky pro připojení k databázovému serveru pomocí protokolu SSL. Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

Informace o tom, které chcete povolit nebo zakázat připojení SSL při vývoji aplikací, naleznete v [postup konfigurace protokolu SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
