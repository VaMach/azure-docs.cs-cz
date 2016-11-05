---
title: Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics | Microsoft Docs
description: Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase
keywords: řešení iot, začínáme s iot
services: stream-analytics
documentationcenter: ''
author: jeffstokes72
manager: jhubbard
editor: cgronlun

ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 09/26/2016
ms.author: jeffstok

---
# Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics
V tomto kurzu se naučíte vytvořit logiku zpracování datového proudu ke shromáždění dat ze zařízení s platformou IoT (Internet věcí). Na skutečném případu použití platformy IoT si budete moci prohlédnout postup rychlého a ekonomického sestavení potřebného řešení.

## Požadavky
* [Předplatné Azure](https://azure.microsoft.com/pricing/free-trial/)
* Ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## Scénář
Contoso, společnost působící v oboru automatizace průmyslu, zcela automatizovala svůj výrobní proces. Stroje v továrně společnosti mají snímače, které dokáží vysílat datové proudy v reálném čase. V tomto scénáři chce manažer provozovny v reálném čase získávat informace na základě dat ze senzorů, hledat jejich pomocí různé vzorce a na jejich základě provádět potřebné akce. K identifikaci zajímavých vzorců v příchozím datovém proudu ze senzorů použije jazyk SAQL (Stream Analytics Query Language).

Samotná data jsou generována zařízením Texas Instruments SensorTag.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Datová část dat je ve formátu JSON a vypadá takto:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

V podobném scénáři z reálného prostředí by se jednalo o stovky podobných snímačů generujících události jako datový proud. V ideálním případě by bylo také nainstalováno nějaké zařízení sloužící jako brána k odesílání událostí do služby [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Úlohu Stream Analytics tyto události ze služby Event Hubs ingestuje a spustí dotazy na analýzu datových proudů v reálném čase. Poté můžete výsledky odeslat do jednoho z [podporovaných výstupů](stream-analytics-define-outputs.md).

Pro snadnější použití tato příručka Začínáme poskytuje soubor ukázkových dat, která byla zaznamenána skutečnými zařízeními SensorTag. Na těchto ukázkových datech můžete spouštět dotazy a zobrazit výsledky. V následujících kurzech se naučíte připojovat své úlohy ke vstupům a výstupům a nasazovat je ve službě Azure.

## Vytvoření úlohy Stream Analytics
1. Na webu [Azure Portal](http://manage.windowsazure.com) klikněte na **STREAM ANALYTICS** a poté kliknutím na **NOVÉ** v dolním levém rohu stránky vytvořte novou analytickou úlohu.
   
    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Klikněte na **RYCHLE VYTVOŘIT**.
3. U možnosti **ÚČET REGIONÁLNÍHO MONITOROVACÍHO ÚLOŽIŠTĚ** klikněte na **VYTVOŘIT NOVÝ ÚČET ÚLOŽIŠTĚ** a zadejte pro něj jedinečný název. Služba Azure Stream Analytics bude tento účet používat k ukládání informací o monitorování pro všechny vaše budoucí úlohy.
   
   > [!NOTE]
   > Tento účet úložiště byste měli pro každý region vytvořit pouze jednou. Toto úložiště bude sdílené napříč všemi úlohami Stream Analytics vytvořenými v příslušné oblasti.
   > 
   > 
4. V dolní části stránky klikněte na **VYTVOŘIT ÚLOHU STREAM ANALYTICS**.
   
    ![Konfigurace účtu úložiště](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Dotaz služby Stream Analytics
Kliknutím na kartu **DOTAZ** přejděte do Editoru dotazů. Na kartě **DOTAZ** je dotaz T-SQL, který provádí transformaci příchozích dat událostí.

## Archivace nezpracovaných dat
Nejjednodušším typem dotazu je průchozí dotaz, který archivuje veškerá vstupní data do definovaného výstupního umístění.

![Dotaz na úlohu archivace](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Nyní z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) stáhněte soubor ukázkových dat do umístění ve svém počítači. Vložte dotaz ze souboru PassThrough.txt. Klikněte na tlačítko **Testovat** a poté v příslušném umístění vyberte datový soubor s názvem HelloWorldASA-InputStream.json.

![Tlačítko Testovat ve Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Výsledek dotazu si můžete prohlédnout v prohlížeči, jak je znázorněno na následujícím snímku obrazovky.

![Výsledky testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Filtrování dat na základě podmínky
Nyní si vyzkoušíte filtrování výsledků na základě podmínky. Cílem je zobrazit výsledky pouze pro události přicházející ze zařízení SensorA. Dotaz je umístěný v souboru Filtering.txt.

![Filtrování datového proudu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Všimněte si, že hodnotu řetězce porovnává dotaz rozlišující velká a malá písmena. Kliknutím na tlačítko **Spustit znovu** spusťte dotaz. Dotaz by měl vrátit 389 řádků z 1860 událostí.

![Druhé výsledky výstupu z testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Upozornění spouštějící pracovní postup společnosti
Vytvoříme podrobnější dotaz. Pro každý typ snímače chceme monitorovat průměrnou teplotu v 30sekundovém intervalu a výsledky zobrazit pouze v případě, že teplota překročí 100 stupňů. Napíšeme následující dotaz a zobrazíme výsledky kliknutím na **Znovu spustit**. Dotaz je umístěný v souboru ThresholdAlerting.txt.

![30sekundový dotaz filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Teď byste měli vidět výsledky, které obsahují pouze 245 řádků a uvádějí jenom názvy snímačů, u kterých průměrná teplota překročila 100 stupňů. Tento dotaz seskupí toky událostí podle hodnoty **dspl**, což je název snímače, a pomocí 30sekundového **přeskakujícího okna**. Dočasné dotazy musí stanovit, jakým způsobem se má načasovat postup. Pomocí klauzule **TIMESTAMP BY** jsme jako klíč přidružení časů ke všem dočasným výpočtům určili sloupec **OUTPUTTIME**. Podrobné informace najdete v článcích na webu MSDN o [správě času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [funkcích práce s okny](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![Teplota nad 100](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Zjištění neexistence událostí
Jak napsat dotaz, abychom dokázali najít chybějící vstupní události? Můžete například zjistit, kdy snímač naposledy odeslal data a následně po dobu jedné minuty neodeslal žádné události. Dotaz je umístěný v souboru AbsenseOfEvent.txt.

![Zjištění neexistence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

V dotazu se používá příkaz **LEFT OUTER JOIN** na stejný datový proud (spojení sama na sebe). Při použití příkazu **INNER JOIN** se vrátí výsledek, pouze když je nalezena shoda.  Příkaz **LEFT OUTER JOIN** však v případě, že pro událost z levé strany spojení není nalezena shoda, vrátí řádek s hodnotami NULL pro všechny sloupce pravé strany. Tato technika je velmi užitečná k vyhledání absence událostí. Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci MSDN.

![Výsledky spojení](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Závěr
Účelem tohoto kurzu je předvést způsob psaní různých dotazů v jazyku Stream Analytics Query Language a zobrazení výsledků v prohlížeči. Je to však jenom začátek. Pomocí Stream Analytics toho můžete dělat mnohem víc. Stream Analytics podporuje celou řadu vstupů a výstupů a může dokonce využít i funkce ve službě Azure Machine Learning. To z něj dělá robustní nástroj pro analýzu datových proudů. Můžete začít s bližším prozkoumáváním Stream Analytics pomocí našich [výukových materiálů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Další informace o tom, jak psát dotazy, najdete v článku o [běžných vzorech dotazů](stream-analytics-stream-analytics-query-patterns.md).

<!--HONumber=Oct16_HO3-->


