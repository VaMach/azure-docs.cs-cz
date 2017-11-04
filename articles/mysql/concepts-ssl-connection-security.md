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
ms.date: 11/02/2017
ms.openlocfilehash: 37e2bbbe1ed4b6a9cca0e0b001e5e632b9b73be2
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Připojení SSL ve službě Azure Database pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení databázový server pro klientské aplikace pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.

## <a name="default-settings"></a>Výchozí nastavení
Ve výchozím nastavení měla by se nakonfigurovat službu databáze vyžadovat připojení SSL při připojování k MySQL.  Doporučujeme, aby se zabránilo zakázání možností protokolu SSL, kdykoli je to možné. 

Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím portálu Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL. 

Připojovací řetězce pro různých programovacích jazyků se zobrazí na portálu Azure. Tyto připojovací řetězce zahrnout požadované parametry protokolu SSL pro připojení k vaší databázi. Na portálu Azure vyberte svůj server. V části **nastavení** záhlaví, vyberte **připojovací řetězce**. Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

Zjistěte, jak povolit nebo zakázat připojení SSL při vývoji aplikace, najdete v tématu [postup konfigurace protokolu SSL](howto-configure-ssl.md). 

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
