<properties 
    pageTitle="Co je služba Data Factory? Služba pro integraci dat | Microsoft Azure" 
    description="Dozvíte se, že Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat." 
    keywords="integrace dat, integrace dat v cloudu, co je azure data factory"
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="07/12/2016" 
    ms.author="spelluru"/>

# Úvod do služby Azure Data Factory, služby pro integraci dat v cloudu

## Co je služba Azure Data Factory? 
Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat. Stejně jako výrobní továrna, která spouští zařízení, ta vezmou suroviny a transformují je na hotové výroby, Data Factory orchestruje stávající služby, které sbírají nezpracovaná data a transformují je na informace připravené k použití. 

Data Factory funguje napříč místními a cloudovými zdroji dat a SaaS a ingestuje, připravuje, transformuje, analyzuje a publikuje vaše data.  Využijte Data Factory k uspořádání služeb do spravovaných kanálů toku dat pro transformaci dat pomocí služeb, jako například [Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) a [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) pro potřeby zpracování velkého objemu dat, a [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) pro uvádění vlastních analytických řešení do praxe.  Přejděte od pouhého tabulkového zobrazení monitorování k využití bohatých vizualizací služby Data Factory pro rychlé zobrazení rodokmenu a závislostí mezi vlastními kanály dat. Monitorujte všechny svoje kanály toku dat z jednoho jednotného zobrazení pro jednodušší identifikaci problémů a nastavení monitorování výstrah.

![Diagram: Přehled služby Data Factory, služby pro integraci dat](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Obrázek 1.** Shromažďujte data z mnoha různých místních zdrojů dat, ingestujte a připravte je, organizujte a analyzujte je pomocí řady transformací, a poté publikujte data připravená k použití.

Službu Data Factory můžete využít kdykoli potřebujete shromáždit data různých tvarů a velikostí, transformovat je, a publikovat je k jejich hlubšímu porozumění – to vše na základě spolehlivého plánu. Data Factory slouží k vytváření vysoce dostupných kanálů toku dat pro mnoho scénářů napříč různými odvětvími pro potřeby kanálu analýzy.  Prodejci na internetu službu používají k vytváření přizpůsobených [doporučení produktů](data-factory-product-reco-usecase.md) na základě chování zákazníků při procházení. Herní studia ji používají k pochopení [účinnosti marketingových kampaní](data-factory-customer-profiling-usecase.md). Prohlédněte si [Zákaznické případové studie](data-factory-customer-case-studies.md) a zjistěte přímo od našich zákazníků, jak a proč používají Data Factory. 

> [AZURE.VIDEO azure-data-factory-overview]

## Klíčové koncepty

Azure Data Factory obsahuje několik klíčových entit, které spolupracují při definování vstupních a výstupních dat, událostí zpracování, plánu a prostředků potřebných k provedení požadovaného toku dat.

![Diagram: Data Factory, služba pro integraci dat v cloudu – základní koncepce](./media/data-factory-introduction/data-integration-service-key-concepts.png)

**Obrázek 2.** Vztahy mezi datovou sadou, aktivitou, kanálem a propojenou službou


### Aktivity
Aktivity definují akce, které se mají provést na vašich datech. Každá aktivita přijímá jako vstup nula nebo více [datových sad](data-factory-create-datasets.md) a produkuje jednu nebo více datových sad jako výstup. Aktivita je jednotka orchestrace v Azure Data Factory. Například můžete použít [aktivitu Kopírování](data-factory-data-movement-activities.md) k orchestraci kopírování dat z jedné datové sady do jiné. Podobně můžete použít [aktivitu Hivu](data-factory-data-transformation-activities.md), která spustí dotaz Hivu na clusteru Azure HDInsight, aby transformoval a analyzoval vaše data. Azure Data Factory poskytuje širokou škálu aktivit transformace, analýzy a přesouvání dat. 

### Kanály
[Kanály](data-factory-create-pipelines.md) jsou logická seskupení aktivit. Používají se k seskupování aktivit, které společně provádějí úlohy, do jednotek. Například může být potřeba posloupnost několika aktivit transformace k vyčištění dat ze souboru protokolu.  Tato posloupnost by mohla obsahovat komplexní plán a závislosti, které je potřeba orchestrovat a automatizovat. Všechny tyto aktivity by mohly být seskupené do jednoho kanálu s názvem „CleanLogFiles“.  Kanál „CleanLogFiles“ by poté mohl být nasazen, naplánován nebo odstraněn jako jedna jednotka místo jednotlivého spravování každé aktivity.

### Datové sady
[Datové sady](data-factory-create-datasets.md) jsou pojmenované odkazy/ukazatele na data, která chcete použít jako vstup nebo výstup aktivity. Datové sady identifikují datové struktury v rámci různých úložišť dat včetně tabulek, souborů, složek a dokumentů.

### Propojená služba
Propojené služby definují informace potřebné pro připojení služby Data Factory k externím prostředkům.  Propojené služby slouží ve službě Data Factory ke dvěma účelům:

- Představují úložiště dat, k nimž mimo jiné patří například místní server SQL, databáze Oracle, sdílená složka nebo účet Azure Blob Storage.   Jak je popsáno výše, datové sady představují struktury v rámci úložišť dat připojených ke službě Data Factory prostřednictvím propojené služby.
- Představují výpočtový prostředek, který může hostovat provádění aktivity.  Například „Aktivita HDInsightHive“ se provádí na clusteru HDInsight Hadoop.

S těmito čtyřmi jednoduchými koncepty datových sad, aktivit, kanálů a propojených služeb jste připraveni začít!  Můžete [sestavit svůj první kanál](data-factory-build-your-first-pipeline.md) od základů nebo nasadit připravenou ukázku podle pokynů v článku [Ukázky Data Factory](data-factory-samples.md). 

## Podporované oblasti
V současné době můžete vytvářet objekty služby Data Factory v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**. Objekt služby Data Factory nicméně může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure za účelem přesouvání dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb. 

Samotná služba Azure Data Factory žádná data neuchovává. Umožňuje vám vytvořit toky řízené daty k orchestraci přesouvání dat mezi [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores) a zpracování dat pomocí [výpočetních služeb](data-factory-compute-linked-services.md) v jiných oblastech nebo v místním prostředí. Také vám umožňuje [monitorovat a spravovat pracovní postupy](data-factory-monitor-manage-pipelines.md) pomocí uživatelského prostředí nebo prostřednictvím kódu programu. 

Všimněte si, že přestože je služba Azure Data Factory dostupná pouze v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**, služba pohánějící přesouvání dat ve službě Data Factory je dostupná [globálně](data-factory-data-movement-activities.md#global) v několika oblastech. V případě, že je úložiště dat umístěné za bránou firewall, se o přesouvání dat postará [Brána správy dat](data-factory-move-data-between-onprem-and-cloud.md) nainstalovaná ve vašem místním prostředí. 

Předpokládejme například, že vaše výpočetní prostředí, jako je cluster Azure HDInsight nebo Azure Machine Learning, běží v oblasti Západní Evropa. Můžete vytvořit a využít instanci služby Azure Data Factory v oblasti Severní Evropa a použít ji k plánování úloh na výpočetních prostředích v oblasti Západní Evropa. Trvá několik milisekund, než služba Data Factory aktivuje úlohu na výpočetním prostředí, ale čas potřebný pro vykonání úlohy na výpočetním prostředí se nemění.

V budoucnu plánujeme zajistit dostupnost služby Azure Data Factory ve všech oblastech podporovaných v Azure.
  
## Další kroky
Postupujte podle podrobných pokynů v následujících kurzech, abyste se dozvěděli, jak vytvářet objekty pro vytváření dat s datovými kanály. 

Kurz | Popis
-------- | -----------
[Vytvoření datového kanálu, který zpracovává data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md) | V tomto kurzu vytvoříte svůj první objekt pro vytváření dat Azure s datovým kanálem, který **zpracovává data** pomocí skriptu Hive v clusteru Azure HDInsight (Hadoop). |
[Vytvoření datového kanálu pro přesun dat mezi dvěma cloudovými úložišti dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | V tomto kurzu vytvoříte objekt pro vytváření dat s kanálem, který **přesouvá data** z Blob Storage do SQL Database.
[Vytvoření datového kanálu pro přesun dat mezi místním úložištěm dat a cloudovým úložištěm dat pomocí Brány správy dat](data-factory-move-data-between-onprem-and-cloud.md) |  V tomto kurzu vytvoříte objekt pro vytváření dat s kanálem, který **přesouvá data** z databáze **místního** SQL Serveru do objektu blob Azure. Jako součást návodu nainstalujete a nakonfigurujete ve svém počítači Bránu pro správu dat. 


<!--HONumber=Aug16_HO4-->


