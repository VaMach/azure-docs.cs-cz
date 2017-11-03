---
title: "Použití transformací dat pro přípravu dat v Azure Machine Learning | Microsoft Docs"
description: "Tento článek obsahuje úplný seznam transformací, které jsou k dispozici pro Azure Machine Learning Příprava dat"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: 4331bb4a16d56f027dd63a97868822fa6ecc92d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Použití transformací dat pro přípravu dat v Azure Machine Learning

A *transformace* v Azure Machine Learning spotřebuje data v daném formátu, provede operace s daty (jako je například změna datového typu) a potom vytvoří data v novém formátu. Každý transformace má svou vlastní rozhraní a chování. Několik transformací společně pomocí kroků v toku dat, můžete zřetězené umožňuje provádět komplexní a opakovatelných transformace na vaše data. Toto je základní funkce datového přípravy.

Následuje seznam transformací dostupné v Azure Machine Learning. 

## <a name="column-selection"></a>Výběr sloupce 
Mnohé z následujících uvedených transformace použít na jeden sloupec nebo mnoho. Pokud chcete vybrat více sloupců, použijte **Ctrl** klíče; nebo pokud chcete vybrat oblast sloupců, použijte **Shift** klíč.

## <a name="transforms-from-main-menu-andor-grid-header"></a>Transformuje z hlavní nabídky nebo mřížky hlavičky 
Transformace můžete získat přístup pomocí volby transformace v hlavní nabídce. Transformace také možné vybrat kliknutím pravým tlačítkem na název sloupce v datové mřížce. Pokud je vybraných více sloupců, klikněte pravým tlačítkem myši některý z nich poskytuje nabídku transformací.

Pouze platné transformace pro datovému typu vybranému jsou nabízena v místní nabídce. V hlavní nabídce nabízí všechny transformací, ale zakáže transformace, které nejsou relevantní pro vybrané sloupce.

Malou podmnožinu kontextové transformace je k dispozici kliknutím pravým tlačítkem na buňku. Tyto transformace kopírování, nahraďte a filtrovat. Tyto soubory jsou datový typ vědět, takže možnosti pro číslo sloupce se liší od pro řetězcový sloupec.

## <a name="derive-column-by-example"></a>Odvození sloupec příklad
Tato transformace umožňuje vytváření nový sloupec jako odvozený jeden nebo více existujících sloupců. Transformace porovná vstupní sloupce (vybrané) a v příkladu zadané a pak Určuje výstup desire do nového sloupce. 

Chcete-li použít tato transformace, vyberte jeden nebo více sloupců. Přidáte nový sloupec odvozené (prázdný) příklad. Zadejte příklad co chcete vidět v odvozených sloupců (za předpokladu, že je odvozený od ostatních sloupců) a "Podle příkladu" technologie pokusí vyplnit všechna ostatní buňky ve sloupci. 

Příklady složitá může být potřeba zadat více než jedním z příkladů. To pokud chcete udělat, vyberte jinou buňku a zadejte další příklad.

"Příklad" technologie využívá vybrané sloupce při pokusu o určení význam příklad. Nebyly vybrány žádné sloupce, když je volána Tato transformace, se používají všechny buňky pro aktuální řádek. Výběru pouze požadovaných sloupců vede k více přesné výsledky.

Před vyvoláním transformovat můžete vybrat sloupce. Po spuštění editoru transformace zaškrtávací políčka v horní části každý sloupec označují, jaké sloupce jsou vybrány jako vstupy. Můžete přidat nebo odebrat sloupce z výběru pomocí zaškrtávacích políček v záhlaví sloupců.

Podrobnější vysvětlení **odvozené sloupec podle příkladu** transformace, společně s další ukázky, najdete v části: [odvozena sloupec odkazem příklad](data-prep-derive-column-by-example.md)  

## <a name="split-column-by-example"></a>Rozdělit sloupec podle příkladu
Tato transformace trvá existující sloupec a pomocí "Příklad" modul se pokusí rozdělení sloupce do  *n*  ostatních sloupců. Je možné spustit automaticky rozdělení u dalších sloupců vygenerovaný.

Podrobnější vysvětlení **rozdělit sloupec podle příkladu** transformace, společně s další ukázky, najdete v části: [rozdělit sloupec podle příkladu Reference](data-prep-split-column-by-example.md)

## <a name="expand-json"></a>Rozbalení notace JSON

Tato transformace umožňuje přidat více sloupců rozšířením sloupec s platný text JSON.

Podrobnější vysvětlení **rozbalte JSON** transformace, společně s další ukázky, najdete v části: [rozbalte odkaz JSON](data-prep-expand-json.md)


## <a name="combine-columns-by-example"></a>Sloučení sloupců příklad

Tato transformace umožňuje přidat nový sloupec kombinací hodnoty z více sloupců. 

Podrobnější vysvětlení **kombinovat sloupce příklad** transformace, společně s další ukázky, najdete v části: [kombinovat sloupce odkazem příklad](data-prep-combine-columns-by-example.md)


## <a name="duplicate-column"></a>Duplicitní sloupce
Tato transformace umožňuje přesnou kopii jeden nebo více vybraných sloupců a poskytuje každý nový název odvozené z původní název sloupce.

## <a name="text-clustering"></a>Text Clustering 
Tato transformace je navržené tak, aby nekonzistentní hodnoty, které by měl být stejný a je seskupit.  

Použijete-li tato transformace, jsou hodnoty v jednom sloupci analyzován z hlediska podobnosti a seskupené do clusterů. Pro každý cluster je kanonický hodnotu, což je hodnota, která nahradí všechny instance v clusteru a instance hodnoty. Lze odebrat dokončení clustery a kanonické hodnota se dá upravit. Instance můžete odebrat z daného clusteru. Kromě toho může být změněna podobnosti prahová hodnota skóre filtr, který se používá na skupinu instancí do clusteru.

Ve výchozím nastavení Tato transformace nahradí všechny hodnoty instance clusteru kanonické hodnoty pro tento cluster, vytvoření nového sloupce tak, aby obsahovala s novými hodnotami. Je také možné přidat podobnosti skóre, pro každou instanci pro nový sloupec, (který může mít název) tak, aby měl jeho použití později na datový tok.

## <a name="replace-values"></a>Nahraďte hodnoty
Tato transformace umožňuje jeden řetězec nahradit jiným. Zdrojový řetězec může být částečné řetězce nebo úplné buňku; Pokud chcete nahrazení můžete použít na jeden sloupec nebo mnoho. Hledaný řetězec podporuje hledání speciální znaky, jakož i regulární znaků. 

## <a name="replace-na-values"></a>Nahraďte hodnoty NA
Tato transformace umožňuje různé různé formy NA (není k dispozici, NA, null, NaN atd.) nebo prázdné řetězce nahradit jednu hodnotu, aby byly konzistentní. Tato transformace podporuje jeden nebo více sloupců. Tato transformace je uvedena pouze když je vybraný sloupec a není k dispozici v nabídce Hlavní transformace při nebyly vybrány žádné sloupce.

## <a name="replace-missing-values"></a>Nahraďte chybějící hodnoty
Tato transformace chybějící data nahradí jednu hodnotu. Tato transformace podporuje jeden nebo více sloupců. Tato transformace je uvedena pouze když je vybraný sloupec a není k dispozici v nabídce Hlavní transformace při nebyly vybrány žádné sloupce.

## <a name="replace-error-values"></a>Nahradí chybové hodnoty
Tato transformace chyby nahradí jednu hodnotu. Tato transformace podporuje jeden nebo více sloupců. Tato transformace je uvedena pouze když je vybraný sloupec a není k dispozici v nabídce Hlavní transformace při nebyly vybrány žádné sloupce.

## <a name="trim-string"></a>Trim – řetězec
Tato transformace odebere počátečních a koncových znaků "prázdný znak" (obsahuje mezery, tabulátory, *atd*), z jednoho nebo více sloupců.

## <a name="adjust-precision"></a>Upravit přesnost
Tato transformace umožňuje nastavit počet desetinných míst pro číselnou sloupec.

## <a name="rename-column"></a>Přejmenovat sloupec
Tato transformace změní název vybraného sloupce. Tato transformace, může být také vyvolaná vložený v záhlaví sloupce klepnutím na název sloupce.

## <a name="remove-column"></a>Odebere sloupec
Tato transformace odebere vybrané sloupce. Tato transformace funguje na jeden sloupec nebo mnoho. 

## <a name="keep-column"></a>Zachovat sloupce
Tato transformace zachová pouze vybrané sloupce. Tato transformace funguje na jeden sloupec nebo mnoho.

## <a name="handle-path-column"></a>Popisovač cesta sloupce
Při importu souboru cesta sloupec se automaticky přidá do datové sady v průvodci Přidat zdroj dat. Obsahuje plně kvalifikovaný název souboru, která tvoří cestu k datové sadě. Tato transformace přidá nebo odebere navíc sloupce z datové sady.

## <a name="convert-field-type-to-numeric"></a>Převod na číselný typ pole
Tato transformace se změní na číselný typ sloupce. Oddělovač můžete zadat, pokud nejsou data celé číslo. Ve výchozím nastavení není tato transformace výzvu pro oddělovač, použijte **upravit** položku nabídky k vyvolání editoru. Tato transformace funguje na jeden sloupec nebo mnoho.

## <a name="convert-field-type-to-date"></a>Typ pole převést na typ Date
Tato transformace změny sloupec typu datum. Formát data a času výchozím nastavení se používá, ale může být přepsána pomocí `strftime` direktivy. Je také možné předřazení hodnoty time s pevnou datum.

Ve výchozím nastavení Tato transformace není výzvu pro formát, **upravit** položky nabídky v výsledné kroku k vyvolání editoru. Tato transformace funguje na jeden sloupec nebo mnoho.

Pouze data mezi. 1677 9 22 a 4. 11 2262 můžete převést na typ date.

## <a name="convert-field-type-to-boolean"></a>Typ pole převést na logickou hodnotu
Tato transformace změní typ sloupce na hodnotu true nebo false. Tato transformace podporuje mapování více hodnot na hodnotu true nebo false, a je možné upravit tato mapování. Podporuje také konstanta, do kterého můžete namapovat hodnoty, které nejsou k dispozici v tabulkách mapování true nebo false. Tato transformace funguje na jeden sloupec nebo mnoho.

## <a name="convert-field-type-to-string"></a>Typ pole převést na řetězec
Tato transformace změní typ sloupce na řetězec. Tato transformace funguje na jeden sloupec nebo mnoho.

## <a name="convert-unix-timestamp-to-datetime"></a>Časové razítko Unix převést na typ DateTime
Tato transformace rozumí formát časového razítka Unix, i pokud je reprezentována jako řetězec v datech a správně převede na typ date v data prep.

## <a name="filter"></a>Filtr
Tato transformace podporuje filtrování na základě hodnot ve sloupcích jednu nebo více řádků. Podmínky filtru jsou závislé na datový typ jednotlivých sloupců, kterých by bylo možné filtr řetězcové sloupce "obsahuje" nebo "neobsahuje". Číselné sloupce lze filtrovat podle "větší než" "menší než" podmínky.

Po vyvolání transformace filtru z hlavní nabídky, nebo z pravým tlačítkem myši na záhlaví sloupce je k dispozici možnost, chcete-li rozvětvit selhání řádky do jiného datového toku. Bude hlavní datový tok pokračovat s filtrované **v** řádků a nový tok dat je vytvořen, který obsahuje všechny řádky, které byly zjištěny **out**.

## <a name="use-first-row-as-headers"></a>Použít první řádek jako záhlaví
Tato transformace zvýší úroveň prvního řádku ze sady dat jako názvy sloupců. Pokud některé sloupce nemají data v prvním řádku, název se generuje automaticky. Pomocí této transformace znamená, že import dat začíná na řádek 2 nejdříve. V závislosti na nastavení přeskočit řádky může import spustit i další dolů v datové sadě. Může taky sloužit k odebrání povýšení a automaticky vygeneruje názvy pouze.

## <a name="join"></a>Spojit
Tato transformace slouží ke spojení dvou datové toky. Můžete vybrat, které výstup připojení k by měla být výsledek: úspěšné řádky z připojení, "left" selhání řádků ze spojení nebo "vpravo na" selhání řádky z spojení.

Průvodce spojení je spuštěno z jednoho datového toku a vybere tento tok dat jako jeden strany spojení. Potom budete vyzváni k jiné tok dat pro připojení k druhé straně. Po dvou toky je vybráno, Průvodce vyžaduje na každé straně společného být vybrána pro připojení na jeden sloupec. Pokud spojovací potřebuje víc než jeden sloupec, vytvořte před spuštěním Průvodce vytvoří nový sloupec (zřetězených), který se má použít jenom pro připojení k odvozených sloupců. Průvodce se pokusí navrhnout spojení klíčů a pokud je to možné automaticky generovat odvozených sloupců.

Po dokončení spojení se zobrazí zobrazení diagramu Sankey připojení k. Šířka čáry relativně k sobě navzájem představuje počet řádků procházení část toku spojení. Na pravé straně panelu umožňuje vybrat úspěšné řádky, vlevo selhání nebo právo výhradně selhání. Také je možné zvolit pouze jeden větev.

## <a name="append-rows"></a>Připojit řádky
Tato transformace připojí dat z jiného tok dat do stávající. Mapuje sloupce podle pozici pro přidání nové řádky na konci.

## <a name="append-columns"></a>Připojit sloupců
Tato transformace připojí nové sloupce z jiné tok dat pro aktuální. Přidá nové sloupce zprava. nepodporuje se pokusí řádek dat v řádcích.

## <a name="summarize"></a>Shrnutí
Tato transformace vypočítá agregace pro kombinace jedinečné hodnoty jeden nebo více vybraných sloupců. 

Agreguje podporována jsou: počet, SUM, MIN, MAX, střední, ODCHYLKU a SMĚRODATNÉ odchylky. Pouze ty, které se vztahují na datový typ je filtrovaná seznamu agregací pro daný sloupec. Agregace, které se nevztahují jsou zakázány. Například není možné vypočítat střední sloupec řetězce, ale je možné k výpočtu nejnižší a nejvyšší.

Jakmile editoru je k dispozici, přetáhněte ze záhlaví sloupce do panelu nahoře vlevo. kde se zobrazí sloupce, které chcete agregovat. Tento panel je hierarchická, takže je možné udělat vnořené agregace. Editor panelu v pravé horní slouží k výběru agregace, které chcete použít pro sloupec. Jeden sloupec se dají agregovat jeden či více krát. Jakmile byl vybrán alespoň jeden agregace, přináší náhled mřížky v pravé dolní data v agregované podobě. 

Tato transformace je obdobou `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Odeberte duplicitní položky
Tato transformace odebere celý řádek, kde existují duplicitní hodnoty v jednom nebo více vybraných sloupců. Pokud nejsou uvedeny žádné sloupce, jsou vybraná, pak pouze řádky, které jsou odebrány jsou ty, které kde všechny hodnoty ve sloupcích jsou stejné.

## <a name="sort"></a>Seřadit
Tato transformace seřadí data. Řazení lze provést jedním sloupcem nebo mnoho a každý sloupec lze seřadit vzestupně (výchozí) nebo sestupném (můžete měnit v editoru).

Tato transformace je obdobou `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Výstup transformace
Následující transformace výstupní data. Je možné, že více zápisu bloků v jednom toku být schopni zapisovat data v různých fázích.

### <a name="write-to-csv"></a>Zápis do sdíleného svazku clusteru
Tato transformace zapíše data ve formátu CSV z aktuální bodu datový tok. Umožňuje kontrolu nad umístění (místní nebo vzdálené) a různá nastavení kolem soubor.

### <a name="write-to-parquet"></a>Zápis do Parquet
Tato transformace zapíše data ve formuláři Parquet z aktuální bodu datový tok. Umožňuje kontrolu nad umístění (místní nebo vzdálené) a různá nastavení kolem soubor.

## <a name="script-based-transforms"></a>Skript na základě transformací
Následující transformace skript (Python) použít k provedení funkce, které chybí v základním produktu. Před použitím některého z těchto transformací, přečtěte si [rozšiřitelnost pomocí Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Přidat sloupec (skript)
Tato transformace umožňuje sloupec, který se se přidá k datům pomocí výrazu jazyka Python.
Další informace najdete v tématu [Python ukázkový kód pro odvození nové sloupce](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Pokročilé filtru (skript)
Tato transformace umožňuje úrovně filtr Python řádků má být proveden zápis.
Další informace najdete v tématu [příklady filtr výrazů](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transformace toku dat (skript)
Tato transformace umožňuje Python, která má být použita pro celou datovou sadu.
Další informace najdete v tématu [toku dat transformace příklad transformace](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="transform-dataflow-script"></a>Transformace toku dat (skript)
Tato transformace umožňuje Python, která má být použita pro oddíl celý data.
Další informace najdete v tématu [toku dat transformace příklad transformace](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Zápis toku dat (skript)
Tato transformace umožňuje Python, která má být použita pro zápis a celé datové sady.
Další informace najdete v tématu [ukázka Python pro odvození nové sloupce](data-prep-appendix9-sample-destination-connections-python.md).



