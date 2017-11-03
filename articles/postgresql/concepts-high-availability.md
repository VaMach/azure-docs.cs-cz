---
title: "Koncepty vysoké dostupnosti v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Toto téma obsahuje informace o vysoké dostupnosti při použití Azure databáze PostgreSQL"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 315eb32a293eec89b274ccd017dba506241533e6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/21/2017
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Koncepty vysoké dostupnosti v databázi Azure pro PostgreSQL
Databáze Azure pro službu PostgreSQL poskytuje zaručené vysokou dostupnost. Finančně zálohovány smlouvu o úrovni služeb (SLA) je 99,99 % při obecné dostupnosti. Není prakticky žádná aplikace výpadek při používání této služby.

## <a name="high-availability"></a>Vysoká dostupnost
Model vysokou dostupnost (HA) je založen na integrovaný mechanismus převzetí služeb při selhání, když dojde k přerušení úrovni uzlu. Přerušení úrovni uzlu mohlo dojít z důvodu selhání hardwaru nebo v reakci na nasazení služby.

Vždy změny PostgreSQL dojít v kontextu transakce. Změny se zaznamenávají synchronně v úložišti Azure, když je transakce potvrzena. Pokud dojde k přerušení úrovni uzlu, serveru databáze automaticky vytvoří nový uzel a připojí ukládání dat do nového uzlu. Žádné aktivní připojení jsou vyřazen a jakékoli aktivních pořadových transakce nejsou potvrzeny.

## <a name="application-retry-logic-is-essential"></a>Logika opakovaných pokusů aplikací je nezbytné
Je důležité, aby PostgreSQL databázové aplikace jsou postaveny ke zjišťování a opakujte vyřadit připojení a transakce se nezdařilo. Při opakování aplikace, připojení aplikace se transparentně přesměruje na nově vytvořit instance, kterou má u nezdařených instancí.

Interně v Azure, brána slouží k přesměrování připojení k nové instanci. Po přerušení celý proces převzetí služeb při selhání obvykle trvá desítkami sekund. Externí připojovací řetězec zůstává stejná pro klientské aplikace.

## <a name="scaling-up-or-down"></a>Škálování nahoru nebo dolů
Podobně jako u HA model, při změně měřítka databázi Azure pro PostgreSQL nahoru nebo dolů, je vytvořena nová instance serveru s po zadanou velikost. Je stávající úložiště dat je odpojený od původní instance a připojena k nové instanci.

Během operace škálování dojde k přerušení připojení databáze. Klientské aplikace jsou odpojené a otevřete nepotvrzené transakce budou zrušeny. Jakmile klientská aplikace opakuje připojení nebo vytvoří nové připojení, přesměruje bránu připojení k nově velikostí instance. 

## <a name="next-steps"></a>Další kroky
- Přehled služby najdete v tématu [databáze Azure pro PostgreSQL – přehled](overview.md)
