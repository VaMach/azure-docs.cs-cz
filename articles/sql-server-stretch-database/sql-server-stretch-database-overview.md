<properties
    pageTitle="Přehled funkce Stretch Database | Microsoft Azure"
    description="Zjistěte, jak funkce Stretch Database migruje vaše historická data transparentně a bezpečně do cloudu Microsoft Azure."
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
    ms.date="05/17/2016"
    ms.author="douglasl"/>

# Přehled funkce Stretch Database

Funkce Stretch Database migruje vaše data transparentně a bezpečně do cloudu Microsoft Azure.

Pokud chcete ihned začít používat funkci Stretch Database, najdete informace v tématu [Začínáme spuštěním průvodce povolením funkce Stretch pro databázi](sql-server-stretch-database-wizard.md).

## Jaké jsou výhody funkce Stretch Database?
Funkce Stretch Database poskytuje následující výhody:

**Poskytuje nákladově efektivní dostupnost pro pomaleji přístupná data** – Díky funkci SQL Server Stretch Database lze dynamicky roztahovat rychleji i pomaleji přístupná data ze systému SQL Server do Microsoft Azure. Na rozdíl od typického úložiště pomaleji přístupných dat jsou vaše data vždy online a k dispozici pro dotazy. Můžete poskytovat delší časové intervaly uchovávání dat i pro rozsáhlé tabulky, jako je třeba historie objednávek zákazníků, aniž by vás to zruinovalo. Využívejte nízké náklady na řešení Azure ve srovnání s místním úložištěm náročným na škálování. Můžete zvolit cenovou úroveň a konfigurovat nastavení na portálu Azure, takže si zachováte kontrolu nad náklady. Škálovat lze podle potřeby nahoru nebo dolů. Na stránce [Ceník funkce SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) najdete podrobnější informace.

**Nevyžaduje změny dotazů ani aplikací** Využívejte bezproblémový přístup ke svým datům v systému SQL Server bez ohledu na to, jestli jsou umístěná lokálně, nebo roztažená do cloudu.  Můžete nastavit zásady, které určují, kde jsou data uložena, a SQL Server provede přesun dat na pozadí. Celá tabulka je neustále online a k dispozici pro dotazy. Funkce Stretch Database navíc nevyžaduje žádné změny existujících dotazů ani aplikací – umístění dat je pro aplikaci zcela transparentní.

**Zjednodušuje údržbu místních dat** – Můžete omezit místní údržbu a zmenšit místní úložiště pro vaše data. Zálohování cloudové části vašich dat probíhá automaticky. Zálohy pro vaše místní data rychleji probíhají rychleji a jsou dokončeny v určeném časové období údržby. Vaše potřeby využití místní úložiště se výrazně snižují. Úložiště Azure může být o 80 % levnější než přidávání na místní disk SSD.

**Zachovává zabezpečení dat i během migrace** – Můžete zůstat v klidu i při zabezpečeném roztahování svých nejdůležitějších aplikací do cloudu. Funkce Always Encrypted systému SQL Server zajišťuje šifrování vašich přesouvaných dat. Zabezpečení na úrovni řádků (RLS) a další pokročilé funkce zabezpečení systému SQL Server dokážou chránit vaše data také v kombinaci s funkcí Stretch Database.

## Co funkce Stretch Database dělá?
Když povolíte funkci Stretch Database pro instanci systému SQL Server, databázi a alespoň jednu tabulku, začne tichá migrace vašich historických dat do Azure.

-   Pokud ukládáte historická data do samostatné tabulky, můžete migrovat celou tabulku.

-   Pokud tabulka obsahuje historická i aktuální data, můžete určit predikát filtru pro výběr řádků, které chcete migrovat.

Funkce Stretch Database zajišťuje, že v případě selhání během migrace nebudou ztracena žádná data. Disponuje také logikou opakování užitečnou při řešení potíží s připojením, které by mohly během migrace nastat. Zobrazení dynamické správy informuje o stavu migrace.

Je možné pozastavit migraci dat kvůli řešení problémů na místním serveru nebo maximalizaci dostupné šířky pásma sítě.

Není třeba měnit existující dotazy a klientské aplikace. I nadále budete mít k dispozici bezproblémový přístup k místním i vzdáleným datům, a to i během migrace dat. V případě vzdálených dotazů se může vyskytnout drobná latence, týká se však pouze dotazů na historická data.

![Přehled funkce Stretch Database][StretchOverviewImage1]

## Je pro vás funkce Stretch Database vhodná?
Pokud se vás týkají následující výroky, funkce Stretch Database vám může usnadnit plnění požadavků a řešení problémů.

|Pokud jste pracovník s rozhodovací pravomocí|Pokud jste správce databáze|
|------------------------------|-------------------|
|Potřebuji uchovávat transakční data po dlouhou dobu.|Začínám ztrácet kontrolu nad velikostí svých tabulek.|
|Někdy potřebuji zadávat dotazy na historická data.|Moji uživatelé prohlašují, že chtějí přístup k historickým datům, ale využívají ho jen zřídka.|
|Používám aplikace, včetně starších aplikací, které nechci aktualizovat.|Potřebuji nakupovat a přidávat další úložiště.|
|Chci najít způsob, jak ušetřit peníze za úložiště.|V rámci smlouvy SLA nemůžu zálohovat nebo obnovovat tak velké tabulky.|

## Jaký druh databází a tabulek je kandidátem pro funkci Stretch Database?
Funkce Stretch Database je určena pro transakční databáze s velkým množstvím historických dat, která jsou obvykle uložena v malém počtu tabulek. Tyto tabulky mohou obsahovat více než miliardu řádků.

Pokud používáte funkci dočasných tabulek systému SQL Server 2016, můžete funkci Stretch Database použít k migraci celé přidružené tabulky historických dat nebo její části do nákladově efektivního úložiště v Azure. Další informace najdete v tématu [Manage Retention of Historical Data in System-Versioned Temporal Tables](https://msdn.microsoft.com/library/mt637341.aspx) (Správa uchovávání historických dat v dočasných tabulkách se systémovou správou verzí).

S využitím Poradce pro funkci Stretch Database, který je funkcí Poradce pro upgrade systému SQL Server 2016, můžete určit databáze a tabulky, pro které má být funkce Stretch Database použita. Další informace najdete v tématu věnovaném [určení databází a tabulek pro funkci Stretch Database](sql-server-stretch-database-identify-databases.md). Další informace o možných blokujících problémech najdete v tématu [Surface area limitations and blocking issues for Stretch Database](sql-server-stretch-database-limitations.md) (Omezení možností útoku a blokující problémy pro funkci Stretch Database).

## <a name="FAQ"></a>Nejčastější dotazy ohledně funkce Stretch Database
**Pracuje funkce Stretch Database s &lt;názvem funkce systému SQL Server&gt;?**
-   Seznam funkcí systému SQL Server, které způsobí, že tabulka není vhodná pro použití funkce Stretch Database, najdete v tématu [Surface area limitations and blocking issues for Stretch Database](sql-server-stretch-database-limitations.md) (Omezení možností útoku a blokující problémy pro funkci Stretch Database).

-   Volitelně můžete stáhnout Poradce pro upgrade systému SQL Server 2016 a spustit Poradce pro funkci Stretch Database, s jehož využitím můžete určit databáze a tabulky, které jsou kandidáty pro použití funkce Stretch Database použita. Další informace najdete v tématu věnovaném [určení databází a tabulek pro funkci Stretch Database](sql-server-stretch-database-identify-databases.md).

**Můžu pro funkci Stretch Database nastavit jako cíl jinou místní instanci systému SQL Server?**
Ne. Funkce Stretch Database nepodporuje jinou místní instanci systému SQL Server jako vzdálený koncový bod.

**Můžu zakázat funkci Stretch Database a přesunout migrovaná data zpět do místní tabulky?**
Ano. Další informace najdete v tématu [Zakázání funkce Stretch Database a vrácení vzdálených dat](sql-server-stretch-database-disable.md).

## Výrazy
**Místní databáze**. Místní databáze systému SQL Server.

**Vzdálený koncový bod**. Umístění v Microsoft Azure obsahující vzdálená data databáze.

**Místní data**. Data v databázi s povolenou funkcí Stretch Database, která nebudou přesunuta do Azure vzhledem ke konfiguraci funkce Stretch Database pro tabulky v databázi.

**Vhodná data**. Data v databázi s povolenou funkcí Stretch Database, která dosud nebyla přesunuta, ale budou přesunuta do Azure vzhledem ke konfiguraci funkce Stretch Database pro tabulky v databázi.

**Vzdálená data**. Data v databázi s povolenou funkcí Stretch Database, která již byla přesunuta do Azure.

## Architektura
Funkce Stretch Database využívá prostředky v Microsoft Azure ke snižování zatížení úložiště archivních dat a zpracování dotazů.

Když pro databázi povolíte funkci Stretch Database, vytvoří se v místním systému SQL Server definice zabezpečeného propojeného serveru. Jako cíl pro tuto definici propojeného serveru je nastaven příslušný vzdálený koncový bod. Když pro tabulku v databázi povolíte funkci Stretch Database, zřídí vzdálené prostředky a začne migrovat příslušná data (pokud je povolená migrace).

Dotazy na tabulky s povolenou funkcí Stretch Database se automaticky spouštějí pro místní databázi a vzdálený koncový bod. Funkce Stretch Database využívá výpočetní výkon v Azure ke spouštění dotazů na vzdálená data tím, že je přepisuje. Toto přepsání je v novém plánu dotazu uvedené jako operátor „vzdálený dotaz“.

![Architektura funkce Stretch Database][StretchOverviewImage2]

## Zabezpečení a oprávnění

### Povolení a zakázání funkce Stretch Database pro instanci SQL Serveru
Když chcete začít konfigurovat databáze pro funkci Stretch Database, musíte nejdřív změnit možnost konfigurace „archivu vzdálených dat“ na úrovni instance pomocí sp\_configure. Tato operace vyžaduje oprávnění správce systému nebo správce serveru. Pokud je tato možnost povolená, můžete konfigurovat databáze pro funkci Stretch Database, migrovat data a zadávat dotazy na data na vzdáleném koncovém bodu.

### Povolení a zakázání funkce Stretch Database pro databázi nebo tabulku
Pokud chcete nakonfigurovat databázi pro funkci Stretch Database, musíte mít oprávnění CONTROL DATABASE. Navíc musíte zadat přihlašovací údaje správce pro vzdálený koncový bod.

Pokud chcete nakonfigurovat tabulku pro funkci Stretch Database, musíte mít pro tuto tabulku oprávnění ALTER a databáze už musí být pro funkci Stretch Database nakonfigurovaná.

### Přístup k datům
K definici propojeného serveru za funkcí Stretch Database mají přístup jenom systémové procesy. Přihlášení uživatele nemohou pomocí definice propojeného serveru vydávat dotazy na vzdálený koncový bod.

Stretch Database nemění model oprávnění stávající databáze. Přihlášení uživatele mohou zadávat dotazy na data v tabulce s funkcí Stretch Database povolenou prostřednictvím místní databáze. Místní databáze provádí pro všechny akce iniciované příslušným uživatelem kontroly oprávnění stejným způsobem jako pro libovolné jiné objekty. Pokud máte oprávnění pro přístup k tabulce s povolenou funkcí Stretch Database, máte přístup k veškerému obsahu, pro který máte oprávnění, bez ohledu na to, kde jsou tato data fyzicky umístěná.

![Model datového přístupu funkce Stretch Database][StretchOverviewImage3]

## Vyzkoušení funkce Stretch Database
**Vyzkoušejte si Stretch Database s ukázkovou databází AdventureWorks.** Pokud chcete získat ukázkovou databázi AdventureWorks, stáhněte si alespoň příslušný soubor databáze a soubor ukázek a skriptů pomocí [tohoto odkazu](https://www.microsoft.com/download/details.aspx?id=49502). Po obnovení ukázkové databáze na instanci SQL Serveru 2016 rozbalte soubor ukázek a otevřete soubor Stretch DB Samples ze složky Stretch DB. Spuštěním skriptů v tomto souboru zkontrolujete místo využité vaší databází před povolením funkce Stretch Database a po ně. Můžete tak sledovat průběh migrace dat a potvrdit, že je možné nadále zadávat dotazy na existující data a vkládat nová data jak během migrace, tak po jejím dokončení.

## Další krok
**Určete databáze a tabulky, které jsou kandidátem pro funkci Stretch Database.** Stáhněte Poradce pro upgrade systému SQL Server 2016 a spustit Poradce pro funkci Stretch Database, s jehož využitím můžete určit databáze a tabulky, které jsou kandidáty pro použití funkce Stretch Database. Poradce pro funkci Stretch Database také odhalí případné problémy s blokováním. Další informace najdete v tématu věnovaném [určení databází a tabulek pro funkci Stretch Database](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Jun16_HO2-->


