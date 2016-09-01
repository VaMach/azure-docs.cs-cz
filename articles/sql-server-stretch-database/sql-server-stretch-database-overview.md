<properties
    pageTitle="Přehled funkce Stretch Database | Microsoft Azure"
    description="Zjistěte, jak funkce Stretch Database migruje vaše studená data transparentně a bezpečně do cloudu Microsoft Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# Přehled funkce Stretch Database

Stretch Database migruje vaše studená data transparentně a bezpečně do cloudu Microsoft Azure.

Pokud chcete ihned začít používat funkci Stretch Database, najdete informace v tématu [Začínáme spuštěním průvodce povolením funkce Stretch pro databázi](sql-server-stretch-database-wizard.md).

## Jaké jsou výhody funkce Stretch Database?
Funkce Stretch Database poskytuje následující výhody:

### Poskytuje efektivní přístup pro studená data
Díky funkci SQL Server Stretch Database lze dynamicky roztahovat teplá i studená data z SQL Serveru do Microsoft Azure. Na rozdíl od typického úložiště pomaleji přístupných dat jsou vaše data vždy online a k dispozici pro dotazy. Můžete poskytovat delší časové intervaly uchovávání dat i pro rozsáhlé tabulky, jako je třeba historie objednávek zákazníků, aniž by vás to zruinovalo. Využívejte nízké náklady na řešení Azure ve srovnání s místním úložištěm náročným na škálování. Můžete zvolit cenovou úroveň a konfigurovat nastavení na portálu Azure, takže si zachováte kontrolu nad náklady. Škálovat lze podle potřeby nahoru nebo dolů. Na stránce [Ceník funkce SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) najdete podrobnější informace.

### Nevyžaduje změny dotazů ani aplikací
Využívejte bezproblémový přístup ke svým datům v SQL Serveru bez ohledu na to, jestli jsou umístěná lokálně, nebo roztažená do cloudu.  Můžete nastavit zásady, které určují, kde jsou data uložena, a SQL Server provede přesun dat na pozadí. Celá tabulka je neustále online a k dispozici pro dotazy. Funkce Stretch Database navíc nevyžaduje žádné změny existujících dotazů ani aplikací – umístění dat je pro aplikaci zcela transparentní.

### Zjednodušuje údržbu místních dat
Můžete omezit místní údržbu a zmenšit místní úložiště pro vaše data. Zálohy pro vaše místní data rychleji probíhají rychleji a jsou dokončeny v určeném časové období údržby. Zálohování cloudové části vašich dat probíhá automaticky. Vaše potřeby využití místní úložiště se výrazně snižují. Úložiště Azure může být o 80 % levnější než přidávání na místní disk SSD.

### Zachovává zabezpečení dat i během migrace
Můžete zůstat v klidu i při zabezpečeném roztahování svých nejdůležitějších aplikací do cloudu. Funkce Always Encrypted systému SQL Server zajišťuje šifrování vašich přesouvaných dat. Zabezpečení na úrovni řádků (RLS) a další pokročilé funkce zabezpečení systému SQL Server dokážou chránit vaše data také v kombinaci s funkcí Stretch Database.

## Co funkce Stretch Database dělá?
Když povolíte funkci Stretch Database pro instanci SQL Serveru, databázi a alespoň jednu tabulku, Stretch Database začne tiše migrovat vaše studená data do Azure.

-   Pokud ukládáte studená data do samostatné tabulky, můžete migrovat celou tabulku.

-   Pokud tabulka obsahuje horká i studená data, můžete určit predikát filtru pro výběr řádků, které chcete migrovat.

**Není třeba měnit existující dotazy a klientské aplikace.** I nadále budete mít k dispozici bezproblémový přístup k místním i vzdáleným datům, a to i během migrace dat. V případě vzdálených dotazů se může vyskytnout drobná latence, týká se však pouze dotazů na studená data.

**Stretch Database zajišťuje, že žádná data nebudou ztracena** v případě selhání během migrace. Disponuje také logikou opakování užitečnou při řešení potíží s připojením, které by mohly během migrace nastat. Zobrazení dynamické správy informuje o stavu migrace.

**Je možné pozastavit migraci dat** kvůli řešení problémů na místním serveru nebo maximalizaci dostupné šířky pásma sítě.

![Přehled funkce Stretch Database][StretchOverviewImage1]

## Je pro vás funkce Stretch Database vhodná?
Pokud se vás týkají následující výroky, funkce Stretch Database vám může usnadnit plnění požadavků a řešení problémů.

|Pokud jste pracovník s rozhodovací pravomocí|Pokud jste správce databáze|
|------------------------------|-------------------|
|Potřebuji uchovávat transakční data po dlouhou dobu.|Začínám ztrácet kontrolu nad velikostí svých tabulek.|
|Někdy potřebuji zadávat dotazy na studená data.|Moji uživatelé prohlašují, že chtějí přístup ke studeným datům, ale využívají ho jen zřídka.|
|Používám aplikace, včetně starších aplikací, které nechci aktualizovat.|Potřebuji nakupovat a přidávat další úložiště.|
|Chci najít způsob, jak ušetřit peníze za úložiště.|V rámci smlouvy SLA nemůžu zálohovat nebo obnovovat tak velké tabulky.|

## Jaký druh databází a tabulek je kandidátem pro funkci Stretch Database?
Funkce Stretch Database je určena pro transakční databáze s velkým množstvím studených dat, která jsou obvykle uložena v malém počtu tabulek. Tyto tabulky mohou obsahovat více než miliardu řádků.

Pokud používáte funkci dočasných tabulek systému SQL Server 2016, můžete funkci Stretch Database použít k migraci celé přidružené tabulky historických dat nebo její části do nákladově efektivního úložiště v Azure. Další informace najdete v tématu [Manage Retention of Historical Data in System-Versioned Temporal Tables](https://msdn.microsoft.com/library/mt637341.aspx) (Správa uchovávání historických dat v dočasných tabulkách se systémovou správou verzí).

S využitím Poradce pro funkci Stretch Database, který je funkcí Poradce pro upgrade systému SQL Server 2016, můžete určit databáze a tabulky, pro které má být funkce Stretch Database použita. Další informace najdete v tématu věnovaném [určení databází a tabulek pro funkci Stretch Database](sql-server-stretch-database-identify-databases.md). Další informace o možných blokujících problémech najdete v tématu [Limitations for Stretch Database](sql-server-stretch-database-limitations.md) (Omezení funkce Stretch Database).

## Vyzkoušení funkce Stretch Database
**Vyzkoušejte si Stretch Database s ukázkovou databází AdventureWorks.** Pokud chcete získat ukázkovou databázi AdventureWorks, stáhněte si alespoň příslušný soubor databáze a soubor ukázek a skriptů pomocí [tohoto odkazu](https://www.microsoft.com/download/details.aspx?id=49502). Po obnovení ukázkové databáze na instanci SQL Serveru 2016 rozbalte soubor ukázek a otevřete soubor Stretch DB Samples ze složky Stretch DB. Spuštěním skriptů v tomto souboru zkontrolujete místo využité vaší databází před povolením funkce Stretch Database a po ně. Můžete tak sledovat průběh migrace dat a potvrdit, že je možné nadále zadávat dotazy na existující data a vkládat nová data jak během migrace, tak po jejím dokončení.

## Další krok
**Určete databáze a tabulky, které jsou kandidátem pro funkci Stretch Database.** Stáhněte Poradce pro upgrade systému SQL Server 2016 a spustit Poradce pro funkci Stretch Database, s jehož využitím můžete určit databáze a tabulky, které jsou kandidáty pro použití funkce Stretch Database. Poradce pro funkci Stretch Database také odhalí případné problémy s blokováním. Další informace najdete v tématu věnovaném [určení databází a tabulek pro funkci Stretch Database](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Aug16_HO4-->


