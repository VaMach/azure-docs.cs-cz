---
title: "Monitorování výkonu databáze ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Seznamte se s možnostmi monitorování vaší databáze pomocí nástrojů Azure a zobrazení dynamické správy."
keywords: "monitorování databáze, výkon cloudové databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/27/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: c0c9d107ff1642d66e96de5409863e4e894d1b6b


---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Monitorování výkonu databáze ve službě Azure SQL Database
Monitorování výkonu databáze SQL v Azure začíná sledováním využití prostředků relativně ke zvolené úrovni výkonu databáze. Monitorování vám pomůže určit, zda má databáze nadbytečnou kapacitu nebo zda má naopak potíže s vyčerpáním prostředků, a podle toho se můžete rozhodnout, zda je třeba změnit úroveň výkonu nebo [úroveň služeb](sql-database-service-tiers.md) vaší databáze. Databázi můžete monitorovat pomocí grafických nástrojů na [portálu Azure](https://portal.azure.com) nebo pomocí [zobrazení dynamické správy SQL](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-the-azure-portal"></a>Monitorování databází na portálu Azure
Na webu [Azure Portal](https://portal.azure.com/) můžete monitorovat využití samostatné databáze tak, že vyberete databázi a kliknete na graf **Monitorování**. Zobrazí se okno **Metrika**, které můžete upravit kliknutím na **Upravit graf**. Přidejte následující metriky:

* Procento CPU
* Procento DTU
* Procento datových V/V
* Procento velikosti databáze

Jakmile přidáte tyto metriky, můžete pokračovat jejich zobrazením v grafu **Monitorování** s dalšími podrobnostmi v okně **Metrika**. Tyto čtyři metriky uvádějí průměrné využití v procentech vzhledem k hodnotě **DTU** vaší databáze. Podrobnosti o jednotkách DTU najdete v článku o [úrovních služeb](sql-database-service-tiers.md).

![Monitorování výkonu databáze v rámci úrovně služeb](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Můžete také nastavit upozornění na výkonové metriky. Klikněte na tlačítko **Přidat upozornění** v okně **Metrika**. Nastavte upozornění podle pokynů průvodce. Můžete určit, zda chcete být upozorněni na překročení zadané prahové hodnoty, nebo naopak když metrika poklesne pod zadanou mez.

Například pokud očekáváte nárůst zatížení databáze, můžete nastavit e-mailové upozornění pro případ, že databáze překročí 80 % kterékoli výkonové metriky. Máte tak možnost předběžného varování, aby byl čas rozhodnout, zda nenastal čas přechodu na vyšší úroveň výkonu.

Metrika výkonu vám také pomůže zjistit, zda je možné úroveň výkonu snížit. Předpokládejme, že používáte databáze S2 v úrovni Standard a všechny metriky ukazují, že databáze v průměru nevyužívá více než 10 % dostupného výkonu. Je pravděpodobné, že databáze bude dobře fungovat i v úrovni Standard S1. Než se však rozhodnete ke snížení úrovně výkonu, zvažte možnost výskytu špiček nebo náhlého kolísání zátěže.

## <a name="monitor-databases-using-dmvs"></a>Monitorování databází pomocí zobrazení dynamické správy
Stejné metriky, které jsou přístupné na portálu, můžete zobrazit také prostřednictvím systémových zobrazení: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) v logické **hlavní** databázi vašeho serveru a [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) v uživatelské databázi. Pomocí zobrazení **sys.resource_stats** můžete sledovat hrubší data v delších časových obdobích. Pomocí zobrazení **sys.dm_db_resource_stats** můžete sledovat data s větším rozlišením a v kratších úsecích. Další informace najdete v tématu [Azure SQL Database – průvodce výkonem](sql-database-performance-guidance.md#monitor-resource-use).

> [!NOTE]
> **Sys.dm_db_resource_stats** vrací prázdný výsledek při použití pro databáze s úrovněmi Web a Business, které jsou již ukončené.
>
>

U elastických fondů můžete monitorovat jednotlivé databáze ve fondu pomocí technik popsaných v této části. Můžete ale také monitorovat fond jako celek. Další informace najdete v tématu [Monitorování a správa elastického fondu](sql-database-elastic-pool-manage-portal.md).



<!--HONumber=Dec16_HO2-->


