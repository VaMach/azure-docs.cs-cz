---
title: Začínáme se službou Azure Search | Microsoft Docs
description: Zjistěte, jak vytvořit první index služby Azure Search pomocí tohoto kurzu a ukázkových dat DocumentDB. Toto cvičení bez kódu založené na portálu využívá Průvodce importem dat.
services: search
documentationcenter: ''
author: HeidiSteen
manager: jhubbard
editor: ''
tags: azure-portal

ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 10/03/2016
ms.author: heidist

---
# Začínáme s Azure Search v portálu
Tento úvod bez kódu vám pomůže začít s Microsoft Azure Search pomocí funkcí integrovaných přímo v portálu. 

Kurz předpokládá využití [ukázkové databáze Azure DocumentDB](#apdx-sampledata), kterou lze jednoduše vytvořit pomocí našich dat a pokynů, ale můžete také přizpůsobit tyto kroky vaším existujícím datům v DocumentDB nebo SQL Database.

> [!NOTE]
> Tento úvodní kurz vyžaduje [předplatné Azure](/pricing/free-trial/?WT.mc_id=A261C142F) a [službu Azure Search](search-create-service-portal.md). 
> 
> 

## Vyhledání služby
1. Přihlaste se k webu [Portál Azure](https://portal.azure.com).
2. Otevřete řídící panel služby Azure Search. Zde je několik způsobů, jak najít řídicí panel.
   
   * Na panelu vlevo klikněte na **Vyhledávací služby**. Panel vlevo obsahuje seznam všech služeb zřízených v rámci vašeho předplatného. Pokud byla definována vyhledávací služba, zobrazuje se v seznamu položka **Služby vyhledávání**.
   * Na panelu vlevo klikněte na **Procházet** a potom zadejte do pole hledání termín „search“, abyste vytvořili seznam všech vyhledávacích služeb vytvořených v rámci vašich předplatných.

## Kontrola místa
Mnoho zákazníků začíná s bezplatnou službou. Tato verze je omezená na tři indexy, tři zdroje dat a tři indexery. Než začnete, ujistěte se, že máte místo pro další položky. Tento návod vytváří od každého objektu jeden.

## Vytvoření indexu a načtení dat
Vyhledávací dotazy provádějí iterace *indexu* obsahujícího data s možností vyhledávání, metadata a konstrukce používané k optimalizaci určitého chování vyhledávání. Jako první krok nadefinujte a naplňte index.

Existuje několik způsobů vytvoření indexu. Pokud jsou vaše data v úložišti, které může procházet služba Azure Search, jako je například SQL Azure Database, SQL Server na virtuálním počítači Azure nebo DocumentDB, můžete vytvořit a naplnit index velmi snadno pomocí *indexeru*.

Aby bylo možné provést tuto úlohu z portálu, použijeme data služby DocumentDB, která lze procházet pomocí indexeru prostřednictvím **Průvodce importem dat**. 

Než budete pokračovat, vytvořte [ukázkovou databázi DocumentDB](#apdx-sampledata), kterou použijeme v tomto kurzu, a pak se vraťte do této části a proveďte následující postup.

<a id="defineDS"></a>

#### Krok 1: Definování zdroje dat
1. Na řídicím panelu služby Azure Search klikněte na panelu příkazů na **Importovat data** a spusťte průvodce, který vytvoří a naplní index.
   
    ![][7]
2. V průvodci klikněte na **Zdroj dat** > **DocumentDB** > **Název** a zadejte název zdroje dat. Zdroj dat je objekt připojení ve službě Azure Search, které lze použít s jinými indexery. Jakmile ho vytvoříte, bude ve službě k dispozici jako „stávající zdroj dat“.
3. Zvolte existující účet DocumentDB, databázi a kolekci. Používáte-li ukázková data, která poskytujeme, vaše definice zdroje dat vypadá například takto:
   
    ![][2]

Všimněte si, že jsme přeskočili dotaz. To je proto, že nyní neimplementujeme sledování změn v naší datové sadě. Pokud vaše datová sada obsahuje pole, které uchovává informace o čase aktualizace záznamu, můžete nakonfigurovat indexer Azure Search, aby používal sledování změn pro selektivní aktualizace indexu.

Kliknutím na **OK** dokončíte tento krok průvodce.

#### Krok 2: Definování indexu
Stále v průvodci klikněte na **Index** a podívejte se na návrhovou plochu, která se používá k vytvoření indexu Azure Search. Index vyžaduje minimálně název a kolekci polí s jedním polem označeným jako klíč dokumentu. Vzhledem k tomu, že používáme datovou sadu DocumentDB, průvodce pole automaticky rozpozná a do indexu se předem načtou pole a přiřazení datových typů. 

  ![][3]

I když jsou pole a datové typy nakonfigurovány, stále je třeba přiřadit atributy. Zaškrtávací políčka v horní části seznamu polí jsou *atributy indexu*, které řídí použití pole. 

* **Retrievable** (Zobrazitelné) znamená, že se zobrazí v seznamu výsledků vyhledávání. Jednotlivá pole můžete označit jako zakázaná pro výsledky hledání zrušením zaškrtnutí tohoto políčka, například když se pole používají jenom ve výrazech filtru. 
* **Filterable** (Filtrovatelné), **Sortable** (Seřaditelné) a **Facetable** (Kategorizovatelné) určují, zda lze pole použít ve filtru, v řazení nebo ve struktuře fasetové navigace. 
* **Searchable** (Prohledávatelné) znamená, že je pole součástí fulltextové vyhledávání. Řetězce jsou obvykle prohledávatelné. Číselná pole a logická pole jsou často označena jako neprohledávatelné. 

Než tuto stránku opustíte, označte pole v indexu tak, aby používala následující možnosti (Retrievable, Searchable a tak dále). Většina polí je zobrazitelná. Většina řetězcových polí je prohledávatelná (není nutné nastavovat klíč jako prohledávatelný). Několik polí, jako je genre, orderableOnline, rating a tags, je také filtrovatelných, seřaditelných a kategorizovatelných. 

| Pole | Typ | Možnosti |
| --- | --- | --- |
| id |Edm.String | |
| albumTitle |Edm.String |Retrievable, Searchable |
| albumUrl |Edm.String |Retrievable, Searchable |
| genre |Edm.String |Retrievable, Searchable, Filterable, Sortable, Facetable |
| genreDescription |Edm.String |Retrievable, Searchable |
| artistName |Edm.String |Retrievable, Searchable |
| orderableOnline |Edm.Boolean |Retrievable, Filterable, Sortable, Facetable |
| tags |Collection(Edm.String) |Retrievable, Filterable, Facetable |
| price |Edm.Double |Retrievable, Filterable, Facetable |
| margin |Edm.Int32 | |
| rating |Edm.Int32 |Retrievable, Filterable, Sortable, Facetable |
| inventory |Edm.Int32 |Retrievable |
| lastUpdated |Edm.DateTimeOffset | |

Pro porovnání představuje následující snímek obrazovky ilustraci indexu vytvořeného podle specifikace v předchozí tabulce.

 ![][4]

Kliknutím na **OK** dokončíte tento krok průvodce.

#### Krok 3: Definování indexeru
Stále v **Průvodci importem dat** klikněte na **Indexer** > **Název**, zadejte název indexeru a použijte výchozí nastavení pro všechny ostatní hodnoty. Tento objekt definuje spustitelný proces. Jakmile ho vytvoříte, může ho dát do plánu opakování, ale pro tentokrát použijte výchozí možnost spuštění indexeru okamžitě jednou, když kliknete na tlačítko **OK**. 

Položky importu dat by se měly všechny naplnit a být připravené.

  ![][5]

Chcete-li spustit průvodce, kliknutím na **OK** spusťte import a průvodce zavřete.

## Kontrola průběhu
Chcete-li zkontrolovat průběh, vraťte se na řídicí panel služby, přesuňte se dolů a dvakrát klikněte na dlaždici **Indexery**, aby se otevřel seznam indexerů. V seznamu byste měli vidět indexer, který jste právě vytvořili, se stavem indikujícím průběh práce nebo úspěch a s počtem dokumentů indexovaných do služby Azure Search.

  ![][6]

## Dotazování indexu
Nyní máte index vyhledávání, který je připraven k dotazování. 

**Průzkumník služby Search** je nástroj pro dotazování, který je integrovaný v portálu. Poskytuje vyhledávací pole, aby mohli ověřit, že vstup vyhledávání vrací očekávaná data. 

1. Klikněte na **Průzkumník služby Search** na panelu příkazů.
2. Všimněte si, který index je aktivní. Pokud to není ten, který jste právě vytvořili, klikněte na panelu příkazů na **Změnit index** a vyberte požadovaný index.
3. Vyhledávací pole ponechte prázdné a kliknutím na tlačítko **Hledat** spusťte hledání pomocí zástupných znaků, které vrátí všechny dokumenty.
4. Zadejte několik dotazů fulltextového vyhledávání. Můžete zkontrolovat výsledky z vašeho hledání pomocí zástupných znaků a seznámit se s umělci, alby a žánry, na které se lze dotazovat.
5. Zkuste jinou syntaxi dotazů pomocí [příkladů, které jsou k dispozici na konci tohoto článku](https://msdn.microsoft.com/library/azure/dn798927.aspx), abyste měli představu, jak upravit dotaz s použitím vyhledávacích řetězců, u kterých je pravděpodobné, že se v indexu najdou.

## Další kroky
Po prvním spuštění průvodce se můžete vrátit a zobrazit nebo upravit jednotlivé komponenty: index, indexer nebo zdroj dat. Některé úpravy, jako je například změna datového typu pole, nejsou u indexu povoleny, ale většinu vlastností a nastavení lze měnit. Chcete-li zobrazit jednotlivé komponenty, klikněte na dlaždice **Index**, **Indexer** nebo **Zdroje dat** na řídicím panelu, aby se zobrazil seznam existujících objektů.

Další informace o ostatních funkcích uvedených v tomto článku najdete pomocí těchto odkazů:

* [Indexery](search-indexer-overview.md)
* [Vytvoření indexu (zahrnuje podrobné vysvětlení atributů indexu)](https://msdn.microsoft.com/library/azure/dn798941.aspx)
* [Průzkumník služby Search](search-explorer.md)
* [Hledání dokumentů (zahrnuje příklady syntaxe dotazů)](https://msdn.microsoft.com/library/azure/dn798927.aspx)

Můžete vyzkoušet stejný pracovní postup s Průvodcem importem dat pro další zdroje dat, jako jsou například Azure SQL Database nebo SQL Server na virtuálním počítači Azure.

> [!NOTE]
> Nově je oznámena podpora indexeru pro procházení služby Azure Blob Storage, ale tato funkce je ve verzi Preview a zatím není součástí portálu. Pokud chcete vyzkoušet tento indexer, budete muset psát kód. Další informace najdete v tématu [Indexování služby Azure Blob Storage ve službě Azure Search](search-howto-indexing-azure-blob-storage.md).
> <a id="apdx-sampledata"></a>
> 
> 

## Dodatek: Vytvoření ukázkových dat v DocumentDB
V této části vytvoříme malou databázi v DocumentDB, kterou lze použít k dokončení úloh v tomto kurzu.

Následující instrukce poskytují obecné pokyny, ale nejsou vyčerpávající. Pokud potřebujete další pomoc s navigací v portálu DocumentDB nebo úlohami, můžete se podívat do dokumentace DocumentDB, ale většina příkazů, které potřebujete, se nachází na panelu příkazů služby v horní části řídicího panelu nebo v okně databáze. 

  ![][1]

### Vytvoření databáze musicstoredb pro tento kurz
1. [Kliknutím sem](https://github.com/HeidiSteen/azure-search-get-started-sample-data) stáhnete soubor ZIP obsahující soubory dat JSON s hudebním obchodem. Poskytujeme pro tuto datovou sadu 246 dokumentů JSON.
2. Přidejte do svého předplatného DocumentDB a pak otevřete řídicí panel služby.
3. Klikněte na **Přidat databázi** a vytvořte novou databázi s ID `musicstoredb`. Po vytvoření se zobrazí na dlaždici databáze v dolní části stránky.
4. Klikněte na název databáze a otevřete okno databáze.
5. Kliknutím na **Přidat kolekce** vytvořte kolekci s ID `musicstorecoll`.
6. Klikněte na **Průzkumník dokumentů**.
7. Klikněte na **Odeslat**.
8. V okně **Odeslat dokument** přejděte do místní složky, která obsahuje dříve stažené soubory JSON. Vyberte soubory JSON v dávkách po 100 kusech nebo menších.
   * 386.json
   * 387.json
   * . . .
   * 486.json
9. Opakujte akci s další dávkou souborů, dokud neodešlete poslední soubor 669.json.
10. Klikněte na **Průzkumník dotazů**, abyste ověřili, že data jsou odeslána a splňují požadavky na odeslání Průzkumníka dokumentů.

Snadným způsobem, jak to udělat, je použít výchozí dotaz, ale můžete také výchozí dotaz upravit tak, aby vybral prvních 300 položek (v této datové sadě je méně než 300 položek).

Měli byste obdržet výstup ve formátu JSON, který začíná dokumentem s číslem 386 a končí dokumentem 669. Po načtení dat se můžete [vrátit zpět na krokům v tomto návodu](#defineDS) a vytvořit index pomocí **Průvodce importem dat**.

<!--Image references-->
[1]: ./media/search-get-started-portal/AzureSearch-GetStart-Docdbmenu1.png
[2]: ./media/search-get-started-portal/AzureSearch-GetStart-DataSource.png
[3]: ./media/search-get-started-portal/AzureSearch-GetStart-DefaultIndex.png
[4]: ./media/search-get-started-portal/AzureSearch-GetStart-FinishedIndex.png
[5]: ./media/search-get-started-portal/AzureSearch-GetStart-ImportReady.png
[6]: ./media/search-get-started-portal/AzureSearch-GetStart-IndexerList.png
[7]: ./media/search-get-started-portal/search-data-import-wiz-btn.png



<!--HONumber=Oct16_HO1-->


