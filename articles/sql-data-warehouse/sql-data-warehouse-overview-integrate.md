---
title: "Sestavení integrované řešení s SQL Data Warehouse | Microsoft Docs"
description: "Nástroje a partnery, se řešení, které se integrují se službou SQL Data Warehouse. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>Využít dalším službám pomocí SQL Data Warehouse
Kromě jeho základní funkce SQL Data Warehouse umožňuje uživatelům využívat mnoho dalších služeb v Azure společně se ho.  Konkrétně aktuálně přesměrovali jsme kroky pro řešení integraci s následujícími službami:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Pracujeme na připojení s více službami napříč ekosystému Azure.

## <a name="power-bi"></a>Power BI
Integrace Power BI umožňuje využít výpočetní výkon služby SQL Data Warehouse s dynamické generování sestav a vizualizaci Power BI. Integrace Power BI aktuálně zahrnuje:

* **Přímé připojení**: více rozšířených připojení s logické přenos směrem dolů vůči SQL Data Warehouse.  To poskytuje rychlejší analysis ve větším měřítku.
* **Otevřít v Power BI**: tlačítko Otevřít v Power BI předává informace o instanci Power BI umožňuje pro bezproblémové připojení.

V tématu [integrací s Power BI](sql-data-warehouse-integrate-power-bi.md) nebo [Power BI dokumentaci](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Další informace.

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory poskytuje uživatelům spravovaná platforma vytvořit komplexní extrakce zatížení kanály.  SQL Data Warehouse integraci s Azure Data Factory zahrnuje následující položky:

* **Uložené procedury**: orchestraci provádění uložené procedury v SQL Data Warehouse.
* **Kopírování**: použití ADF pro přesun dat do SQL Data Warehouse.  Tato operace můžete v pozadí využívají mechanismus přesun ADF standardní dat nebo PolyBase. 

V tématu [integrací s Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) nebo [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Další informace.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning je plně spravovaná analytics službu, která umožňuje uživatelům vytvářet složité modely využití velké sady prediktivní nástroje.  SQL Data Warehouse je podporovaný jako zdrojovém i cílovém pro tyto modely následující funkce:

* **Čtení dat:** jednotky modely škálované pomocí T-SQL s SQL Data Warehouse.
* **Zapisovat Data:** potvrzení změny z kteréhokoli modelu zpět do SQL Data Warehouse.

V tématu [integrací se službami Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) nebo [dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Další informace.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics je komplexní, plně spravovaná infrastruktury pro zpracování a využívají data události vygenerovaná z centra událostí Azure.  Integrace s SQL Data Warehouse umožňuje dat účinně zpracovat a uložená spolu s relačních dat povolení hlubší, pokročilejší analýzy datových proudů.  

* **Výstup úlohy:** odeslání výstupu z úlohy Stream Analytics přímo do SQL Data Warehouse.

V tématu [integrací se službami Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) nebo [dokumentace Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) Další informace.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
