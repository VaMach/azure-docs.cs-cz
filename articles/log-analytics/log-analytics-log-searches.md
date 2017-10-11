---
title: "Najít data pomocí protokolu hledání v Azure Log Analytics | Microsoft Docs"
description: "Prohledávání protokolů umožňuje zjišťovat kombinace a korelace jakýchkoli dat o počítačích z více zdrojů v rámci prostředí."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: bf237a837297cb8f1ab3a3340139133adcd2b244
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Najít data pomocí protokolu hledání v analýzy protokolů

>[!NOTE]
> Tento článek popisuje protokolu vyhledávání v aktuální jazyk dotazu analýzy protokolů.  Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak se seznamte s [principy protokolu vyhledá v analýzy protokolů (Nový)](log-analytics-log-search-new.md).


Základem analýzy protokolů je funkce vyhledávání protokolu, která umožňuje zkombinovat a korelovat žádná data počítače z více zdrojů ve vašem prostředí. Řešení se také používá technologii protokolu vyhledávání, aby vám metriky seskupit kolem oblasti konkrétní problém.

Na stránce hledání můžete vytvořit dotaz a pak při hledání, můžete filtrovat výsledky pomocí ovládacích prvků omezující vlastnosti. Můžete také vytvořit pokročilými dotazy transformace, filtr a sestavy na výsledky.

Běžné dotazy vyhledávání protokolu se zobrazí na většina stránek řešení. V konzoli OMS můžete kliknutím na tlačítko dlaždice nebo přejít k podrobnostem na jiné položky k zobrazení podrobností o položce pomocí protokolu vyhledávání.

V tomto kurzu budeme zabývat příklady nepokrývají všechny základní informace při použití hledání protokolů.

Jsme budete začínat jednoduchý, praktické příklady a potom stavět na je, abyste měli představu o případy praktická použití o tom, jak pomocí syntaxe statistiky, které chcete z data.

Poté, co jste obeznámeni s vyhledávání techniky, můžete zkontrolovat [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Použití základní filtrů
První věc, kterou potřebujete vědět, je, že první část vyhledávání dotazu před spuštěním "|" znak svislé čáry znak, je vždy *filtru*. Můžete si ho představit jako klauzule WHERE v TSQL – Určuje *co* podmnožinu dat načítat z úložiště dat OMS. Hledání v úložišti dat je z velké části o zadání charakteristiky data, která mají být extrahovány, takže je přirozené dotazu by začínat klauzuli WHERE.

Nejzákladnější filtry, které můžete použít jsou *klíčová slova*, jako je například "Chyba" nebo "časový limit nebo název počítače. Tyto typy dotazů jednoduché obecně vrátí různých tvarů dat v rámci stejné sadu výsledků dotazu. Je to proto analýzy protokolů má jiný *typy* dat v systému.

### <a name="to-conduct-a-simple-search"></a>Umožňuje provádět jednoduché hledání
1. Na portálu OMS, klikněte na tlačítko **hledání protokolů**.  
    ![dlaždice hledání](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. V poli dotazu zadejte `error` a pak klikněte na **vyhledávání**.  
    ![došlo k chybě hledání](./media/log-analytics-log-searches/oms-search-error.png)  
    Příklad dotazu pro `error` na následujícím obrázku vrátil 100 000 **událostí** záznamy (shromažďují Správa protokolu), 18 **ConfigurationAlert** záznamů (generované konfigurace Assessment) a 12 **ConfigurationChange** záznamy (zachycenou sledování změn).   
    ![výsledky hledání](./media/log-analytics-log-searches/oms-search-results01.png)  

Tyto filtry nejsou skutečně typy/tříd objektů. *Typ* je právě značku, vlastnost nebo řetězec nebo název nebo kategorie, který je připojen k část data. Některé dokumenty v systému jsou označené jako **typu: ConfigurationAlert** a některé jsou označené jako **typu: výkonu**, nebo **typu: událost**a tak dále. Každý výsledek hledání, dokument, záznamu nebo položka zobrazí všechny nezpracované vlastnosti a jejich hodnoty pro každou z těchto položek dat, a tyto názvy polí můžete použít k určení ve filtru, pokud chcete načíst pouze záznamy, kde toto danou hodnotu pole má.

*Typ* je pouze pole, které mají všechny záznamy, není liší od jiné pole. To bylo vytvořeno na základě hodnoty pole typu. Tento záznam bude mít různých formě. Náhodně **typ = výkonu**, nebo **typ = událostí** je také syntaxi, která budete muset naučit se dotázat na údaje o výkonu nebo události.

Za názvem pole a před hodnotu, můžete použít dvojtečkou (:) nebo symbol rovná se (=). **Typ: událost** a **typ = událostí** jsou ekvivalentní v význam, můžete zvolit styl dáváte přednost.

Pokud ano, typ = výkonu záznamy mají pole s názvem "název_čítače, a pak můžete napsat dotaz tvaru `Type=Perf CounterName="% Processor Time"`.

Tím získáte pouze data výkonu kde název čítače výkonu je "% času procesoru".

### <a name="to-search-for-processor-time-performance-data"></a>K vyhledání údaje o výkonu času procesoru
* Do pole vyhledávání dotazu zadejte`Type=Perf CounterName="% Processor Time"`

Můžete také vybrat určité a použít **InstanceName = _ "Celkem"** v dotazu, který je čítačů výkonu systému Windows. Můžete také vybrat omezující vlastnost a jiné **: hodnota pole**. Filtr se automaticky přidá do filtru na panelu dotazů. To vidíte na následujícím obrázku. Zobrazuje kde klikněte na tlačítko Přidat **InstanceName: "_Total"** na dotaz, aniž by museli zadávat nic.

![omezující vlastnost vyhledávání](./media/log-analytics-log-searches/oms-search-facet.png)

Stává dotazu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

V tomto příkladu nemusíte určit **typ = výkonu** získat k těmto výsledkům. Vzhledem k tomu, že pole název_čítače a InstanceName existovat pouze pro záznamy typu = výkonu, dotaz je dost konkrétní, aby vrátí stejné výsledky jako déle, předchozí:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Důvodem je, že všechny filtry v dotazu, jsou vyhodnoceny jako používán *a* mezi sebou. Efektivní, další pole přidat kritéria, můžete získat menší, konkrétní a přesnějších výsledků.

Například dotaz `Type=Event EventLog="Windows PowerShell"` je stejný jako `Type=Event AND EventLog="Windows PowerShell"`. Vrátí všechny události, které byly přihlášení a shromažďují z protokolu událostí Windows PowerShell. Pokud přidáte filtr několikrát opakovaně výběrem stejné omezující vlastnost, pak tento problém je čistě kosmetické – ho může zaplnit panelu Hledat, ale stále vrátí stejné výsledky protože implicitní operátor AND je vždycky k dispozici.

Implicitní operátor AND můžete snadno vrátit explicitně pomocí operátoru NOT. Například:

`Type:Event NOT(EventLog:"Windows PowerShell")`nebo jeho ekvivalent `Type=Event EventLog!="Windows PowerShell"` vrátí všechny události z všechny protokoly, které nejsou v prostředí Windows PowerShell protokolu.

Nebo můžete například použít jiné logický operátor 'Nebo'. Následující dotaz vrátí záznamy, pro které protokolu událostí buď systém nebo aplikace.

```
EventLog=Application OR EventLog=System
```

Pomocí výše uvedeném dotazu, získáte položky pro oba protokoly ve stejné sadě výsledků.

Ale pokud odeberete nebo ponecháním implicitní a na místě, pak následující dotaz nebude vytváří výsledky protože není k dispozici položku protokolu událostí, který patří do oba protokoly. Každá položka protokolu událostí byla zapsána do pouze jeden z dva protokoly.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Použít další filtry
Následující dotaz vrátí položky pro 2 protokoly událostí pro všechny počítače, které jste odeslali data.

```
EventLog=Application OR EventLog=System
```

![Výsledky vyhledávání](./media/log-analytics-log-searches/oms-search-results03.png)

Výběrem jedné z pole nebo filtry budou upřesněte dotaz pro konkrétní počítač, s výjimkou všechny ostatní výsledky. Výsledný dotaz bude vypadat takto.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Což je totéž následujícím z důvodu implicitní operátorem a.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

V následujícím pořadí explicitní vyhodnotí každý dotaz. Poznámka: v závorkách.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Stejně jako pole v protokolu událostí, je možné načíst data pouze pro sadu konkrétní počítače přidáním nebo. Například:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Podobně to následující dotaz vrátí **% času procesoru** pro pouze vybrané dva počítače.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Typy polí
Při vytváření filtrů, byste měli porozumět rozdíly při práci s různými typy pole vrácené protokolu hledání.

**Prohledatelná pole** zobrazit modře ve výsledcích hledání.  Prohledatelná pole v konkrétní podmínek vyhledávání můžete použít na pole, jako jsou následující:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Uvolněte prohledatelná pole text** jsou uvedeny šedě ve výsledcích hledání.  Není možné použít s podmínek vyhledávání, které jsou specifické pro pole jako pole s možností vyhledávání.  Jejich prohledávání pouze při provádění dotazu napříč všechna pole, podobně jako tento.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Logické operátory
Číselná pole a data a času, můžete vyhledat pomocí hodnoty *větší než*, *menší než*, a *menší než nebo rovna*. Jednoduché operátory můžete použít jako >, <>, =, < =,! = v panelu vyhledávání dotazu.

Pro určité časové období se můžete dotazovat specifickém protokolu událostí. Například následující klávesovými výraz vyjádřený za posledních 24 hodin.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Hledání s použitím logický operátor
* Do pole vyhledávání dotazu zadejte`EventLog=System TimeGenerated>NOW-24HOURS`  
    ![hledání s logická hodnota](./media/log-analytics-log-searches/oms-search-boolean.png)

I když můžete řídit časový interval graficky a většina časy, můžete chtít udělat, existují výhody včetně filtr času přímo do dotazu. Například to vyhovující postup pomocí řídicích panelů, kde můžete přepsat čas pro každou dlaždici, bez ohledu na to *globální* selektor čas na stránce řídicího panelu. Další informace najdete v tématu [čas záleží na řídicím panelu](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Při filtrování podle času, mějte na paměti, kterou můžete získat výsledky pro *průnik* dvou časových období: verze zadaná v portálu OMS (S1) a jeden zadaný v dotazu (S2).

![průnik](./media/log-analytics-log-searches/oms-search-intersection.png)

To znamená, pokud časových období nekříží, například na portálu OMS, kde si zvolíte **tato týdnu** a v dotazu, kde můžete definovat **minulého týdne**, nejsou k dispozici žádné průnik a neobdržíte žádné výsledky.

Operátory porovnání použitý pro pole TimeGenerated jsou také užitečná v jiných situacích. Například s číselná pole.

Například uděleno, že konfigurace Assessment výstrahy mít následující hodnoty závažnosti:

* 0 = informace
* 1 = upozornění
* 2 = kritický

Můžete dotazovat na upozornění a kritickou výstrahy a také vyloučit informační ty, které jsou s následující dotaz:

```
Type=ConfigurationAlert  Severity>=1
```


Můžete také použít dotazy na rozsah. To znamená, že může poskytovat začátek a konec rozsahu hodnot v pořadí. Například pokud chcete události z protokolu událostí nástroje Operations Manager, kde ID události je větší než nebo rovna hodnotě 2100 ale není větší než. 2199, pak následující dotaz vrátí je.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> Je nutné použít syntaxi rozsah je oddělovače: hodnota pole dvojtečkou (:) a *není* rovná se (=). Horní a dolní hranice rozsahu uzavřít do hranatých závorek a je oddělit dvě tečky (.).
>
>

## <a name="manipulate-search-results"></a>Manipulace s výsledky hledání
Pokud chcete vyhledat data, budete chtít Upřesnit vyhledávací dotaz a mít funkční úroveň kontroly nad výsledky. Pokud jsou načteny výsledky, můžete použít příkazy k jejich transformaci.

Příkazy v analýzy protokolů hledání *musí* podle za znak znak svislé čáry (|). Filtr musí být vždy první část řetězce dotazu. Definuje sadu dat, kterou právě pracujete s a následně "prostřednictvím kanálu předá" výsledků do příkazu. Kanál potom můžete přidat další příkazy. Toto je volně podobná zřetězením příkazů Windows Powershellu.

Obecně platí jazyka vyhledávání analýzy protokolů se pokusí postupujte podle styl prostředí PowerShell a pokyny, chcete-li podobná IT specialisté a k usnadnění křivku.

Příkazy mají názvy příkazů, takže lze snadno určit, co dělají.  

### <a name="sort"></a>Seřadit
Příkaz řazení umožňuje definovat pořadí řazení podle jednoho nebo více polí. I když nepoužijete, ve výchozím nastavení, se vynucuje čas sestupném pořadí. Nejnovější výsledky jsou vždy v horní části výsledků vyhledávání. To znamená, že při spuštění vyhledávání, s `Type=Event EventID=1234` co je pro vás skutečně proveden je:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Je to způsobeno je typ možnosti, které jste se seznámili s v protokolech. Například v prohlížeči událostí systému Windows.

Řazení můžete změnit způsob, jakým budou vráceny výsledky. Následující příklady ukazují, jak to funguje.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Výše uvedené jednoduché příklady ukazují, jak fungují příkazy – se změní obrazec výsledky, které filtr.

### <a name="limit-and-top"></a>Omezení a nahoře
Jiného méně známé příkazu je omezení. Limit je příkaz prostředí PowerShell jako. Limit je funkčně totožný s příkazem nejvyšší. Následující dotazy vrátí stejné výsledky.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Hledání s použitím horní
* Do pole vyhledávání dotazu zadejte`Type=Event EventID=600 | Top 1`   
    ![horní vyhledávání](./media/log-analytics-log-searches/oms-search-top.png)

Na předchozím obrázku jsou záznamy 358 tisíc s ID události = 600. Pole, omezující vlastnosti a filtry na levé straně vždy zobrazovat informace o výsledky vrácené *podle části filtru* dotazu, která je součástí před všechny znakem. **Výsledky** podokně pouze vrátí poslední 1 výsledek, protože v ukázkovém příkazu ve tvaru a transformovat výsledky.

### <a name="select"></a>Vyberte
Příkaz SELECT se chová jako Select-Object v prostředí PowerShell. Vrátí filtrované výsledky, které nemají všechny své původní vlastnosti. Místo toho vybere pouze vlastnosti, které zadáte.

#### <a name="to-run-a-search-using-the-select-command"></a>Ke spuštění vyhledávání pomocí příkazu select
1. Do pole hledání zadejte `Type=Event` a pak klikněte na **vyhledávání**.
2. Klikněte na tlačítko **+ zobrazit další** v jednom z výsledků a zobrazit všechny vlastnosti, které mají výsledky.
3. Vyberte některým z nich explicitně a dotaz se změní na `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Vyberte vyhledávání](./media/log-analytics-log-searches/oms-search-select.png)

Tento příkaz je zvláště užitečné, pokud chcete řídit výstupní vyhledávání a vyberte pouze ty části data, která skutečně vás pro vaše zkoumání, což často není úplný záznam. To je také užitečné, když mají záznamy různých typů *některé* společných vlastností, ale ne *všechny* jejich vlastnosti jsou společné. Můžete generovat výstup, který může vypadat více přirozeně tabulku nebo fungovat, i když exportován do souboru CSV a pak massaged v aplikaci Excel.

## <a name="use-the-measure-command"></a>Použijte příkaz měr
MÍRA je jedním z nejvíce univerzální příkazy v analýzy protokolů hledání. Umožňuje použít statistické *funkce* dat a agregačních výsledků seskupených podle dané pole. Existuje více statistické funkce, které podporuje měr.

### <a name="measure-count"></a>Míra count()
První statistické funkce pro práci s a jeden z nejjednodušší zjistit, je *count()* funkce.

Výsledkem všechny vyhledávací dotaz, jako `Type=Event`, zobrazeny filtry také nazývané omezující vlastnosti na levé straně výsledků vyhledávání. Filtry zobrazit distribuční hodnot pomocí dané pole pro výsledky vyhledávání provést.

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Například na předchozím obrázku se zobrazí **počítače** pole a uvádí, že v rámci téměř 739 tisíc události ve výsledcích, jsou 68 jedinečné a jiné hodnoty pro **počítače** pole v těchto záznamech. Na dlaždici se zobrazí pouze 5, které jsou nejběžnější 5 hodnot, které jsou zapsány v horní části **počítače** polí), seřazené podle počtu dokumentů, které obsahují tuto konkrétní hodnotu v tomto poli. Na obrázku uvidíte, že – mezi tyto události téměř 369 tisíc – 90 tisíc pocházet z počítače OpsInsights04.contoso.com, 83 tisíc z DB03.contoso.com počítače a tak dále.

Co dělat, když chcete zobrazit všechny hodnoty, protože dlaždice se zobrazí pouze pouze první 5?

To je, co dělat s funkcí count() příkaz měr. Tato funkce nepoužívá žádné parametry. Můžete zadat pouze pole, podle kterého chcete seskupit podle – **počítače** pole v tomto případě:

`Type=Event | Measure count() by Computer`

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Ale **počítače** je právě používá pole *v* každá položka dat – nejsou spojené žádné relační databáze a neexistuje žádný samostatné **počítače** objektu kdekoli. Pouze hodnoty *v* data můžete popisují, které entitě generované je a řadu dalších vlastností a aspekty dat – proto termín *omezující vlastnost*. Však můžete stejně dobře seskupovat podle další pole. Vzhledem k tomu, že původní výsledky téměř 739 tisíc události, které jsou přesměrovat do příkazu měr také mít pole s názvem **EventID**, můžete použít stejné techniky k seskupení podle tohoto pole a získat počet událostí podle ID události:

```
Type=Event | Measure count() by EventID
```

Pokud si nejste zájem o počet skutečné záznamů, které obsahují konkrétní hodnotu, ale místo toho pokud chcete pouze seznam hodnot sami, můžete přidat *vyberte* příkaz na konci ho a právě vyberte první sloupec:

```
Type=Event | Measure count() by EventID | Select EventID
```

Potom můžete získat komplikovanější a předem řazení výsledků v dotazu nebo klepnutí na sloupce v mřížce, příliš.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Hledání s použitím míru count
* Do pole vyhledávání dotazu zadejte`Type=Event | Measure count() by EventID`
* Připojit `| Select EventID` na konec dotazu.
* Nakonec připojit `| Sort EventID asc` na konec dotazu.

Existuje několik důležitých bodů zdůraznil a Všimněte si:

Nejprve výsledky, které vidíte nejsou původní nezpracovaná výsledky už. Místo toho jsou agregované výsledky – v podstatě skupiny výsledků. Tato akce není problém, ale byste měli vědět, že jste interakci se příliš neliší tvaru dat, která se liší od původní nezpracovaná obrazec, který se vytvoří za chodu v důsledku agregace a statistické funkce.

Druhý, **měření počtu** aktuálně vrací hodnotu pouze prvních 100 odlišné výsledky. Toto omezení se nevztahuje na jiné statistických funkcí. Ano obvykle musíte nejprve pomocí přesnější filtru k vyhledání konkrétní položky před použitím count() měr.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Pomocí funkce max a min příkaz měr
Existují různé scénáře, kde **měr Max()** a **měr Min()** jsou užitečné. Ale vzhledem k tomu, že jednotlivé funkce je opačné vzájemně, jsme budete ilustrují Max() a můžete experimentovat s Min() sami.

Pokud jste odeslat dotaz pro události zabezpečení, mají **úroveň** vlastnost, která se může lišit. Například:

```
Type=SecurityEvent
```

![Počáteční počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Pokud chcete zobrazit nejvyšší hodnotu pro všechny zabezpečení události, které jsou uvedeny běžné počítače, Seskupit podle pole, můžete použít

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![hledání měr maximální počítače](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Se zobrazí, který pro počítače, které měl **úroveň** záznamy, většina z nich má aspoň 8, na úrovni mnoho měl úroveň 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maximální doba měr vyhledávání vygenerované počítači](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Tato funkce pracuje s čísla, ale spolupracuje také s pole data a času. Je vhodné zkontrolovat poslední nebo poslední časové razítko pro jakékoliv dat indexované pro každý počítač. Například: Pokud poslední události zabezpečení ohlásil pro každý počítač?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Příkaz měr pomocí funkce AVG.
Statistické funkce Avg() použít s měr můžete vypočítat průměrnou hodnotu pro některé pole a seskupení výsledků podle stejné nebo jiné pole. To je užitečné v různých případech, například údaje o výkonu.

Začneme s údaje o výkonu. Všimněte si, že OMS aktuálně shromažďuje čítače výkonu pro počítače s Windows a Linux.

K vyhledání *všechny* údaje o výkonu, je nejzákladnější dotazu:

```
Type=Perf
```

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg01.png)

První věc, můžete si všimnout je, že analýzy protokolů ukazuje tři perspektivy: seznam, který ukazuje, který zobrazuje skutečné záznamy za grafy; Tabulky, který ukazuje tabulkového zobrazení data čítače výkonu; a metriky, které zobrazuje grafy pro čítače výkonu.

Na předchozím obrázku jsou dvě sady pole označené, které označují následující:

* První sady identifikuje název čítače výkonu systému Windows, název objektu a název Instance filtru dotazu. Jedná se o pole, které pravděpodobně budete nejčastěji používat jako omezující vlastnosti nebo filtry
* **Přepočtené** je skutečná hodnota čítače. V tomto příkladu je hodnota *75*.
* **TimeGenerated** je 12:51, ve 24hodinovém formátu.

Zde je zobrazení metriky v grafu.

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg02.png)

Po výklad o záznamů tvaru výkonu a nutnosti přečtěte si informace o jinými technikami, hledání můžete míry Avg() k agregaci číselná data tohoto typu.

Zde je jednoduchý příklad:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![hledání samplevalue průměr](./media/log-analytics-log-searches/oms-search-avg03.png)

V tomto příkladu vyberte výkonu celkový čas procesoru čítač a průměrné počítačem. Pokud chcete zúžit výsledky, aby pouze posledních 6 hodin, můžete použít ovládací prvek filtru čas nebo v dotazu zadat následujícím způsobem:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>K vyhledávání pomocí příkazu míry pomocí funkce AVG.
* Dotaz do vyhledávacího pole zadejte `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

Můžete agregovat a chybě korelace dat. *napříč* počítače. Představte si například, že máte na skupinu hostitelů v nějaká farmy, kde každý uzel je jakékoli jiné jednu a se právě všechny stejného typu práce a zhruba vyrovnáváním zatížení. Může dojít, jejich čítače, které vše v jednom přejděte s následující dotaz a získat průměry pro celou farmu. Můžete spustit výběrem počítače s následujícím příkladu:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Teď, když máte počítače, také pouze chcete vybrat dvě klíčové ukazatele výkonu (KPI): % využití procesoru a % volného místa na disku. Ano tuto část dotazu se změní na:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Teď můžete přidat počítače a čítače s v následujícím příkladu:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Protože máte velmi konkrétní výběru, **měření Avg()** příkaz může vrátit průměr není počítačem, ale celou farmu, jednoduše tak, že seskupení podle název_čítače. Například:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

To vám dává užitečné compact zobrazení několik klíčových ukazatelů výkonu vaše prostředí.

![seskupení průměr vyhledávání](./media/log-analytics-log-searches/oms-search-avg04.png)

Na řídicím panelu můžete snadno použít vyhledávací dotaz. Můžete například uložit vyhledávací dotaz a vytvořit řídicí panel z něj s názvem *webové farmy klíčových ukazatelů výkonu*. Další informace o použití řídicích panelů najdete v tématu [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).

![řídicí panel vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Příkaz míry pomocí funkce sum
Funkce sum je podobná jiných funkcí příkazu měr. Můžete zobrazit příklad o tom, jak používat funkci sum na [W3C IIS protokoly vyhledávání ve statistice provozu Microsoft Azure](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Můžete použít Max() a Min() s textového řetězce, hodnoty data a času a čísla. Pomocí textového řetězce jsou seřazeny podle abecedy a získat první a poslední.

Sum() však nelze použít s jakoukoli jinou hodnotu než číselné pole. To platí také pro Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Pomocí funkce percentilu pomocí příkazu míry
Funkce percentilu je podobná Avg() a Sum() v, můžete ji použít pouze pro číselné pole. Můžete použít všechny percentilu mezi 1 až 99 na číselné pole. Můžete také použít obě **percentilu** a **pct** příkazy. Zde je několik příkladů:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Použít where příkazu
Tam, kde příkaz lze použít jako filtr, ale můžete použijí v kanálu pro další filtrování agregované výsledky, které vyrobila příkaz měr – oproti nezpracovaná výsledky, jsou filtrovány na začátku dotazu.

Například:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Můžete přidat další kanálu "|" znak a kde příkaz, který má získat pouze počítače, jejichž průměrné využití procesoru je větší než 80 %, se v následujícím příkladu:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Pokud jste obeznámeni s nástrojem Microsoft System Center - Operations Manager si můžete představit where příkazu v podmínkách management pack. Pokud v příkladu bylo pravidlo, první část dotazu by byl zdroj dat a where by příkaz vypadal detekce stavu.

Dotaz můžete použít jako dlaždici v **vlastní řídicí panel**, jako monitorování řazení zobrazíte, když jsou přetíženy počítače procesory. Další informace o řídicí panely, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md). Můžete také vytvořit a používat řídicí panely pomocí mobilní aplikace. Další informace najdete v tématu [OMS mobilní aplikace ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). V dlaždicích dolní dva na následujícím obrázku, uvidíte monitorování zobrazí seznam, jako číslo. V podstatě chcete vždy číslo, které má být nula a seznamu byly prázdné. Jinak hodnota označuje podmínku výstrahy. V případě potřeby můžete se podívat na které počítače jsou přetížena.

![mobilní řídicí panel](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Použití v operátoru
*IN* operátor, spolu s *NOT IN* umožňuje používat subsearches, které jsou hledání, které obsahují další hledání jako argument. Jsou obsaženy v složené závorky {} v rámci jiného *primární* nebo *vnější* vyhledávání. Výsledek subsearch, často seznam odlišné výsledky, se pak použije jako argument v jeho primární vyhledávání.

Můžete subsearches tak, aby odpovídaly podmnožiny dat, nelze popisují přímo v hledaný výraz, ale který se dá vygenerovat z vyhledávání. Například, pokud vás zajímá pomocí jednoho hledání najít všechny události z *počítače s chybějícími aktualizacemi zabezpečení*, pak je třeba navrhnout subsearch, který nejprve identifikuje, že *počítače s chybějícími aktualizacemi zabezpečení* předtím, než nalezne události, které patří do těchto hostitelích.

Ano, může express *počítače s aktuálně chybějícími požadovanými aktualizacemi zabezpečení* s následující dotaz:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Jakmile se v seznamu, můžete jako vnitřní vyhledávání hledání ke kanálu seznam počítačů, do vnější (primární) vyhledávání, který bude hledat události pro tyto počítače. To uděláte tak, že uzavření vnitřní hledání do složených závorek a napájení své výsledky jako možné hodnoty nebo pole filtru do vnějšího vyhledávání pomocí operátor. Dotaz by vypadat podobně jako:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Také oznámení filtr času v informacích o vnitřní hledání použít, protože vyhodnocení aktualizací systému pořídí snímek všech počítačů každých 24 hodin. Vnitřní dotaz můžete nastavit víc jednoduchý a přesné tak, že pouze jeden den. Vnější hledání místo toho použije se čas výběr v uživatelském rozhraní načítání událostí z posledních 7 dnů. V tématu [logické operátory](#boolean-operators) Další informace o času operátory.

Protože jste skutečně používejte pouze výsledky hledání vnitřní jako filtr hodnotu pro jeden vnější, můžete také použít příkazy ve vnější vyhledávání. Například můžete seskupit stále výše zmíněných událostí s jiného příkazu měr:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Obecně platí budete chtít vnitřní dotaz rychle spustit, protože analýzy protokolů má straně služby časové limity pro něj a také vrácení malé množství výsledků. Pokud vnitřní dotaz vrátí více výsledků, získá zkrátí seznam výsledků, které by mohly způsobit potenciálně vnější vyhledávání vrací nesprávné výsledky.

Jiným pravidlem je, že vnitřní hledání aktuálně nutné poskytnout *agregované* výsledky. Jinými slovy, musí obsahovat *měr* příkaz; je nelze kanálu aktuálně nezpracovaná výsledky do vnějšího vyhledávání.

Navíc může být pouze jeden operátor IN a musí být poslední filtru v dotazu. Nemůže být více operátorů v nebo by – to v podstatě brání spuštění více subsearches: důležité je, že pouze jeden dílčí/vnitřní hledání je možné pro každé vnější vyhledávání.

I když tyto limity IN umožňuje různé druhy korelační hledání a umožňuje definovat podobný skupin jako je například počítače, uživatele nebo soubory – ať pole ve vašich datech obsahovat. Zde jsou další příklady:

**Všechny aktualizace chybí z počítačů, kde je zakázaný nastavení automatických aktualizací**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Všechny chybové události z počítače se systémem SQL Server (=, kde se má spustit vyhodnocení SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Všechny události zabezpečení z počítačů, které jsou řadiče domény se (=, kde byla spuštěna hodnocení AD)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Které jiné účty přihlášení do stejných počítačů, kde má přihlášený účet BACONLAND\jochan?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Použít příkaz distinct
Jak již název naznačuje, tento příkaz poskytuje seznam jedinečných hodnot pro pole. Je překvapivě jednoduchý, ale velmi užitečné. Stejné bylo možné dosáhnout pomocí příkazu count() měr stejně, jako vidíte níže.

```
Type=Event | Measure count() by Computer
```

![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Pokud však všechny vás zajímá je právě seznam jedinečných hodnot a není počet dokumentů, které mají, že může poskytovat hodnoty a potom DISTINCT čisticí a snadnější čtení výstup a kratší syntaxe, jako vidíte níže.

```
Type=Event | Distinct Computer
```
![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Použijte funkci countdistinct pomocí příkazu míry
Funkce countdistinct spočítá počet jedinečných hodnot v rámci jednotlivých skupin. Například může být použít počítat počet jedinečných počítačů, vytváření sestav pro každý typ:

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Pomocí příkazu interval měr
S téměř v reálném čase výkonu shromažďování dat, můžete shromažďovat a vizualizovat všechny čítače výkonu v analýzy protokolů. Jednoduše zadat dotaz **typu: výkonu** vrátí tisíce metriky grafy na základě počtu čítače a serverů ve vašem prostředí analýzy protokolů. S průběhem metriky na vyžádání můžete si prohlédnout metriky celkového ve vašem prostředí na vysoké úrovni a podrobné informace na podrobnější data, jako je třeba.

Řekněme, že budete chtít vědět, co je průměrné využití procesoru pro všechny počítače. Prohlížení průměr využití procesoru pro každý počítač nemusí být užitečné, protože může získat vyhlazené výsledky. Aby viděl další podrobnosti, můžete agregovat sady výsledků v menší časové okno bloky a podívejte se do časové řady mezi různými dimenzemi. Například můžete provést po hodinách průměr využití procesoru pro všechny počítače takto:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Průměrný interval měr](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Ve výchozím nastavení tyto výsledky se zobrazí v řádku interaktivního více řad grafu.  Tento graf podporuje řady přepnutím (s změny měřítka osy y), přibližování a ukazatele.  Možnost zobrazení tabulky je stále k dispozici pro prohlížení nezpracovaných dat v případě potřeby.

Můžete taky Seskupit podle jiných polí. V tomto příkladu dívám se na všech čítačů v % pro jeden určitý počítač a chcete vědět, co je percentily každou hodinu 70 každých čítače:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Poznámka je, že tyto dotazy nejsou omezeny na čítače výkonu. Lze následně použít na všechny metriky. V tomto příkladu dívám se na protokoly služby IIS W3C. Chci vědět, co je maximální doba, kterou trvá v intervalu 5 minut pro zpracování každé žádosti:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Použití více agregace v jednom dotazu
Více klauzulí agregační můžete zadat v příkazu měr.  Každé z nich může být alias nezávisle.  Není-li ji alias výsledný název pole bude, že agregační funkce, která se používá (tj. "avg(CounterValue)" pro avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Tady je další příklad:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Další kroky
Další informace o protokolu hledání najdete v tématu:

* Použití [vlastní pole v analýzy protokolů](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.
* Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) zobrazíte všechna pole hledání a omezující vlastnosti, které jsou k dispozici v analýzy protokolů.
