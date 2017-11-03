---
title: "Přehled vývoje aplikace databáze pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Zavádí aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: 6a9bd8f88383b5186e470163bc67f9233172fd49
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Přehled vývoje aplikací pro databázi Azure pro databázi MySQL 
Tento článek popisuje aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL. 

> [!TIP]
> Kurz ukazuje, jak na vytvoření serveru, vytvoření brány firewall na serveru, zobrazení vlastností serveru, vytvoření databáze a připojení a dotazování pomocí workbench a mysql.exe, najdete v části [navrhnout první databáze Azure pro databázi MySQL](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Jazyk a platforma
K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy. Můžete najít odkazy na ukázky kódu v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Nástroje
MySQL community verze, kompatibilní s MySQL běžné nástroje pro správu jako je například nástroje Workbench nebo MySQL, jako je například mysql.exe, používá Azure databáze pro databázi MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)a další. Také můžete portál Azure, rozhraní příkazového řádku Azure a rozhraní REST API pro interakci s službu databáze.

## <a name="resource-limitations"></a>Omezení prostředků
Azure databáze pro databázi MySQL spravuje prostředky k dispozici pro server pomocí dvou různých mechanismů: 
- Zásady správného řízení zdrojů.
- Vynucení omezení.

## <a name="security"></a>Zabezpečení
Databáze pro databázi MySQL Azure poskytuje prostředky pro omezení přístupu, ochranu dat, konfigurace uživatelé a role a aktivity sledování pro databázi MySQL.

## <a name="authentication"></a>Authentication
Azure databáze pro databázi MySQL podporuje serveru ověřování uživatelů a přihlášení.

## <a name="resiliency"></a>Odolnost
Dojde-li k přechodné chybě při připojování k databázi MySQL, by měl váš kód opakujte volání. Doporučujeme logika opakovaných pokusů použít stáhnul logiku tak, aby ji není zahlcovat databázi SQL s více klienty najednou opakování.

- Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk podle vašeho výběru v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Správa připojení
Databázová připojení jsou omezené prostředku, proto doporučujeme rozumný použití připojení při přístupu k databázi MySQL můžete dosáhnout lepšího výkonu.
- Přístup k databázi pomocí sdružování připojení nebo trvalé připojení.
- Přístup k databázi pomocí připojení krátkou životnost. 
- Logika opakovaných pokusů pro použití ve vaší aplikaci v místě pokus o připojení k zachycení selhání vyplývající z souběžných připojení bylo dosaženo maximální povolený počet. V logice opakování nastavit chvíli trvat a potom počkejte, než pro náhodné dobu před pokusy o další připojení.