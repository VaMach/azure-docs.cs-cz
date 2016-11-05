---
title: Načtení dat pomocí Azure Data Factory | Microsoft Docs
description: Naučte se načítat data pomocí Azure Data Factory.
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: ''
tags: azure-sql-data-warehouse

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess

---
# <a name="load-data-with-azure-data-factory"></a>Načtení dat pomocí Azure Data Factory
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)  
> 
> 

Tento kurz ukazuje, jak ve službě Azure Data Factory vytvořit kanál pro přesun dat z Azure Storage Blob do služby Azure SQL Data Warehouse. V následujících krocích provedete toto:

* Nastavení ukázkových dat v Azure Storage Blob.
* Připojení prostředků k Azure Data Factory
* Vytvoření kanálu pro přesun dat z úložiště objektů blob do SQL Data Warehouse

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Než začnete
Seznamte se s Azure Data Factory. Potřebné informace najdete v tématu [Úvod do Azure Data Factory][].

### <a name="create-or-identify-resources"></a>Vytvoření nebo určení prostředků
Před zahájením tohoto kurzu musíte mít následující prostředky:

* **Objekt Blob služby Azure Storage:** V tomto kurzu se používá objekt Blob služby Azure Storage jako zdroj dat pro kanál Azure Data Factory, takže byste měli mít nějaký k dispozici pro uložení ukázkových dat. Pokud ho ještě nemáte, naučte se [vytvořit účet úložiště][].
* **SQL Data Warehouse:** V tomto kurzu se přesouvají data z objektu blob úložiště Azure do SQL Data Warehouse. Je proto nutné, abyste měli online datový sklad, ve kterém jsou načtená ukázková data AdventureWorksDW. Pokud ještě datový sklad nemáte, přečtěte si, jak si ho [zřídit][Create a SQL Data Warehouse]. Pokud sice máte datový sklad, ale nemáte v něm ukázková data, můžete je do něj [načíst ručně][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: Služba Azure Data Factory dokončí vlastní operaci načtení, takže je nutné ji mít, abyste pomocí ní mohli vytvořit kanál pro přesun dat. Pokud ji ještě nemáte, zjistěte, jak si ji vytvořit v kroku 1 tématu [Začínáme se službou Azure Data Factory (Data Factory Editor)][Začínáme se službou Azure Data Factory (Data Factory Editor)].
* **AZCopy**: AZCopy potřebujete ke zkopírování ukázkových dat z místního klienta do objektu blob služby Azure Storage. Postup instalace najdete v [dokumentaci k AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Krok 1: Kopírování ukázkových dat do objektu blob služby Azure Storage
Jakmile budete mít vše připraveno, můžete zkopírovat ukázková data do Azure Storage Blob.

1. [Stáhněte si ukázková data][]. Tato data přidají další tři roky prodejních dat do ukázkových dat AdventureWorksDW.
2. Tímto příkazem AZCopy zkopírujte tři roky dat do objektu blob služby Azure Storage.
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a>Krok 2: Připojení prostředků k Azure Data Factory
Teď, když jsou data tam, kde mají být, můžeme vytvořit kanál Azure Data Factory pro přesun dat z Azure Blob Storage do SQL Data Warehouse.

Začněte tím, že otevřete [Azure Portal][Azure Portal] a z nabídky na levé straně vyberete datovou továrnu.

### <a name="step-21-create-linked-service"></a>Krok 2.1: Vytvoření propojené služby
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

### <a name="step-22-define-the-dataset"></a>Krok 2.2: Definování datové sady
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
3. Nyní si nadefinujeme datovou sadu pro službu SQL Data Warehouse. Začneme stejným způsobem – kliknutím na Nová datová sada a pak na Azure SQL Data Warehouse.
   
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

## <a name="step-3-create-and-run-your-pipeline"></a>Krok 3: Vytvoření a spuštění kanálu
Nakonec ve službě Azure Data Factory nastavíme a spustíme kanál.  Právě tato operace provede vlastní přesun dat.  Úplný přehled operací, které můžete s SQL Data Warehouse a Azure Data Factory provádět, najdete [tady][Move data to and from Azure SQL Data Warehouse using Azure Data Factory] .

V části Vytvořit a nasadit klikněte na Další příkazy a poté klikněte na Nový kanál.  Po vytvoření kanálu můžete pomocí níže uvedeného kódu přenést data do datového skladu:

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

## <a name="next-steps"></a>Další kroky
Pokud se chcete dozvědět víc, začněte těmito zdroji informací:

* [Studijní postup Azure Data Factory][].
* [Azure SQL Data Warehouse Connector][]. Toto je základní referenční téma pro používání služby Azure Data Factory se službou Azure SQL Data Warehouse.

Tato témata obsahují podrobné informace o službě Azure Data Factory. Jsou věnovaná službám Azure SQL Database nebo HDInsight, ale informace v nich platí také pro službu Azure SQL Data Warehouse.

* [Kurz: Začínáme s Azure Data Factory.][] Jde o základní kurz pro zpracování dat pomocí služby Azure Data Factory. V tomto kurzu vytvoříte svůj první kanál, který využívá službu HDInsight k transformaci a měsíční analýze webových protokolů. Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.
* [Kurz: Kopírování dat z Azure Blob Storage do Azure SQL Database][]. V tomto kurzu vytvoříte ve službě Azure Data Factory kanál ke zkopírování dat z Azure Storage Blob do služby Azure SQL Database.

<!--Image references-->

<!--Article references-->
[Dokumentace k AZCopy]: ../storage/storage-use-azcopy.md
[Konektor služby Azure SQL Data Warehouse]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Vytvoření účtu úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Začínáme se službou Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Načtení ukázkových dat do služby SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Kurz: Kopírování dat z Azure Storage Blob do služby Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Kurz: Začínáme se službou Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Postup výuky pro službu Azure Data Factory]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure Portal]: https://portal.azure.com
[Stáhnout ukázková data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!---HONumber=Oct16_HO3-->


