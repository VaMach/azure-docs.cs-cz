---
title: "Začínáme s nástroje elastické databáze | Microsoft Docs"
description: "Základní vysvětlení funkce nástroje elastické databáze Azure SQL Database, včetně Snadné spuštění ukázkové aplikace."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: 600334c58ce62a1e53e8a57dd1566bd211249164
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Začínáme s nástroje elastické databáze
Tento dokument vás seznámí s možnosti vývojáře díky čemuž spuštění ukázkové aplikace. Ukázka vytvoří jednoduchou aplikaci horizontálně dělené a jsou zde popsány klíčových funkcí nástroje elastické databáze. Ukázka ukazuje funkce [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md).

Chcete-li nainstalovat knihovny, přejděte na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Knihovny se instaluje s ukázkovou aplikaci, která je popsána v následující části.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2012 nebo novějším s C#. Stáhněte si bezplatnou verzi na [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 nebo vyšší. Chcete-li získat nejnovější verzi, přečtěte si téma [instalace NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-the-sample-app"></a>Stažení a spuštění ukázkové aplikace
**Elastické databáze nástroje pro Azure SQL – Začínáme** ukázkovou aplikaci znázorňuje nejdůležitějších aspektů vývojového prostředí pro horizontálně dělenou aplikace, které používejte nástroje elastické databáze. Zaměřuje se na případů použití klíče pro [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md), [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md), a [víc horizontálních dotazování](sql-database-elastic-scale-multishard-querying.md). Ke stažení a spuštění vzorového, postupujte takto: 

1. Stažení [elastické databáze nástroje pro Azure SQL – ukázka Začínáme](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) z webu MSDN. Rozbalte vzorku, který se umístění, které zvolíte.

2. Chcete-li vytvořit projekt, otevřete **ElasticScaleStarterKit.sln** řešení z **C#** adresáře.

3. V řešení pro ukázkový projekt, otevřete **app.config** souboru. Potom postupujte podle pokynů v souboru a přidejte název serveru Azure SQL Database a vaše přihlašovací údaje (uživatelské jméno a heslo).

4. Sestavte a spusťte aplikaci. Po zobrazení výzvy, povolte pro obnovování balíčků NuGet, řešení Visual Studio. Tím se stáhne nejnovější verzi klientské knihovny pro elastické databáze z NuGet.

5. Experimentovat s různými možnostmi Další informace o možnostech knihovny klienta. Poznamenejte si kroky, které aplikace provede v konzole pro výstup a Nebojte se prozkoumat kód na pozadí.
   
    ![Průběh][4]

Blahopřejeme k – jste úspěšně vytvořen a spuštění vaší první horizontálně dělené aplikace pomocí nástroje elastické databáze v databázi SQL. Pomocí sady Visual Studio nebo SQL Server Management Studio k připojení k vaší databázi SQL a rychle zobrazit v horizontálních oddílů, vytvořené v ukázce. Všimnete nové ukázkové databáze horizontálního oddílu a databázi správce mapy horizontálního oddílu, které vytvořil vzorku.

> [!IMPORTANT]
> Doporučujeme, aby vám zůstaly synchronizované s aktualizacemi a Azure SQL Database vždy použít nejnovější verzi aplikace Management Studio. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-the-code-sample"></a>Klíče kusy ukázka kódu
* **Správa horizontálních oddílů a horizontálního oddílu map**: kód ukazuje, jak pracovat s horizontálních oddílů, rozsahy a mapování v souboru **ShardManagementUtils.cs**. Další informace najdete v tématu [škálování databáze pomocí správce mapy horizontálního oddílu](http://go.microsoft.com/?linkid=9862595).  

* **Závislé na data směrování**: směrování transakce na pravém horizontálního oddílu se zobrazí v **DataDependentRoutingSample.cs**. Další informace najdete v tématu [závislé na Data směrování](http://go.microsoft.com/?linkid=9862596). 

* **Dotazování přes víc horizontálních oddílů**: dotazování napříč horizontálních oddílů je zobrazená v souboru **MultiShardQuerySample.cs**. Další informace najdete v tématu [víc horizontálních dotazování](http://go.microsoft.com/?linkid=9862597).

* **Přidání prázdný horizontálních oddílů**: iterativní přidání nové prázdné horizontálních oddílů se provádí pomocí kódu v souboru **CreateShardSample.cs**. Další informace najdete v tématu [škálování databáze pomocí správce mapy horizontálního oddílu](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Jiné operace elastické škálování
* **Rozdělení existující horizontálního oddílu**: poskytuje možnost rozdělení horizontálních oddílů **nástroji pro sloučení rozdělení**. Další informace najdete v tématu [přesouvání dat mezi instancemi cloudu databázemi](sql-database-elastic-scale-overview-split-and-merge.md).

* **Slučování existující horizontálních oddílů**: sloučení horizontálního oddílu jsou také provést pomocí **nástroji pro sloučení rozdělení**. Další informace najdete v tématu [přesouvání dat mezi instancemi cloudu databázemi](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Náklady
Nástroje elastické databáze jsou volné. Při použití nástroje elastické databáze jste neobdrželi jakýchkoli dalších poplatků nad náklady na vaše používání Azure. 

Ukázkové aplikace například vytvoří nové databáze. Náklady na to závisí na edici SQL databáze, kterou zvolíte a Azure využití vaší aplikace.

Informace o cenách najdete v části [podrobnosti o cenách SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Další kroky
Další informace o nástroje elastické databáze najdete na následujících stránkách:

* Ukázky kódu: 
  * [Nástroje pro elastické databáze Azure SQL – Začínáme](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Nástroje pro elastické databáze Azure SQL – integrace Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Pružnost horizontálního oddílu v Centru skriptů](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [elastické škálování oznámení](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [implementace Škálováním na více systémů pomocí horizontálního dělení s Video knihovny klienta elastické databáze](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Kanál 9: [elastické škálování přehled – video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskusní fórum: [fórum Azure SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* K měření výkonu: [čítače výkonu pro správce horizontálního oddílu mapy](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

