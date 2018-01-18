---
title: "Jak používat Azure Table storage s Pythonem | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: a786f82d94a1a0039ed65a618670f872ffa3e3c2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-with-python"></a>Jak používat Azure Table storage s Pythonem

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Tento průvodce vám ukáže, jak provádět běžné scénáře Azure Table storage pomocí Python [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python). Pokryté scénáře zahrnují vytváření a odstraňování tabulek a vkládání a dotazování entity.

Při práci prostřednictvím scénáře v tomto kurzu, budete pravděpodobně chtít odkazovat [sada SDK úložiště Azure pro referenční dokumentace rozhraní API Python](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a>Nainstalovat úložiště Azure SDK pro Python

Po vytvoření účtu úložiště, je dalším krokem k instalaci [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python). Podrobnosti o instalaci sady SDK najdete [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) soubor v sadě SDK úložiště pro Python úložišti na Githubu.

## <a name="create-a-table"></a>Vytvoření tabulky

Pro práci se službou Azure Table v Python, je nutné naimportovat [TableService] [ py_TableService] modulu. Vzhledem k tomu, že je budete pracovat s entity tabulky, musíte taky [Entity] [ py_Entity] třídy. Přidejte tento kód v horní Python soubor k importu obě:

```python
from azure.storage.table import TableService, Entity
```

Vytvoření [TableService] [ py_TableService] objekt, předávání v klíč účet a název účtu úložiště. Nahraďte `myaccount` a `mykey` se název účtu a klíč a volání [create_table] [ py_create_table] pro vytvoření tabulky ve službě Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky

Pokud chcete přidat entitu, nejprve vytvořit objekt, který představuje vaše entitu a předat objekt, který má [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metoda. Objekt entity může být slovník nebo objekt typu [Entity][py_Entity]a definuje vlastnost názvy a hodnoty vaší entity. Každou entitu musíte zahrnout požadované [PartitionKey a RowKey](#partitionkey-and-rowkey) vlastnosti kromě všechny vlastnosti, které definujete pro entitu.

Tento příklad vytvoří objekt Slovník reprezentující entitu, potom předává jej do [insert_entity] [ py_insert_entity] metody přidat do tabulky:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Tento příklad vytvoří [Entity] [ py_Entity] objekt a potom předává jej do [insert_entity] [ py_insert_entity] metody přidat do tabulky:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey a RowKey

Musíte zadat oba **PartitionKey** a **RowKey** vlastnosti pro každou entitu. Toto jsou jedinečné identifikátory entity, jako společně tvoří primární klíč entity. Pomocí těchto hodnot mnohem rychlejší než dalších vlastností entity můžete dotazovat, protože pouze tyto vlastnosti jsou indexované se můžete dotazovat.

Použití služby Table **PartitionKey** inteligentně entity tabulky distribuovat mezi uzly úložiště. Entity, které mají stejnou **PartitionKey** jsou uložené na stejném uzlu. **RowKey** je jedinečný Identifikátor entity v oddílu patří do.

## <a name="update-an-entity"></a>Aktualizace entity

Chcete-li aktualizovat všechny hodnoty vlastností entity, zavolejte [update_entity] [ py_update_entity] metoda. Tento příklad ukazuje, jak nahradit stávající entity aktualizovaná verze:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Pokud ještě neexistuje typ entity, která se právě aktualizuje, se nezdaří operace aktualizace. Pokud chcete uložit entity toho, zda existuje nebo Ne, použít [insert_or_replace_entity][py_insert_or_replace_entity]. V následujícím příkladu první volání nahradí stávající entity. Druhé volání bude vložit novou entitu, protože žádná entita s zadaný klíč oddílu a RowKey existuje v tabulce.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> [Update_entity] [ py_update_entity] metoda nahrazuje všechny vlastnosti a hodnoty existující entity, který můžete také použít k odebrání vlastnosti z existující entity. Můžete použít [merge_entity] [ py_merge_entity] metodu za účelem aktualizace stávající entity s hodnotami vlastností nové nebo upravené bez úplného nahrazení entity.

## <a name="modify-multiple-entities"></a>Úprava více entit

Aby atomic zpracování požadavku ve službě Table, můžete odeslat více operací společně v dávce. Nejprve pomocí [TableBatch] [ py_TableBatch] třídu a přidejte k jedné dávkové operace více. Pak zavolejte [TableService][py_TableService].[ commit_batch] [ py_commit_batch] k odeslání operace v atomické operace. Všechny entity má být změněn ve službě batch musí být ve stejném oddílu.

Tento příklad přidá dvě entity společně v dávce:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Dávky můžete použít taky s syntaxe kontextu správce:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Dotaz pro entitu

Dotaz pro entitu v tabulce, předat jeho PartitionKey a RowKey k [TableService][py_TableService].[ get_entity] [ py_get_entity] metoda.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit

Můžete zadat dotaz pro sadu entit zadáním řetězec filtru s **filtru** parametr. Tento příklad vyhledá všechny úkoly v Praze použitím filtru na PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity

Můžete taky omezit vlastnosti, které se vrátí pro každou entitu v dotazu. Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entit nebo sad výsledků dotazu. Použití **vyberte** parametr a předejte jí názvy vlastností, které chcete vrácen do klienta.

Dotaz v následujícím kódu vrátí pouze popisy entit v tabulce.

> [!NOTE]
> Následující fragment kódu funguje pouze s Azure Storage. Emulátor úložiště není podporován.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Odstranění entity

Odstranění entity předáním jeho PartitionKey a RowKey k [delete_entity] [ py_delete_entity] metoda.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Odstranění tabulky

Pokud již nepotřebujete pro tabulku nebo některé z entit v něm, zavolejte [delete_table] [ py_delete_table] metoda trvale odstranit z úložiště Azure v tabulce.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Další postup

* [Azure SDK úložiště pro Python API – referenční informace](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Úložiště Azure SDK pro jazyk Python](https://github.com/Azure/azure-storage-python)
* [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): bezplatná aplikace a platformy pro vizuální práci s daty Azure Storage ve Windows, systému macOS a Linux.

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch