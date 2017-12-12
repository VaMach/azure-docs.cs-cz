---
title: "Porovnání verzí 1 a 2 služby Azure Data Factory | Dokumentace Microsoftu"
description: "Tento článek porovnává verze 1 a 2 služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: kromerm
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: makromer
ms.openlocfilehash: f55348e9974de17c6d7e704e185f1ea9b21ff66e
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="compare-azure-data-factory-versions-1-and-2"></a>Porovnání verzí 1 a 2 služby Azure Data Factory
Tento článek porovnává verzi 2 (V2) s verzí 1 (V1) služby Azure Data Factory. Pokud se chcete seznámit s verzí 1, přečtěte si [úvod ke službě Data Factory verze 1](v1/data-factory-introduction.md). Pokud se chcete seznámit s verzí 2, přečtěte si [úvod ke službě Data Factory verze 2](introduction.md).

## <a name="feature-comparison"></a>Porovnání funkcí
Následující tabulka porovnává funkce V1 a V2. 

| Funkce | V1 | V2 | 
| ------- | --------- | --------- | 
| Datové sady | Pojmenované zobrazení dat odkazující na data, která chcete ve svých aktivitách použít jako vstupy a výstupy. Datové sady identifikují data v rámci různých úložišť dat, jako jsou tabulky, soubory, složky a dokumenty. Datová sada objektu blob Azure například určuje kontejner objektů blob a složku v úložišti objektů blob, ze kterých by měla aktivita číst data.<br/><br/>**Dostupnost** definuje model dělení časového intervalu pro zpracování (například hodiny, dny atd.). | Datové sady jsou ve V2 stejné. Nemusíte ale pro datové sady definovat plány **dostupnosti**. Můžete definovat aktivační prostředek, který může naplánovat kanály z paradigmatu plánovače hodin. Další informace najdete v tématech [Aktivační události](concepts-pipeline-execution-triggers.md#triggers) a [Datové sady](concepts-datasets-linked-services.md). | 
| Propojené služby | Propojené služby jsou velmi podobné připojovacím řetězcům, které definují informace o připojení, které služba Data Factory potřebuje pro připojení k externím prostředkům. | Stejné jako u Data Factory V1, ale s novou vlastností **connectVia**, která využívá výpočetní prostředí Integration Runtime Data Factory V2. Další informace najdete v tématech věnovaných [prostředím Integration Runtime](concepts-integration-runtime.md) a [vlastnostem propojených služeb pro Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties). |
| Kanály | Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál je logické seskupení aktivit, které dohromady provádějí určitou úlohu. Pro naplánování a spuštění kanálů jste použili startTime, endTime, isPaused. | Kanály jsou neměnné skupiny aktivit, které mají být provedeny s daty. Plánování aktivit v kanálu je ale rozdělené do nových prostředků aktivačních událostí. Kanály v Data Factory V2 si můžete představit spíš jako „jednotky pracovních postupů“, které plánujete samostatně prostřednictvím aktivačních událostí. <br/><br/>Kanály v Data Factory V2 nevyužívají spouštění v „časových intervalech“. Koncepce Data Factory V1 pro startTime, endTime a isPaused se ve verzi Data Factory V2 už nevyužívají. Další informace najdete v tématech [Spouštění kanálů a aktivační události](concepts-pipeline-execution-triggers.md) a [Kanály a aktivity](concepts-pipelines-activities.md). |
| Aktivity | Aktivity definují akce, které se mají v kanálu provést s vašimi daty. Podporují se aktivity přesunu dat (aktivita kopírování) a transformace dat (jako je Hive, Pig a MapReduce). | Aktivity ve verzi Data Factory V2 jsou nadále definované akce v rámci kanálu. V2 zavádí nové [aktivity toku řízení](concepts-pipelines-activities.md#control-activities). Tyto aktivity se využívají v toku řízení (smyčky a větvení). Aktivity přesunu a transformace dat podporované ve V1 se podporují i ve V2. Ve verzi V2 můžete definovat aktivity transformace bez použití datových sad. |
| Hybridní přesun dat a odesílání aktivit | Dřív se používalo označení [brána správy dat](v1/data-factory-data-management-gateway.md). Podporuje přesun dat mezi místním prostředím a cloudem. Nově se používá označení Integration Runtime.| Brána správy dat se teď označuje jako Integration Runtime v místním prostředí. Poskytuje stejné funkce jako ve V1. <br/><br/> Azure-SSIS Integration Runtime ve V2 také podporuje nasazování a spouštění balíčků služby SQL Server Integration Services (SSIS) v cloudu. Podrobnosti najdete v článku [Integration Runtime](concepts-integration-runtime.md).|
| Parametry | Není k dispozici | Parametry jsou páry klíč-hodnota nastavení konfigurace, které jsou jen pro čtení a jsou definované v kanálech. Pokud kanál spouštíte ručně, můžete předat argumenty pro tyto parametry. Pokud používáte aktivační událost plánovače, může tato aktivační událost také předávat hodnoty parametrů. Aktivity v rámci kanálu využívají hodnoty parametrů.  |
| Výrazy | Data Factory V1 umožňuje používat funkce a systémové proměnné v dotazech pro výběr dat a ve vlastnostech aktivit nebo datových sad. | V Data Factory V2 můžete používat výrazy kdekoli v řetězcové hodnotě JSON. Další informace najdete v tématu věnovaném [výrazům a funkcím ve verzi V2](control-flow-expression-language-functions.md).|
| Spuštění kanálu | Není k dispozici | Jedna instance spuštění kanálu. Například předpokládejme, že máte kanál, který se provádí v 8:00, 9:00 a 10:00. V tomto případě by tedy existovala tři samostatná spuštění kanálu. Každé spuštění kanálu má jedinečné ID spuštění kanálu, což je identifikátor GUID, který jedinečně určuje konkrétní spuštění kanálu. Instance spuštění kanálu se obvykle vytvářejí předáváním argumentů do parametrů definovaných v kanálech. |
| Spuštění aktivit | Není k dispozici | Instance spuštění aktivity v kanálu. | 
| Spuštění aktivačních událostí | Není k dispozici | Instance spuštění aktivační události. Další informace najdete v tématu [Aktivační události](concepts-pipeline-execution-triggers.md). |
| Plánování | Plánování je založené na dostupnosti datové sady a počátečním/koncovém času kanálu. | Aktivační událost plánovače nebo spuštění prostřednictvím externího plánovače. Další informace najdete v tématu [Spouštění kanálů a aktivační události](concepts-pipeline-execution-triggers.md). |

Následující části poskytují další informace o možnostech verze 2: 

## <a name="control-flow"></a>Tok řízení  
V zájmu podpory rozmanitých toků a vzorů integrace v moderních datových skladech služba Data Factory V2 zajišťuje nový flexibilní model datového kanálu, který už není svázaný s daty časových řad. Mezi běžné toky, které dříve nebyly podporované, ale teď jsou, patří například tyto:   

### <a name="chaining-activities"></a>Řetězení aktivit
Pokud jste chtěli použít řetězení ve verzi 1, museli jste nakonfigurovat výstup aktivity jako vstup jiné aktivity. Ve verzi 2 můžete v rámci kanálu řetězit aktivity v sekvenci. Můžete v definici aktivity použít vlastnost **dependsOn** pro řetězení s upstreamovou aktivitou. Další informace a příklad najdete v článcích [Kanály a aktivity](concepts-pipelines-activities.md#multiple-activities-in-a-pipeline) a [Větvení a řetězení aktivit](tutorial-control-flow.md). 

### <a name="branching-activities"></a>Větvení aktivit
Ve verzi V2 můžete nově větvit aktivity v rámci kanálu. [Aktivita podmínky If](control-flow-if-condition-activity.md) funguje stejně jako příkaz `if` v programovacích jazycích. Vyhodnotí sadu aktivit, když se podmínka vyhodnotí jako `true`, a jinou sadu aktivit, když se podmínka vyhodnotí jako `false`. Příklady větvení aktivit najdete v kurzu [Větvení a řetězení aktivit](tutorial-control-flow.md).

### <a name="parameters"></a>Parametry 
Parametry můžete definovat na úrovni kanálu a při volání kanálu na vyžádání nebo pomocí aktivační události potom předávat argumenty. Aktivity mohou využívat argumenty předávané do kanálu. Další informace najdete v tématu [Kanály a aktivační události](concepts-pipeline-execution-triggers.md). 

### <a name="custom-state-passing"></a>Předávání vlastního stavu
Výstupy aktivity včetně stavu může používat následující aktivita v kanálu. V definici JSON aktivity můžete například pomocí následující syntaxe získat přístup k výstupu předchozí aktivity: `@activity('NameofPreviousActivity').output.value`. Tato funkce umožňuje sestavovat pracovní postupy, kde se mezi aktivitami mohou předávat hodnoty.

### <a name="looping-containers"></a>Kontejnery smyček
[Aktivita ForEach](control-flow-for-each-activity.md) definuje opakovaný tok řízení ve vašem kanálu. Tato aktivita se používá k opakování v kolekci a spouští zadané aktivity ve smyčce. Implementace smyčky této aktivity se podobá struktuře smyčky Foreach v programovacích jazycích. 

Aktivita [Until](control-flow-until-activity.md) funguje stejně jako smyčka Do-Until v programovacích jazycích. Provádí ve smyčce sadu aktivit, dokud se podmínka přidružená k aktivitě nevyhodnotí jako pravdivá. Ve službě Data Factory můžete pro aktivitu Until určit hodnotu časového limitu.  

### <a name="trigger-based-flows"></a>Toky založené na aktivačních událostech
Kanály se můžou spouštět na vyžádání nebo v nastavenou hodinu. Podrobné informace o aktivačních událostech najdete v článku věnovaném [kanálům a aktivačním událostem](concepts-pipeline-execution-triggers.md). 

### <a name="invoke-a-pipeline-from-another-pipeline"></a>Vyvolání kanálu z jiného kanálu
[Aktivita spuštění kanálu](control-flow-execute-pipeline-activity.md) umožňuje kanálu služby Data Factory volat jiný kanál.

### <a name="delta-flows"></a>Toky rozdílů
Klíčovým případem použití ve vzorech ETL jsou „rozdílová načtení“, to znamená načtení jenom těch dat, která se od poslední iterace kanálu změnila. Nové možnosti ve verzi 2, jako jsou [aktivita vyhledávání](control-flow-lookup-activity.md), flexibilní plánování a tok řízení, přirozeným způsobem podporují tento případ použití. Kurz s podrobnými pokyny najdete tady: [Tutorial: Incremental copy](tutorial-incremental-copy-powershell.md) (Kurz: Vytváření přírůstkových kopií).

### <a name="other-control-flow-activities"></a>Další aktivity toku řízení
Data Factory V2 podporuje několik dalších aktivit toku řízení: 

Aktivita řízení | Popis
---------------- | -----------
[ForEachActivity](control-flow-for-each-activity.md) | Aktivita ForEach definuje ve vašem kanálu opakovaný tok řízení. Tato aktivita se používá k opakování v kolekci a spouští zadané aktivity ve smyčce. Implementace smyčky této aktivity se podobá struktuře smyčky Foreach v programovacích jazycích.
[WebActivity](control-flow-web-activity.md) | Webová aktivita slouží k volání vlastního koncového bodu REST z kanálu služby Data Factory. Můžete předávat datové sady a propojené služby, které má aktivita používat a ke kterým má mít přístup. 
[Aktivita vyhledávání](control-flow-lookup-activity.md) | Aktivita vyhledávání slouží ke čtení nebo vyhledání záznamu / názvu tabulky / hodnoty z jakéhokoli externího zdroje. Na tento výstup mohou dále odkazovat následující aktivity. 
[Aktivita GetMetadata](control-flow-get-metadata-activity.md) | Aktivita GetMetadata slouží k načtení metadat jakýchkoli dat ve službě Azure Data Factory. 
[Aktivita Wait](control-flow-wait-activity.md) | Kanál zadanou dobu čeká (pozastaví se).

## <a name="deploy-ssis-packages-to-azure"></a>Nasazení balíčků služby SSIS do Azure 
Pokud chcete přesunout úlohy služby SSIS (SQL Server Integration Services) do cloudu, vytvořte službu Data Factory V2 a zřiďte prostředí Azure-SSIS Integration Runtime (IR). Azure-SSIS IR je plně spravovaný cluster virtuálních počítačů Azure (uzlů) vyhrazených ke spouštění balíčků služby SSIS v cloudu. Jakmile zřídíte Azure-SSIS IR, můžete stejné nástroje použít pro nasazení balíčků SSIS do místního prostředí SSIS. Můžete například pomocí nástrojů SQL Server Data Tools (SSDT) nebo SQL Server Management Studio (SSMS) nasadit balíčky SSIS do tohoto modulu runtime v Azure. Podrobné pokyny najdete v kurzu [Nasazení balíčků služby SSIS (SQL Server Integration Services) do Azure](tutorial-deploy-ssis-packages-azure.md). 

## <a name="flexible-scheduling"></a>Flexibilní plánování
V Data Factory V2 nemusíte pro datové sady definovat plány dostupnosti. Můžete definovat aktivační prostředek, který může naplánovat kanály z paradigmatu plánovače hodin. Pro zajištění flexibilního modelu plánování/spuštění můžete také kanálům předat parametry z aktivační události. Kanály v Data Factory V2 nevyužívají spouštění v „časových intervalech“. Koncepce Data Factory V1 pro startTime, endTime a isPaused se ve verzi Data Factory V2 už nevyužívají. Tady je postup sestavení a naplánování kanálu ve službě Data Factory V2: [Spouštění kanálů a aktivační události](concepts-pipeline-execution-triggers.md).

## <a name="support-for-more-data-stores"></a>Podpora pro další úložiště dat
V2 podporuje kopírování dat do a z většího počtu úložišť dat než V1. Seznam podporovaných úložišť dat najdete v následujících článcích:

- [V1 – Podporované zdroje dat](v1/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
- [V2 – Podporované zdroje dat](copy-activity-overview.md#supported-data-stores-and-formats)

## <a name="support-for-on-demand-spark-cluster"></a>Podpora pro cluster Spark na vyžádání
V2 podporuje vytvoření clusteru HDInsight Spark na vyžádání. Pokud chcete vytvořit cluster Spark na vyžádání, v definici propojené služby HDInsight na vyžádání jako typ clusteru zadejte Spark. Potom můžete nakonfigurovat aktivitu Spark ve vašem kanálu tak, aby používala tuto propojenou službu. Při spuštění této aktivity za běhu služba Data Factory automaticky vytvoří cluster Spark. Další informace najdete v následujících článcích:

- [Aktivita Spark ve V2](transform-data-using-spark.md)
- [Propojená služba Azure HDInsight na vyžádání](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)

## <a name="custom-activities"></a>Vlastní aktivity
Ve V1 implementujete kód aktivity DotNet (vlastní) tak, že vytvoříte projekt knihovny tříd .NET se třídou, která implementuje metodu Execute rozhraní IDotNetActivity. Proto váš vlastní kód musí být napsaný v .NET Frameworku 4.5.2 a spouštět se na uzlech provést na uzlech fondu služby Azure Batch se systémem Windows. 

Pro vlastní aktivity ve V2 není nutné implementovat rozhraní .NET. Teď můžete přímo spouštět příkazy, skripty a vlastní kód, který je zkompilovaný jako spustitelný soubor. 

Další informace najdete v tématu věnovaném [rozdílu mezi vlastní aktivitou ve verzi V1 a V2](transform-data-using-dotnet-custom-activity.md#difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1).

## <a name="sdks"></a>Sady SDK
Veze 2 služby Data Factory poskytuje širší nabídku sad SDK, které je možné využít k vytváření, správě a monitorování kanálů.

- **.NET SDK:** Sada .NET SDK je pro V2 aktualizovaná. 
- **PowerShell:** Rutiny PowerShellu jsou pro V2 aktualizované. Rutiny V2 mají v názvu **DataFactoryV2**. Například: Get-AzureRmDataFactoryV2. 
- **Python SDK:** Tato sada SDK je ve V2 nová.
- **REST API:** Rozhraní REST API je pro V2 aktualizované. 

Sady SDK aktualizované pro V2 nejsou zpětně kompatibilní s klienty verze 1. 

## <a name="authoring-experience"></a>Prostředí pro vytváření
Data Factory V1 umožňuje vytvářet kanály pomocí Data Factory Editoru na webu Azure Portal. Data Factory V2 momentálně podporuje jenom programové vytváření datových továren (.NET SDK, REST API, PowerShell, Python atd.). Podpora uživatelského rozhraní ještě není dostupná.  Data Factory V1 pro vytváření také podporuje SDK, REST a PowerShell.

## <a name="monitoring-experience"></a>Prostředí pro monitorování
Ve V2 můžete k monitorování datových továren použít také [Azure Monitor](monitor-using-azure-monitor.md). Nové rutiny PowerShellu podporují monitorování prostředí [Integration Runtime](monitor-integration-runtime.md). Verze V1 i V2 podporují vizuální monitorování prostřednictvím aplikace, která se dá spustit z webu Azure Portal.


## <a name="next-steps"></a>Další kroky
Naučte se vytvořit datovou továrnu na základě podrobných pokynů v následujících průvodcích pro rychlý start: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST API](quickstart-create-data-factory-rest-api.md). 
