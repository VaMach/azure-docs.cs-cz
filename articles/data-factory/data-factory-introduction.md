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
ms.date: 04/21/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 260208e7c7a08110eb3c885ef86ec4c18ff42fc9
ms.openlocfilehash: 40552b5d3cea5b04826c08e7b4b1d046a9fcefba
ms.lasthandoff: 04/23/2017


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Úvod do služby Azure Data Factory, služby pro integraci dat v cloudu
## <a name="what-is-azure-data-factory"></a>Co je služba Azure Data Factory?
Jak se ve světě velkých objemů dat využívají existující data ve firmách? Je možné rozšířit data generovaná v cloudu pomocí referenčních dat z místních zdrojů dat nebo jiných různorodých zdrojů dat? Proto je potřeba platforma pro shromažďování a zpracování dat z široké škály zdrojů. Azure Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje **přesun** a **transformaci** dat. Můžete vytvářet řešení pro integraci dat, která mohou ingestovat vstupní data z různorodých úložišť dat, transformovat a zpracovávat je, a výstupní data publikovat do dalších úložišť dat. 

![Diagram: Přehled služby Data Factory, služby pro integraci dat](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Obrázek 1.** Ingestujte data z mnoha různých místních zdrojů dat, připravte, transformujte a analyzujte je, a poté publikujte data připravená k použití.


## <a name="what-does-it-offer"></a>Co nabízí? 
Projekty integrace dat se tradičně zabývají vytvářením procesů extrakce, transformace a načítání (ETL), které extrahují data z různých zdrojů v rámci organizace, transformují je, aby byly v souladu s cílovým schématem podnikového datového skladu, a načítají je do podnikového datového skladu, jak je znázorněno na následujícím obrázku. K podnikovému datovému skladu se pak přistupuje jako k jedinému zdroji pravdivých informací pro řešení analýzy BI.

![Tradiční ETL](media/data-factory-introduction/traditional-etl.png)
**Tradiční ETL**

Dnešní prostředí dat pro podniky neustále exponenciálně roste v objemu, rozmanitosti a složitosti, jak je znázorněno na následujícím obrázku. Díky různým formám a rychlostem vzniku místních a cloudových dat je různorodější než kdy jindy. Zpracování dat se musí odehrávat v různých zeměpisných umístěních a zahrnuje kombinaci open source softwaru, komerčních řešení a vlastních služeb zpracování, jejichž integrace a údržba je nákladná a složitá. Flexibilita potřebná k přizpůsobení se dnešnímu měnícímu se prostředí velkých objemů dat je příležitostí ke spojení tradičních podnikových datových skladů s možnostmi, které vyžaduje moderní systém vytváření informací. Azure Data Factory je kompoziční platforma, která pracuje s tradičními podnikovými datovými sklady a měnícím se prostředím dat, aby umožnila podnikům využívat veškerá dostupná data k rozhodování založeném na datech.

![Nové prostředí velkých objemů dat](media/data-factory-introduction/new-big-data-landscape.png)
**Nové prostředí velkých objemů dat**

Služba Azure Data Factory umožňuje podnikům využívat tuto různorodost tím, že poskytuje platformu pro **sestavování kanálů vytváření informací zahrnujících služby pro zpracování, ukládání a přesun dat**, a pro správu důvěryhodných datových prostředků.

Služba Azure Data Factory umožňuje:
- **Jednoduše pracovat s různými systémy pro ukládání a zpracování dat**. 

    Podniky mají data nejrůznějších typů, která se nachází v různých zdrojích. Prvním krokem při sestavování systému vytváření informací je připojení ke všem požadovaným zdrojům dat a zpracování, jako jsou například služby SaaS, sdílené složky, FTP nebo webové služby, a přesun dat podle potřeby do centralizovaného umístění pro následné zpracování.

    Bez služby Data Factory musí podniky sestavovat vlastní komponenty pro přesun dat nebo vyvíjet vlastní služby pro integraci a zpracování těchto zdrojů dat. To je nákladné a integrace a údržba takových systémů je složitá. Kromě toho často postrádají monitorování a upozorňování na podnikové úrovni a ovládací prvky, které může nabídnout plně spravovaná služba.

    Se službou Data Factory můžete použít aktivitu kopírování v datovém kanálu pro přesun dat z místních i cloudových zdrojů dat do centralizovaného úložiště v cloudu pro další analýzu. Například můžete shromažďovat data v Azure Data Lake Store a později je transformovat pomocí výpočetní služby Azure Data Lake Analytics. Nebo můžete data shromažďovat v Azure Blob Storage a později je transformovat pomocí clusteru Azure HDInsight Hadoop.
- **Transformovat data na důvěryhodné informace**. 

    Jakmile budou data v centralizovaném úložišti dat v cloudu, budete chtít vytvořit a nasadit datové kanály pro spolehlivé vytváření transformovaných dat podle udržitelného a řízeného plánu, abyste mohli do produkčních prostředí dodávat důvěryhodná data. Transformaci dat v Azure Data Factory provádí aktivity transformace, jako je například Hive, Pig, MapReduce nebo aktivita Provedení dávky služby Azure Machine Learning a vlastní aktivity v jazyce C# spuštěné v clusteru Azure HDInsight Hadoop, na virtuálních počítačích se službou Azure Machine Learning nebo ve fondu virtuálních počítačů Azure Batch.
- **Monitorovat datové kanály na jednom místě**.

    S různorodým portfoliem dat je důležité mít spolehlivý a úplný přehled o službách pro ukládání, zpracování a přesun dat. Data Factory vám pomůže rychle vyhodnotit komplexní stav datového kanálu, určit problémy a v případě potřeby provést opravné akce. Vizuálně sledujte data a vztahy mezi daty napříč libovolnými zdroji. Zobrazte úplné historické záznamy o spouštění úloh, stavu systému a závislostech z jediného monitorovacího řídicího panelu.

## <a name="how-does-it-work"></a>Jak to funguje? 
V Azure Data Factory se vytváření informací dělí na tři fáze:

![Tři fáze vytváření informací](media/data-factory-introduction/three-information-production-stages.png)

- **Připojení a shromažďování:** V této fázi se data z různých zdrojů shromažďují na jednom místě.
- **Transformace a rozšíření:** V této fázi se shromážděná data zpracovávají nebo transformují.
- **Publikování**. V této fázi se data publikují, aby je mohly využívat nástroje BI, analytické nástroje a další aplikace.

## <a name="key-components"></a>Klíčové komponenty
Předplatné Azure může obsahovat jednu nebo více instancí služby Azure Data Factory (neboli datových továren). Azure Data Factory se skládá ze čtyř klíčových komponent, které společně poskytují platformu, na které můžete pro váš tok dat vytvářet jednoduché i komplexní orchestrace přesunu a transformace dat.

### <a name="activity"></a>Aktivita
Aktivity definují akce, které se mají provést na vašich datech. Například můžete použít aktivitu kopírování ke kopírování dat z jednoho úložiště dat do jiného. Podobně můžete použít aktivitu Hivu, která spustí dotaz Hivu na clusteru Azure HDInsight, aby transformoval a analyzoval vaše data. Data Factory podporuje dva typy aktivit: aktivity přesunu dat a transformace dat.

Každá aktivita může mít nula nebo více vstupních datových sad a může generovat jednu nebo více výstupních datových sad. 


### <a name="data-movement-activities"></a>Aktivity přesunu dat
Aktivita kopírování ve službě Data Factory kopíruje data ze zdrojového úložiště dat do úložiště dat jímky. Data Factory podporuje následující typy úložišť dat. Data z libovolného zdroje lze zapsat do libovolné jímky. Kliknutím na úložiště dat se dozvíte, jak kopírovat data z a do daného úložiště.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Podrobnosti najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Aktivity transformace dat
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Podrobnosti najdete v článku [Aktivity transformace dat](data-factory-data-transformation-activities.md).

Pokud potřebujete přesunout data do nebo z úložiště dat, které aktivita kopírování nepodporuje, nebo transformovat data pomocí vlastní logiky, vytvořte **vlastní aktivitu .NET**. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

### <a name="pipeline"></a>Kanál
Kanál je skupina aktivit. Aktivity v kanálu společně provádí úlohy. Kanál může například obsahovat skupinu aktivit, které ingestují data z objektu blob Azure a pak na clusteru HDInsight spustí dotaz Hive pro rozdělení dat protokolu. Výhodou tohoto přístupu je, že vám kanál umožňuje spravovat aktivity jako sadu, a ne každou jednotlivě. Například můžete nasadit a naplánovat kanál namísto jednotlivých aktivit.

### <a name="datasets"></a>Datové sady
Datové sady představují datové struktury v rámci úložišť dat, které jednoduše odkazují na data, která chcete ve vašich aktivitách použít jako vstupy nebo výstupy. Datová sada služby Azure Blob například určuje kontejner objektů blob a složku ve službě Azure Blob Storage, ze kterých by měl kanál číst data. 

### <a name="linked-services"></a>Propojené služby
Propojené služby jsou velmi podobné připojovacím řetězcům, které definují informace o připojení, které služba Data Factory potřebuje pro připojení k externím prostředkům. Můžete si to představit tak, že datová sada reprezentuje strukturu dat a propojená služba definuje připojení ke zdroji dat.  Propojené služby slouží ve službě Data Factory ke dvěma účelům:

* Představují **úložiště dat**, k nimž mimo jiné patří například místní SQL Server, databáze Oracle, sdílená složka nebo účet Azure Blob Storage. Seznam podporovaných úložišť dat najdete v oddílu [Aktivity přesunu dat](#data-movement-activities).
* Představují **výpočetní prostředek**, který může hostovat provádění aktivity. Například aktivita HDInsightHive se spouští na clusteru HDInsight Hadoop. Seznam podporovaných výpočetních prostředí najdete v oddílu [Aktivity transformace dat](#data-transformation-activities).

### <a name="relationship-between-data-factory-entities"></a>Vztah mezi entitami služby Data Factory
![Diagram: Data Factory, služba pro integraci dat v cloudu – Klíčové koncepty](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Obrázek 2** Vztahy mezi datovou sadou, aktivitou, kanálem a propojenou službou

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

