<properties
    pageTitle="Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics | Stream Analytics"
    description="Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase"
    keywords="řešení iot, začínáme s iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"
/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="08/11/2016"
    ms.author="jeffstok"
/>

# Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics

V tomto kurzu se naučíte vytvořit logiku zpracování datového proudu ke shromáždění dat ze zařízení s platformou IoT (Internet věcí). Na skutečném případu použití platformy IoT si budete moci prohlédnout postup rychlého a ekonomického sestavení potřebného řešení.

## Požadavky

-   [předplatné Azure](https://azure.microsoft.com/pricing/free-trial/)
-   Ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## Scénář

Contoso je společnost působící v oboru automatizace průmyslu, která zcela automatizovala svůj výrobní proces. Stroje v továrně společnosti mají snímače, které dokáží vysílat datové proudy v reálném čase. V tomto scénáři chce manažer provozovny v reálném čase získávat informace na základě dat ze senzorů, hledat jejich pomocí různé vzorce a na jejich základě provádět potřebné akce. K identifikaci zajímavých vzorců v příchozím datovém proudu ze senzorů použije jazyk SAQL (Stream Analytics Query Language).

Samotná data jsou generována zařízením Texas Instrument SensorTag.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Datová část dat je ve formátu JSON a vypadá takto:

    
    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  
    
V podobném scénáři z reálného prostředí by se jednalo o stovky podobných snímačů generujících události jako datový proud. V ideálním případě by bylo také nainstalováno nějaké zařízení sloužící jako brána k odesílání událostí do služby [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Úlohu Stream Analytics tyto události ze služby Event Hubs ingestuje a spustí dotazy na analýzu datových proudů v reálném čase. Potom můžete výsledky odeslat do jednoho z [podporovaných výstupů](stream-analytics-define-outputs.md).

Kvůli jednoduššímu použití je v této příručce Začínáme k dispozici ukázkový soubor dat získaných ze skutečných zařízení SensorTag, pro která můžete spouštět různé dotazy a analyzovat jejich výsledky. V následujících kurzech se naučíte připojovat své úlohy ke vstupům a výstupům a nasazovat je ve službě Azure.

## Vytvoření úlohy služby Stream Analytics

Na [portálu Azure](http://manage.windowsazure.com) vyberte službu Stream Analytics a kliknutím na **Nový** v dolním levém rohu stránky vytvořte novou analytickou úlohu.

![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

Klikněte na **Rychle vytvořit**.

V poli **Regional Monitoring Storage Account** (Účet regionálního monitorovacího úložiště) vyberte **Vytvořit nový účet úložiště** a zadejte pro něj libovolný jedinečný název. Služba Azure Stream Analytics bude tento účet používat k ukládání informací o monitorování pro všechny vaše budoucí úlohy.

> [AZURE.NOTE] Pro každý region byste měli vytvořit pouze jeden účet úložiště, který budou sdílet všechny úlohy služby Stream Analytics vytvořené v daném regionu.

V dolní části stránky klikněte na **Create Stream Analytics Job** (Vytvořit úlohu služby Stream Analytics).

![Konfigurace účtu úložiště](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Dotaz služby Stream Analytics

Kliknutím na kartu Dotaz přejděte do Editoru dotazů. Na kartě Dotaz je dotaz T-SQL, který provádí transformaci příchozích dat událostí.

## Archivace nezpracovaných dat

Nejjednodušším typem dotazu je průchozí dotaz, který archivuje veškerá vstupní data do definovaného výstupního umístění.

![Dotaz na úlohu archivace](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Do umístění ve svém počítači stáhněte ukázkový datový soubor z [GitHubu](https://aka.ms/azure-stream-analytics-get-started-iot). Zkopírujte a vložte dotaz ze souboru **PassThrough.txt**. Níže klikněte na tlačítko Testovat a v příslušném umístění vyberte datový soubor s názvem **HelloWorldASA-InputStream.json**.

![Tlačítko Testovat ve Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Výsledek dotazu si můžete prohlédnout v prohlížeči, jak je znázorněno níže.

![Výsledky testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Filtrování dat na základě podmínky

Nyní si vyzkoušíte filtrování výsledků na základě podmínky. Cílem je zobrazit výsledky pouze pro události přicházející ze zařízení SensorA. Dotaz je umístěný v souboru **Filtering.txt**.

![filtrování datového proudu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Upozorňujeme, že v tomto případě porovnáváme řetězcovou hodnotu, u které se rozlišují velká a malá písmena. Kliknutím na položku **Spustit znovu** spusťte dotaz. Dotaz by měl vrátit pouze 389 z 1860 událostí.

![Druhé výsledky výstupu z testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Upozornění spouštějící pracovní postup společnosti

Teď vytvoříme podrobnější dotaz. Tento dotaz bude monitorovat průměrnou teplotu v 30sekundovém intervalu a výsledky zobrazí jenom v případě, pokud teplota překročí 100 stupňů. Dotaz zkopírujete ze souboru a spustíte kliknutím na **Znovu spustit**. Dotaz je umístěný v souboru **ThresholdAlerting.txt**.

![30sekundový dotaz filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Teď byste měli vidět, že výsledky obsahují jenom 245 řádků a uvádějí jenom snímače, u kterých průměrná teplota překročila 100 stupňů. V tomto dotazu jsme tok událostí seskupili podle hodnoty **dspl**, což je název snímače, a pomocí 30sekundového **přeskakujícího okna**. Při provádění podobných dočasných dotazů je důležité stanovit, jakým způsobem načasujeme postup. Pomocí klauzule **TIMESTAMP BY** je jako klíč zpracování času pro všechny dočasné výpočty určen sloupec time (čas). Podrobné informace najdete v tématech na webu MSDN zaměřených na [správu času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [funkci práce s okny](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![Teplota nad 100](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Zjištění neexistence událostí

Jak napsat dotaz, abychom dokázali najít chybějící vstupní události? Je to poměrně jednoduché. Můžete například zjistit, kdy snímač naposledy odeslal data a potom po dobu jedné minuty neodeslal žádné události. Dotaz je umístěný v souboru **AbsenseOfEvent.txt**.

![Zjištění neexistence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

V dotazu se používá příkaz **LEFT OUTER JOIN** na stejný datový proud (spojení sama na sebe). Při použití vnitřního spojení se vrátí výsledek, pouze když je nalezena shoda.  Spojení **LEFT OUTER JOIN** však v případě, že pro událost z levé strany spojení není nalezena shoda, vrátí řádek s hodnotami NULL pro všechny sloupce pravého řádku. Tato technika je velmi užitečná k vyhledání absence událostí. Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci MSDN.

![výsledky spojení](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Závěr

Účelem tohoto kurzu je předvést způsob psaní různých dotazů v jazyku Stream Analytics Query Language a zobrazení výsledků v prohlížeči. Je to však jenom začátek. Pomocí služby Stream Analytics toho můžete dělat mnohem víc. Stream Analytics podporuje celou řadu vstupů a výstupů a může dokonce využít i funkce ve službě Azure Machine Learning. To z něj dělá robustní nástroj pro analýzu datových proudů. Další informace o službě Stream Analytics najdete v [mapě kurzů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) a informace o psaní dotazů najdete v článku [Běžné vzory dotazů](./stream-analytics-stream-analytics-query-patterns.md).



<!---HONumber=Aug16_HO4-->


