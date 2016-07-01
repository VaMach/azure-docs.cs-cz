<properties
    pageTitle="Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics | Stream Analytics"
    description="Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase"
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
    ms.date="05/03/2016"
    ms.author="jeffstok"
/>

# Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics

V tomto kurzu se naučíte vytvořit logiku zpracování datového proudu ke shromáždění dat ze zařízení s platformou IoT (Internet věcí). Na skutečném případu použití platformy IoT si budete moci prohlédnout postup rychlého a ekonomického sestavení potřebného řešení.

## Požadavky

-   [předplatné Azure](https://azure.microsoft.com/pricing/free-trial/)
-   ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted)

## Scénář

Contoso je výrobní společnost působící v oboru automatizace průmyslu, která zcela automatizovala svůj výrobní proces. Stroje v továrně společnosti v reálném čase vysílají datové proudy. V tomto scénáři chce manažer provozovny v reálném čase získávat informace na základě dat ze senzorů, hledat jejich pomocí různé vzorce a na jejich základě provádět potřebné akce. K identifikaci zajímavých vzorců v příchozím datovém proudu ze senzorů použije jazyk SAQL (Stream Analytics Query Language).

Samotná data jsou generována zařízením Texas Instrument SensorTag.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Datová část dat je ve formátu JSON a vypadá takto:

    
    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  
    
V podobném scénáři ve skutečném světě by se jednalo o stovky podobných senzorů generujících datový proud událostí. V ideálním případě by bylo také nainstalováno nějaké zařízení sloužící jako brána k odesílání událostí do služby [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Tyto události ze služby Event Hubs by zpracovávala úloha služby Stream Analytics, prováděla by v reálném čase jejich analýzy definované ve formě dotazů a odesílala výsledky do požadovaných výstupů.

V této příručce Začínáme máte k dispozici ukázkový soubor dat získaných ze skutečných zařízení SensorTag, pro která můžete spouštět různé dotazy a analyzovat jejich výsledky. V následujících kurzech se naučíte připojovat své úlohy ke vstupům a výstupům a nasazovat je ve službě Azure.

## Vytvoření úlohy služby Stream Analytics

Na [Portálu Azure](http://manage.windowsazure.com) otevřete službu Stream Analytics a kliknutím na **Nový** v levém dolním rohu stránky vytvořte novou analytickou úlohu.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

Klikněte na **Rychle vytvořit**.

V poli **Regional Monitoring Storage Account** (Účet regionálního monitorovacího úložiště) vyberte **Vytvořit nový účet úložiště** a zadejte pro něj libovolný jedinečný název. Služba Azure Stream Analytics bude tento účet používat k ukládání informací o monitorování pro všechny vaše budoucí úlohy.

> [AZURE.NOTE] Pro každý region byste měli vytvořit pouze jeden účet úložiště, který budou sdílet všechny úlohy služby Stream Analytics vytvořené v daném regionu.

V dolní části stránky klikněte na **Create Stream Analytics Job** (Vytvořit úlohu služby Stream Analytics).

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Dotaz služby Stream Analytics

Kliknutím na kartu Dotaz přejděte do Editoru dotazů. Na kartě Dotaz je dotaz SQL, který provádí transformaci příchozích dat.

## Archivace nezpracovaných dat

Nejjednodušším typem dotazu je průchozí dotaz, který archivuje veškerá vstupní data do definovaného výstupního umístění.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Nyní z webu [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted) do umístění ve svém počítači stáhněte soubor ukázkových dat. Zkopírujte a vložte dotaz ze souboru **PassThrough.txt**. Níže klikněte na tlačítko Testovat a v umístění stažení vyberte datový soubor s názvem **HelloWorldASA-InputStream.json**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Výsledek dotazu si můžete prohlédnout níže v prohlížeči.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Filtrování dat na základě podmínky

Nyní si vyzkoušíte filtrování výsledků na základě podmínky. Cílem je zobrazit výsledky pouze pro události přicházející ze zařízení SensorA. Dotaz je v souboru **Filtering.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Upozorňujeme, že v tomto případě porovnáváte řetězcovou hodnotu, u které se rozlišují velká a malá písmena. Kliknutím na položku **Spustit znovu** spusťte dotaz. Dotaz by měl vrátit pouze 389 z 1860 událostí.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Upozornění spouštějící pracovní postup společnosti

Nyní si vyzkoušíte zajímavější dotaz. Tento dotaz bude monitorovat průměrnou teplotu v 30sekundovém okně a zobrazí výsledky, pouze pokud teplota překročí 100 stupňů. Dotaz zkopírujete ze souboru a spustíte kliknutím na Znovu spustit. Dotaz je v souboru **ThresholdAlerting.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Vidíte, že nyní výsledky obsahují pouze 245 řádků s daty senzorů, u kterých teplota překročila 100 stupňů. V tomto dotazu jsou události datového proudu seskupeny podle hodnoty dspl, což je název senzoru, a pomocí 30sekundového **přeskakujícího okna**. Při provádění podobných dočasných dotazů je důležité stanovit, jakým způsobem se bude pracovat s časem. Pomocí klauzule **TIMESTAMP BY** je jako klíč zpracování času pro všechny dočasné výpočty určen sloupec time (čas). Podrobné informace naleznete v tématech na webu MSDN zaměřených na [správu času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [časová okna](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Vyhledání nedostatku dat

Pomocí dotazu lze určit i nedostatek určitých dat. Můžete například určit, kdy senzor naposledy odeslal data a poté po dobu jedné minuty neodeslal žádné události. Dotaz je v souboru **AbsenseOfEvent.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

V dotazu se používá příkaz **LEFT OUTER JOIN** na stejný datový proud (spojení sama na sebe). Při použití vnitřního spojení se vrátí výsledek, pouze když je nalezena shoda.  Spojení **LEFT OUTER JOIN** však v případě, že pro událost z levé strany spojení není nalezena shoda, vrátí řádek s hodnotami NULL pro všechny sloupce pravého řádku. Tato technika je velmi užitečná k vyhledání absence událostí. Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci na webu MSDN.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Závěr

V tomto kurzu jste se naučili programovat různé dotazy jazyka SAQL pro různé vzorce dat a zobrazovat jejich výsledky v prohlížeči. Je to však jenom začátek. Pomocí služby Stream Analytics toho můžete dělat mnohem víc. V následujícím kurzu se naučíte připojit úlohu služby Stream Analytics ke vstupům a výstupům a nasadit ji v Azure. Další informace o službě Stream Analytics můžete začít vyhledávat pomocí [mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) a další informace o programování dotazů najdete v článku [Běžné typy dotazů](./stream-analytics-stream-analytics-query-patterns.md#query-example-detect-the-absence-of-events).



<!--HONumber=Jun16_HO2-->


