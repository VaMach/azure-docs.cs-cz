---
title: "Vytvářet výstrahy, SQL Database pomocí portálu Azure | Microsoft Docs"
description: "Použití portálu Azure k vytvoření databáze SQL výstrahy, které můžete aktivovat oznámení nebo automatizace při splnění zadané podmínky."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: fd21c9b5e573ac6a47fef88c2a9d31c52618ecb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Použijte portál Azure k vytvoření výstrahy pro Azure SQL Database a datového skladu

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak nastavit výstrahy Azure SQL Database a datového skladu pomocí portálu Azure. Tento článek také obsahuje osvědčené postupy pro nastavení výstrah tečky.    

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** -výstrahy aktivuje, když hodnota zadané metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při nejprve je splněna podmínka, a pak později, pokud podmínka je už plněny.    
* **Aktivity protokolu události** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k překročení určitého počtu událostí.

Můžete nakonfigurovat výstrahu při aktivaci, proveďte následující:

* odesílat oznámení e-mailu Správce služeb a spolusprávci
* Odeslat e-mail na další e-mailů, které zadáte.
* Volat webhook, jehož

Můžete nakonfigurovat a získat informace o použití pravidla výstrah

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Vytvořit pravidlo výstrahy na metrika pomocí portálu Azure
1. V [portál](https://portal.azure.com/), vyhledejte prostředků, které vás zajímají monitorování a vyberte ho.
2. Tento krok je jiný pro databázi SQL a elastické fondy a datovým Skladem SQL: 

   - **Databáze SQL & pouze elastické fondy**: vyberte **výstrahy** nebo **výstrah pravidla** části monitorování. Text a ikona se mohou mírně lišit pro různé prostředky.  
   
     ![Monitorování](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **SQL DW pouze**: vyberte **monitorování** části Běžné úlohy. Klikněte **DWU využití** grafu.

     ![BĚŽNÉ ÚLOHY](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Vyberte **přidat upozornění** příkazů a vyplňte příslušná pole.
   
    ![Přidání oznámení](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.
5. Vyberte **metrika** chcete monitorovat, a potom vyberte **podmínku** a **prahová hodnota** hodnoty pro metriku. Taky **období** času, který metriky pravidlo je nutné splnit před výstrahy aktivačních událostí. Tak například, pokud používáte období "PT5M" a upozornění hledá procesoru vyšší než 80 %, výstraha aktivuje při **průměrná** procesoru 80 % přesahoval 5 minut. Jakmile dojde k první aktivační událost, se znovu spustí, když průměrné využití procesoru je nižší než 80 % více než 5 minut. Procesor měření dochází každých 1 minuta. V následující tabulce najdete podporované časových oken a typ agregace, které každý výstrahy používá ne všechny výstrahy použít průměrnou hodnotu.   
6. Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci se má při aktivuje výstrahu.
7. Pokud chcete další e-mailů příjmu oznámení, když se aktivuje výstraha, přidejte je do **email(s) další správce** pole. Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*
8. Umístit do platný identifikátor URI, **Webhooku** pole, pokud chcete, aby je volána, když se aktivuje výstrahu.
9. Vyberte **OK** po dokončení vytvoření výstrahy.   

Během několika minut výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="managing-your-alerts"></a>Správa upozornění
Po vytvoření výstrahy, můžete ji vybrat a:

* Zobrazte graf zobrazující prahovou hodnotu metriky a skutečnými hodnotami z předchozí den.
* Upravit nebo odstranit.
* **Zakázat** nebo **povolit** ji, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.


## <a name="sql-database-alert-values"></a>Hodnoty výstrah databáze SQL

| Typ prostředku | Název metriky | Popisný název | Typ agregace | Minimální výstrahy časový interval|
| --- | --- | --- | --- | --- |
| Databáze SQL | cpu_percent | Procento CPU | Průměr | 5 minut |
| Databáze SQL | physical_data_read_percent | Procento datových V/V | Průměr | 5 minut |
| Databáze SQL | log_write_percent | Procento vstupně-výstupní operace protokolu | Průměr | 5 minut |
| Databáze SQL | dtu_consumption_percent | Procento DTU | Průměr | 5 minut |
| Databáze SQL | Úložiště | Velikost celkový databáze | Maximální počet | 30 minut |
| Databáze SQL | connection_successful | Úspěšné připojení | Celkem | 10 minut |
| Databáze SQL | connection_failed | Neúspěšné připojení | Celkem | 10 minut |
| Databáze SQL | blocked_by_firewall | Blokováno bránou Firewall | Celkem | 10 minut |
| Databáze SQL | zablokování | Zablokování | Celkem | 10 minut |
| Databáze SQL | storage_percent | Procento velikosti databáze | Maximální počet | 30 minut |
| Databáze SQL | xtp_storage_percent | Percent(Preview) úložiště OLTP v paměti | Průměr | 5 minut |
| Databáze SQL | workers_percent | Procento pracovních procesů | Průměr | 5 minut |
| Databáze SQL | sessions_percent | Procento relací | Průměr | 5 minut |
| Databáze SQL | dtu_limit | Omezení jednotek DTU | Průměr | 5 minut |
| Databáze SQL | dtu_used | Použít DTU | Průměr | 5 minut |
||||||
| Elastický fond | cpu_percent | Procento CPU | Průměr | 10 minut |
| Elastický fond | physical_data_read_percent | Procento datových V/V | Průměr | 10 minut |
| Elastický fond | log_write_percent | Procento vstupně-výstupní operace protokolu | Průměr | 10 minut |
| Elastický fond | dtu_consumption_percent | Procento DTU | Průměr | 10 minut |
| Elastický fond | storage_percent | Procento úložiště | Průměr | 10 minut |
| Elastický fond | workers_percent | Procento pracovních procesů | Průměr | 10 minut |
| Elastický fond | eDTU_limit | omezení eDTU | Průměr | 10 minut |
| Elastický fond | storage_limit | Limit úložiště | Průměr | 10 minut |
| Elastický fond | eDTU_used | eDTU použít | Průměr | 10 minut |
| Elastický fond | storage_used | Využité úložiště | Průměr | 10 minut |
||||||               
| Datový sklad SQL | cpu_percent | Procento CPU | Průměr | 10 minut |
| Datový sklad SQL | physical_data_read_percent | Procento datových V/V | Průměr | 10 minut |
| Datový sklad SQL | Úložiště | Velikost celkový databáze | Maximální počet | 10 minut |
| Datový sklad SQL | connection_successful | Úspěšné připojení | Celkem | 10 minut |
| Datový sklad SQL | connection_failed | Neúspěšné připojení | Celkem | 10 minut |
| Datový sklad SQL | blocked_by_firewall | Blokováno bránou Firewall | Celkem | 10 minut |
| Datový sklad SQL | service_level_objective | Cíl na úrovni služby databáze | Celkem | 10 minut |
| Datový sklad SQL | dwu_limit | dwu limit | Maximální počet | 10 minut |
| Datový sklad SQL | dwu_consumption_percent | Procento DWU | Průměr | 10 minut |
| Datový sklad SQL | dwu_used | DWU použít | Průměr | 10 minut |
||||||


## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) včetně typy informací, můžete sledovat a shromažďovat.
* Další informace o [konfigurace webhooky ve výstrahách](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Získat [přehled diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.
* Získat [přehled metriky kolekce](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.
