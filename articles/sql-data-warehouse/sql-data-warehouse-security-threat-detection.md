---
title: "Začínáme s SQL Data Warehouse služba detekce hrozeb"
description: "Jak začít pracovat s detekce hrozeb."
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 7f5dab6936e8cac10ac7a4a7dc4c3be116de5ad5
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-threat-detection"></a>Začínáme s detekce hrozeb.
> [!div class="op_single_selector"]
> * [Auditování](sql-data-warehouse-auditing-overview.md)
> * [Detekce hrozeb](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Přehled
Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální ohrožení databáze. Detekce hrozeb je ve verzi preview a je podporována pro SQL Data Warehouse.

Detekce hrozeb poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Uživatele můžete prozkoumat podezřelé události pomocí [auditování Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v datovém skladu.
Detekce hrozeb zjednodušuje na potenciální hrozby adresu do datového skladu, aniž by museli být odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.

Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL. Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace. Útočníci využít výhod vložit škodlivý příkazy SQL, do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi aplikace ohrožení zabezpečení.

## <a name="set-up-threat-detection-for-your-database"></a>Nastavení detekce hrozeb pro vaši databázi
1. Spuštění portálu Azure v [https://portal.azure.com](https://portal.azure.com).
2. Přejděte do okna konfigurace služby SQL Data Warehouse, kterou chcete sledovat. V okně Nastavení vyberte **auditování a detekce hrozeb**.
   
    ![Navigační podokno][1]
3. V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.
   
    ![Navigační podokno][2]
4. Zapnout **ON** detekce hrozby.
5. Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění neobvyklé datového skladu aktivit.
6. Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** okno Konfigurace a uložení nové nebo aktualizované auditování a zásady detekce hrozby.
   
    ![Navigační podokno][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Prozkoumejte data neobvyklé aktivity skladu při zjištění podezřelé události
1. Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. <br/>
   E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru a čas události. Kromě toho bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze.<br/>
   
    ![Navigační podokno][4]
2. V e-mailu, klikněte na **protokolu auditování databáze SQL Azure** odkaz, který bude spuštění portálu Azure a zobrazit příslušné záznamy v době podezřelé události auditování.
   
    ![Navigační podokno][5]
3. Klikněte na záznamy auditu zobrazíte další podrobnosti v databázi podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.
   
    ![Navigační podokno][6]
4. V okně auditování záznamy, klikněte na tlačítko **otevřete v aplikaci Excel** otevřete předem nakonfigurovaná v aplikaci excel šablony k importu a spuštění hlubší analýzy protokol auditu v době podezřelé události.<br/>
   **Poznámka:** v aplikaci Excel 2010 nebo novější, Power Query a **rychle zkombinovat** nastavení je povinné
   
    ![Navigační podokno][7]
5. Ke konfiguraci **rychle zkombinovat** nastavení - v **POWER QUERY** pásu karet vyberte **možnosti** zobrazíte dialogové okno Možnosti. Vyberte v části o ochraně osobních údajů a vyberte druhou možnost - 'Ignorovat úrovně soukromí a potenciálně tak vylepšit výkon':
   
    ![Navigační podokno][8]
6. Načíst protokoly auditu SQL, zajistěte, aby parametrů v nastavení kartě jsou správně nastavena a poté vyberte pásu karet, Data a klikněte na tlačítko 'aktualizovat vše.
   
    ![Navigační podokno][9]
7. Výsledky se zobrazí v **protokoly auditu SQL** listu, která umožňuje spustit hlubší analýzu neobvyklé aktivity, které byly zjištěny a zmírnit dopad událostí zabezpečení ve vaší aplikaci.

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
