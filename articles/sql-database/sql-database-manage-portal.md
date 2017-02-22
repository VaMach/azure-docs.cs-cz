---
title: "Správa služby Azure SQL Database pomocí portálu Azure Portal | Dokumentace Microsoftu"
description: "Stručná referenční příručka pro správu relační databáze v cloudu pomocí portálu Azure Portal"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 420b2153f6115dd712d3033e30f11f79b18cd80f
ms.openlocfilehash: be89a2799af3bdc2938f73e3d54f00f81d9ab9cd


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Správa databází služby Azure SQL Database pomocí portálu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

[Azure Portal](https://portal.azure.com/) umožňuje vytvářet, monitorovat a spravovat databáze a servery služby Azure SQL Database. Tento článek obsahuje stručný popis a odkazy na podrobnosti týkající se běžných úloh.

> [!TIP]
> Pokud potřebujete kurz, ve kterém se naučíte vytvořit server, vytvořit bránu firewall založenou na serveru, zobrazit vlastnosti serveru, připojit se pomocí nástroje SQL Server Management Studio, vytvářet dotazy v hlavní databázi, vytvořit vzorovou databázi a prázdnou databázi nebo se připojit pomocí nástroje SQL Server Management Studio a vytvářet dotazy ve vzorové databázi, přejděte na stránku [kurzu Začínáme](sql-database-get-started.md).

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Zobrazení databází, serverů a fondů služby Azure SQL Database
Dostupné služby SQL Database zobrazíte kliknutím na **Další služby** a zadáním **SQL** do vyhledávacího pole:

![SQL Database](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Jak můžu vytvářet nebo prohlížet databáze služby Azure SQL Database?
Kliknutím na **Databáze SQL** otevřete okno **Databáze SQL** a potom klikněte na databázi, se kterou chcete pracovat, nebo klikněte na **+ Přidat** a vytvořte databázi SQL. Podrobnosti najdete v části [Vytvoření databáze SQL během několika minut pomocí portálu Azure Portal](sql-database-get-started.md).

![Databáze SQL](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Jak můžu vytvářet nebo prohlížet servery služby Azure SQL?
Kliknutím na **SQL Servery** otevřete okno **SQL Servery** a potom klikněte na server, se kterým chcete pracovat, nebo klikněte na **+ Přidat** a vytvořte SQL server. Podrobnosti najdete v části [Vytvoření databáze SQL během několika minut pomocí portálu Azure Portal](sql-database-get-started.md).

![SQL servery](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Jak můžu vytvářet nebo prohlížet elastické fondy služby SQL?
Klepnutím na **Elastické fondy SQL** otevřete okno **Elastické fondy SQL** a potom klikněte na fond, se kterým chcete pracovat, nebo klikněte na **+ Přidat** a vytvořte fond. Podrobnosti najdete v tématu [Vytvoření nového elastického fondu pomocí portálu Azure Portal](sql-database-elastic-pool-create-portal.md).

![Elastické fondy SQL](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>Jak můžu aktualizovat nebo zobrazit nastavení databáze SQL?
Pokud chcete zobrazit nebo aktualizovat nastavení databáze, klikněte na požadované nastavení v okně Databáze SQL:

![Nastavení databáze SQL](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Jak můžu najít plně kvalifikovaný název serveru databáze SQL?
Název serveru vaší databáze zobrazíte kliknutím na **Přehled** v okně **Databáze SQL** a vyhledáním názvu serveru:

![Nastavení databáze SQL](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Jak můžu pomocí správy pravidel brány firewall řídit přístup ke svému SQL serveru a databázi?
Pokud chcete zobrazit, vytvořit nebo aktualizovat pravidla brány firewall, klikněte na **Nastavit bránu firewall serveru** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Konfigurace pravidla brány firewall na úrovni serveru ve službě Azure SQL Database pomocí portálu Azure Portal](sql-database-configure-firewall-settings.md).

![pravidla brány firewall](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Jak můžu změnit úroveň služby nebo úroveň výkonu u databáze SQL?
Pokud chcete aktualizovat úroveň služby nebo výkonu u databáze SQL, klikněte na **Cenová úroveň (DTU škálování)** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Změna úrovně služby a úrovně výkonu (cenové úrovně) databáze SQL](sql-database-scale-up.md).

![cenové úrovně](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Jak můžu konfigurovat auditování a detekci hrozeb u databáze SQL?
Pokud chcete u databáze SQL nakonfigurovat auditování a detekci hrozeb, klikněte na **Auditování a detekce hrozeb** v okně **Databáze SQL**. Podrobnosti najdete v tématech [Začínáme s auditováním databází služby SQL Database](sql-database-auditing-get-started.md) a [Začínáme s detekcí hrozeb služby SQL Database](sql-database-threat-detection-get-started.md).

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Jak můžu u databáze SQL nakonfigurovat dynamické maskování dat?
Pokud chcete u databáze SQL nakonfigurovat dynamické maskování dat, klikněte na **Dynamické maskování dat** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Začínáme s dynamickým maskováním dat služby SQL Database](sql-database-dynamic-data-masking-get-started.md).

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Jak můžu u databáze SQL nakonfigurovat transparentní šifrování dat (TDE)?
Pokud chcete u databáze SQL nakonfigurovat transparentní šifrování dat, klikněte na **Transparentní šifrování dat** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Povolení šifrování TDE u databáze pomocí portálu](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Jak můžu zobrazit nebo změnit maximální velikost databáze SQL?
Pokud chcete zobrazit nebo změnit velikost databáze SQL, klikněte na **Velikost databáze** v okně **Databáze SQL**. Maximální velikost databáze můžete aktualizovat změnou úrovně služby nebo výkonu. Podrobnosti najdete v tématu [Změna úrovně služby a úrovně výkonu (cenové úrovně) databáze SQL](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Jak můžu monitorovat a zlepšit výkon databáze SQL?
Pokud chcete monitorovat a zlepšovat výkonové charakteristiky databáze SQL, klikněte na **Přehled výkonu** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Nástroj SQL Database Performance Insight](sql-database-performance.md).

## <a name="how-do-i-configure-geo-replication"></a>Jak můžu nakonfigurovat geografickou replikaci?
Pokud chcete u databáze SQL nastavit geografickou replikaci, klikněte na **Geografická replikace** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Konfigurace geografické replikace u databáze služby Azure SQL Database pomocí portálu Azure Portal](sql-database-geo-replication-portal.md).

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Jak můžu nastavit převzetí služeb při selhání do geograficky replikované databáze SQL?
Pokud chcete nastavit převzetí služeb při selhání do geograficky replikované sekundární databáze, klikněte na **Geografická replikace** v okně **Databáze SQL** a pak klikněte na **Převzetí služeb při selhání**. Podrobnosti najdete v tématu [Zahájení plánovaného nebo neplánovaného převzetí služeb při selhání u služby Azure SQL Database pomocí portálu Azure Portal](sql-database-geo-replication-failover-portal.md).

## <a name="how-do-i-copy-a-sql-database"></a>Jak můžu zkopírovat databázi SQL?
Pokud chcete zkopírovat databázi SQL, klikněte na **Kopírovat** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Kopírování databáze služby Azure SQL Database pomocí portálu Azure Portal](sql-database-copy-portal.md).

![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Jak můžu archivovat databázi služby Azure SQL Database do souboru BACPAC?
Pokud chcete vytvořit archiv BACPAC s databází SQL, klikněte na **Exportovat** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Archivace databáze služby SQL Azure Database do souboru BACPAC pomocí portálu Azure Portal](sql-database-export.md).

![Export databáze SQL](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Jak můžu obnovit databázi SQL k předchozímu bodu v čase?
Pokud chcete obnovit databázi SQL, klikněte na **Obnovit** v okně **Databáze SQL**. Podrobnosti najdete v tématu [Obnovení databáze služby Azure SQL Database k předchozímu bodu v čase pomocí portálu Azure Portal](sql-database-point-in-time-restore.md).

![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Jak můžu vytvořit databázi služby Azure SQL Database ze souboru BACPAC?
Pokud chcete vytvořit databázi SQL ze souboru BACPAC, klikněte na **Importovat databázi** v okně **SQL Server**. Podrobnosti najdete v tématu [Import souboru BACPAC k vytvoření databáze služby Azure SQL Database](sql-database-import.md).

![SQL server](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Jak můžu obnovit odstraněnou databázi SQL?
Pokud chcete obnovit odstraněnou databázi SQL, klikněte na **Odstraněné databáze** v okně **SQL Server** (u SQL serveru, který obsahoval odstraněnou databázi). Podrobnosti najdete v tématu [Obnovení odstraněné databáze služby Azure SQL Database pomocí portálu Azure Portal](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Jak můžu odstranit databázi SQL?
Pokud chcete odstranit databázi SQL, klikněte na **Odstranit** v okně **Databáze SQL**. 

![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Další zdroje
* [SQL Database](sql-database-technical-overview.md)
* [Monitorování a správa elastického fondu pomocí portálu Azure Portal](sql-database-elastic-pool-manage-portal.md)




<!--HONumber=Dec16_HO3-->


