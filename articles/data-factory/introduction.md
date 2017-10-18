---
title: "Úvod do služby Azure Data Factory | Dokumentace Microsoftu"
description: "Seznamte se se službou Azure Data Factory, cloudovou službou pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2017
ms.author: shlo
ms.openlocfilehash: ef7055342a04057acfba9dad350f654aa4de6096
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-data-factory"></a>Úvod do Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-introduction.md)
> * [Verze 2 – Preview](introduction.md)

Ve světě velkých objemů dat jsou nezpracovaná, neuspořádaná data často uložená v relačních, nerelačních a jiných systémech úložiště. Nezpracovaná data sama o sobě ale nemají potřebný kontext ani význam, aby mohla analytikům, datovým vědcům a osobám rozhodujícím v rámci podniků poskytnout smysluplný přehled. Je tedy zapotřebí služba, která umožňuje procesy orchestrace a operacionalizace, aby se tato obrovská úložiště nezpracovaných dat proměnila v obchodní informace, podle kterých se lze rozhodovat. Azure Data Factory je spravovaná cloudová služba vytvořená pro tyto komplexní projekty hybridní extrakce, transformace a načítání (ETL), extrakce, načítání a transformace (ELT) a integrace dat.

Představte si například společnost vyrábějící hry, která shromažďuje celé petabajty herních protokolů generovaných hrami v cloudu. Tyto protokoly chce analyzovat, aby získala informace o preferencích zákazníků, demografických ukazatelích, chování uživatelů atd. a na základě těchto ukazatelů mohla identifikovat příležitosti křížového a následného prodeje, vyvíjet nové, zajímavé funkce, podpořit obchodní růst a zlepšovat zkušenosti zákazníků.

Aby společnost mohla tyto protokoly analyzovat, potřebuje použít referenční data, jako jsou informace o zákaznících, hrách a marketingových kampaních, které jsou uložené v místním úložišti dat. Společnost chce využít tato data z místního úložiště dat a zkombinovat je s dalšími daty protokolů, která má uložená v cloudovém úložišti dat. Za účelem získání informací hodlá zpracovat spojená data pomocí clusteru Spark v cloudu (HDInsight) a nakonec transformovaná data publikovat do cloudového datového skladu, například Azure SQL Data Warehouse, aby z nich mohla snadno vytvářet sestavy. Tento pracovní postup má být automatizovaný, monitorovaný a spravovaný na každodenní bázi a má se spustit, jakmile se v kontejneru úložiště objektů blob objeví soubory.

Právě v takových scénářích práce s daty nachází uplatnění platforma Azure Data Factory. Je to **cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat**. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). V neposlední řadě služba Azure Data Factory umožňuje uspořádání nezpracovaných dat do smysluplných úložišť dat a datových jezer, která pomáhají při obchodním rozhodování.

![Zobrazení nejvyšší úrovně služby Data Factory](media/introduction/big-picture.png)

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si téma [Úvod do služby Data Factory verze 1](v1/data-factory-introduction.md).

## <a name="how-does-it-work"></a>Jak to funguje?
Kanály (pracovní postupy řízené daty) ve službě Azure Data Factory obvykle provádí následující čtyři kroky:

![Čtyři kroky pracovního postupu řízeného daty](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Připojení a shromažďování

Podniky mají data různých typů uložená v oddělených místních zdrojích nebo v cloudu a data strukturovaná, nestrukturovaná i polostrukturovaná, která přicházejí v různých intervalech a různou rychlostí. Prvním krokem při sestavování systému vytváření informací je připojení ke všem požadovaným zdrojům dat a zpracování, jako jsou například služby typu software jako služba (SaaS), databáze, sdílené složky nebo webové služby FTP, a přesun dat podle potřeby do centralizovaného umístění pro následné zpracování.

Bez služby Data Factory musí podniky sestavovat vlastní komponenty pro přesun dat nebo vyvíjet vlastní služby pro integraci a zpracování těchto zdrojů dat. To je nákladné a integrace a údržba takových systémů je složitá. Kromě toho často postrádají monitorování a upozorňování na podnikové úrovni a ovládací prvky, které může nabídnout plně spravovaná služba.

Se službou Data Factory můžete pomocí [aktivity kopírování](copy-activity-overview.md) v datovém kanálu přesouvat data z místních i cloudových úložišť dat do centralizovaného úložiště v cloudu pro účely další analýzy. Například můžete shromažďovat data v Azure Data Lake Store a později je transformovat pomocí výpočetní služby Azure Data Lake Analytics. Nebo můžete data shromažďovat v Azure Blob Storage a později je transformovat pomocí clusteru Azure HDInsight Hadoop.

### <a name="transform-and-enrich"></a>Transformace a rozšíření
Jakmile budou data v centralizovaném úložišti dat v cloudu, budete chtít shromážděná data zpracovat nebo transformovat pomocí výpočetních služeb, jako je HDInsight Hadoop, Spark, Data Lake Analytics a Machine Learning. Budete chtít spolehlivě produkovat transformovaná data podle udržitelného a řízeného plánu, abyste mohli do produkčních prostředí dodávat důvěryhodná data.

### <a name="publish"></a>Publikování
Nezpracovaná data jsou teď převedená do podoby, která umožňuje využití v rámci podniku. Tato data načtete do služby Azure Data Warehouse, Azure SQL DB, Azure CosmosDB nebo jiného analytického nástroje, na který se mohou vaši obchodní uživatelé nasměrovat ze svých nástrojů business intelligence.

### <a name="monitor"></a>Monitorování
Jakmile úspěšně sestavíte a nasadíte kanál integrace dat, který ze zpracovaných dat získává obchodní hodnotu, budete chtít monitorovat naplánované aktivity a kanály a jejich míru úspěšnosti a chyb. Azure Data Factory obsahuje integrovanou podporu monitorování kanálů prostřednictvím služby Azure Monitor, rozhraní API, PowerShellu, Microsoft Operations Management Suite (OMS) a panelů stavu na webu Azure Portal.

## <a name="whats-different-in-version-2"></a>Co je nového ve verzi 2?
Služba Azure Data Factory verze 2 staví na původní službě pro přesouvání a transformaci dat Azure Data Factory a rozšiřuje ji pro pestřejší škálu scénářů integrace dat s upřednostněním cloudu. Azure Data Factory verze 2 nabízí tyto funkce:

- Tok řízení a škálování
- Nasazení a spouštění balíčků služby SSIS v Azure

Po vydání verze 1 jsme zjistili, že zákazníci potřebují vytvářet komplexní hybridní scénáře integrace dat, které vyžadují přesouvání a zpracování dat jak v cloudu, tak i v místním prostředí a cloudových virtuálních počítačích. Z těchto požadavků vyplynula potřeba přesouvat a zpracovávat data v zabezpečených prostředích virtuálních sítí a horizontálně navyšovat kapacitu pomocí výkonu zpracování na vyžádání.

Jelikož se datové kanály stávají kritickou součástí strategie služeb obchodní analýzy, pochopili jsme, že tyto kritické aktivity práce s daty vyžadují flexibilní plánování a podporu rostoucích objemů načítaných dat a spouštění na základě událostí. S rostoucí složitostí těchto orchestrací vzrůstá také potřeba, aby služba podporovala běžná paradigmata pracovních postupů, včetně větvení, smyček a podmíněného zpracování.

Verze 2 také umožňuje migraci stávajících balíčků služby SSIS (SQL Server Integration Services) do cloudu a pozvednout službu SSIS na službu Azure spravovanou v rámci ADF a využívající novou funkci „Integration Runtime“ (IR). Přidání funkce SSIS IR do verze 2 vám dává možnost spouštět, spravovat, monitorovat a sestavovat balíčky služby SSIS v cloudu.

### <a name="control-flow-and-scale"></a>Tok řízení a škálování 
V zájmu podpory rozmanitých toků a vzorů integrace v moderních datových skladech služba Data Factory umožňuje nový flexibilní model datového kanálu, který už není svázaný s daty časových řad. V této verzi můžete modelovat se může modelovat podmíněné příkazy a větvení v toku řízení datového kanálu a explicitně předávat parametry v těchto tocích a mezi nimi.

Získáváte teď svobodu modelovat jakýkoli styl toku potřebný pro integraci dat, který se může odesílat na vyžádání nebo opakovaně podle hodinového plánu. Mezi běžné toky, které dřív nebyly podporované, ale teď jsou, patří například tyto:   

- Tok řízení:
    - Řetězení aktivit v sekvenci v rámci kanálu
    - Větvení aktivit v rámci kanálu
    - Parametry
        - Parametry se dají definovat na úrovni kanálu a při volání kanálu na vyžádání nebo pomocí aktivační události se dají předávat argumenty.
        - Aktivity mohou využívat argumenty předávané kanálu.
    - Předávání vlastního stavu
        - Výstupy aktivity včetně stavu může používat následující aktivita v kanálu.
    - Kontejnery smyček
        - For-each 
- Toky založené na aktivačních událostech
    - Kanály se mohou spouštět na vyžádání nebo v nastavenou hodinu.
- Toky rozdílů
    - Pomocí parametrů můžete definovat horní mez pro rozdílové kopírování při přesouvání tabulek dimenzí nebo referenčních tabulek z relačního úložiště v místním prostředí nebo cloudu a načíst data do jezera. 

Další informace najdete v článku [Kurz: Tok řízení](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>Nasazení balíčků služby SSIS do Azure 
Pokud chcete přesunout úlohy služby SSIS, můžete vytvořit službu Data Factory verze 2 a zřídit prostředí Azure-SSIS Integration Runtime (IR). Prostředí Azure-SSIS IR je plně spravovaný cluster virtuálních počítačů Azure (uzlů) vyhrazených ke spouštění balíčků služby SSIS v cloudu. Podrobné pokyny najdete v tomto kurzu: [Nasazení balíčků služby SSIS do Azure](tutorial-deploy-ssis-packages-azure.md). 
 

### <a name="sdks"></a>Sady SDK
Pokud jste pokročilý uživatel a hledáte programové rozhraní, verze 2 poskytuje pestrou nabídku sad SDK, které slouží k vytváření, správy a monitorování kanálů pomocí vašeho oblíbeného integrovaného vývojového prostředí (IDE).

- .NET SDK – Sada .NET SDK je aktualizovaná pro verzi 2. 
- PowerShell – Rutiny PowerShellu jsou aktualizované pro verzi 2. Rutiny verze 2 mají v názvu **DataFactoryV2**. Například: Get-AzureRmDataFactoryV2. 
- Python SDK – Tato sada SDK je ve verzi 2 nová.
- REST API – Rozhraní REST API je aktualizováno pro verzi 2.  

Sady SDK aktualizované pro verzi 2 nejsou zpětně kompatibilní s klienty verze 1. 

### <a name="monitoring"></a>Monitorování
Verze 2 v současné době podporuje monitorování datových továren pouze pomocí sad SDK. Portál ještě monitorování datových továren verze 2 nepodporuje. 

## <a name="load-the-data-into-a-lake"></a>Načtení dat do jezera
Služba Data Factory má více než 30 konektorů, které umožňují načítat data z hybridních a heterogenních prostředí do Azure.  V článku [Průvodce výkonem a laděním](copy-activity-performance.md) najdete nejnovější výsledky výkonu z interního testování a návrhy pro ladění. Kromě toho jsme nedávno povolili vysokou dostupnost a škálovatelnost prostředí Integration Runtime v místním prostředí, které nainstalujete v privátním síťovém prostředí za účelem plnění požadavků velkých podnikových zákazníků na nejvyšší úrovni a které poskytuje vyšší dostupnost a lepší škálovatelnost.

## <a name="top-level-concepts-in-version-2"></a>Koncepty nejvyšší úrovně ve verzi 2
Předplatné Azure může obsahovat jednu nebo více instancí služby Azure Data Factory (neboli datových továren). Azure Data Factory se skládá ze čtyř klíčových komponent, které společně poskytují platformu, na které můžete vytvářet pracovní postupy řízené daty s kroky pro přesun a transformaci dat.

### <a name="pipeline"></a>Kanál
Datová továrna může mít jeden nebo víc kanálů. Kanál je logické seskupení aktivit, které dohromady provádějí určitou jednotku práce. Aktivity v kanálu společně provádí úlohy. Kanál může například obsahovat skupinu aktivit, které ingestují data z objektu blob Azure a pak na clusteru HDInsight spustí dotaz Hive pro rozdělení dat. Výhodou tohoto přístupu je, že vám kanál umožňuje spravovat aktivity jako sadu, a ne každou jednotlivě. Aktivity v kanálu je možné zřetězit, aby probíhaly postupně, nebo mohou probíhat souběžně a nezávisle na sobě.

### <a name="activity"></a>Aktivita
Aktivity představují krok zpracování v rámci kanálu. Například můžete použít aktivitu kopírování ke kopírování dat z jednoho úložiště dat do jiného. Podobně můžete použít aktivitu Hivu, která spustí dotaz Hivu na clusteru Azure HDInsight, aby transformoval a analyzoval vaše data. Data Factory podporuje tři typy aktivit: aktivity přesunu dat, aktivity transformace dat a aktivity řízení.

### <a name="datasets"></a>Datové sady
Datové sady představují datové struktury v rámci úložišť dat, které jednoduše odkazují na data, která chcete ve vašich aktivitách použít jako vstupy nebo výstupy. 

### <a name="linked-services"></a>Propojené služby
Propojené služby jsou velmi podobné připojovacím řetězcům, které definují informace o připojení, které služba Data Factory potřebuje pro připojení k externím prostředkům. Můžete si to představit tak, že propojená služba definuje připojení ke zdroji dat a datová sada reprezentuje strukturu těchto dat. Například propojená služba Azure Storage určuje připojovací řetězec pro připojení k účtu služby Azure Storage. A datová sada služby Azure Blob určuje kontejner objektů blob a složku obsahující data.

Propojené služby slouží ve službě Data Factory ke dvěma účelům:

- Představují **úložiště dat**, k nimž mimo jiné patří například místní SQL Server, databáze Oracle, sdílená složka nebo účet Azure Blob Storage. Seznam podporovaných úložišť dat najdete v článku [Aktivita kopírování](copy-activity-overview.md).
- Představují **výpočetní prostředek**, který může hostovat provádění aktivity. Například aktivita HDInsightHive se spouští na clusteru HDInsight Hadoop. Seznam aktivit transformace a podporovaných výpočetních prostředí najdete v článku o [transformaci dat](transform-data.md).

### <a name="triggers"></a>Triggery
Aktivační události jsou jednotkou zpracování, která určuje, kdy se má zahájit provádění kanálu. Existují různé druhy aktivačních událostí pro různé typy událostí. Ve verzi Preview podporujeme aktivační událost plánovače s hodinovým plánem. 


### <a name="pipeline-runs"></a>Spuštění kanálu
Spuštění kanálu je instance provádění kanálu. Instance spuštění kanálu se obvykle vytvářejí předáváním argumentů do parametrů definovaných v kanálech. Argumenty je možné předat ručně nebo v rámci definice aktivační události.

### <a name="parameters"></a>Parametry
Parametry jsou dvojice klíčů a hodnot v rámci konfigurace jen pro čtení.  Parametry jsou definované v kanálu a argumenty definovaných parametrů se předávají během provádění z kontextu spuštění vytvořeného aktivační událostí nebo ručně prováděným kanálem. Aktivity v rámci kanálu využívají hodnoty parametrů.
Datová sada je parametr silného typu a opakovaně použitelná/odkazovatelná entita. Aktivita může odkazovat na datové sady a může využívat vlastnosti určené v definici datové sady.

Propojená služba je také parametr silného typu obsahující informace o připojení k úložišti dat nebo výpočetnímu prostředí. Je to také opakovaně použitelná/odkazovatelná entita.

### <a name="control-flow"></a>Tok řízení
Jedná se o orchestraci aktivit kanálu, která zahrnuje zřetězení aktivit v sekvenci, větvení, parametry, které se dají definovat na úrovni kanálu, a argumenty předávané při volání kanálu na vyžádání nebo z aktivační události. Zahrnuje také předávání vlastního stavu a kontejnery smyček, tedy iterátory For-each.


Další informace o konceptech služby Data Factory najdete v následujících článcích:

- [Datové sady a propojené služby](concepts-datasets-linked-services.md)
- [Kanály a aktivity](concepts-pipelines-activities.md)
- [Prostředí Integration Runtime](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Podporované oblasti:

V současné době můžete vytvářet datové továrny v oblastech USA – východ a USA – východ 2. Objekt služby Data Factory nicméně může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure za účelem přesouvání dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb.

Samotná služba Azure Data Factory žádná data neuchovává. Umožňuje vytvářet pracovní postupy řízené daty k orchestraci přesouvání dat mezi podporovanými úložišti dat a zpracování dat pomocí výpočetních služeb v jiných oblastech nebo v místním prostředí. Také vám umožňuje monitorovat a spravovat pracovní postupy pomocí uživatelského prostředí nebo prostřednictvím kódu programu.

Přestože je služba Data Factory dostupná jenom v oblastech USA – východ a USA – východ 2, služba pohánějící přesouvání dat ve službě Data Factory je dostupná globálně v několika oblastech. Pokud je úložiště dat za bránou firewall, o přesun dat se postará Brána správy dat nainstalovaná ve vašem místním prostředí.

Předpokládejme například, že vaše výpočetní prostředí, jako je cluster Azure HDInsight nebo Azure Machine Learning, běží v oblasti Západní Evropa. Můžete vytvořit instanci služby Azure Data Factory v oblasti Severní Evropa a použít ji k plánování úloh na výpočetních prostředích v oblasti Západní Evropa. Trvá několik milisekund, než Data Factory aktivuje úlohu na výpočetním prostředí, ale čas potřebný pro vykonání úlohy na výpočetním prostředí se nemění.

## <a name="next-steps"></a>Další kroky
Naučte se vytvořit datovou továrnu na základě podrobných pokynů v následujících průvodcích pro rychlý start: [prostředí PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST API](quickstart-create-data-factory-rest-api.md) a portál Azure. 
