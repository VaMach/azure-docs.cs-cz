---
title: "Azure tabulce úložiště funkcí vazby | Microsoft Docs"
description: "Pochopit, jak používat Azure Storage vazby v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 486b7c31c914ba7bb2d75e3f83ccf346a09104e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Vazby tabulky Azure funkce úložiště
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a vazby tabulky Azure Storage kódu v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro tabulky služby Azure Storage.

Vazbu tabulky úložiště podporuje následující scénáře:

* **Přečtěte si jeden řádek v C# nebo Node.js funkce** – nastavte `partitionKey` a `rowKey`. `filter` a `take` vlastnosti nejsou v tomto scénáři používají.
* **Přečtěte si více řádků ve funkci jazyka C#** – modul runtime Functions poskytuje `IQueryable<T>` objekt vázán k tabulce. Typ `T` musí být odvozeny od `TableEntity` nebo implementovat `ITableEntity`. `partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nejsou použity v tomto scénáři, můžete pomocí `IQueryable` objekt, který chcete provést všechny potřebné filtrování. 
* **Přečtěte si více řádků v uzlu funkce** – nastavte `filter` a `take` vlastnosti. Není nastavený `partitionKey` nebo `rowKey`.
* **Zápis jeden nebo více řádků ve funkci jazyka C#** – modul runtime Functions poskytuje `ICollector<T>` nebo `IAsyncCollector<T>` vázán k tabulce, kde `T` Určuje schéma entity, které chcete přidat. Obvykle zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to. `partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nejsou v tomto scénáři používají.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Vazba vstupní tabulky úložiště
Vazba vstupní tabulky Azure Storage umožňuje používat úložiště tabulku ve vaší funkci. 

Vstupní tabulka úložiště funkci používá těchto objektů JSON `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Je třeba počítat s následujícím: 

* Použití `partitionKey` a `rowKey` společně ke čtení jedné entity. Tyto vlastnosti jsou volitelné. 
* `connection`musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících. Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
V jazyce C# funkce, vytvoření vazby vstupní tabulka entity (nebo entity) pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je datový typ chcete deserializaci dat do, a `paramName` je název, který jste zadali v [vstupní vazby](#input). V Node.js funkce získáte přístup vstupní tabulka entity (nebo entity) pomocí `context.bindings.<name>`.

Ve funkcích Node.js nebo C# lze deserializovat vstupní data. Deserializovaná objekty mají `RowKey` a `PartitionKey` vlastnosti.

V jazyce C# funkce můžete také vytvořit vazbu na některý z následujících typů a Functions runtime se pokusí rekonstruovat data tabulky pomocí typu:

* Žádný typ, který implementuje`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Vstupní ukázka
By měl mít následující function.json, který používá aktivační procedury fronty načíst řádek jednu tabulku. Určuje formát JSON `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Označuje, že klíč řádku pochází z řetězec zprávy fronty.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Naleznete v ukázce pro specifický jazyk, který čte entity jednu tabulku.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Vstupní ukázka v jazyce F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Vstupní ukázka v Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Úložiště table výstup vazby
Výstupní tabulky Azure Storage, vazba umožňuje zápis entity do úložiště tabulky ve vaší funkci. 

Výstup funkce používá následující objekty JSON v tabulce úložiště `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Je třeba počítat s následujícím: 

* Použití `partitionKey` a `rowKey` společně k zápisu jedné entity. Tyto vlastnosti jsou volitelné. Můžete také zadat `PartitionKey` a `RowKey` při vytváření objektů entity ve vašem kódu funkce.
* `connection`musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících. Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V jazyce C# funkce, je vytvoření vazby na výstupní tabulky pomocí pojmenované `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je datový typ chcete serializovat data do, a `paramName` je název, který jste zadali v [výstup vazby](#output). Ve funkcích Node.js, můžete přístup k tabulce výstup pomocí `context.bindings.<name>`.

Může serializovat objekty v Node.js nebo C#. V C# funkce můžete také vytvořit vazbu na následující typy:

* Žádný typ, který implementuje`ITableEntity`
* `ICollector<T>`(pro výstup více entit. V tématu [ukázka](#outcsharp).)
* `IAsyncCollector<T>`(asynchronní verzi `ICollector<T>`)
* `CloudTable`(s použitím sady SDK úložiště Azure. V tématu [ukázka](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Následující *function.json* a *run.csx* příklad ukazuje, jak napsat více entity tabulky.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

V tématu vzorku pro specifický jazyk, který vytvoří několik entit tabulky.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Ukázka: Přečtěte si více entity tabulky v jazyce C#  #
Následující *function.json* a příklad kódu C# přečte entity pro klíč oddílu, který je uveden v zprávy ve frontě.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Kód jazyka C# přidá odkaz na sady SDK úložiště Azure tak, aby typ entity můžete odvozena od `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

