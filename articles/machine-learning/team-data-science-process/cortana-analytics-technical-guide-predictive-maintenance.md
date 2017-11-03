---
title: "V letecký s Azure – technické příručce Cortana Intelligence řešení prediktivní údržby | Microsoft Docs"
description: "Technické příručce k šabloně řešení s Microsoft Cortana Intelligence pro prediktivní údržby v letecký, nástrojů a Transport."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: a6d9cd05eb370399fc95cc64bae892e8b5a73fe2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technické příručce k šabloně Cortana Intelligence řešení pro prediktivní údržby v letecký a jiné firmy

## <a name="important"></a>**Důležité upozornění**
Tento článek je zastaralá. Informace jsou stále relevantní pro aktuální problém, tj prediktivní údržby v letecký, ale nelze nalézt nejnovější článek s nejaktuálnější informace [zde](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**Potvrzení**
Tento článek je dílem datových vědců Jan Potokar Gauher Shaheen, Fidan Boylu Uz a softwaru pracovníka Dana Grecoe ve společnosti Microsoft.

## <a name="overview"></a>**Přehled**
Šablony řešení jsou navrženy pro urychlit proces vytváření ukázkové E2E nad Cortana Intelligence Suite. Nasazené šablona bude zřídit předplatné s nezbytnými součástmi Cortana Intelligence a vytvářet vztahy mezi nimi. Je také doplňuje datovém kanálu s ukázkovými daty vygenerovaná aplikací generátor dat, která bude stáhnout a nainstalovat na místním počítači po nasazení řešení šablony. Data vygenerovaná ze generátor bude hydrate datového kanálu a spuštění generování machine learning předpovědi, které lze následně vizualizovat na řídicím panelu Power BI. Proces nasazení vás provede několik kroků nastavte přihlašovací údaje řešení. Ujistěte se, že záznam tyto přihlašovací údaje, jako je například název řešení, uživatelské jméno a heslo, které zadáte během nasazení.  

Cílem tohoto dokumentu je vysvětlit referenční architektura a různé součásti, které jsou zřízené v rámci vašeho předplatného v rámci této šablony řešení. Dokument komunikuje také o tom, jak nahradit ukázková data vlastní, abyste mohli zobrazit insights a predikcím ze svoje vlastní data skutečná data. Kromě toho dokumentu popisuje části šablony řešení, která by bylo potřeba změnit, pokud chcete přizpůsobit řešení s vlastními daty. Pokyny o tom, jak vytvořit řídicí panel Power BI pro tuto šablonu řešení najdete na konci.

> [!TIP]
> Můžete stáhnout a vytisknout [PDF verzi tohoto dokumentu](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Celkový přehled**
![Architektura prediktivní údržby](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Při nasazení řešení, jsou aktivované různé služby Azure v rámci Cortana Analytics Suite (*tj.* Centra událostí, Stream Analytics, HDInsight, Data Factory strojového učení, *atd*). Diagram architektury výše ukazuje na vysoké úrovni, jak sestavený prediktivní Údržba leteckých řešení šablony ze začátku do konce. Bude moct šetření tyto služby na portálu azure tak, že kliknete na je diagram řešení šablony vytvořené pomocí nasazení řešení s výjimkou HDInsight, jako je služba zřízena na vyžádání při související kanálu aktivity jsou nutná ke spuštění a odstraněné později.
Můžete si stáhnout [plné velikosti verze diagramu](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Následující části popisují každého jednotlivého.

## <a name="data-source-and-ingestion"></a>**Zdroj dat a přijímání**
### <a name="synthetic-data-source"></a>Zdroj dat syntetického
Pro tuto šablonu použít zdroj dat se generují z aplikace pracovní plochy, který bude stáhnout a spustit místně po úspěšné nasazení. Najdete pokyny ke stažení a instalaci této aplikace v panelu Vlastnosti po výběru prvního uzlu názvem generátor dat prediktivní údržby v diagramu šablonu řešení. Tuto aplikaci kanály [centra událostí Azure](#azure-event-hub) služby s datových bodů nebo událostí, které se použijí ve zbývající části toku řešení. Tento zdroj dat je tvořená nebo odvozené od veřejně dostupnými daty z [datové úložiště NASA](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) pomocí [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Aplikace generování událostí naplní centra událostí Azure pouze tehdy, když je prováděna v počítači.

### <a name="azure-event-hub"></a>Centra událostí Azure
[Centra událostí Azure](https://azure.microsoft.com/services/event-hubs/) služba je příjemce vstup syntetické zdroj dat popsané výše.

## <a name="data-preparation-and-analysis"></a>**Příprava dat a analýzy**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služby slouží k poskytování téměř v reálném čase analýzy vstupního datového proudu z [centra událostí Azure](#azure-event-hub) služby a publikovat výsledky do [Power BI](https://powerbi.microsoft.com) řídicí panel, jakož i všechny nezpracované příchozí události k archivaci [Azure Storage](https://azure.microsoft.com/services/storage/) služby pro pozdější zpracování pomocí [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služby.

### <a name="hd-insights-custom-aggregation"></a>Vlastní agregační HD statistiky
Služba Azure HD Insight se používá ke spuštění [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty (řízená Azure Data Factory) k poskytování agregace na nezpracované události, které byly archivovat pomocí služby Azure Stream Analytics.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) služba se používá (řízená Azure Data Factory) aby předpovědi na zbývající životnosti (RUL) konkrétní leteckého motoru, zadané vstupy přijata.

## <a name="data-publishing"></a>**Publikování dat**
### <a name="azure-sql-database-service"></a>Služba Azure SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) služby se používá k ukládání (spravované službou Azure Data Factory) předpovědi přijatých službou Azure Machine Learning, které bude potřeba v [Power BI](https://powerbi.microsoft.com) řídicího panelu.

## <a name="data-consumption"></a>**Spotřeba dat**
### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) služby se používá k zobrazení řídicí panel, který obsahuje agregace a výstrahy poskytované [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služby a také RUL předpovědi uložené v [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) které byly vytvořeny pomocí [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) služby. Návod, jak vytvořit řídicí panel Power BI pro tuto šablonu řešení naleznete v následující části.

## <a name="how-to-bring-in-your-own-data"></a>**Jak mají být předány svoje vlastní data**
Tato část popisuje, jak převést svoje vlastní data do Azure a oblasti, které by vyžadovaly změny data, která přepnutím do této architektury.

Není pravděpodobné, že žádné datové sady přepnutím by odpovídat datové sady používané [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) použít pro tuto šablonu řešení. Seznámení s daty a požadavky na bude velmi důležitý v tom, jak upravit tuto šablonu pro práci s vlastními daty. Pokud je to první vystavení ke službě Azure Machine Learning, úvod k němu můžete získat pomocí v příkladu v [postup vytvoření prvního experimentu](../studio/create-experiment.md).

V následujících částech se popisují v částech šablony, která bude vyžadovat změny, pokud je zavedená nová datová sada.

### <a name="azure-event-hub"></a>Centra událostí Azure
Služba centra událostí Azure je velmi obecná tak, aby se data můžou být publikované rozbočovače ve formátu CSV nebo formátu JSON. Žádné speciální zpracování dojde v Centru událostí Azure, ale je důležité, abyste rozuměli tomu data, která je dodáni do ní.

Tento dokument nepopisuje ingestovat data, ale můžete snadno odesílat události nebo data do centra událostí Azure pomocí rozhraní API centra událostí.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Služba Azure Stream Analytics slouží k poskytování téměř v reálném čase analytics čtení z datových proudů a odesíláním dat do libovolný počet zdrojů.

Pro prediktivní Údržba leteckých šablony řešení Azure Stream Analytics dotazu se skládá z čtyři poddotazech, každý náročné události z centra událostí Azure service a výstupy museli čtyř různých umístění. Tyto výstupy obsahovat tři datové sady Power BI a jedno umístění úložiště Azure.

Dotaz Azure Stream Analytics lze najít:

* Přihlášení na portál Azure
* Vyhledání úlohy Stream Analytics ![Stream Analytics ikonu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) které byly generovány při řešení nasazená (*například*, **maintenancesa02asapbi** a **maintenancesa02asablob** pro řešení prediktivní údržby)
* Výběr
  
  * ***VSTUPY*** zobrazení dotazu vstup
  * ***DOTAZ*** zobrazíte samotný dotaz
  * ***VÝSTUPY*** k zobrazení různých výstupy

Informace o vytváření dotazů Azure Stream Analytics najdete v [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx) na webu MSDN.

Dotazy v tomto řešení výstupní tři datové sady s téměř analýzu v reálném čase informace o příchozí datový proud na řídicí panel Power BI, který je dodáván jako součást této šablony řešení. Protože je implicitní znalosti o příchozí data formátu, tyto dotazy by bylo nutné změnit podle vaší formát data.

Dotaz v druhé úloze Stream Analytics **maintenancesa02asablob** jednoduše všechny výstupy [centra událostí](https://azure.microsoft.com/services/event-hubs/) události [Azure Storage](https://azure.microsoft.com/services/storage/) a proto vyžaduje žádné změnou bez ohledu na formát vaše data, protože informace o úplné události je streamování na úložiště.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služeb orchestruje přesouvání a zpracování dat. V prediktivní Údržba leteckých řešení šablony objektu pro vytváření dat se skládá ze tří [kanály](../../data-factory/v1/data-factory-create-pipelines.md) , přesunout a zpracování dat pomocí různých technologií.  Objekt pro vytváření dat může přístup otevřením uzel pro vytváření dat v dolní části šablony diagram řešení vytvořen s nasazení řešení. Tím přejdete k objektu pro vytváření dat na portálu Azure. Pokud se zobrazí chyby v rámci vaší datové sady, můžete těch, které jsou z důvodu nasazení před generátor dat byla spuštěna pro vytváření dat ignorovat. Tyto chyby, nebudou bránit datovou továrnu nebude fungovat.

![Chyby datové sady objektu pro vytváření dat](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Tato část popisuje nezbytné [kanály](../../data-factory/v1/data-factory-create-pipelines.md) a [aktivity](../../data-factory/v1/data-factory-create-pipelines.md) součástí [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Níže je zobrazení diagramu řešení.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Dva kanály tento objekt pro vytváření obsahovat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty, které se používají k vytvoření oddílů a agregovat data. Pokud už jsme zmínili, skripty budou umístěny v [Azure Storage](https://azure.microsoft.com/services/storage/) účet vytvořený během instalace. Jejich umístění bude: maintenancesascript\\\\skriptu\\\\hive\\ \\ (nebo name].blob.core.windows.net/maintenancesascript https://[Your řešení).

Podobně jako [Azure Stream Analytics](#azure-stream-analytics-1) dotazy, [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty mají implicitní znalosti o příchozí data formátu, tyto dotazy by bylo potřeba změnit podle vaší formát data.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
To [kanálu](../../data-factory/v1/data-factory-create-pipelines.md) obsahuje jediné aktivity – [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , která se spouští [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript k oddílu dat umístit do [Azure Storage](https://azure.microsoft.com/services/storage/) během [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) úlohy.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro toto rozdělení úloh je ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
To [kanálu](../../data-factory/v1/data-factory-create-pipelines.md) obsahuje několik aktivit a jehož konečný výsledek je scored predikcím ze [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentu přidružená k této šabloně řešení.

Aktivity obsažené v tomto jsou:

* [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , která se spouští [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript k provádění agregací a konstruování potřebné pro [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment.
  [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro toto rozdělení úloh je ***PrepareMLInput.hql***.
* [Kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje výsledků [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) aktivity na jednu [Azure Storage](https://azure.microsoft.com/services/storage/) objekt blob, který může být přístup [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity, která volá [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment, což vede k výsledky provádí požadavek put v jediném [Azure Storage](https://azure.microsoft.com/services/storage/) objektů blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
To [kanálu](../../data-factory/v1/data-factory-create-pipelines.md) obsahuje jediné aktivity - [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje výsledky [Azure Machine Learning](#azure-machine-learning) experimentovat z ***MLScoringPipeline*** k [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) který zřízenou jako součást instalace šablony řešení.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentovat použité pro tuto šablonu řešení poskytuje zbývající doby životnosti (RUL) leteckého motoru. Experimentu je specifický pro datové sady spotřebované a proto bude vyžadovat změny nebo nahrazení specifické pro data, která je převeden do stavu v.

Informace o vytváření experimentu Azure Machine Learning najdete v tématu [prediktivní údržby: Krok 1 ze 3, Příprava dat a funkce konstruování](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Monitorování průběhu**
 Po spuštění generátor dat kanálu začne HYDRATOVANÝ získat a spustit různé komponenty řešení si to chtěl / do akce následující příkazy vydané službou Data Factory. Existují dva způsoby, jak můžete monitorovat kanál.

1. Jednu úlohu služby Stream Analytics zapíše nezpracovaná příchozí data do úložiště objektů blob. Pokud kliknete na úložiště objektů Blob součást řešení z obrazovky můžete úspěšně nasazené řešení a potom klikněte na Otevřít v pravém panelu, bude trvat, abyste [portálu pro správu](https://portal.azure.com/). Potom klikněte na objekty BLOB. Další panelu zobrazí se seznam kontejnerů. Klikněte na **maintenancesadata**. V panelu Další se zobrazí **rawdata** složky. Ve složce rawdata, zobrazí se složky s názvy, například hodinu = 17 hodinu = 18 atd. Pokud se zobrazí tyto složky, znamená to úspěšně je nezpracovaná data se vygenerované v počítači a uložené v úložišti objektů blob. Měli byste vidět soubory sdíleného svazku clusteru, které by měl mít konečné velikosti v MB v těchto složkách.
2. Posledním krokem kanálu je zapisovat data (např. predikcím ze strojového učení) do databáze SQL. Možná bude muset počkat maximálně tři hodiny pro zobrazení dat v databázi SQL. Je možné sledovat, kolik dat je k dispozici ve vaší databázi SQL prostřednictvím [portál azure](https://manage.windowsazure.com/). V levém panelu vyhledejte databází SQL ![SQL ikonu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) a klikněte na něj. Vyhledejte databázi **pmaintenancedb** a klepněte na ni. Na další stránce v dolní části klikněte na SPRAVOVAT
   
    ![Ikona Správa](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    Zde můžete kliknutím na nový dotaz a dotaz pro počet řádků (například vyberte count(*) z PMResult). S růstem vaší databáze, měli zvýšit počet řádků v tabulce.

## <a name="power-bi-dashboard"></a>**Řídicí panel Power BI**
### <a name="overview"></a>Přehled
Tato část popisuje, jak nastavit řídicí panel Power BI můžete vizualizovat váš dat v reálném čase ze služby Azure Stream Analytics (aktivní trase), a také batch předpovědi výsledky z Azure machine learning (neaktivní trase).

### <a name="setup-cold-path-dashboard"></a>Instalační program neaktivní trase řídicí panel
V datovém kanálu neaktivní trase nezbytné cílem je získat prediktivní RUL (zbývající dobu životnosti) každý motor letadla po jeho dokončení letu (cyklus). Výsledek předpovědi se aktualizuje každých 3 hodiny pro predikci letecké motory, které dokončily letu během posledních 3 hodiny.

Power BI se připojí k Azure SQL database jako svůj zdroj dat, kde jsou uložené výsledky předpovědi. Poznámka: 1) při nasazení řešení, skutečné předpovědi se zobrazí v databázi v rámci 3 hodiny.
Soubor pbix, které byly dodány s stahování generátor obsahuje počáteční hodnoty data, takže můžete hned vytvořit řídicí panel Power BI. 2) v tomto kroku předpokladů je ke stažení a instalaci bezplatný software [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Následující kroky vás povede k připojení k databázi SQL, které se spouštějí v době nasazení řešení, které obsahuje dat soubor pbix (*například*. výsledky předpovědi) pro vizualizaci.

1. Získáte přihlašovací údaje databáze.
   
   Budete potřebovat **databáze název serveru, název databáze, uživatelské jméno a heslo** než přejdete k dalším krokům. Tady jsou kroky pro pokyny, jak je vyhledat.
   
   * Jednou **databáze SQL Azure** v šabloně řešení změní zelená diagramu, klikněte na něj a potom klikněte na **, otevřete'**.
   * Zobrazí se nové kartě nebo okno prohlížeče, který zobrazí stránku portálu Azure. Klikněte na tlačítko **'skupiny prostředků,** na levém panelu.
   * Vyberte odběr, který používáte pro nasazení řešení a potom vyberte **' YourSolutionName\_ResourceGroup'**.
   * V nové pop na panely, klikněte na tlačítko ![SQL ikonu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) ikonu pro přístup k vaší databázi. Název databáze je vedle této ikona (*například*, **'pmaintenancedb'**) a **název databázového serveru** uvedena pod vlastnost název serveru a by měl vypadat podobně jako **YourSoutionName.database.windows.net**.
   * Vaše databáze **uživatelské jméno** a **heslo** jsou stejné jako uživatelské jméno a heslo dříve zaznamenaných během nasazení řešení.
2. Aktualizujte zdroj dat souboru sestavy neaktivní trase s Power BI Desktop.
   
   * Ve složce ve vašem počítači, které jste stáhli a unzipped soubor generátor, dvakrát klikněte **PowerBI\\PredictiveMaintenanceAerospace.pbix** souboru. Pokud se při otevření souboru se zobrazí všechny zprávy upozornění, je ignorujte. Nahoře soubor, klikněte na **'upravit dotazy**.
     
     ![Úpravy dotazů](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Uvidíte dvě tabulky **RemainingUsefulLife** a **PMResult**. Vyberte první tabulky a klikněte na ![ikona nastavení dotazu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) vedle **'Source'** v části **"použít postup** na pravé straně **'Nastavení dotazu'** panelu. Ignorujte všechny zprávy upozornění, které se zobrazují.
   * V okně pop, nahraďte **"Server"** a **'Databázi'** s vlastními názvy serveru a databáze a pak klikněte na **"OK"**. Pro název serveru, je nutné zadat port 1433 (**YourSoutionName.database.windows.net, 1433**). Ponechat toto pole databáze jako **pmaintenancedb**. Ignorujte upozornění, které se zobrazují na obrazovce.
   * V dalším okně pop, uvidíte dvě možnosti, v levém podokně (**Windows** a **databáze**). Klikněte na tlačítko **'Databázi'**, vyplňte vaše **'Uživatelského jména'** a **'Password'** (Toto je uživatelské jméno a heslo, které jste zadali při prvním nasazení řešení a vytvoření Azure SQL database). V ***vyberte, kterou úroveň použít tato nastavení***, zkontrolujte úrovně možnost databáze. Pak klikněte na tlačítko **"Připojit"**.
   * Klikněte na druhé tabulky **PMResult** klikněte ![navigační ikonu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) vedle **'Source'** v části **"použít postup** na pravé straně **'Nastavení dotazu'** panelu a aktualizovat název serveru a databáze jako výše uvedené kroky a klikněte na tlačítko OK.
   * Jakmile jste se na základě zpět na předchozí stránku, zavřete okno. Objeví se zpráva out – klikněte na tlačítko **použít**. Nakonec kliknutím **Uložit** tlačítko a uložte změny. Souboru Power BI je nyní navázat připojení k serveru. Pokud vaše vizualizace jsou prázdná, zkontrolujte, zda že vymazat výběry na vizualizaci, která bude vizualizovat všechna data kliknutím na ikonu mazání v pravém horním rohu legend. Použijte tlačítko Aktualizovat, aby odpovídala nová data v vizualizacemi. Na začátku zobrazí, jenom data počáteční hodnoty na vaše vizualizace jako objekt pro vytváření dat je naplánovaná aktualizace každých 3 hodiny. Po 3 hodiny zobrazí se nové předpovědi promítnuta vaší vizualizace při aktualizaci data.
3. (Volitelné) Publikovat k řídicímu neaktivní trase [Power BI online](http://www.powerbi.com/). Všimněte si, že tento krok vyžaduje účet Power BI (nebo účtu Office 365).
   
   * Klikněte na tlačítko **"Publikovat"** a později několik sekund zobrazí se okno zobrazení "Publikování Power BI úspěchu!" s zelená značka zaškrtnutí. Kliknutím na odkaz níže "Otevřete PredictiveMaintenanceAerospace.pbix v Power BI". Podrobné pokyny najdete v části [publikování z Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Chcete-li vytvořit nový řídicí panel: klikněte na tlačítko  **+**  přihlásit do **řídicí panely** část v levém podokně. Zadejte název "Prediktivní údržby ukázku" pro tento nový řídicí panel.
   * Jakmile otevřete sestavu, klikněte na tlačítko ![ikonu PŘIPNUTÍ](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) připnete všemi vizualizacemi na řídicí panel. Podrobné pokyny najdete v části [dlaždici připnout na řídicí panel Power BI ze sestavy](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Přejděte na stránku řídicího panelu a upravit velikost a umístění vaší vizualizace a upravit jejich názvy. Podrobné pokyny o tom, jak upravit dlaždice naleznete v tématu [Upravit vedle sebe - změny velikosti, přesunout, přejmenovat, kódu pin, odstranit, přidání hypertextového odkazu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Zde je příklad řídicí panel s vizualizacemi některé neaktivní trase připnuli k němu.  V závislosti na tom, jak dlouho spustíte generátor vaše data může být vaše čísla na vizualizacemi jiný.
     <br/>
     ![Poslední zobrazení](./media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * Chcete naplánovat aktualizaci dat, umístěte ukazatel myši nad **PredictiveMaintenanceAerospace** datovou sadu, klikněte na tlačítko ![tří teček ikonu](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) a potom zvolte **naplánovat aktualizaci**.
     <br/>
     **Poznámka:** Pokud se zobrazí upozornění masáž, klikněte na tlačítko **upravit pověření** a zkontrolujte, zda vaše přihlašovací údaje databáze jsou stejné jako popsané v kroku 1.
     <br/>
     ![Plán aktualizace](./media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Rozbalte **naplánovat aktualizaci** části. Zapněte "zachovávat aktuálnost dat".
     <br/>
   * Naplánujte aktualizace na základě potřeb. Další informace naleznete v tématu [aktualizace dat v Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Instalační program aktivní trase řídicí panel
Následující kroky vám pomohou postup vizualizovat reálném čase datovým výstupem z úlohy Stream Analytics, které byly vygenerovány v době nasazení řešení. A [Power BI online](http://www.powerbi.com/) účet je potřeba provést následující kroky. Pokud účet nemáte, můžete [vytvořit](https://powerbi.microsoft.com/pricing).

1. Přidáte výstup Power BI v Azure Stream Analytics (ASA).
   
   * Budete muset postupovat podle pokynů v [Azure Stream Analytics & Power BI: řídicí panel analýzy v reálném čase pro streamování dat v reálném čase viditelnost](../../stream-analytics/stream-analytics-power-bi-dashboard.md) nastavit výstup úlohy Azure Stream Analytics jako řídicí panel Power BI.
   * Dotaz ASA má tři výstupů, které jsou **aircraftmonitor**, **aircraftalert**, a **flightsbyhour**. Dotaz můžete zobrazit kliknutím na kartu dotaz. Odpovídající každé z těchto tabulek, musíte přidat výstup do ASA. Když přidáte první výstup (*například* **aircraftmonitor**) zajistěte, aby **Alias pro výstup**, **název datové sady** a **název tabulky** jsou stejné (**aircraftmonitor**). Opakujte postup pro přidání výstupy pro **aircraftalert**, a **flightsbyhour**. Jakmile jste přidali všechny tři výstupní tabulky a spustit úlohy ASA, měli byste obdržet zprávu s potvrzením (*například*, "Úlohy analýzy datového proudu počáteční maintenancesa02asapbi bylo úspěšné").
2. Přihlaste se k [Power BI online](http://www.powerbi.com)
   
   * Na levém panelu části datové sady v pracovním prostoru ***datovou sadu*** názvy **aircraftmonitor**, **aircraftalert**, a **flightsbyhour** by se měla objevit. Toto je dat, která se poslat ze služby Azure Stream Analytics v předchozím kroku. Datová sada **flightsbyhour** nemusí být uvedeny ve stejnou dobu jako další dvě datové sady vzhledem k povaze příkazu jazyka SQL za ní. Ale by měl zobrazit za hodinu.
   * Zajistěte, aby ***vizualizace*** podokně je otevřený a je zobrazena na pravé straně obrazovky.
3. Jakmile máte dat odesílaných do Power BI, můžete začít vizualizace dat. Níže je příklad řídicí panel s vizualizacemi některé aktivní trase připnuli k němu. Můžete vytvořit další dlaždice řídicího panelu podle příslušné datové sady. V závislosti na tom, jak dlouho spustíte generátor vaše data může být vaše čísla na vizualizacemi jiný.

    ![Zobrazení řídicího panelu](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Tady jsou některé kroky k vytvoření jednoho z výše uvedených – dlaždice "loďstva zobrazení senzor 11 vs. Prahová hodnota šířka 48,26" dlaždice:
   
   * Klikněte na datovou sadu **aircraftmonitor** na levém panelu části datové sady.
   * Klikněte **spojnicový graf** ikonu.
   * Klikněte na tlačítko **zpracovaných** v **pole** tak, aby se zobrazí v části "Osou" v podokně **vizualizace** podokně.
   * Klikněte na tlačítko "s.11" a "s.11\_výstraha" tak, aby obě vyskytovat v části "Hodnoty". Klikněte na malou šipku vedle **s.11** a **s.11\_výstraha**, změňte "Sum" na "Průměr".
   * Klikněte na tlačítko **Uložit** nahoře a název sestavy "aircraftmonitor". Sestavy s názvem "aircraftmonitor" bude zobrazen v **sestavy** tématu **Navigátor** podokna na levé straně.
   * Klikněte **Pin Visual** ikonu v pravém horním rohu tento spojnicový graf. Okno "Kód Pin pro řídicí panel" může zobrazovat můžete vybrat řídicí panel. Vyberte "Prediktivní údržby ukázku" a potom klikněte na tlačítko "Pin".
   * Najeďte myší tuto dlaždici na řídicím panelu, klikněte na ikonu "upravit" v pravém horním rohu změnit její název do "loďstva zobrazení senzor 11 vs. Prahová hodnota šířka 48,26" a subtitle na"Průměr napříč firemního vozového v čase".

## <a name="how-to-delete-your-solution"></a>**Postup odstranění řešení**
Ověřte, zda je zastavit generátor dat při použití nejsou aktivně řešení jako spuštění generátoru dat bude způsobit vyšší náklady. Pokud nepoužíváte ji odstraňte řešení. Odstranění řešení odstraní všechny součásti, které jsou zřízené v rámci vašeho předplatného, při nasazení řešení. Chcete-li odstranit řešení klikněte na název vašeho řešení na levém panelu řešení šablonu a klikněte na odstranit.

## <a name="cost-estimation-tools"></a>**Odhad nákladů nástroje**
Tyto dva nástroje jsou k dispozici vám pomůžou lépe pochopit celkové náklady spojené systémem prediktivní Údržba leteckých řešení šablony ve vašem předplatném:

* [Nástroj odhadu náklady na Microsoft Azure (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure náklady odhadu Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

