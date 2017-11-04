---
title: Synchronizaci dat (Preview) | Microsoft Docs
description: "Tento přehled zavádí synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: 34bc9588745eb24d8b8c2e81389a9e5144497b34
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL

Synchronizaci dat SQL je služba založená na Azure SQL Database, která umožňuje synchronizaci dat, kterou vyberete obousměrně napříč více databází SQL a instance systému SQL Server.

Synchronizace dat je založena kolem koncept synchronizace skupiny. Synchronizace skupiny je skupina databází, které chcete synchronizovat.

Synchronizace skupiny má následující vlastnosti:

-   **Schématu synchronizace** popisuje data, která se synchronizuje.

-   **Směr synchronizace** může být obousměrně nebo můžete pouze v jednom směru toku. To znamená, může být směr synchronizace *rozbočovače na člen* nebo *člena do centra*, nebo obojí.

-   **Interval synchronizace** jak často dochází k synchronizaci.

-   **Zásady překladu IP adres konflikt** je úrovni zásady skupiny, který může být *rozbočovače wins* nebo *člen wins*.

Synchronizaci dat používá k synchronizaci dat hvězdicové topologii. Definujte jedna z databází ve skupině jako databázi rozbočovače. Zbytek databáze jsou databází člena. Synchronizace nastávají jenom mezi rozbočovače a jednotlivé členy.
-   **Rozbočovače databáze** musí být databáze SQL Azure.
-   **Databází člena** může být databáze SQL, databáze systému SQL Server pro místní nebo instance systému SQL Server na virtuálních počítačích Azure.
-   **Databáze Sync** obsahuje metadata a protokolu pro synchronizaci dat. Synchronizace databáze musí být, že Azure SQL Database umístěný ve stejné oblasti jako databázi rozbočovače. Databáze Sync je zákazník vytvořit a zákazníka, které jsou ve vlastnictví.

> [!NOTE]
> Pokud používáte databázi na místní, budete muset [konfigurace místního agenta.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)

![Synchronizaci dat mezi databázemi](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Kdy používat synchronizaci dat

Synchronizace dat je užitečné v případech, kde data musí být pořád aktuální napříč několika databází SQL Azure nebo databáze systému SQL Server. Zde jsou nejčastěji se využívá případů pro synchronizaci dat:

-   **Synchronizace dat hybridní:** se synchronizací dat, abyste zajistili, že data synchronizovat mezi vaší místní databáze a databáze SQL Azure povolit hybridní aplikace. Tato funkce může odvolat pro zákazníky, kteří jsou s přechodu do cloudu a chcete uvést některé své aplikace do Azure.

-   **Distribuované aplikace:** v mnoha případech je vhodné k rozdělení různé úlohy do různých databází. Například pokud máte velké provozní databáze, ale také muset spustit úlohy vytváření sestav nebo analýza tato data, je vhodné mít druhý databázi pro tento další úlohy. Tento postup minimalizuje dopad na výkon na vaše produkční úlohy. Můžete používat synchronizaci dat aby tyto dva databáze synchronizovány.

-   **Globálně distribuované aplikace:** mnoho firem rozmístěny v několika oblastech a i několika zemích. Chcete-li minimalizovat latence sítě, je vhodné vaše data v oblasti blízko vás. S synchronizaci dat se snadnou vejdou databází v oblastech po celém světě synchronizovány.

Není doporučeno synchronizaci dat pro následující scénáře:

-   Zotavení po havárii

-   Škálování pro čtení

-   ETL (OLTP na OLAP)

-   Migrace ze systému SQL Server pro místní databáze Azure SQL

## <a name="how-does-data-sync-work"></a>Jak funguje synchronizaci dat? 

-   **Sledování změn dat:** synchronizaci dat sleduje změny pomocí vložit, aktualizovat a odstranit aktivační události. Změny se zaznamenávají v tabulce straně v uživatelské databázi.

-   **Synchronizace dat:** synchronizaci dat slouží v hvězdicové modelu. Rozbočovače synchronizuje s každý člen jednotlivě. Změny z centra, se stáhnou do člen a pak změny z člen odeslány do rozbočovače.

-   **Řešení konfliktů:** synchronizaci dat nabízí dvě možnosti pro řešení konfliktů *rozbočovače wins* nebo *člen wins*.
    -   Pokud vyberete *rozbočovače wins*, změny v centru vždy přepsat změny v člena.
    -   Pokud vyberete *člen wins*, změny v změn přepsání člena v centru. Pokud existuje více než jednoho člena, konečná hodnota závisí na člena, který synchronizuje nejdřív.

## <a name="limitations-and-considerations"></a>Omezení a důležité informace

### <a name="performance-impact"></a>Vliv na výkon
Synchronizace dat se používají vložit, aktualizovat a odstranit aktivačních událostí ke sledování změn. Vytvoří straně tabulky v databázi uživatelů pro sledování změn. Tyto aktivity sledování změn mají vliv na vaše zatížení databáze. Vyhodnocení vašeho vrstvy služeb a upgradujte v případě potřeby.

### <a name="eventual-consistency"></a>Konzistence typu případné
Vzhledem k tomu, že synchronizace dat je na základě aktivační události, není zaručena konzistence transakcí. Microsoft zaručuje, že jsou všechny změny provedené nakonec a synchronizaci dat není způsobit ztrátu dat.

### <a name="unsupported-data-types"></a>Nepodporované datové typy

-   FileStream

-   UDT SQL/CLR

-   Kolekci XMLSchemaCollection (XML podporována)

-   Kurzor, časové razítko, Hierarchyid

### <a name="requirements"></a>Požadavky

-   Každá tabulka musí mít primární klíč. Neměnit hodnotu primární klíč v některém z řádků. Pokud máte k tomu, odstranit řádek a znovu ji vytvořte s novou hodnotu primárního klíče. 

-   Tabulka nemůže obsahovat sloupec identity, který není primární klíč.

-   Názvy objektů (databáze, tabulek a sloupců) nesmí obsahovat tisknutelná znaků tečkou (.), zbývající hranaté závorky ([), nebo právo hranatá závorka (]).

-   Musí být povolena izolace snímku. Další informace najdete v tématu [izolaci snímku v systému SQL Server](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="limitations-on-service-and-database-dimensions"></a>Omezení dimenzí služby a databáze

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| **Dimenze**                                                      | **Limit**              | **Alternativní řešení**              |
| Maximální počet skupin synchronizace všechny databáze může patřit do.       | 5                      |                             |
| Maximální počet koncových bodů ve skupině jedním synchronizačním              | 30                     | Vytvoření více skupin synchronizace |
| Maximální počet koncových bodů místně v jednom synchronizace skupiny. | 5                      | Vytvoření více skupin synchronizace |
| Databáze, tabulky, schémat a sloupce                       | 50 znaků na název |                             |
| Tabulky ve skupině pro synchronizaci                                          | 500                    | Vytvoření více skupin synchronizace |
| Sloupce v tabulce v skupiny synchronizace                              | 1000                   |                             |
| Velikost řádek dat v tabulce                                        | 24 mb                  |                             |
| Interval minimální synchronizace                                           | 5 minut              |                             |

## <a name="common-questions"></a>Nejčastější dotazy

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Jak často můžete synchronizaci dat synchronizovat data? 
Minimální frekvence se každých pět minut.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Můžete používat synchronizaci dat k synchronizaci mezi místní databáze systému SQL Server pouze? 
Ne přímo. Pro synchronizaci mezi místní databáze systému SQL Server nepřímo, ale vytvoření centra databáze v Azure a následným přidáním do skupiny synchronizace místní databáze.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Lze používat synchronizaci dat na počáteční hodnoty data z provozní databáze pro prázdnou databázi a pak je synchronizovat zachovat? 
Ano. Vytvoření schématu ručně v nové databáze pomocí skriptování z původní. Jakmile vytvoříte schéma, přidáte do skupiny synchronizace zkopírovat data a udržovat synchronizované tabulky.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Proč se zobrazuje, tabulky, které I nevytvořila?  
Synchronizaci dat vytváří straně tabulky v databázi pro sledování změn. Neodstraňujte ho nebo synchronizaci dat přestane fungovat.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Chybová zpráva, která je uvedená "nelze vložit hodnoty NULL do sloupce \<sloupec\>. Sloupec nepovoluje hodnoty Null." Co to znamená, a jak můžete opravit chyby? 
Tato chybová zpráva indikuje mezi dvěma následující problémy:
1.  Může být tabulky bez primárního klíče. Chcete-li tento problém vyřešit, přidejte primární klíč pro všechny tabulky, které se synchronizuje.
2.  Může být klauzule WHERE v příkazu CREATE INDEX. Synchronizace nezpracovává tuto podmínku. Chcete-li tento problém vyřešit, odeberte klauzuli WHERE nebo ručně změnit všechny databáze. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Jak synchronizaci dat zpracovat. cyklické odkazy? To znamená, když stejná data je synchronizovaný v několika skupinách pro synchronizaci a udržuje v důsledku změny?
Synchronizaci dat nemůže pracovat. cyklické odkazy. Ujistěte se, že je vyhnout. 

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Jak můžete exportovat a importovat databáze se synchronizací dat?
Jakmile vyexportovat databázi do souboru .bacpac a importovat, a vytvořit novou databázi, je nutné provést následující dva kroky můžete používat synchronizaci dat v databázi nové:
1.  Vyčištění objektů synchronizaci dat a straně tabulek na **novou databázi** pomocí [tento skript](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Tento skript odstraní všechny požadované objekty synchronizaci dat z databáze.
2.  Znovu vytvořte skupiny synchronizace s novou databází. Pokud původní skupiny synchronizace již nepotřebujete, odstraňte jej.

## <a name="next-steps"></a>Další kroky

Další informace o synchronizaci dat SQL najdete v tématu:

-   [Začínáme s synchronizaci dat SQL](sql-database-get-started-sql-data-sync.md)

-   Dokončete příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL:
    -   [Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Další informace o databázi SQL najdete v tématu:

-   [Databáze SQL – přehled](sql-database-technical-overview.md)

-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
