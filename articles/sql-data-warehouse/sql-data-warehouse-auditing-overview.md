---
title: "Auditování v Azure SQL Data Warehouse | Microsoft Docs"
description: "Začínáme s auditování v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditování v Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Auditování](sql-data-warehouse-auditing-overview.md)
> * [Detekce hrozeb](sql-data-warehouse-security-threat-detection.md)
> 
> 

Auditování SQL Data Warehouse umožňuje záznam, který protokolují se události v databázi auditování v účtu úložiště Azure. Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení. Auditování SQL Data Warehouse taky integruje s Microsoft Power BI pro procházení analýz a generování sestav.

Nástroje auditování povolit a zajištění dodržování standardů dodržování předpisů, ale nezaručují dodržování předpisů. Další informace o Azure programy dodržování standardů tuto podporu, najdete v části <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zabezpečení Azure</a>.

* [Základy auditování databáze]
* [Nastavení auditování pro vaši databázi]
* [Analýza protokolů auditu a sestavy]

## <a id="subheading-1"></a>Základy Azure SQL Data Warehouse databáze auditování
Auditování databáze SQL Data Warehouse umožňuje:

* **Zachovat** monitorovat vybrané události. Můžete definovat kategorie akce databáze k auditování.
* **Sestava** v databázové aktivitě. Abyste mohli rychle začít s aktivity a události vytváření sestav můžete předem nakonfigurované sestavy a řídicí panel.
* **Analýza** sestavy. Můžete najít podezřelé události, neobvyklé aktivity a trendy.

Můžete nakonfigurovat auditování pro kategorii událostí následující:

**Nešifrovaná SQL** a **parametry SQL** pro které jsou klasifikovány protokolů auditu shromážděných jako  

* **Přístup k datům**
* **Změny schématu (DDL)**
* **Změny dat (DML)**
* **Účty, rolí a oprávnění (DCL)**
* **Uložené procedury**, **přihlášení** a, **transakce správu**.

Pro každou kategorii událostí auditování z **úspěch** a **selhání** operace jsou nakonfigurovány odděleně.

Další informace o aktivitách a auditovat události najdete v tématu <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">odkazu formátu protokolu auditu (soubor dokumentace ke stažení)</a>.

Protokoly auditu jsou uložené v účtu úložiště Azure. Můžete definovat po dobu uchování protokolu auditu.

Zásady pro auditování lze definovat pro konkrétní databázi nebo jako výchozí zásady serveru. Výchozí zásady auditování serveru se vztahuje na všechny databáze na serveru, které nemají konkrétní přepsání databáze auditování definované zásady.

Před nastavením auditování kontrolu, pokud používáte auditu ["Starších klientů."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Nastavení auditování pro vaši databázi
1. Spusťte <a href="https://portal.azure.com" target="_blank">portál Azure</a>.
2. Přejděte na **nastavení** okna SQL Data Warehouse, které chcete auditovat. V **nastavení** vyberte **auditování a detekce hrozeb**.
   
    ![][1]
3. Dál povolte auditování kliknutím **ON** tlačítko.
   
    ![][3]
4. V okně Konfigurace auditování vyberte **podrobnosti úložiště** otevřete okno úložiště protokoly auditu. Vyberte účet úložiště Azure, kam bude uložena protokoly a dobu uchování. 
>[!TIP]
>Použijte stejný účet úložiště pro všechny databáze, auditované k plnému využití mimo šablony předem nakonfigurované sestavy.
   
    ![][4]
5. Klikněte **OK** tlačítko pro uložení podrobnosti o konfiguraci úložiště.
6. V části **protokolování podle událostí**, klikněte na tlačítko **úspěch** a **selhání** protokolovaly všechny události, nebo zvolte kategorií jednotlivých událostí.
7. Pokud konfigurujete auditování pro databázi, musíte změnit připojovací řetězec vašeho klienta zajistit, že data auditování pořízen správně. Zkontrolujte [upravit FDQN serveru v připojovacím řetězci](sql-data-warehouse-auditing-downlevel-clients.md) tématu pro připojení klientů nižší úrovně.
8. Klikněte na **OK**.

## <a id="subheading-3"></a>Analýza protokolů auditu a sestavy
Protokoly auditu je agregován v kolekci úložiště tabulek s **SQLDBAuditLogs** předpony v účtu úložiště Azure, který jste zvolili během instalace. Můžete zobrazit soubory protokolů pomocí některého nástroje, jako třeba <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.

Šablona sestavy předkonfigurované řídicího panelu je k dispozici jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ke stažení tabulku aplikace Excel</a> můžete rychle analyzovat data protokolu. Abyste mohli použít šablonu na vaše protokoly auditu, budete potřebovat Excel 2013 nebo novější a Power Query, kterou si můžete stáhnout <a href="http://www.microsoft.com/download/details.aspx?id=39379">zde</a>.

Šablona má fiktivních ukázkových dat v něm a můžete nastavit Power Query pro import auditní protokol přímo z vašeho účtu úložiště Azure.

## <a id="subheading-4"></a>Opětovné generování klíče úložiště
V produkčním prostředí budete pravděpodobně pravidelně aktualizovat klíče úložiště. Při aktualizaci klíče, je nutné uložit zásadu. Proces je následující:

1. V okně Konfigurace auditování (popsaný výše v instalačním programu auditování část) přepnout **přístupový klíč k úložišti** z *primární* k *sekundární* a **Uložit**.

   ![][4]
2. Přejděte do okna konfigurace úložiště a **znovu vygenerovat** *primární přístupový klíč*.
3. Přejděte zpět do okna Konfigurace auditování, přepněte **přístupový klíč k úložišti** z *sekundární* k *primární* a stiskněte klávesu **Uložit**.
4. Přejděte zpět na úložiště uživatelského rozhraní a **znovu vygenerovat** *sekundární přístupový klíč* (jako příprava pro další aktualizace cyklu klíče.

## <a id="subheading-5"></a>Automatizace (prostředí PowerShell nebo REST API)
Můžete taky nakonfigurovat auditování v Azure SQL Data Warehouse pomocí následujících nástrojů automatizace:

* **Rutiny prostředí PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Odebrat AzureRMSqlDatabaseAuditing][103]
   * [Odebrat AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Použití AzureRMSqlServerAuditingPolicy][107]

<!--Anchors-->
[Základy auditování databáze]: #subheading-1
[Nastavení auditování pro vaši databázi]: #subheading-2
[Analýza protokolů auditu a sestavy]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy