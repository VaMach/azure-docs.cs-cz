---
title: Co je Azure Databricks? | Dokumentace Microsoftu
description: "Další informace o co je Azure Databricks a jak přináší Spark na Databricks do Azure. Azure Databricks je optimalizovaná pro platformu Microsoft Azure cloud services platforma Apache Spark na základě analýzy."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: 7ced38cda2669cf03e51f50fbbbeea0344da9277
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="what-is-azure-databricks"></a>Co je Azure Databricks?

Azure Databricks je optimalizovaná pro platformu Microsoft Azure cloud services platforma Apache Spark na základě analýzy. Navržený s umístit Apache Spark, Databricks jsou integrované s Azure zajistit instalaci jedním kliknutím, zjednodušenou pracovní postupy a interaktivní pracovní prostor, který umožňuje spolupráci mezi datových vědců, technici dat a obchodní analytici.

![Co je Azure Databricks? ] (./media/what-is-azure-databricks/azure-databricks-overview.png "Co je Azure Databricks?")

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark na základě analýzy platformy

Azure Databricks obsahuje kompletní open source Apache Spark clusteru technologie a možnosti. Spark v Azure Databricks zahrnuje následující součásti:

![Apache Spark v Azure Databricks](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Apache Spark v Azure Databricks")

* **Spark SQL a DataFrames**: Spark SQL je modul Spark pro práci s strukturovaná data. DataFrame je distribuované kolekce dat, které jsou uspořádány do pojmenované sloupce. Je ekvivalentní k tabulce v relační databáze nebo data rámce v R nebo Python.

* **Streamování**: v reálném čase zpracování dat a analýza pro aplikace pro analýzu a interaktivní. Se integruje s HDFS, Flume a Kafka.

* **MLib**: Machine Learning knihovny, který se skládá z běžných informací algoritmy a nástrojů, včetně klasifikace, regrese, clustering, spolupráce filtrování, dimenzionalitu snížení, jakož i základní optimalizace primitiv.

* **GraphX**: grafy a výpočetní graf pro široký rozsah případů ze kognitivní analytics zkoumání dat použít.

* **Spark Core API**: zahrnuje podporu pro R, SQL, Python, Scala a Java.

## <a name="apache-spark-in-azure-databricks"></a>Apache Spark v Azure Databricks

Azure Databricks založený na možnostech Spark tím, že poskytuje správu nula cloudové platformy, která zahrnuje:

- Plně spravované clustery Spark
- Interaktivní pracovní prostor pro zkoumání a vizualizaci
- Platforma pro pohánějící vaše oblíbené aplikace založené na Spark

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Plně spravované clustery Apache Spark v cloudu

Azure Databricks má zabezpečený a spolehlivý provozním prostředí v cloudu, spravované a podporované odborníky Spark. Můžete:

* Vytvoření clusterů v sekundách.
* Dynamicky škálování nahoru a dolů clusterů, včetně clusterů bez serveru a jejich sdílení mezi týmy. 
* Pomocí clusteru programově pomocí rozhraní REST API. 
* Použijte zabezpečení dat integrace možnosti postavenou na Spark umožňující sjednocení data bez centralizace. 
* Získáte okamžitý přístup k nejnovějším funkcím Apache Spark při každém vydání.

### <a name="databricks-runtime"></a>Modul Databricks Runtime
Modul Databricks Runtime je postavená na Apache Spark a je nativně vytvořené pro cloudu Azure. 

Pomocí **bez serveru** možnost, Azure Databricks úplně abstrahuje složitosti infrastruktury a potřebu specializované znalosti, jak připravit a nakonfigurovat infrastrukturu data. Bez serveru možnost pomáhá data, která vědců rychle iterovat jako tým.

Pro data technici, kteří jsou pro vás o výkon provozní úlohy, Azure Databricks poskytuje Spark modul, který je rychlejší a původce prostřednictvím různých optimalizace na vrstvu vstupně-výstupních operací a zpracování vrstvu (Databricks vstupně-výstupní).

### <a name="workspace-for-collaboration"></a>Pracovní prostor pro spolupráci

Prostřednictvím prostředí spolupráce a integrované Azure Databricks zjednodušuje proces zkoumání dat, při vytváření prototypu a spuštění datové aplikace v Spark.

* Určete, jak se data s zkoumání dat snadno použít.
* Zdokumentujte průběh v poznámkových bloků v R, Python, Scala nebo SQL.
* Vizualizovat data v několika kliknutí a pomocí známých nástrojů, jako je Matplotlib, ggoplot nebo d3.
* Vytvářet dynamické sestavy pomocí interaktivních řídicích panelů.
* Použijte Spark a pracovat s daty současně.

## <a name="enterprise-security"></a>Zabezpečení Enterprise

Azure Databricks poskytuje podnikové úrovni zabezpečení Azure, včetně integrace služby Azure Active Directory, ovládací prvky založené na rolích a SLA, které chrání vaše data a vaší firmy.

* Integrace s Azure Active Directory umožňuje spustit kompletní řešení založená na Azure pomocí Azure Databricks.
* Přístupu na základě role Azure Databricks umožňuje podrobné uživatelských oprávnění pro poznámkové bloky, clustery, úlohy a data.
* SLA na podnikové úrovni. 

## <a name="integration-with-azure-services"></a>Integrace se službami Azure

Azure Databricks úzce integruje s Azure databáze a úložiště: SQL Data Warehouse, Cosmos DB, Data Lake Store a úložiště objektů Blob. 

## <a name="integration-with-power-bi"></a>Integrace s Power BI
Díky bohaté integraci s nástrojem Power BI umožňuje Azure Databricks vyhledat a sdílet zvládat přehledy snadno a rychle. Můžete taky jiné nástroje BI například Tableau Software prostřednictvím JDBC nebo ODBC koncovým bodům clusteru.

## <a name="next-steps"></a>Další kroky

* [Rychlý úvod: Spustit úlohu Spark v Azure Databricks](quickstart-create-databricks-workspace-portal.md)
* [Práce s clustery Spark](https://docs.azuredatabricks.net/user-guide/clusters/index.html)
* [Práce s poznámkových bloků](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)
* [Vytváření úloh Spark](https://docs.azuredatabricks.net/user-guide/jobs.html)

 









