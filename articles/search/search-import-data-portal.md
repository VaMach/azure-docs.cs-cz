<properties
    pageTitle="Import dat do Azure Search pomocí indexerů na webu Azure Portal | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Použití Průvodce importem dat do služby Azure Search na webu Azure Portal k procházení dat ze služby Azure Blob Storage, Table Storage, SQL Database a systému SQL Server na virtuálních počítačích Azure."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="paulettm"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# Import dat do služby Azure Search pomocí portálu

Azure Portal poskytuje v řídicím panelu služby Azure Search průvodce **Importem dat** pro načítání dat do indexu. 

  ![Import dat na panelu příkazů][1]

Průvodce interně konfiguruje a vyvolává *indexer*, čímž automatizuje několik kroků procesu indexování: 

- Připojení k externímu zdroji dat v aktuálním předplatném Azure
- Automatické generování schématu indexu na základě struktury zdroje dat
- Vytváření dokumentů na základě sady řádků získané ze zdroje dat
- Odeslání dokumentů do indexu vaší vyhledávací služby

Tento pracovní postup můžete vyzkoušet v DocumentDB s použitím vzorových dat. Pokyny najdete v tématu [Začínáme se službou Azure Search na webu Azure Portal](search-get-started-portal.md).

## Zdroje dat podporované Průvodcem importem dat

Nástroje a automatizace indexování jsou dostupné pro následující zdroje dat: 

- Azure SQL Database
- Relační data systému SQL Server na virtuálním počítači Azure
- Azure DocumentDB
- Azure Blob Storage (ve verzi Preview)
- Azure Table Storage (ve verzi Preview)

Plochá datová sada je požadovaný vstup. Importovat můžete pouze z jedné tabulky, jednoho zobrazení databáze nebo ekvivalentní datové struktury. Tuto datovou strukturu byste měli vytvořit před spuštěním průvodce.

Všimněte si, že několik indexerů je stále ve verzi Preview, to znamená, že definice indexeru je podpořena rozhraním API ve verzi Preview. Další informace a odkazy najdete v tématu [Přehled indexerů](search-indexer-overview.md).

## Připojení k datům

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) a otevřete řídicí panel služby. Kliknutím na **Vyhledávací služby** na panelu odkazů můžete zobrazit stávající služby v rámci aktuálního předplatného. 

2. Kliknutím na **Import dat** na panelu příkazů otevřete vysouvací okno Import dat.  

3. Klikněte na **Připojit k datům** a zadejte definici zdroje dat používanou indexerem. V případě zdrojů dat v rámci předplatného průvodce obvykle může rozpoznat a přečíst informace o připojení, čímž minimalizuje celkové požadavky na konfiguraci.

| | |
|--------|------------|
|**Stávající zdroj dat** | Pokud již ve vyhledávací službě máte definované indexery, můžete pro další import vybrat stávající definici zdroje dat.|
|**Azure SQL Database** | Název služby, přihlašovací údaje uživatele s oprávněním ke čtení a název databáze můžete zadat na této stránce nebo přes připojovací řetězec technologie ADO.NET. Chcete-li zobrazit nebo přizpůsobit vlastnosti, zvolte možnost připojovacího řetězce. <br/><br/>Na této stránce je třeba určit tabulku nebo zobrazení poskytující sadu řádků. Tato možnost se zobrazí po úspěšném připojení v podobě rozevíracího seznamu, ze kterého můžete vybírat.|
|**SQL Server na virtuálním počítači Azure** | Zadejte plně kvalifikovaný název služby, ID a heslo uživatele a databázi jako připojovací řetězec. Abyste mohli použít tento zdroj dat, je třeba mít v místním úložišti dříve nainstalovaný certifikát šifrující připojení. <br/><br/>Na této stránce je třeba určit tabulku nebo zobrazení poskytující sadu řádků. Tato možnost se zobrazí po úspěšném připojení v podobě rozevíracího seznamu, ze kterého můžete vybírat.
|**DocumentDB** |Požadavky zahrnují účet, databázi a kolekci. Všechny dokumenty v kolekci budou zahrnuty v indexu. Můžete definovat dotaz, který zploští nebo vyfiltruje sadu řádků nebo který rozpozná změněné dokumenty pro následné operace aktualizace dat.|
|**Azure Blob Storage** | Požadavky zahrnují účet úložiště a kontejner. Pokud se názvy objektů blob řídí zásadami virtuálního pojmenovávání pro účely seskupování, můžete volitelně zadat část názvu obsahující virtuální adresář jako složku v kontejneru. Další informace najdete v oddílu [Indexování služby Blob Storage (ve verzi Preview)](search-howto-indexing-azure-blob-storage.md). |
|**Azure Table Storage** | Požadavky zahrnují účet úložiště a název tabulky. Volitelně můžete zadat dotaz pro načtení podmnožiny tabulek. Další informace najdete v oddílu [Indexování služby Table Storage (ve verzi Preview)](search-howto-indexing-azure-tables.md). |

## Přizpůsobení cílového indexu

Předběžný index je obvykle odvozen z datové sady. Přidejte, upravte nebo odstraňte pole a dokončete schéma. Případně nastavte atributy na úrovni pole k určení jeho chování při následném vyhledávání.

1. V části **Přizpůsobit cílový index** zadejte název a **Klíč** sloužící k jedinečné identifikaci každého dokumentu. Hodnota Klíč musí být řetězec. Pokud hodnoty polí obsahují mezery nebo pomlčky, ujistěte se, že v části **Import dat** nastavíte rozšířené možnosti, které pro tyto znaky potlačí kontrolu platnosti.

2. Zkontrolujte a zrevidujte zbývající pole. Název a typ pole jsou obvykle předvyplněné. Můžete změnit typ dat.

3. Pro každé pole nastavte atribut indexu:

 - Retrievable (Zobrazitelné) – vrátí pole ve výsledcích vyhledávání.
 - Filterable (Filtrovatelné) – umožňuje na pole odkazovat ve výrazech filtru.
 - Sortable (Seřaditelné) – umožňuje použití pole při řazení.
 - Facetable (Kategorizovatelné) – povoluje použití pole pro fasetovou navigaci.
 - Searchable (Prohledávatelné) – umožňuje fulltextové vyhledávání.
  
4. Chcete-li určit analyzátor jazyka na úrovni pole, klikněte na kartu **Analyzátor**. V této chvíli lze určit pouze analyzátory jazyka. Použití vlastního analyzátoru nebo nejazykového analyzátoru, jako například analyzátoru klíčových slov, vzoru a dalších, bude vyžadovat psaní kódu.

 - Kliknutím na **Prohledávatelné** určíte fulltextové vyhledávání pole a povolíte rozevírací seznam Analyzátor.
 - Vyberte analyzátor, který chcete. Podrobnosti naleznete v oddílu [Vytvoření indexu pro vícejazyčné dokumenty](search-language-support.md).

5. Kliknutím na **Našeptávač** povolíte pro vybraná pole našeptávání dotazů.


## Import dat

1. V části **Import dat** zadejte název indexeru. Připomínáme, že indexer je výsledkem Průvodce importem dat. Pokud jej později budete chtít zobrazit nebo upravit, místo opětovného spuštění průvodce jej vyberete z portálu. 

2. Zadejte plán založený na časovém pásmu oblasti, ve které je služba zřízená.

3. Nastavením rozšířených možností můžete zadat prahové hodnoty, podle kterých se určí, zda může indexování pokračovat v případě zrušení dokumentu. Případně můžete určit, zda pole **Klíč** mohou obsahovat mezery a lomítka.  

## Úprava existujícího indexeru

Dvojím kliknutím na dlaždici Indexer v řídícím panelu služby vysuňte seznam všech indexerů vytvořených pro vaše předplatné. Dvakrát klikněte na indexery, které chcete spustit, upravit nebo odstranit. Index můžete nahradit jiným existujícím indexem, změnit zdroj dat a nastavit možnosti prahových hodnot chybu během indexování.

## Úprava existujícího indexu

Ve vyhledávání systému Azure budou strukturální aktualizace indexu vyžadovat opětovné vytvoření tohoto indexu, který se skládá z odstranění indexu, opětovného vytvoření indexu a načtení dat. Strukturální aktualizace zahrnují změnu datového typu a přejmenování nebo odstranění pole.

Úpravy, které nevyžadují opětovné sestavení zahrnují přidání nového pole, změnu vyhodnocování profilů, změna navrhovatelů nebo změnu analyzátorů jazyka. Další informace naleznete v [aktualizaci indexu](https://msdn.microsoft.com/library/azure/dn800964.aspx).

## Další krok

Další informace o indexerech najdete na těchto odkazech:

- [Indexování služby Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indexování DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indexování služby Blob Storage (ve verzi Preview)](search-howto-indexing-azure-blob-storage.md)
- [Indexování služby Table Storage (ve verzi Preview)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png




<!--HONumber=sep16_HO1-->


