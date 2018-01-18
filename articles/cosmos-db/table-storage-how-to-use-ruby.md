---
title: "Používání úložiště tabulek Azure s Ruby | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: decc6ffb38a4358d3593642f9cedb59d08f6bfef
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-with-ruby"></a>Jak používat Azure Table storage s Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Table. Ukázky jsou zapsány pomocí rozhraní API Ruby. Pokryté scénáře zahrnují **vytváření a odstraňování tabulek, vkládání a dotazování entity v tabulce**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Pokyny pro vytvoření Ruby aplikace naleznete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro přístup k úložišti
Pokud chcete používat Azure Storage, musíte stáhnout a použít Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují se službami REST úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získat balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. Typ **gem instalace azure** v příkazovém okně instalace gem a závislostí.

### <a name="import-the-package"></a>Import balíčku
Použít svém oblíbeném textovém editoru, přidejte na začátek souboru Ruby, kde máte v úmyslu používat úložiště následující:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
Modul azure, bude číst proměnné prostředí **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** informace požadované pro připojení k účtu úložiště Azure. Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu před použitím **Azure::TableService** následujícím kódem:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

K získání těchto hodnot z klasický nebo účet správce prostředků úložiště na portálu Azure:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Přejděte na účet úložiště, který chcete použít.
3. V okně nastavení na pravé straně klikněte na tlačítko **přístupové klíče**.
4. V okně klíče přístup, který se zobrazí uvidíte přístupový klíč 1 a 2 přístupový klíč. Můžete použít kteroukoli z nich.
5. Kliknutím na ikonu kopírování do schránky zkopírujte klíč.

## <a name="create-a-table"></a>Vytvoření tabulky
**Azure::TableService** objekt vám umožňuje spolupracovat s tabulkami a entity. Chcete-li vytvořit tabulku, použijte **vytvořit\_table()** metoda. Následující příklad vytvoří tabulku nebo vytiskne chybu, pokud existuje.

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky
Chcete-li přidat entitu, nejprve vytvořte objekt algoritmu hash, která definuje vlastnosti vaší entity. Všimněte si, že pro každou entitu musíte zadat **PartitionKey** a **RowKey**. Jsou jedinečné identifikátory vaší entity a jsou hodnoty, které může být dotazován mnohem rychleji než vaše jiných vlastností. Azure Storage používá **PartitionKey** automaticky distribuovat entity v tabulce přes mnoho uzly úložiště. Entity se stejným **PartitionKey** jsou uložené na stejném uzlu. **RowKey** je jedinečný Identifikátor entity v oddílu patří do.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Aktualizace entity
Existuje několik metod, které jsou k dispozici pro aktualizace stávající entity:

* **Aktualizovat\_entity():** aktualizace stávající entity nahrazením ho.
* **sloučení\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity.
* **Vložit\_nebo\_sloučení\_entity():** aktualizace stávající entity podle jeho nahrazení. Pokud žádná entita existuje, bude vložen nový:
* **Vložit\_nebo\_nahradit\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity. Pokud existuje žádné entity, se vloží novou.

Následující příklad ukazuje, aktualizuje entitu s využitím **aktualizace\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

S **aktualizace\_entity()** a **sloučení\_entity()**, pokud typ entity, která jsou aktualizace neexistuje, operace aktualizace se nezdaří. Proto pokud chcete uložit entity bez ohledu na to, jestli už existuje, měli byste místo toho použít **vložit\_nebo\_nahradit\_entity()** nebo **vložit\_nebo\_sloučení\_entity()**.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit
Někdy má smysl odeslat více operací společně v dávce zajistit atomic zpracování serverem. To provést, je třeba nejprve vytvořit **Batch** objekt a potom pomocí **provést\_batch()** metodu **TableService**. Následující příklad ukazuje, odesílání dvě entity s RowKey 2 a 3 v dávce. Všimněte si, že funguje pouze u entit s stejný klíč oddílu.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Dotaz pro entitu
Chcete-li prohledávat entitu v tabulce, použijte **získat\_entity()** metoda předáním názvu tabulky **PartitionKey** a **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit
Chcete-li prohledávat sady entit v tabulce, vytvoření objektu hash dotazu a použijte **dotazu\_entities()** metoda. Následující příklad ukazuje získání všech entit se stejným **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Je-li nastavit výsledek je příliš velký pro jediný dotaz vrátit, které můžete použít k načtení další stránky vrátí token pokračování.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Dotaz na tabulku můžete načíst jenom několik z entity. Tímto způsobem, nazývá "projekce" omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. Použijte klauzuli select a předejte názvy vlastností, které chcete převést klientovi.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Odstranění entity
Pokud chcete odstranit entitu, použijte **odstranit\_entity()** metoda. Je třeba předat názvu tabulky a který obsahuje entitu, PartitionKey a RowKey entity.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Odstranění tabulky
Pokud chcete odstranit tabulku, použijte **odstranit\_table()** metoda a předejte jí název tabulky, které chcete odstranit.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Další postup

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.
* [Azure SDK pro Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu

