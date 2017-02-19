---
title: "Úvod do Data Factory, služby pro integraci dat | Dokumentace Microsoftu"
description: "Dozvíte se, že Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat."
keywords: integrace dat, integrace dat v cloudu, co je azure data factory
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 757e91c3c702a62fb143f45a3144069400c5a757
ms.openlocfilehash: e2c2611d6f1094d2c1ba2bdb2398b0b17723af8a


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Úvod do služby Azure Data Factory, služby pro integraci dat v cloudu
## <a name="what-is-azure-data-factory"></a>Co je služba Azure Data Factory?
Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje **přesun** a **transformaci** dat. Pomocí služby Data Factory můžete vytvářet řešení pro integraci dat, která mohou ingestovat data z různých úložišť dat, transformovat a zpracovávat je, a výsledná data publikovat do úložišť dat.

Služba Data Factory umožňuje vytvářet datové kanály, které přesouvají a transformují data, a následně tyto kanály spouštět podle zadaného plánu (každou hodinu, denně, týdně atd.). Poskytuje také bohaté vizualizace pro zobrazení rodokmenu a závislostí mezi vašimi datovými kanály a monitorování kanálů z jednoho jednotného zobrazení pro jednodušší identifikaci problémů a nastavení výstrah monitorování.

![Diagram: Přehled služby Data Factory, služby pro integraci dat](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Obrázek&1;.** Ingestujte data z mnoha různých místních zdrojů dat, připravte, transformujte a analyzujte je, a poté publikujte data připravená k použití.

## <a name="pipelines-and-activities"></a>Kanály a aktivity
V řešení Data Factory vytváříte jeden nebo více datových **kanálů**. Kanál je logické seskupení aktivit. Používají se k seskupování aktivit, které společně provádějí úlohy, do jednotek.

**Aktivity** definují akce, které se mají provést na vašich datech. Například můžete použít aktivitu kopírování ke kopírování dat z jednoho úložiště dat do jiného. Podobně můžete použít aktivitu Hivu, která spustí dotaz Hivu na clusteru Azure HDInsight, aby transformoval a analyzoval vaše data. Data Factory podporuje dva typy aktivit: aktivity přesunu dat a transformace dat.

## <a name="data-movement-activities"></a>Aktivity přesunu dat
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Podrobnosti najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).

## <a name="data-transformation-activities"></a>Aktivity transformace dat
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Podrobnosti najdete v článku [Aktivity transformace dat](data-factory-data-transformation-activities.md).

Pokud potřebujete přesunout data do nebo z úložiště dat, které aktivita kopírování nepodporuje, nebo transformovat data pomocí vlastní logiky, vytvořte **vlastní aktivitu .NET**. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

## <a name="linked-services"></a>Propojené služby
Propojené služby definují informace potřebné pro připojení služby Data Factory k externím prostředkům (například Azure Storage, místní SQL Server, Azure HDInsight). Propojené služby slouží ve službě Data Factory ke dvěma účelům:

* Představují **úložiště dat**, k nimž mimo jiné patří například místní SQL Server, databáze Oracle, sdílená složka nebo účet Azure Blob Storage. Seznam podporovaných úložišť dat najdete v oddílu [Aktivity přesunu dat](#data-movement-activities).
* Představují **výpočetní prostředek**, který může hostovat provádění aktivity. Například aktivita HDInsightHive se spouští na clusteru HDInsight Hadoop. Seznam podporovaných výpočetních prostředí najdete v oddílu [Aktivity transformace dat](#data-transformation-activities).

## <a name="datasets"></a>Datové sady
Propojené služby propojují úložiště dat s objektem pro vytváření dat Azure. Datové sady představují datové struktury v úložištích dat. Například propojená služba Azure Storage poskytuje službě Data Factory informace o připojení, aby se mohla připojit k účtu služby Azure Storage. Datová sada služby Azure Blob určuje kontejner objektů blob a složku ve službě Azure Blob Storage, ze kterých by měl kanál číst data. Podobně propojená služba Azure SQL poskytuje informace o připojení pro databázi Azure SQL a datová sada Azure SQL určuje tabulku, která obsahuje data.   

## <a name="relationship-between-data-factory-entities"></a>Vztah mezi entitami služby Data Factory
Data Factory obsahuje několik klíčových entit, které spolupracují při definování vstupních a výstupních dat a zpracování událostí, plánu a prostředků potřebných k provedení požadovaného toku dat.

![Diagram: Data Factory, služba pro integraci dat v cloudu – Klíčové koncepty](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Obrázek 2** Vztahy mezi datovou sadou, aktivitou, kanálem a propojenou službou

S těmito čtyřmi jednoduchými koncepty propojených služeb, aktivit a kanálů jste připraveni začít! Můžete [sestavit svůj první kanál](data-factory-build-your-first-pipeline.md).

## <a name="supported-regions"></a>Podporované oblasti
V současné době můžete vytvářet datové továrny v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**. Objekt služby Data Factory nicméně může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure za účelem přesouvání dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb.

Samotná služba Azure Data Factory žádná data neuchovává. Umožňuje vám vytvořit toky řízené daty k orchestraci přesouvání dat mezi [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a zpracování dat pomocí [výpočetních služeb](data-factory-compute-linked-services.md) v jiných oblastech nebo v místním prostředí. Také vám umožňuje [monitorovat a spravovat pracovní postupy](data-factory-monitor-manage-pipelines.md) pomocí uživatelského prostředí nebo prostřednictvím kódu programu.

Přestože je služba Data Factory dostupná pouze v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**, služba pohánějící přesouvání dat ve službě Data Factory je dostupná [globálně](data-factory-data-movement-activities.md#global) v několika oblastech. V případě, že je úložiště dat umístěné za bránou firewall, se o přesouvání dat postará [Brána správy dat](data-factory-move-data-between-onprem-and-cloud.md) nainstalovaná ve vašem místním prostředí.

Předpokládejme například, že vaše výpočetní prostředí, jako je cluster Azure HDInsight nebo Azure Machine Learning, běží v oblasti Západní Evropa. Můžete vytvořit instanci služby Azure Data Factory v oblasti Severní Evropa a použít ji k plánování úloh na výpočetních prostředích v oblasti Západní Evropa. Trvá několik milisekund, než Data Factory aktivuje úlohu na výpočetním prostředí, ale čas potřebný pro vykonání úlohy na výpočetním prostředí se nemění.

V budoucnu plánujeme zajistit dostupnost služby Azure Data Factory ve všech oblastech podporovaných v Azure.

## <a name="next-steps"></a>Další kroky
Chcete-li se dozvědět, jak sestavit datové továrny s datovými kanály, postupujte podle podrobných pokynů v následujících kurzech:

| Kurz | Popis |
| --- | --- |
| [Přesun dat mezi dvěma cloudovými úložišti dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |V tomto kurzu vytvoříte objekt pro vytváření dat s kanálem, který **přesouvá data** ze služby Blob Storage do SQL Database. |
| [Transformace dat pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md) |V tomto kurzu sestavíte svůj první objekt pro vytváření dat Azure s datovým kanálem, který **zpracovává data** pomocí skriptu Hive v clusteru Azure HDInsight (Hadoop). |
| [Přesun dat mezi místním úložištěm dat a cloudovým úložištěm dat pomocí brány správy dat](data-factory-move-data-between-onprem-and-cloud.md) |V tomto kurzu sestavíte objekt pro vytváření dat s kanálem, který **přesouvá data** z databáze **místního** systému SQL Server do objektu blob Azure. Jako součást návodu nainstalujete a nakonfigurujete ve svém počítači Bránu správy dat. |



<!--HONumber=Jan17_HO4-->


