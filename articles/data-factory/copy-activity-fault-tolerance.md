---
title: "Odolnost proti chybám aktivity kopírování v Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přidat odolnost proti chybám aktivitě kopírování v Azure Data Factory přeskočením nekompatibilní řádky."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jingwang
ms.openlocfilehash: 5b2658cecba80ef871cc38b930b0e52bc3952530
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Odolnost proti chybám aktivity kopírování v Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Verze 2 – Preview](copy-activity-fault-tolerance.md)

Aktivita kopírování v Azure Data Factory nabízí dva způsoby, jak zpracovat nekompatibilní řádků při kopírování dat mezi zdroj a jímka úložišti dat:

- Můžete přerušit a selhání kopie aktivity po nekompatibilní data došlo (výchozí nastavení).
- Kopírování všech dat přidáním odolnost proti chybám a přeskočení řádky nekompatibilní data můžete pokračovat. Kromě toho se můžete přihlásit nekompatibilní řádky úložiště objektů Blob v Azure. Poté můžete prozkoumat do protokolu a zjistěte příčinu selhání, opravte dat ve zdroji dat a zkuste to aktivitě kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [kopírovat aktivity odolnost proti chybám V1](v1/data-factory-copy-activity-fault-tolerance.md).


 ## <a name="supported-scenarios"></a>Podporované scénáře
Aktivita kopírování podporuje tři scénáře pro zjišťování, přeskočí a protokolování nekompatibilní data:

- **Nekompatibilita mezi zdrojového datového typu a nativní typ jímky**. <br/><br/> Příklad: kopírování dat ze souboru CSV v úložišti objektů Blob k databázi SQL s definici schématu, která obsahuje tři sloupce typu INT. Řádky soubor CSV, které obsahují číselná data, jako je například 123,456,789 jsou úspěšně zkopírovat do úložiště jímky. Ale řádky, které obsahují jiné než číselné hodnoty, jako je například 123,456, abc jsou rozpoznána jako nekompatibilní a se přeskočí.
- **Neshoda mezi počtem sloupců mezi zdroj a jímka**. <br/><br/> Příklad: kopírování dat ze souboru CSV v úložišti objektů Blob k databázi SQL s definici schématu, která obsahuje šest sloupce. Řádky soubor CSV, které obsahují šesti sloupce jsou úspěšně zkopírovat do úložiště jímky. Řádky soubor CSV, které obsahují více nebo méně než šest sloupce jsou rozpoznána jako nekompatibilní a se přeskočí.
- **Porušení primárního klíče při zápisu do relační databáze**.<br/><br/> Příklad: kopírování dat z SQL serveru do databáze SQL. Ve službě SQL database podřízený je definovaný primární klíč, ale na zdrojovém serveru SQL je definován žádný primární klíč. Duplicitní řádky, na které existují ve zdroji nelze zkopírovat do jímky. Aktivita kopírování zkopíruje pouze první řádek zdrojová data do jímky. Další zdroje řádky, které obsahují duplicitní hodnotu primárního klíče jsou rozpoznána jako nekompatibilní a se přeskočí.

## <a name="configuration"></a>Konfigurace
Následující příklad uvádí definici JSON konfigurace přeskočení nekompatibilní řádky v aktivitě kopírování:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "AzureBlobLinkedService",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```
Vlastnost | Popis | Povolené hodnoty | Požaduje se
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Určuje, jestli chcete přeskočit nekompatibilní řádky během kopírování, nebo ne. | True<br/>NEPRAVDA (výchozí) | Ne
redirectIncompatibleRowSettings | Skupina vlastností, které může být zadán, pokud chcete protokolovat nekompatibilní řádky. | &nbsp; | Ne
linkedServiceName | Propojené služby Azure Storage k ukládání protokol, který obsahuje přeskočených řádků. | Název azurestorage nebo AzureStorageSas propojené služby, která odkazuje na instanci úložiště, který chcete použít k uložení souboru protokolu. | Ne
Cesta | Cesta souboru protokolu, který obsahuje přeskočených řádků. | Zadejte cestu úložiště objektů Blob, které chcete používat k protokolování nekompatibilní data. Pokud nezadáte cestu, služby pro vás vytvoří kontejner. | Ne

## <a name="monitor-skipped-rows"></a>Monitorování přeskočených řádků
Po dokončení kopírování aktivity při spuštění, zobrazí se počet přeskočených řádků ve výstupu aktivitě kopírování:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Pokud nakonfigurujete protokolu nekompatibilní řádky, můžete nějakého najít soubor protokolu v této cestě: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

Soubory protokolu lze pouze soubory csv. Původní data přeskočen bude protokolována s čárkou jako oddělovač sloupec, podle potřeby. Přidáme další dva sloupce "ErrorCode" a "ErrorMessage" v další k původní zdrojová data v souboru protokolu, kde se můžete podívat kořenové příčinu nekompatibilita. Kód chyby a ErrorMessage budou nabízeny pomocí dvojité uvozovky. 

Příklad obsahu souboru protokolu je následující:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Další kroky
Najdete v aktivitě kopírování článcích:

- [Kopie aktivity – přehled](copy-activity-overview.md)
- [Výkonu kopie aktivity](copy-activity-performance.md)


