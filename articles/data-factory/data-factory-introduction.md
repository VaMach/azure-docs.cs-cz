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
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 537bdee67ed9648c3cba2099553d847399609705
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017


---
<a id="introduction-to-azure-data-factory" class="xliff"></a>

# Úvod do Azure Data Factory 
<a id="what-is-azure-data-factory" class="xliff"></a>

## Co je služba Azure Data Factory?
Jak se ve světě velkých objemů dat využívají existující data ve firmách? Je možné rozšířit data generovaná v cloudu pomocí referenčních dat z místních zdrojů dat nebo jiných různorodých zdrojů dat? Například herní společnost shromažďuje mnoho protokolů vyprodukovaných hrami v cloudu. Tyto protokoly chce analyzovat za účelem získání informací o preferencích zákazníků, demografických ukazatelích, chování uživatelů atd. a na základě těchto ukazatelů identifikovat příležitosti křížového a následného prodeje, vyvíjet nové, zajímavé funkce podporující obchodní růst a zlepšovat zkušenosti zákazníků. 

Aby společnost mohla tyto protokoly analyzovat, potřebuje použít referenční data, jako jsou informace o zákaznících, hrách a marketingových kampaních, které jsou uložené v místním úložišti dat. Společnost proto chce ingestovat data z cloudového úložiště dat a referenční data z místního úložiště dat. Následně chce data zpracovat pomocí Hadoopu v cloudu (Azure HDInsight) a publikovat výsledná data do cloudového datového skladu, jako je Azure SQL Data Warehouse, nebo do místního úložiště dat, jako je SQL Server. Tento pracovní postup chce spouštět jednou týdně. 

Společnost potřebuje platformu, která jí umožní vytvořit pracovní postup se schopností ingestovat data z místních i cloudových úložišť dat, transformovat nebo zpracovat data pomocí existujících výpočetních služeb, jako je Hadoop, a publikovat výsledky do místního nebo cloudového úložiště dat, aby je mohly využívat aplikace BI. 

![Přehled služby Data Factory](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory je platforma pro tento druh scénářů. Je to **cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat**. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI).  

Spíše než o tradiční platformu extrakce, transformace a načítání (ETL) se jedná o platformu extrakce a načítání (EL) a následné transformace a načtení (TL). Prováděné transformace slouží k transformaci nebo zpracování dat pomocí výpočetních služeb, a nikoli k provádění transformací například za účelem přidání odvozených sloupců, spočtení řádků, řazení dat atd. 

V současné době jsou ve službě Azure Data Factory data využívaná a produkovaná pracovními postupy **data dělená podle času** (po hodinách, dnech, týdnech atd.). Kanál například může číst vstupní data, zpracovat je a vyprodukovat výstupní data jednou denně. Pracovní postup můžete také spustit jenom jednou.  
  

<a id="how-does-it-work" class="xliff"></a>

## Jak to funguje? 
Kanály (pracovní postupy řízené daty) ve službě Azure Data Factory obvykle provádí následující tři kroky:

![Tři fáze služby Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

<a id="connect-and-collect" class="xliff"></a>

### Připojení a shromažďování
Podniky mají data různých typů, která se nachází v různorodých zdrojích. Prvním krokem při sestavování systému vytváření informací je připojení ke všem požadovaným zdrojům dat a zpracování, jako jsou například služby SaaS, sdílené složky, FTP nebo webové služby, a přesun dat podle potřeby do centralizovaného umístění pro následné zpracování.

Bez služby Data Factory musí podniky sestavovat vlastní komponenty pro přesun dat nebo vyvíjet vlastní služby pro integraci a zpracování těchto zdrojů dat. To je nákladné a integrace a údržba takových systémů je složitá. Kromě toho často postrádají monitorování a upozorňování na podnikové úrovni a ovládací prvky, které může nabídnout plně spravovaná služba.

Se službou Data Factory můžete použít aktivitu kopírování v datovém kanálu pro přesun dat z místních i cloudových zdrojů dat do centralizovaného úložiště v cloudu pro další analýzu. Například můžete shromažďovat data v Azure Data Lake Store a později je transformovat pomocí výpočetní služby Azure Data Lake Analytics. Nebo můžete data shromažďovat v Azure Blob Storage a později je transformovat pomocí clusteru Azure HDInsight Hadoop.

<a id="transform-and-enrich" class="xliff"></a>

### Transformace a rozšíření
Jakmile budou data v centralizovaném úložišti dat v cloudu, budete chtít shromážděná data zpracovat nebo transformovat pomocí výpočetních služeb, jako je HDInsight Hadoop, Spark, Data Lake Analytics a Machine Learning. Budete chtít spolehlivě produkovat transformovaná data podle udržitelného a řízeného plánu, abyste mohli do produkčních prostředí dodávat důvěryhodná data. 

<a id="publish" class="xliff"></a>

### Publikování 
Přenášejte transformovaná data z cloudu do místních zdrojů, jako je SQL Server, nebo je uchovávejte ve zdrojích cloudového úložiště, aby je mohly využívat nástroje business intelligence (BI), analytické nástroje a další aplikace.

<a id="key-components" class="xliff"></a>

## Klíčové komponenty
Předplatné Azure může obsahovat jednu nebo více instancí služby Azure Data Factory (neboli datových továren). Azure Data Factory se skládá ze čtyř klíčových komponent, které společně poskytují platformu, na které můžete vytvářet pracovní postupy řízené daty s kroky pro přesun a transformaci dat. 

<a id="pipeline" class="xliff"></a>

### Kanál
Datová továrna může mít jeden nebo víc kanálů. Kanál je skupina aktivit. Aktivity v kanálu společně provádí úlohy. Kanál může například obsahovat skupinu aktivit, které ingestují data z objektu blob Azure a pak na clusteru HDInsight spustí dotaz Hive pro rozdělení dat. Výhodou tohoto přístupu je, že vám kanál umožňuje spravovat aktivity jako sadu, a ne každou jednotlivě. Například můžete nasadit a naplánovat kanál namísto jednotlivých aktivit. 

<a id="activity" class="xliff"></a>

### Aktivita
Kanál může mít jednu nebo víc aktivit. Aktivity definují akce, které se mají provést na vašich datech. Například můžete použít aktivitu kopírování ke kopírování dat z jednoho úložiště dat do jiného. Podobně můžete použít aktivitu Hivu, která spustí dotaz Hivu na clusteru Azure HDInsight, aby transformoval a analyzoval vaše data. Data Factory podporuje dva typy aktivit: aktivity přesunu dat a transformace dat.

<a id="data-movement-activities" class="xliff"></a>

### Aktivity přesunu dat
Aktivita kopírování ve službě Data Factory kopíruje data ze zdrojového úložiště dat do úložiště dat jímky. Data Factory podporuje následující typy úložišť dat. Data z libovolného zdroje lze zapsat do libovolné jímky. Kliknutím na úložiště dat se dozvíte, jak kopírovat data z a do daného úložiště.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Podrobnosti najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).

<a id="data-transformation-activities" class="xliff"></a>

### Aktivity transformace dat
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Podrobnosti najdete v článku [Aktivity transformace dat](data-factory-data-transformation-activities.md).

<a id="custom-net-activities" class="xliff"></a>

### Vlastní aktivity .NET
Pokud potřebujete přesunout data do nebo z úložiště dat, které aktivita kopírování nepodporuje, nebo transformovat data pomocí vlastní logiky, vytvořte **vlastní aktivitu .NET**. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

<a id="datasets" class="xliff"></a>

### Datové sady
Aktivita přijímá jako vstup nula nebo více datových sad a produkuje jednu nebo více datových sad jako výstup. Datové sady představují datové struktury v rámci úložišť dat, které jednoduše odkazují na data, která chcete ve vašich aktivitách použít jako vstupy nebo výstupy. Datová sada služby Azure Blob například určuje kontejner objektů blob a složku ve službě Azure Blob Storage, ze kterých by měl kanál číst data. Nebo datová sada tabulky SQL Azure určuje tabulku, do které aktivita zapisuje výstupní data. 

<a id="linked-services" class="xliff"></a>

### Propojené služby
Propojené služby jsou velmi podobné připojovacím řetězcům, které definují informace o připojení, které služba Data Factory potřebuje pro připojení k externím prostředkům. Můžete si to představit tak, že propojená služba definuje připojení ke zdroji dat a datová sada reprezentuje strukturu těchto dat. Například propojená služba Azure Storage určuje připojovací řetězec pro připojení k účtu služby Azure Storage. A datová sada služby Azure Blob určuje kontejner objektů blob a složku obsahující data.   

Propojené služby slouží ve službě Data Factory ke dvěma účelům:

* Představují **úložiště dat**, k nimž mimo jiné patří například místní SQL Server, databáze Oracle, sdílená složka nebo účet Azure Blob Storage. Seznam podporovaných úložišť dat najdete v oddílu [Aktivity přesunu dat](#data-movement-activities).
* Představují **výpočetní prostředek**, který může hostovat provádění aktivity. Například aktivita HDInsightHive se spouští na clusteru HDInsight Hadoop. Seznam podporovaných výpočetních prostředí najdete v oddílu [Aktivity transformace dat](#data-transformation-activities).

<a id="relationship-between-data-factory-entities" class="xliff"></a>

### Vztah mezi entitami služby Data Factory
![Diagram: Data Factory, služba pro integraci dat v cloudu – Klíčové koncepty](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Obrázek 2** Vztahy mezi datovou sadou, aktivitou, kanálem a propojenou službou

<a id="supported-regions" class="xliff"></a>

## Podporované oblasti
V současné době můžete vytvářet datové továrny v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**. Objekt služby Data Factory nicméně může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure za účelem přesouvání dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb.

Samotná služba Azure Data Factory žádná data neuchovává. Umožňuje vytvářet pracovní postupy řízené daty k orchestraci přesouvání dat mezi [podporovanými úložišti dat](#data-movement-activities) a zpracování dat pomocí [výpočetních služeb](#data-transformation-activities) v jiných oblastech nebo v místním prostředí. Také vám umožňuje [monitorovat a spravovat pracovní postupy](data-factory-monitor-manage-pipelines.md) pomocí uživatelského prostředí nebo prostřednictvím kódu programu.

Přestože je služba Data Factory dostupná pouze v oblastech **Západní USA**, **Východní USA** a **Severní Evropa**, služba pohánějící přesouvání dat ve službě Data Factory je dostupná [globálně](data-factory-data-movement-activities.md#global) v několika oblastech. Pokud je úložiště dat za bránou firewall, o přesun dat se postará [Brána správy dat](data-factory-move-data-between-onprem-and-cloud.md) nainstalovaná ve vašem místním prostředí.

Předpokládejme například, že vaše výpočetní prostředí, jako je cluster Azure HDInsight nebo Azure Machine Learning, běží v oblasti Západní Evropa. Můžete vytvořit instanci služby Azure Data Factory v oblasti Severní Evropa a použít ji k plánování úloh na výpočetních prostředích v oblasti Západní Evropa. Trvá několik milisekund, než Data Factory aktivuje úlohu na výpočetním prostředí, ale čas potřebný pro vykonání úlohy na výpočetním prostředí se nemění.

<a id="get-started-with-creating-a-pipeline" class="xliff"></a>

## Začínáme s vytvořením kanálu
Datové kanály ve službě Azure Data Factory můžete vytvořit pomocí některého z těchto nástrojů nebo rozhraní API: 

- portál Azure
- Visual Studio
- PowerShell
- .NET API
- REST API
- Šablona Azure Resource Manageru 

Chcete-li se dozvědět, jak sestavit datové továrny s datovými kanály, postupujte podle podrobných pokynů v následujících kurzech:

| Kurz | Popis |
| --- | --- |
| [Přesun dat mezi dvěma cloudovými úložišti dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |V tomto kurzu vytvoříte objekt pro vytváření dat s kanálem, který **přesouvá data** ze služby Blob Storage do SQL Database. |
| [Transformace dat pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md) |V tomto kurzu sestavíte svůj první objekt pro vytváření dat Azure s datovým kanálem, který **zpracovává data** pomocí skriptu Hive v clusteru Azure HDInsight (Hadoop). |
| [Přesun dat mezi místním úložištěm dat a cloudovým úložištěm dat pomocí brány správy dat](data-factory-move-data-between-onprem-and-cloud.md) |V tomto kurzu sestavíte objekt pro vytváření dat s kanálem, který **přesouvá data** z databáze **místního** systému SQL Server do objektu blob Azure. Jako součást návodu nainstalujete a nakonfigurujete ve svém počítači Bránu správy dat. |

