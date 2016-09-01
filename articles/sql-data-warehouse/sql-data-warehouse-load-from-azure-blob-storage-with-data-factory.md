<properties
   pageTitle="Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory) | Microsoft Azure"
   description="Naučte se načítat data pomocí Azure Data Factory."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

 Tento návod ukazuje, jak v Azure Data Factory vytvořit kanál pro přesun dat z úložiště objektů blob Azure do SQL Data Warehouse. V následujících krocích provedete toto:

+ Nastavení ukázkových dat v objektu blob služby Azure Storage
+ Připojení prostředků k Azure Data Factory
+ Vytvoření kanálu pro přesun dat z úložiště objektů blob do SQL Data Warehouse

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## Než začnete

Seznamte se s Azure Data Factory. Potřebné informace najdete v tématu [Úvod do Azure Data Factory][].

### Vytvoření nebo určení prostředků

Před zahájením tohoto kurzu musíte mít následující prostředky.

   + **Objekt Blob služby Azure Storage:** V tomto kurzu se používá objekt Blob služby Azure Storage jako zdroj dat pro kanál Azure Data Factory, takže byste měli mít nějaký k dispozici pro uložení ukázkových dat. Pokud ho ještě nemáte, naučte se [vytvořit účet úložiště][].

   + **SQL Data Warehouse:** V tomto kurzu se přesouvají data z objektu blob úložiště Azure do SQL Data Warehouse. Je proto nutné, abyste měli online datový sklad, ve kterém jsou načtená ukázková data AdventureWorksDW. Pokud ještě datový sklad nemáte, přečtěte si, jak si ho [zřídit][Vytvoření SQL Data Warehouse]. Pokud sice máte datový sklad, ale nemáte v něm ukázková data, můžete je do něj [načíst ručně][Načtení ukázkových dat do SQL Data Warehouse].

   + **Azure Data Factory:** Azure Data Factory dokončí vlastní operaci načtení, takže je nutné mít ji, abyste ji mohli použít k vytvoření kanálu pro přenos dat. Pokud ji ještě nemáte, zjistěte, jak si ji vytvořit (v kroku 1 tématu [Začínáme s Azure Data Factory (Data Factory Editor)][]).

   + **AZCopy**: AZCopy potřebujete ke zkopírování ukázkových dat z místního klienta do objektu blob služby Azure Storage. Postup instalace najdete v [dokumentaci k AZCopy][].

## Krok 1: Kopírování ukázkových dat do objektu blob služby Azure Storage

Jakmile budete mít vše připraveno, můžete zkopírovat ukázková data do objektu blob služby Azure Storage.

1. [Stáhněte si ukázková data][]. Tato data přidají další tři roky prodejních dat do ukázkových dat AdventureWorksDW.

2. Tímto příkazem AZCopy zkopírujte tři roky dat do objektu blob služby Azure Storage.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## Krok 2: Připojení prostředků k Azure Data Factory

Teď, když jsou data tam, kde mají být, můžeme vytvořit kanál Azure Data Factory pro přesun dat z Azure Blob Storage do SQL Data Warehouse.

Začněte tím, že otevřete [Azure Portal][] a z nabídky na levé straně vyberete datovou továrnu.

### Krok 2.1: Vytvoření propojené služby

Propojte si účet úložiště Azure a SQL Data Warehouse se svojí datovou továrnou.  

1. Začněte proces registrace kliknutím na část Propojené služby svojí datové továrny a potom klikněte na Nové datové úložiště. Zvolte název, pod kterým chcete úložiště Azure zaregistrovat, jako typ vyberte Úložiště Azure a pak zadejte název a klíč účtu.

2. SQL Data Warehouse zaregistrujete tak, že přejdete do části Vytvořit a nasadit, vyberete možnost Nové datové úložiště a pak vyberete Azure SQL Data Warehouse. Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétní informace.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### Krok 2.2: Definování datové sady

Po vytvoření propojených služeb budeme muset definovat datové sady.  Tady to znamená definovat strukturu dat přesouvaných z vašeho úložiště datového skladu.  O vytváření si toho můžete přečíst víc.

1. Tento proces začněte tím, že v datové továrně přejdete do části Vytvořit a nasadit.

2. Klikněte na Nová datová sada a potom na Azure Blob Storage. Tím si svoje úložiště připojíte k datové továrně.  K definování svých dat v Azure Blob Storage můžete použít níže uvedený skript:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```


3. Teď si také definujeme datovou sadu pro SQL Data Warehouse.  Začneme stejným způsobem – kliknutím na Nová datová sada a pak na Azure SQL Data Warehouse.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## Krok 3: Vytvoření a spuštění kanálu

Nakonec v Azure Data Factory nastavíme a spustíme kanál.  Právě tato operace provede vlastní přesun dat.  Úplný přehled operací, které můžete s SQL Data Warehouse a Azure Data Factory provádět, najdete [tady][Přesun dat do a z Azure SQL Data Warehouse pomocí Azure Data Factory].

V části Vytvořit a nasadit klikněte na Další příkazy a pak klikněte na Nový kanál.  Po vytvoření kanálu můžete pomocí níže uvedeného kódu přenést data do datového skladu:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## Další kroky

Pokud se chcete dozvědět víc, začněte těmito zdroji informací:

- [Studijní postup Azure Data Factory][].
- [Azure SQL Data Warehouse Connector][]. Toto je základní referenční téma pro používání služby Azure Data Factory se službou Azure SQL Data Warehouse.


Tato témata obsahují podrobné informace o službě Azure Data Factory. Jsou věnovaná službám Azure SQL Database nebo HDinsight, ale informace v nich platí také pro Azure SQL Data Warehouse.

- [Kurz: Začínáme s Azure Data Factory.][] Jde o základní kurz pro zpracování dat pomocí služby Azure Data Factory. V tomto kurzu vytvoříte svůj první kanál, který využívá HDInsight k transformaci a měsíční analýze webových protokolů. Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.
- [Kurz: Kopírování dat z Azure Blob Storage do Azure SQL Database][]. V tomto kurzu vytvoříte kanál v Azure Data Factory ke zkopírování dat z Azure Blob Storage do Azure SQL Database.


<!--Image references-->

<!--Article references-->
[dokumentaci k AZCopy]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Vytvoření SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[vytvořit účet úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Začínáme s Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Úvod do Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Načtení ukázkových dat do SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Přesun dat do a z Azure SQL Data Warehouse pomocí Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Kurz: Kopírování dat z Azure Blob Storage do Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Kurz: Začínáme s Azure Data Factory.]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Studijní postup Azure Data Factory]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure Portal]: https://portal.azure.com
[Stáhněte si ukázková data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Aug16_HO4-->


