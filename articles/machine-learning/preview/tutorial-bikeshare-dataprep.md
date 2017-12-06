---
title: "Kurz – Příprava pokročilé dat pomocí Azure Machine Learning Workbench kolo sdílené složky"
description: "A přípravu kurzu začátku do konce dat pomocí Azure Machine Learning Workbench"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: article
ms.date: 09/21/2017
ms.openlocfilehash: e85515c29d8f626c7eb2bfb636dc6c18da78b5c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Kurz kolo share: Advanced Příprava dat pomocí Azure Machine Learning Workbench
Služby Azure Machine Learning (preview) je vědecké zpracování dat integrované, klient server a pokročilou analýzu řešení pro odborníky v oblasti datových vědců připravit data, vývoji experimenty a nasazení modely v cloudovém měřítku.

V tomto kurzu pomocí služby Azure Machine Learning (preview) Další postup:
> [!div class="checklist"]
> * Příprava dat interaktivně pomocí nástroje Azure Machine Learning Data přípravy
> * Import, transformace a vytvořit testovací datové sady
> * Generovat balíček Příprava dat
> * Spusťte balíček přípravu dat používá Python
> * Generovat datovou sadu školení podle opakovaného použití balíčku Příprava dat pro další vstupní soubory

> [!IMPORTANT]
> V tomto kurzu připraví pouze data, nelze sestavit předpovědi modelu.
>
> Můžete připravit data pro učení vlastní modely předpovědi. Například může vytvořit model k předvídání kolo vyžádání během časového období 2 hodin.

## <a name="prerequisites"></a>Požadavky
* Azure Machine Learning Workbench musí být nainstalovaný místně. Další informace najdete [instalace rychlý Start](quickstart-installation.md).
* Znalost vytvoření nového projektu v nástroje Workbench.

## <a name="data-acquisition"></a>Získávání dat
Tento kurz používá [datovou sadu Boston Hubway](https://s3.amazonaws.com/hubway-data/index.html) a Boston počasí data z [NOAA](http://www.noaa.gov/).

1. Stahování datových souborů z následujících odkazů na vašem místním vývojovém prostředí. 
   * [Data o počasí Boston](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway cestě data z webu Hubway.

      - [201501. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701. hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Po stažení rozbalte každý soubor .zip.

## <a name="learn-about-the-datasets"></a>Další informace o datové sady
1. __Počasí Boston__ soubor obsahuje následující pole související počasí, ohlášeny hodinu:
   * DATUM
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. __Hubway__ uspořádá data do souborů rok a měsíc. Například soubor s názvem `201501-hubway-tripdata.zip` obsahuje soubor CSV obsahující data pro leden 2015. Datové pole obsahuje každý řádek představující výlet kolo následující pole:

   * Doba trvání cesty (v sekundách)
   * Datum a čas spuštění
   * Datum a čas ukončení
   * Název stanice & ID spustit
   * Atribut Name elementu end stanice & ID
   * ID kolo
   * Typ uživatele (běžné = 24 hodin nebo 72 hodin průchodu uživatele. Člen = – člen měsíční nebo roční)
   * PSČ (Pokud je uživatel)
   * Pohlaví (samoobslužné hlášené člen)

## <a name="create-a-new-project"></a>Vytvoření nového projektu
1. Spusťte **Azure Machine Learning Workbench** z nabídky start nebo Spouštěče.

2. Vytvořte nový projekt Azure Machine Learning.  Klikněte na tlačítko  **+**  tlačítko **projekty** stránky, nebo **soubor** > **nový**.
   - Použití **prázdného projektu** šablony.
   - Název projektu **BikeShare**. 

## <a id="newdatasource"></a>Vytvořit nový zdroj dat.

1. Vytvořte nový zdroj dat. Klikněte **Data** na levém panelu nástrojů tlačítko (ikona cylindr). Zobrazí se **zobrazení dat**.

   ![Obrázek kartě zobrazení dat.](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Přidání zdroje dat. Vyberte  **+**  ikonu a vyberte **přidat zdroj dat**.

   ![Obrázek položky přidat zdroj dat](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Přidat data počasí

1. **Úložiště dat**: Vyberte úložiště dat, který obsahuje data. Vzhledem k tomu, že používáte soubory, vyberte **souborům / Directory**. Vyberte **Další** pokračujte.

   ![Obrázek soubory nebo položku adresáře](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Soubor výběr**: přidejte počasí data. Procházet a vybrat `BostonWeather.csv` soubor, který jste předtím stáhli. Klikněte na **Další**.

   ![Obrázek výběru souborů s BostonWeater.csv vybrané](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Podrobnosti souborů**: Zkontrolujte schéma souboru, který je zjištěn. Azure Machine Learning Workbench analyzuje data v souboru a odvodí, že schéma tak, aby použít.

   ![Obrázek podrobnosti souboru](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Nástroje Workbench nemusí rozpoznat správné schéma v některých případech. Vždycky ověřte, že parametry jsou správné pro datovou sadu. Údaje o počasí ověřte, jestli jsou nastavené na následující hodnoty:
   >
   > * __Typ souboru__: soubor s oddělovači (csv, tsv, txt, atd.)
   > * __Oddělovač__: čárkami [,]
   > * __Komentář znak řádku__: není nastavena žádná hodnota.
   > * __Přeskočit řádky režimu__: nevynechte
   > * __Kódování souborů__: utf-8
   > * __Zvýšení úrovně hlavičky režimu__: použít záhlaví z první soubor

   Ve verzi preview dat by měl zobrazit následující sloupce:
   * **Cesta**
   * **DATUM**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Chcete-li pokračovat, vyberte **Další**.

4. **Datové typy**: Zkontrolujte typy dat, které se rozpoznávají automaticky. Azure Machine Learning Workbench analyzuje data v souboru a odvodí typů dat, který chcete použít.

   Tato data změnit `DATA TYPE` všech sloupců do `String`.

   > [!NOTE]
   > `String`abyste měli na očích možnosti nástroje Workbench později v tomto kurzu se používá. 

   ![Obrázek datové typy](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Chcete-li pokračovat, vyberte __Další__. 

5. **Vzorkování**: Chcete-li vytvořit schéma vzorkování, vyberte **+ nový** tlačítko. Vyberte novou __horní 10000__ řádek, který je added a pak vyberte __upravit__. Nastavit __ukázka strategie__ k **celého souboru**a potom vyberte **použít**.

   ![Obrázek přidání nové strategie vzorkování](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Použít __celého souboru__ strategie, vyberte __celého souboru__ položku a pak vyberte __nastavená jako aktivní__. Vedle položky se objeví hvězdička __celého souboru__ to znamená, že je aktivní strategie.

   ![Obrázek celého souboru jako aktivní strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Chcete-li pokračovat, vyberte **Další**.

6. **Cesta sloupec**: __cesta sloupec__ část vám pomůže jako sloupec v importovaných dat obsahovat úplnou cestou k souboru. Vyberte __neobsahují cestu sloupec__.

   > [!TIP]
   > Včetně cesty jako sloupec je užitečné, pokud chcete importovat do složky mnoho soubory s názvy jiný soubor. Je také užitečné, pokud se názvy souborů obsahují informace, které mají být extrahovány později.

   ![Obrázek cesta sloupci nezahrnují](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Dokončit**: Chcete-li dokončit vytváření zdroje dat, vyberte **Dokončit** tlačítko.

    Karta nový zdroj dat s názvem __BostonWeather__ otevře. Ukázka dat se zobrazí v zobrazení mřížky. Ukázka je založena na schéma active vzorkování dříve zadaný.

    Upozornění **kroky** podokně na pravé straně obrazovky se zobrazí jednotlivé akce prováděné při vytváření tohoto zdroje dat.

   ![Zobrazení zdroje dat, ukázka a kroky bitové kopie](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Zobrazit metriky zdroje dat

Vyberte __metriky__ tlačítka v horní pravé zobrazení mřížky na kartě. Toto zobrazení ukazuje distribuce a další souhrnné statistiky jen Vzorkovaná data.

![Zobrazí obrázek metriky](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Chcete-li konfiguraci zobrazení statistik, použijte **zvolte metrika** rozevíracího seznamu. Zkontrolujte a zrušte zaškrtnutí políčka metriky existuje, chcete-li změnit zobrazení mřížky.

Vrátit __zobrazení dat__, vyberte __Data__ z levé horní části stránky.

## <a name="add-data-source-to-data-preparation-package"></a>Přidat zdroje dat do data přípravy balíčku

1. Vyberte __Příprava__ zahájíte Příprava data. 

2. Po zobrazení výzvy zadejte název balíčku Příprava dat, například `BikeShare Data Prep`. 

3. Vyberte __OK__ pokračujte.

   ![Obrázek dialogového okna Příprava](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Nový balíček s názvem **BikeShare Data Prep** se zobrazí pod __Příprava dat__ části __Data__ kartě. 

   Chcete-li zobrazit balíčku, vyberte tuto položku. 

5. Vyberte  **>>**  tlačítko rozbalte zobrazíte __toky dat__ obsažené v balíčku. V tomto příkladu __BostonWeather__ pouze toku dat je.

   > [!IMPORTANT]
   > Balíček může obsahovat více toky dat.

   ![Obrázek toky dat obsažené v balíčku](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtrování dat podle hodnoty
1. Filtrování dat, klikněte pravým tlačítkem na buňku s určitou hodnotu a vyberte __filtru__a pak typ filtru.

2. V tomto kurzu vyberte buňku, která obsahuje hodnotu `FM-15` a potom nastavte filtr na filtr **rovná**.  Teď je filtrovaná data pouze vrací řádky kde __REPORTTYPE__ je `FM-15`.

   ![Obrázek dialogového okna filtru](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM – 15 je typ z meteorologických Terminálové leteckých rutiny počasí sestavy (METAR). FM – 15 sestavy jsou dodržovány empirically být nejúplnější, s malým množstvím chybějící data.

## <a name="remove-a-column"></a>Odebrat sloupec

Již nepotřebujete __REPORTTYPE__ sloupce. Klikněte pravým tlačítkem na záhlaví sloupce a vyberte **odebrat sloupec**.

   ![Obrázek možnost odebrat sloupce](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Změna datových typů a odeberte chyby
1. Stisknutím __Ctrl (příkaz ⌘ na Mac)__ při výběru záhlaví sloupců vám umožní vybrat více sloupců najednou. To lze použijte k výběru následující záhlaví sloupců:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Klikněte pravým tlačítkem na** jednu z hlaviček vybraného sloupce a vyberte **typ pole převod na číselný**. Tento datový typ sloupce převede na číselnou.

   ![Převod na číselný více sloupců](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filtrovat chybové hodnoty. Datový typ převodu problémů mají některé sloupce. Tento problém je označená červenou barvu v __pruh dat kvality__ pro sloupec.

   Chcete-li odebrat řádků s chybami, klikněte pravým tlačítkem na **HOURLYDRYBULBTEMPF** záhlaví sloupce. Vyberte **filtrovat sloupce**. Použít výchozí **I má** jako **zachovat řádky**. Změna **podmínky** rozevírací nabídku a vyberte **není chyba**. Vyberte **OK** použít filtr.

4. Chcete-li odstranit řádky zbývající chyby v ostatních sloupců, opakujte tento postup filtru pro **HOURLYRelativeHumidity** a **HOURLYWindSpeed** sloupce.

## <a name="use-by-example-transformations"></a>Používat jako příklad transformace

Data v předpovědi použít pro dvě hodiny čas bloky, je nutné vypočítat průměrný počasí po dobu dvou hodin. K tomuto účelu můžete použít následující akce:

* Rozdělení **datum** sloupce do samostatné **datum** a **čas** sloupce. Najdete v části Další podrobné pokyny.

* Odvození **Hour_Range** sloupec z **čas** sloupce. Následující části Podrobné pokyny.

* Odvození **datum\_hodinu\_rozsah** sloupec z **datum** a **Hour_Range** sloupce. Následující části Podrobné pokyny.

### <a name="split-column-by-example"></a>Rozdělení sloupce podle příkladu

1. Rozdělení **datum** sloupce do samostatné datum a čas sloupce. Klikněte pravým tlačítkem na **datum** záhlaví sloupce a vyberte **rozdělit sloupec podle příkladu**.

   ![Obrázek rozdělit sloupec podle příklad položky](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench automaticky identifikuje smysluplný oddělovač a vytvoří dva sloupce rozdělením data do hodnoty data a času. 

3. Vyberte __OK__ tak, aby přijímal výsledky operace rozdělení.

   ![Obrázek rozdělení sloupce DATE_1 a DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Odvození sloupce podle příkladu

1. Odvození rozsah dvě hodiny, klikněte pravým tlačítkem myši __datum\_2__ záhlaví sloupce a vyberte **odvozena sloupec podle příkladu**.

   ![Obrázek odvodit sloupec podle příklad položky](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Nový prázdný sloupec se přidá s hodnotami null.

2. Klikněte na první prázdné buňky do nového sloupce. Uveďte příklad časové rozmezí potřeby zadáním `12AM-2AM` v nový sloupec a poté stiskněte klávesu enter.

   ![Obrázek nový sloupec s hodnotou z 12: 00 – 2: 00](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML Workbench syntetizuje program na základě příkladů zadaný a použije stejný program na zbývající řádky. Všechny ostatní řádky se vyplní automaticky založena na příkladu, které jste zadali. Workbench také analyzuje data a pokusí k identifikaci případů okraj. 
  
3. Text **analyzování dat** výše mřížky označuje, že Workbench se pokouší zjistit případech okraj. Až budete hotoví, stav se změní na **zkontrolujte další návrhy řádek** nebo **žádné návrhy**. V tomto příkladu **zkontrolujte další návrhy řádek** je vrácen.

4. Chcete-li zkontrolovat navrhované změny, vyberte **zkontrolujte další návrhy řádek**. Na zobrazení je zvýrazněna buňku, která zkontrolujte a opravte (v případě potřeby).

   ![Obrázek Zkontrolujte řádek](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Vyberte __OK__ tak, aby přijímal transformace.
 
5. Jste vráceni do zobrazení mřížky dat pro __BostonWeather__. Mřížky nyní obsahuje tři sloupce, který jste přidali dříve.

   ![Obrázek zobrazení mřížky s přidané řádky](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Na se zachovají všechny změny provedené **kroky** podokně. Přejděte ke kroku, který jste vytvořili v **kroky** podokně, klikněte na šipku dolů a vyberte **upravit**. Okno Upřesnit pro **odvozena sloupec podle příkladu** se zobrazí. Zde se zachovají všechny příklady. Příklady můžete také přidat ručně dvojím kliknutím na řádek v tabulce níže. Vyberte **zrušit** se vraťte zpět do hlavní mřížky bez použití změn. Toto zobrazení můžete taky přejít výběrem **rozšířeném režimu** při provádění **odvozena sloupec podle příkladu** transformace.

6. Chcete-li přejmenovat sloupec, dvakrát klikněte na záhlaví sloupce a typ **hodinu rozsah**. Stiskněte klávesu **Enter** uložte tuto změnu.

   ![Přejmenování sloupce](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Odvození rozsah data a hodinu, vybrat víc **datum\_1** a **hodinu rozsah** sloupce, klikněte pravým tlačítkem a pak vyberte **odvodit sloupec podle příkladu**.

   ![Odvozování sloupec příklad](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Typ `Jan 01, 2015 12AM-2AM` jako v příkladu na prvním řádku a stiskněte klávesu **Enter**.

   Nástroje Workbench určuje transformaci založena na příkladu, které zadáte. V tomto příkladu výsledkem je, že formát data je změněn a zřetězen s dvou hodin.

   ![Obrázek příkladu, 01 ledna 2015 12: 00 – 2: 00](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)


8. Počkejte na změní ze stavu **analyzování dat** k **zkontrolujte další návrhy řádek**. To může trvat několik sekund. Vyberte stav odkazu přejděte na navrhované řádek. 

   ![Obrázek navrhované řádek ke kontrole](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Řádek má hodnotu null, protože zdrojové hodnoty date může být pro dd/mm/rrrr nebo mm/dd/rrrr. Zadejte správnou hodnotu `Jan 13, 2015 2AM-4AM` a stiskněte klávesu **Enter**. Nástroje workbench používá dva příklady ke zlepšení odvození zbývající řádků.

   ![Obrázek správně formátovaných dat](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Vyberte **OK** tak, aby přijímal pro transformaci.

   ![Obrázek mřížky dokončené transformace](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Můžete použít rozšířené režimu **odvodit sloupec podle příkladu** pro tento krok kliknutím na šipku dolů ve **kroky** podokně. V datové mřížce jsou políček vedle názvů sloupců **datum\_1** a **hodinu rozsah** sloupce. Zrušte zaškrtnutí políčka vedle **hodinu rozsah** sloupec a zjistěte, jak se tak změní výstup. Chybí **hodinu rozsah** sloupec jako vstup, **12: 00 – 2: 00** se považuje za konstanta a připojí se k odvozené hodnoty. Vyberte **zrušit** se vraťte zpět do hlavní mřížky bez použití změn.

10. Chcete-li přejmenovat sloupec, dvakrát klikněte na záhlaví. Změňte název **rozsah hodinu** a potom stiskněte klávesu **Enter**.

11. Vícenásobný výběr **datum**, **datum\_1**, **datum\_2**, a **hodinu rozsah** sloupce. Klikněte pravým tlačítkem a pak vyberte **odebrat sloupec**.

## <a name="summarize-data-mean"></a>Shrnout data (střední)

Dalším krokem je shrnout počasí provedením střední hodnoty, které jsou seskupené podle rozsahu hodinu.

1. Vyberte **rozsah hodinu** sloupec a potom vyberte **Summarize** z **transformuje** nabídky.

    ![Transformuje nabídky](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Pro shrnutí dat, přetažením sloupců z mřížky v dolní části stránky do podokna vlevo a vpravo nahoře. V levém podokně obsahuje text **přetažením sloupců sem data skupiny**. V pravém podokně obsahuje text **přetažením sloupců sem shrnout data o**. 

    Přetáhněte **rozsah hodinu** sloupec z mřížky v dolní části do levého podokna. Přetáhněte **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity**, a **HOURLYWindSpeed** do pravého podokna. 

    V pravém podokně vyberte **znamenat** jako **agregační** měr pro každý sloupec. Klikněte na tlačítko **OK** k dokončení shrnutí.

   ![Obrázek obrazovky souhrnná data](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Transformace pomocí skriptu toku dat

Změna dat v číselné sloupce na rozsah 0-1 umožňuje některých modelů a umožňuje konvergovat rychle. Aktuálně neexistuje žádné předdefinované transformace na obecně se tato transformace, ale skript v jazyce Python lze použít k provedení této operace.

1. Z **transformace** nabídce vyberte možnost **transformace toku dat**.

2. Zadejte následující kód do textového pole, která se zobrazí. Pokud jste použili názvy sloupců, kód by měla fungovat bez úprav. V Pythonu píšete logiku jednoduché normalizaci min-max.

    > [!WARNING]
    > Skript předpokládá, že názvy sloupců použít dříve v tomto kurzu. Pokud máte různé názvy sloupce, musíte změnit názvy ve skriptu.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > Skript Pythonu musí vracet `df` na konci. Tato hodnota se používá k naplnění mřížky.
    
    ![Transformace dialogové okno skriptu toku dat](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Vyberte __OK__ pomocí skriptu. Číselné sloupce v mřížce teď obsahují hodnoty v rozsahu 0-1.

    ![Mřížky obsahující hodnoty mezi 0 a 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Dokončíte přípravu dat počasí. V dalším kroku připravte data cesty.

## <a name="load-trip-data"></a>Načtení dat cesty

1. Chcete-li importovat `201701-hubway-tripdata.csv` souboru, postupujte podle kroků v [vytvořit nový zdroj dat](#newdatasource) části. Během procesu importu použijte následující možnosti:

    * __Schéma vzorkování__: **celého souboru** vzorkování schéma, zkontrolujte, že ukázku aktivní, a 
    * __Datový typ__: přijměte výchozí hodnoty.

2. Po importu dat, vyberte __Příprava__ tlačítko Zahájit přípravu data. Vyberte existující **BikeShare Data Prep.dprep** balíček a potom vyberte __OK__.

    Přidá tento proces **toku dat** ke stávající **Příprava dat** souboru místo vytvoření nového.

    ![Vyberte existující balíček bitové kopie](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Jakmile se načetl mřížky, rozbalte položku __toky dat__. Existují dvě toky dat: **BostonWeather** a **201701. hubway tripdata**. Vyberte **201701. hubway tripdata** položku.

    ![Obrázek položky 201701. hubway tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Použijte nástroj inspector mapy

Pro přípravu dat existuje několik užitečné vizualizací názvem **inspektoři** pro řetězec, číselnou a zeměpisného data, která pomáhají data lépe porozumět a při identifikaci odlehlé hodnoty. Použít kontrola mapy pomocí následujících kroků:

1. Vícenásobným výběrem **spustit stanice šířky** a **spustit zeměpisnou délku stanice** sloupce. Klikněte pravým tlačítkem na některý ze sloupců a pak vyberte **mapy**.

    > [!TIP]
    > Chcete-li povolit vícenásobným výběrem, podržte klávesu __Ctrl (příkaz ⌘ na Mac)__ a vyberte záhlaví pro každý sloupec.

    ![Obrázek vizualizace mapy](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Chcete-li maximalizovat vizualizace mapy, vyberte **Maximalizovat** ikonu. Podle mapy do okna, vyberte **E** ikonu na levém horním straně vizualizaci.

    ![Maximalizovaném okně bitové kopie](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Klikněte na **minimalizaci** tlačítko se vrátit do zobrazení mřížky.

## <a name="use-column-statistics-inspector"></a>Použít sloupec statistiky kontrola

Chcete-li použít nástroj inspector statistiky sloupce, klikněte pravým tlačítkem na __tripduration__ sloupce a vyberte __statistiky sloupce__.

![Položka statistiky sloupce](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Tento postup přidá novou vizualizaci s názvem __tripduration statistiky__ v __INSPEKTOŘI__ podokně.

![Obrázek inspector statistiky tripduration](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Maximální doba trvání cesty hodnotu **961,814 minut**, který je o dva roky. Zdá se, že se některé odlehlé hodnoty v datové sadě.

## <a name="use-histogram-inspector"></a>Použijte nástroj inspector histogram

Došlo k pokusu o identifikaci odlehlé hodnoty, klikněte pravým tlačítkem __tripduration__ sloupce a vyberte __Histogram__.

![Histogram inspector](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Histogramu není užitečná, jako odlehlé hodnoty jsou zkosení grafu.

## <a name="add-column-using-script"></a>Přidat sloupec pomocí skriptu

1. Klikněte pravým tlačítkem na **tripduration** sloupce a vyberte **přidat sloupec (skript)**.

    ![Obrázek v nabídce Přidat sloupec (skript)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. V __přidat sloupec (skript)__ dialogové okno, použijte následující hodnoty:

    * __Název nového sloupce__: logtripduration
    * __Vložit tento nový sloupec po__: tripduration
    * __Nový kód pro sloupec__:`math.log(row.tripduration)`
    * __Typ bloku kódu__: výraz

   ![Dialogové okno Přidat sloupec (skript)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Vyberte __OK__ přidat **logtripduration** sloupce.

4. Klikněte pravým tlačítkem na sloupce a vyberte **Histogram**.

    ![Histogram logtripduration sloupce](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Tento histogram vizuálně, zdá normálního rozdělení s neobvyklé poškozené databáze.

## <a name="use-advanced-filter"></a>Použití rozšířeného filtru

Pomocí filtru na základě dat aktualizuje inspektoři nové rozdělení. Klikněte pravým tlačítkem na **logtripduration** sloupce a vyberte **sloupce filtru**. V __upravit__ dialogové okno, použijte následující hodnoty:

* __Filtrování v tomto sloupci číslo__: logtripduration
* __Chci__: zachová řádky
* __Když__: všechny následující podmínky mají hodnotu True (nebo logické)
* __Pokud v tomto sloupci__: menší než
* __Hodnota__: 9

![Možnosti filtru.](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Vyberte __OK__ použít filtr.

![Aktualizované histogramy po filtru](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Účinek bylo

1. Maximalizovat **logtripduration** histogram. Není blue histogram jako překryvný obrázek na šedé histogram. Toto zobrazení je volána **bylo vliv**:

    * **Šedé histogram** představuje rozdělení před provedením operace (v tomto případě filtrování operaci).
    * **Blue histogram** představuje histogramu po operaci. 

   Účinek bylo pomáhá s vizualizace účinku operace na data.

   ![Obrázek účinek bylo](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Všimněte si, že blue histogram, zobrazí se kratší ve srovnání s předchozí. To je kvůli automatické opětovné bucketing dat v nový rozsah.

2. Bylo odebrat, vyberte __upravit__ a zrušte zaškrtnutí políčka __zobrazit bylo__.

    ![Možnosti pro histogram](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Vyberte **OK** zakázat bylo účinek a pak minimalizovat histogramu.

### <a name="remove-columns"></a>Odebrání sloupců

V cestě dat každý řádek představuje výstupní událost kolo. Pro účely tohoto kurzu potřebujete jenom **starttime** a **spustit id stanice** sloupce. Odeberte ostatní sloupce podle vyberete víc těmito dvěma sloupci, klikněte pravým tlačítkem myši na záhlaví sloupce a potom vyberte **zachovat sloupec**. Ostatních sloupců se odeberou.

![Obrázek možnosti zachovat sloupce](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Shrnout data (počet)

To Shrneme kolo vyžádání období 2 hodin, použijte odvozených sloupců.

1. Klikněte pravým tlačítkem na **starttime** sloupce a vyberte **odvozena sloupec příklad**

    ![Obrázek odvozena příklad možnosti](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Například, zadejte hodnotu `Jan 01, 2017 12AM-2AM` pro první řádek.

    > [!IMPORTANT]
    > V předchozím příkladu odvozující sloupců používá několik kroků k odvození sloupec, který obsažené datum a čas období. V tomto příkladu vidíte, že tuto operaci lze provést jako jeden krok tím, že poskytuje příklad finální výstup.

    > [!NOTE]
    > Můžete udělit příklad proti některé řádky. V tomto příkladu hodnota `Jan 01, 2017 12AM-2AM` je platný pro první řádek dat.

    ![Obrázek příkladu data](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)
   
3. Počkejte, až aplikace vypočítá hodnoty pro všechny řádky. To může trvat několik sekund. Po dokončení analýzy používat __zkontrolujte další návrhy řádek__ odkaz na zkontrolujte data.

   ![Obrázek dokončené analýzy s Zkontrolujte propojení](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Zkontrolujte správnost počítané hodnoty. Pokud ne, aktualizujte hodnotu očekávanou hodnotou a stiskněte klávesu enter. Potom počkejte, než pro analýzu k dokončení. Dokončení **zkontrolujte další návrhy řádek** zpracovat, dokud neuvidíte **žádné návrhy**. Jakmile se zobrazí **žádné návrhy**, aplikace zvážení případech okraj a je s syntetizovaná program. Je osvědčeným postupem proveďte visual kontrolu Transformovaná data, před přijetím transformace. 

4. Vyberte **OK** tak, aby přijímal pro transformaci. Přejmenujte nově vytvořený sloupec, který se **rozsah hodinu**.

    ![Obrázek přejmenovat sloupec](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Klikněte pravým tlačítkem na **starttime** záhlaví sloupce a vyberte **odebrat sloupec**.

6. Chcete-li shrnutí dat, vyberte __Summarize__ z __transformace__ nabídky. Pokud chcete vytvořit transformace, proveďte následující akce:

    * Přetáhněte __rozsah hodinu__ a __spustit id stanice__ do podokna doleva (Seskupit podle).
    * Přetáhněte __spustit id stanice__ vpravo (shrnout data o) podokně.

   ![Obrázek možnosti shrnutí](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Vyberte **OK** tak, aby přijímal shrnutí výsledků.

## <a name="join-dataflows"></a>Připojení k toky dat

K připojení dat počasí pomocí cesty data, použijte následující kroky:

1. Vyberte __připojení__ z __transformuje__ nabídky.

2. __Tabulky__: vyberte **BostonWeather** jako levé toku dat a **201701. hubway tripdata** jako správné toku dat. Chcete-li pokračovat, vyberte **Další**.

    ![Obrázek výběr tabulek](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Sloupce Key__: vyberte **rozsah hodinu** sloupec v obě tabulky a potom vyberte __Další__.

    ![Obrázek připojení na klíčové sloupce](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Typ spojení__: vyberte __odpovídající řádky__ jako typ spojení a potom vyberte __Dokončit__.

    ![Typ spojení odpovídající řádky](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Tento proces vytvoří nové toku dat, s názvem __připojení výsledek__.

## <a name="create-additional-features"></a>Vytvořte další funkce

1. Pokud chcete vytvořit sloupec, který obsahuje den v týdnu, klikněte pravým tlačítkem na **rozsah hodinu** sloupce a vyberte **odvodit sloupec podle příkladu**. Použít hodnotu __Sun__ pro datum, které došlo k chybě v neděli. Například **01. ledna 2017 12: 00 – 2: 00**. Stiskněte klávesu **Enter** a pak vyberte **OK**. Přejmenujte tento sloupec __den v týdnu__.

    ![Obrázek vytvoření nový sloupec, který obsahuje den v týdnu](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Pokud chcete vytvořit sloupec obsahující časové období pro řádek, klikněte pravým tlačítkem na **rozsah hodinu** sloupce a vyberte **odvodit sloupec podle příkladu**. Použít hodnotu **12: 00 – 2: 00** pro řádek obsahující **01. ledna 2017 12: 00 – 2: 00**. Stiskněte klávesu **Enter** a pak vyberte **OK**. Přejmenujte tento sloupec **období**.

    ![Obrázek sloupec období](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. K odebrání **rozsah hodinu** a **rDate hodinu rozsah** sloupce, stiskněte **Ctrl (příkaz ⌘ na Mac)** a vyberte každý záhlaví sloupce. Klikněte pravým tlačítkem a pak vyberte **odebrat sloupec**.

## <a name="read-data-from-python"></a>Čtení dat z Pythonu

Můžete spustit balíček přípravy data z Pythonu nebo PySpark a načíst výsledek v podobě **dat rámce**.

Pokud chcete vygenerovat například skript v jazyce Python, klikněte pravým tlačítkem na __BikeShare Data Prep__ a vyberte __generovat soubor dat přístupového kódu__. Soubor Python příkladu se vytvoří v vaše **složky projektu**který je taky načíst na kartě v rámci nástroje workbench. Následující skript jazyka Python je příkladem generovaného kódu:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

V tomto kurzu je název souboru `BikeShare Data Prep.py`. Tento soubor se používá později v tomto kurzu.

## <a name="save-test-data-as-a-csv-file"></a>Testovací data uložit do souboru CSV

Uložit **připojení výsledek** toku dat do. Soubor CSV, musíte změnit `BikeShare Data Prep.py` skriptu. Aktualizace skript Pythonu pomocí následujícího kódu:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Vyberte **spustit** z horní části obrazovky. Skript je odeslán jako **úlohy** v místním počítači. Jakmile se stav úlohy změní na __dokončeno__, soubor byl zapsán do zadaného umístění.

## <a name="substitute-data-sources"></a>Nahraďte zdroje dat

V předchozích krocích jste použili `201701-hubway-tripdata.csv` a `BostonWeather.csv` zdroje dat pro přípravu v testovacích datech. Pokud chcete použít balíček s ostatními soubory dat služební cestě, použijte následující kroky:

1. Vytvořte novou **zdroj dat** pomocí kroků uvedených výše, s následujícími změnami do procesu:

    * __Soubor výběr__: při výběru souboru, vybrat víc šest zbývající tripdata cesty. CSV soubory.

        ![Načíst šesti zbývající soubory](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > __+ 5__ položka znamená, že se pět další soubory nad rámec ten, který je uveden.

    * __Podrobnosti souborů__: nastavte __povýšit hlavičky režimu__ k **všechny soubory mají stejné hlavičky**. Tato hodnota určuje, že každý soubor obsahuje hlavičku stejné.

        ![Podrobnosti o výběru souborů](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Název tohoto zdroje dat, uložte, protože se používá v dalších krocích.

2. Vyberte ikonu složky k prohlížení souborů ve vašem projektu. Rozbalte __aml\_konfigurace__ adresář a pak vyberte `local.runconfig` souboru.

    ![Obrázek umístění local.runconfig](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Přidejte následující řádky na konci `local.runconfig` souboru a potom vyberte ikonu disku k uložení souboru.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Tato změna nahradí původní zdroj dat ten, který obsahuje šest cestě datových souborů.

## <a name="save-training-data-as-a-csv-file"></a>Uložit Cvičná data do souboru CSV

Přejděte na soubor Python `BikeShare Data Prep.py` upravit dříve a zadejte jinou cestu souboru pro uložení jsou Cvičná Data.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Chcete-li odeslat novou úlohu, použijte **spustit** ikonu v horní části stránky. A **úlohy** odeslána s novou konfiguraci. Výstup této úlohy je Cvičná Data. Tato data se vytvoří pomocí stejné Data přípravné kroky, které jste předtím vytvořili. To může trvat několik minut na dokončení úlohy.

## <a name="next-steps"></a>Další kroky
Dokončili jste kurz kolo sdílení dat přípravy. V tomto kurzu jste použili Azure Machine Learning services (preview) Další postup:
> [!div class="checklist"]
> * Příprava dat interaktivně pomocí nástroje Azure Machine Learning Data přípravy
> * Import, transformace a vytvořit testovací datové sady
> * Generovat balíček Příprava dat
> * Spusťte balíček přípravu dat používá Python
> * Generovat datovou sadu školení podle opakovaného použití balíčku Příprava dat pro další vstupní soubory

Další informace o přípravě dat v dalším kroku:
> [!div class="nextstepaction"]
> [Průvodce přípravou dat uživatele](data-prep-user-guide.md)
