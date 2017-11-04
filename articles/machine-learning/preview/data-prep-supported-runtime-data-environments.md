---
title: "Podporované kombinace prostředí provádění a dat pro Azure Machine Learning Data přípravy | Microsoft Docs"
description: "Tento dokument obsahuje úplný seznam podporovaných kombinací jiné moduly runtime a zdroje dat pro Azure Machine Learning Data přípravy"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 413bc8a0e0347498c004b93fb37f51d86ad029f5
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="supported-matrix-for-this-release"></a>Podporované matice pro tuto verzi 
Pokud váš kód načte data pomocí Azure Machine Learning Data zdroje nebo Azure Machine Learning Data přípravy, získávání buď Pandas nebo dataframe Spark, následující kombinace experimentu výpočetní prostředí a data umístění jsou podporovány:

|     |Místní soubory  |Azure Blob Storage  |Databáze systému SQL Server ***  |
|---------|---------|---------|---------|---------|
|Místní Python    |     Podporuje se    |Nepodporuje se         | Nepodporuje se        |         |
|Python docker (virtuální počítač s Linuxem)     |Podporované v souborech projektu pouze *         | Nepodporuje se        |        Nepodporuje se |         |
|PySpark docker (virtuální počítač s Linuxem)     |Podporované v souborech projektu pouze *     |Podporuje se         | Podporuje se**        |         |
|Python virtuálního počítače Azure dat vědecké účely     |Podporované v souborech projektu pouze *         |Nepodporuje se         |Nepodporuje se         |         |
|PySPark virtuální počítač Azure dat vědecké účely     | Podporované v souborech projektu pouze *        |Nepodporuje se         |Nepodporuje se         |         |
|Azure HDInsight PySpark     | Nepodporuje se        |Podporuje se         |Podporuje se**         |         |
|Azure HDInsight Python     | Nepodporuje se        | Nepodporuje se        | Nepodporuje se        |         |

Azure Data Lake Store se aktuálně nepodporuje pro všechny výpočetní cíl.

* Pokud místní cesty souborů se používá, jsou soubory v projektu zkopírován do výpočetní prostředí a přečtěte si existuje. Nejsou zkopíruje soubory mimo projekt a cesty už vyřešit ve výpočetním prostředí. Zvažte použití nahrazení zdroje dat tak, aby váš kód můžete použít místní soubor při místním spuštění. Potom přepněte na objekt blob úložiště Azure pro jinou konfiguraci spuštění. Také můžete vzorkování podporu pro zdroje dat pro správu běží na velkých objemů dat pouze při některých konfiguracích spuštění.

** Používá Maven JDBC SQL Server ovladač 6.2.1. Je nutné zajistit, že tento balíček (nebo kompatibilní jeden) je součástí souboru spark_dependencies.yml pro výpočetní prostředí.

Podporuje Azure SQL Database, Azure SQL Data Warehouse nebo SQL Server, pokud že databázi lze přejít z výpočetním prostředí. 
