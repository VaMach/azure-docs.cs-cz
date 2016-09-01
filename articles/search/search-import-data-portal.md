<properties
    pageTitle="Import dat do Azure Search pomocí indexerů na portálu Azure | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Jak používat indexery na portálu Azure."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="06/08/2016"
    ms.author="heidist"/>

# Import dat do služby Azure Search pomocí portálu

Portál Azure obsahuje v řídícím panelu služby Azure Search příkaz **Import dat** pro načítání dat do indexu. Tento příkaz využívá předdefinovanou funkci indexerů, která prochází existující zdroj dat, vytváří a odesílá dokumenty na základě sady řádků získané ze zdroje dat.

Import dat v průvodci se skládá ze 3 částí:

- připojení ke zdroji dat
- cílový index, do kterého se data odesílají (průvodce ho pro vás často může vytvořit)
- plán, který se spustí hned nebo v pravidelných intervalech

Chcete-li použít indexer nebo příkaz **Import dat**, váš primární zdroj dat musí být jedním z podporovaných zdrojů dat: databáze SQL Azure, relační databáze SQL Serveru na virtuálním počítači Azure, nebo Azure DocumentDB.

Importovat můžete pouze z jedné tabulky, jednoho zobrazení, nebo ekvivalentní datové struktury. Možná budete nejdříve muset ve zdroji dat svojí aplikace vytvořit datovou strukturu, abyste tak získali správná metadata a datové vstupy do svého indexu vyhledávání.

Tento pracovní postup můžete vyzkoušet s použitím vzorových dat. Chcete-li začít, navštivte stránku [Začínáme se službou Azure Search na portálu Azure](search-get-started-portal.md).

##Konfigurace importu dat

1. Přihlaste se k [Portálu Azure](https://portal.azure.com).

2. Otevřete řídící panel služby Azure Search. Zde je několik způsobů, jak najít řídicí panel.
    - Na panelu vlevo klikněte na **Domů**. Domovská stránka obsahuje dlaždice pro všechny služby v rámci vašeho předplatného. Řídící panel služby otevřete kliknutím na dlaždici.
    - Na panelu vlevo klikněte na **Procházet vše**  >  **Filtrovat podle**  >  **Služby vyhledávání** a vyhledejte svoji službu Vyhledávání.

3. V horní části řídícího panelu služby uvidíte panel příkazů, který obsahuje i příkaz **Import dat**. Kliknutím na **Import dat** otevřete vysouvací okno Import dat.

4. Klikněte na **Připojit k datům** a zadejte definici zdroje dat používanou indexerem. Mezi možnosti patří:
    -   Stávající zdroj dat odkazuje na dříve vytvořenou definici zdroje dat pro indexer. Pokud již ve službě vyhledávání máte definované indexery, můžete změnit účel definice zdroje dat pro další import.
    -   Azure SQL slouží k určení připojení zdroje dat k databázi SQL v Azure nebo k databázi SQL Serveru na virtuálním počítači Azure.
    -   DocumentDB slouží k určení připojení zdroje dat pro konkrétní typ zdroje dat.

   U Azure SQL i DocumentDB musí databáze existovat v rámci vašeho předplatného. Můžete vložit připojovací řetězec, pokud ho znáte, nebo vybrat zdroj dat, který dříve vytvořil někdo, kdo má pro vaše předplatné oprávnění k zápisu.

5. Výchozí index dokončíte kliknutím na **Upravit cílový index**. 
    - Je vyžadován **Klíč**. Pole, které pro klíč vyberete, musí být pole řetězce obsahující jedinečné hodnoty.
    - Název a typ pole jsou obvykle předvyplněné. Můžete změnit typ dat.
    - Pro každé pole vyberte atributy:
        - Retrievable (Zobrazitelné) – vrátí pole ve výsledcích vyhledávání.
        - Filterable (Filtrovatelné) – umožňuje na pole odkazovat ve výrazech filtru.
        - Sortable (Seřaditelné) – umožňuje použití pole při řazení.
        - Facetable (Kategorizovatelné) – povoluje použití pole pro fasetovou navigaci.
        - Searchable (Prohledávatelné) – umožňuje fulltextové vyhledávání.
    - Chcete-li určit analyzátor jazyka na úrovni pole, klikněte na kartu **Analyzátor**. Podrobnosti naleznete v oddílu [Vytvoření indexu pro vícejazyčné dokumenty](search-language-support.md).
    - Chcete-li povolit automatické dokončování nebo našeptávání dotazů, klikněte na **Modul pro návrhy**.

6. Klikněte na **Importovat data** a pomocí možnosti Spustit nyní spusťte operaci importování, nebo nastavte plán opakování.

Právě dokončená operace importování dat na pozadí vytvořila indexer. Nyní můžete upravovat přímo indexer a měnit tak všechny jeho součásti.

##Úprava existujícího indexeru

Dvojím kliknutím na dlaždici Indexer v řídícím panelu služby vysuňte seznam všech indexerů vytvořených pro vaše předplatné. Dvakrát klikněte na indexery, které chcete spustit, upravit nebo odstranit. Index můžete nahradit jiným existujícím indexem, změnit zdroj dat a nastavit možnosti prahových hodnot chybu během indexování.

##Úprava existujícího indexu

Ve vyhledávání systému Azure budou strukturální aktualizace indexu vyžadovat opětovné vytvoření tohoto indexu, který se skládá z odstranění indexu, opětovného vytvoření indexu a načtení dat. Strukturální aktualizace zahrnují změnu datového typu a přejmenování nebo odstranění pole.

Úpravy, které nevyžadují opětovné sestavení zahrnují přidání nového pole, změnu vyhodnocování profilů, změna navrhovatelů nebo změnu analyzátorů jazyka. Další informace naleznete v [aktualizaci indexu](https://msdn.microsoft.com/library/azure/dn800964.aspx).



<!--HONumber=Aug16_HO4-->


