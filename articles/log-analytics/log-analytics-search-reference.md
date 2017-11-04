---
title: "Azure Log Analytics hledání odkaz | Microsoft Docs"
description: "Analýzy protokolů hledání odkaz popisuje jazyk vyhledávání a poskytuje syntaxe dotazu Obecné možnosti můžete můžete použít při vyhledávání pro data a filtrování výrazy a zúžit vyhledávání."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 264ea071dc0b15964af07c68cbf0dee896b07a3e
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="log-analytics-search-reference"></a>Referenční dokumentace vyhledávání analýzy protokolů

>[!NOTE]
> Tento článek popisuje vyhledávání protokolu pomocí starší verze dotazovací jazyk v analýzy protokolů.  Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak se seznamte s [referenční příručka jazyka pro nový jazyk](https://go.microsoft.com/fwlink/?linkid=856079).

V následujícím referenčním oddílu o vyhledávání jazyk popisuje možnosti syntaxe obecné dotazu, můžete použít při vyhledávání pro data a filtrování výrazy a zúžit vyhledávání. Popisuje také příkazy, které můžete provést akci pro data načtená.

Další informace o pole, vrátí se v hledání a omezující vlastnosti, které vám pomohou zjistit informace o podobné kategorie dat, v [pole hledání a omezující vlastnost odkazovat části](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Syntaxe dotazu obecné
Syntaxe pro obecné dotazování vypadá takto:

```
filterExpression | command1 | command2 …
```

Výraz filtru (`filterExpression`) definuje "kde" podmínky pro dotaz. Příkazy použít do výsledků vrácených dotazem. Více příkazů musí být odděleny znakem (|).

### <a name="general-syntax-examples"></a>Příklady syntaxe obecné
Příklady:

```
system
```

Tento dotaz vrací výsledky, které obsahují slovo *systému* v každé pole, které pro fulltextové indexování nebo podmínek vyhledávání.

> [!NOTE]
> Ne všechna pole jsou indexované tímto způsobem, ale nejběžnější textové pole (například názvy a popisy) obvykle jsou.
>
>

```
system error
```

Tento dotaz vrací výsledky, které obsahují slova *systému* a *chyba*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Tento dotaz vrací výsledky, které obsahují slova *systému* a *chyba*. Potom setřídí výsledky podle *ManagementGroupName* pole (ve vzestupném pořadí) a potom *TimeGenerated* pole (v sestupném pořadí). Jak dlouho trvá pouze prvních 10 výsledky.

> [!IMPORTANT]
> Názvy všech polí a hodnoty pro pole řetězce a text se velká a malá písmena.
>
>

## <a name="filter-expressions"></a>Výrazy filtru
Následující podčásti vysvětlují výrazech filtru.

### <a name="string-literals"></a>Textové literály
Řetězcový literál je řetězec, který nelze rozpoznat analyzátorem jako klíčové slovo nebo předdefinované datový typ (například číslo nebo datum).

Příklady:

```
These all are string literals
```

Tento dotaz vyhledává výsledky, které obsahují výskyty všechna pět slova. Pokud chcete provést vyhledávání složitých řetězec, uzavřete řetězcový literál v uvozovkách. Například:

```
"Windows Server"
```

Tento příkaz vrátí jenom výsledky s přesné shody pro *systému Windows Server*.

### <a name="numbers"></a>Čísla
Analyzátor podporuje desítkové celé číslo a číslo s plovoucí desetinnou čárkou syntaxe pro číselné pole.

Příklady:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Data a časy
Má každá část data v systému *TimeGenerated* vlastnosti, která představuje původní datum a čas záznamu. Některé typy dat může mít další datum a čas pole (například *změněno*).

Časovou osu **graf a času** selektor v Azure Log Analytics ukazuje distribuční výsledků v čase (podle aktuální dotaz spuštěn). To je založené na *TimeGenerated* pole. Datum a čas mají konkrétní řetězec formátu, který lze použít v dotazech dotaz omezit na určitý časový rámec. Syntaxe můžete taky odkazovat na relativní časové intervaly (například "mezi před 3 dny a 2 hodinami").

Tady jsou platné formuláře syntaxe kalendářních dat a časů:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Například:

```
TimeGenerated:2013-10-01T12:20
```

Předchozí příkaz vrátí jenom záznamy se *TimeGenerated* hodnota přesně 12:20 na 1. října 2013.

Analyzátor také nyní podporuje klávesovými hodnotě Datum a čas. (Nepravděpodobné, že to předá výsledky, protože data doesn't make prostřednictvím systému to rychlé.)

Tyto příklady jsou stavební bloky, které chcete použít pro relativní a absolutní datum. V následujících třech témata zobrazí se jejich použití v rozšířené filtry s příklady, které používají rozsahy relativní datum.

### <a name="datetime-math"></a>Matematické datum a čas
Pomocí data a času matematické operátory posunutí nebo zaokrouhlit hodnotě Datum a čas, a to pomocí jednoduché výpočty datum a čas.

Syntaxe:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operátor | Popis |
| --- | --- |
| / |Zaokrouhlí na jednotku, zadané datum a čas. Například nyní / den zaokrouhlí aktuální datum a čas na půlnoc aktuálního dne. |
| + nebo - |Posune datum a čas zadaný počet jednotek. Například nyní + 1 hodina posune aktuální datum a čas o jednu hodinu dopředu. 2013-10-01T12:00-10 dní posune hodnoty Date zpět o 10 dní. |

Matematické operátory datum a čas můžete zřetězené společně. Například:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Následující tabulka uvádí podporované jednotky datum a čas.

| Datum a čas jednotky | Popis |
| --- | --- |
| ROK, LET. |Zaokrouhlí do aktuálního roku a posune o zadaný počet roků. |
| MĚSÍC, MĚSÍCŮ |Zaokrouhlí do aktuálního měsíce nebo posune o zadaném počtu měsíců. |
| DEN, DNŮ, DATUM |Zaokrouhlí na aktuální den v měsíci, nebo posune o zadaný počet dnů. |
| HODINA, ČAS |Zaokrouhlí číslo na aktuální hodinu nebo posuny podle zadaného počtu hodin. |
| MINUTA, MINUT |Zaokrouhlí do aktuální minuty nebo posune o zadaný počet minut. |
| SECOND, SECONDS. |Druhý zaokrouhlí na aktuální nebo posune o zadaný počet sekund. |
| MILISEKUND, POČET MILISEKUND, MILLI, MILLIS |Zaokrouhlí na aktuální milisekundu nebo posune o zadaný počet milisekund, po. |

### <a name="field-facets"></a>Omezující vlastnosti pole
Pomocí pole omezující vlastnosti můžete zadejte podmínku vyhledávání konkrétních polí a jejich přesné hodnoty. To se liší od zápis "bez textu" dotazů pro různé podmínky v celém indexu. Jste viděli již tato technika v několika předchozích příkladech. Následují příklady složitější.

**Syntaxe**

```
field:value
```

```
field=value
```

**Popis**

Vyhledá pole pro tuto konkrétní hodnotu. Hodnota může být řetězcový literál, číslo nebo datum a čas.

Například:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntaxe**

*pole > hodnota*

*pole < hodnota*

*pole > = hodnota*

*pole < = hodnota*

*pole! = hodnota*

**Popis**

Poskytuje porovnání.

Například:

```
TimeGenerated>NOW+2HOURS
```

**Syntaxe**

```
field:[from..to]
```

**Popis**

Poskytuje rozsah používání omezujících vlastností.

Například:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>V
**IN** – klíčové slovo umožňuje vybrat ze seznamu hodnot. V závislosti na syntaxi, kterou používáte může se jednat jednoduchý seznam hodnot, které poskytnete, nebo seznam hodnot z agregace.

Syntaxe 1:

```
field IN {value1,value2,value3,...}
```

Tuto syntaxi umožňuje zahrnout všechny hodnoty v jednoduchých seznamů.



Příklady:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Syntaxe 2:

```
(Outer query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Tuto syntaxi vám umožní vytvořit agregace. Seznam hodnot můžete informačního kanálu pak z tohoto agregace do jiné vnější search (primární), která vypadá pro události se tyto hodnoty. To uděláte tak, že uzavření vnitřní hledání do složených závorek a napájení své výsledky jako možných hodnot pro pole ve vnější vyhledávání pomocí operátor.

Vnitřní dotaz příklad: *počítače aktuálně chybějící aktualizace zabezpečení* s následující dotaz agregace:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Poslední dotaz, který vyhledá *všechny události systému Windows pro počítače, které jsou aktuálně chybějící aktualizace zabezpečení* vypadá zhruba takto:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
**Obsahuje** – klíčové slovo vám umožní filtrovat pro záznamy s polem, které obsahuje zadaný řetězec. To je malá a velká písmena, lze použít pouze u polí s řetězcem a nesmí obsahovat žádné řídicí znaky.

Syntaxe:

```
field:contains("string")
```

Příklad:

```
Type:contains("Event")
```

Vrátí záznamy s typem, který obsahuje řetězec "Událost". Mezi příklady patří **událostí**, **SecurityEvent**, a **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Regulární výrazy
Můžete určit podmínku vyhledávání pro pole s regulárním výrazem, pomocí **Regex** – klíčové slovo. Úplný popis syntaxe můžete použít v regulárních výrazech naleznete v tématu [pomocí regulárních výrazů k filtrování protokolu hledání v analýzy protokolů](log-analytics-log-searches-regex.md).

Syntaxe:

```
field:Regex("Regular Expression")
```

Příklad:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Logické operátory
Logické operátory podporu jazyků dotazu (*a*, *nebo*, a *není*) a jejich aliasy stylu jazyka C (*&&*,  *||* , a *!*, v uvedeném pořadí). Závorky můžete použít k seskupení těchto operátorů.

Příklady:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Logický operátor argumenty filtru nejvyšší úrovně, můžete vynechat. V takovém případě se předpokládá, operátor a.

| Výraz filtru | Ekvivalent hodnoty |
| --- | --- |
| Systémová chyba |systém a chyb |
| systému "Windows Server" nebo závažnosti: 1 |systém a ("Windows Server" nebo závažnosti: 1) |

### <a name="wildcarding"></a>Použití zástupných znaků
Dotazovací jazyk podporuje používání ( \* ) znaku, který představuje jeden nebo více znaků pro hodnotu v dotazu.

Příklad:

 Vyhledáte všechny počítače s "SQL" v názvu, jako je například "Redmond-SQL".

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> V tuto chvíli nelze použít zástupné znaky v rámci uvozovky. Například zpráva `"*This text*"` zvažuje (\*) použít jako literál (\*) znaků.


## <a name="commands"></a>Příkazy


Příkazy se vztahuje na výsledky, které jsou v dotazu. Pomocí znaku svislá čára (|) použít příkaz načtené výsledky. Více příkazů musí být odděleny znakem.

> [!NOTE]
> Názvy příkazů může být napsán v velká nebo malá písmena, na rozdíl od názvy polí a data.
>
>

### <a name="sort"></a>Seřadit
Syntaxe:

    sort field1 asc|desc, field2 asc|desc, …

Řazení výsledků podle určitého pole. Přípona asc nebo desc seřadit výsledky ve vzestupném nebo sestupném pořadí je volitelný. Je-li vynechán, *asc* se předpokládá, že pořadí řazení. Pro **TimeGenerated** pole, *desc* pořadí řazení se předpokládá, tak, aby vracel nejnovější výsledky nejprve ve výchozím nastavení.

### <a name="toplimit"></a>Horní/Limit
Syntaxe:

    top number


    limit number
Omezuje odpověď na hlavních výsledky.

Příklad:

    Type:Alert errors detected | top 10

Vrátí top 10 odpovídající výsledky.

### <a name="skip"></a>Přeskočit
Syntaxe:

    skip number

Přeskočí počet výsledků.

Příklad:

    Type:Alert errors detected | top 10 | skip 200

Vrátí top 10 odpovídající výsledky začínající na výsledek 200.

### <a name="select"></a>Vyberte
Syntaxe:

    select field1, field2, ...

Omezí výsledky na pole, které zvolíte.

Příklad:

    Type:Alert errors detected | select Name, Severity

Omezení vrácených výsledků pole ke *název* a *závažnost*.

### <a name="measure"></a>Míra
*Měr* příkaz se používá k aplikování statistických funkcí k nezpracované výsledků. To je velmi užitečné k získání *Seskupit podle* zobrazení nad daty. Pokud použijete příkaz měr, analýzy protokolů hledání zobrazí tabulku s agregované výsledky.

**Syntaxe:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Agreguje výsledky podle *skupinové pole*a vypočítá hodnoty agregované měr pomocí *aggregatedField*.

| Míra statistické funkce | Popis |
| --- | --- |
| *Vlastnost aggregateFunction* |Název agregační funkci (rozlišování malých a velkých písmen). Jsou podporovány následující agregační funkce: počet, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99). |
| *aggregatedField* |Pole, které je agregaci. Toto pole je volitelné pro agregační funkce COUNT, ale musí být existující číselné pole pro SUM, MAX, MIN, AVG, STDDEV, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99). Některé z může být také aggregatedField **rozšíření** podporované funkce. |
| *fieldAlias* |(Volitelné) alias pro počítané agregovaná hodnota. Pokud není zadáno, je název pole **AggregatedValue**. |
| *skupinové pole* |Název pole, která nastavit výsledek se seskupují po. |
| *Interval* |Časový interval, ve formátu:**nnnNAME**. **nnn**je kladné celé číslo. **NÁZEV** je název intervalu. Názvy podporovaný interval jsou velká a malá písmena a zahrnují: MILISEKUNDU [S], [S] SEKUNDU MINUTU [S], [S] HODINU dne [S], [S] měsíce a roku [S]. |

Interval možnost lze použít pouze v polích časových skupinu (například *TimeGenerated* a *TimeCreated*). V současné době to nevynucuje službou, ale pole bez datum a čas, který je předán do back-end způsobí, že chyba v běhu. Pokud se implementuje ověření schématu, rozhraní API služby odmítne dotazy, které používají pole bez datum a čas pro interval agregace. Aktuální *měr* implementace podporuje interval seskupení pro všechny agregační funkci.

Pokud je vynechaná klauzule BY, ale není zadaný interval (jako druhý syntaxe), *TimeGenerated* pole se předpokládá, že ve výchozím nastavení.

Příklady:

**Příklad 1**

    Type:Alert | measure count() as Count by ObjectId

Výstrahy podle skupin *ObjectID*a vypočítá počet výstrah pro každou skupinu. Agregovaná hodnota vrácena jako *počet* pole (alias).

**Příklad 2**

    Type:Alert | measure count() interval 1HOUR

Pomocí skupin výstrahy podle intervalu 1 hodin *TimeGenerated* pole a vrátí počet výstrah v každém intervalu.

**Příklad 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Stejné jako v předchozím příkladu, ale s aliasem agregované pole (*AlertsPerHour*).

**Příklad 4**

    * | míra count() podle 5DAYS TimeCreated intervalu

Výsledky jsou seskupeny podle intervaly 5 dnů pomocí *TimeCreated* pole a vrátí počet výsledků v každém intervalu.

**Příklad 5**

    Type:Alert | measure max(Severity) by WorkflowName

Skupiny výstrahy podle názvu úlohy a vrátí hodnotu maximální závažnost výstrahy pro každý pracovní postup.

**Příklad 6**

    Type:Alert | measure min(Severity) by WorkflowName

Stejné jako v předchozím příkladu, ale *min* agregovat funkce.

**Příklad 7**

    Type:Perf | measure avg(CounterValue) by Computer

Skupiny výkonu počítačem a vypočítá průměrnou (průměr).

**Příklad 8**

    Type:Perf | measure sum(CounterValue) by Computer

Stejný jako předchozí příklad, ale používá *součet*.

**Příklad 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Stejný jako předchozí příklad, ale používá *stddev*.

**Příklad 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Stejný jako předchozí příklad, ale používá *percentile70*.

**Příklad 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Stejný jako předchozí příklad, ale používá *pct70*. Všimněte si, že *PCT ##* je pouze alias *PERCENTILU ##* funkce.

**Příklad 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Skupiny výkonu nejdřív počítač a potom CounterName a vypočítá průměrnou (průměr).

**Příklad 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Získá nejvyšší pět pracovních s maximální počet výstrah.

**Příklad 14**

    * | míra countdistinct(Computer) podle typu

Spočítá počet jedinečných počítačů, vytváření sestav pro každého typu.

**Příklad 15**

    * | míra countdistinct(Computer) intervalu 1 hodina

Spočítá počet jedinečných počítačů, vytváření sestav pro každou hodinu.

**Příklad 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

Skupiny % času procesoru počítačem a vrátí průměrnou hodnotu pro každou hodinu.

**Příklad 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

Skupiny W3CIISLog metodou a vrátí maximální pro každých 5 minut.

**Příklad 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

% Času procesoru počítačem skupiny a vrátí minimální, průměr, 75 percentilu a maximální pro každou hodinu.

**Příklad 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Skupiny % času procesoru nejprve podle počítače a poté podle názvu Instance a vrátí minimální, průměr, 75 percentilu a maximální pro každou hodinu.

**Příklad 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Vypočítá maximální počet zápisů disku za minutu pro každý disk ve vašem počítači.

### <a name="where"></a>kde
Syntaxe:

```
**where** AggregatedValue>20
```

Lze použít pouze po *měr* příkaz pro další filtrování agregované výsledky, které *měr* má vytvořeného agregační funkce.

Příklady:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Odstranění duplicitních dat
Syntaxe:

    Dedup FieldName

Vrátí první dokument pro každou jedinečnou hodnotu pole daného nalezen.

Příklad:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Tento příklad vrátí jedna událost (nejnovější událost) na ID události.

### <a name="join"></a>Spojit
Spojí dva dotazy k jedné sadě výsledků výsledky.  Podporuje několik typů spojení, které jsou popsané v tabulce postupujte podle kroků.

| Typ spojení | Popis |
|:--|:--|
| vnitřní | Vrátí pouze záznamy s odpovídající hodnotou v obou dotazy. |
| vnější | Vrátí všechny záznamy z obou dotazů.  |
| Vlevo  | Vrátí všechny záznamy z levé dotazu a odpovídající záznamy z pravé dotazu. |


- Spojení aktuálně nepodporují dotazy, které zahrnují **IN** – klíčové slovo, **měr** příkaz nebo **rozšíření** příkaz, pokud je cílem pole pravé dotazu.
- Můžete zahrnout aktuálně pouze jednoho pole ke spojení.
- Hledání jednoduchého nesmí obsahovat více než jedno připojení.

**Syntaxe**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Příklady**

Pro ilustraci typů různých spojení, zvažte připojení typu dat shromážděných z vlastního protokolu volána MyBackup_CL s prezenčního signálu pro jednotlivé počítače.  Tyto datové typy mají následující data.

`Type = MyBackup_CL`

| TimeGenerated | Počítač | LastBackupStatus |
|:---|:---|:---|
| 4/20/2017 01:26:32.137 AM | Srv01.contoso.com | Úspěch |
| 4/20/2017 02:13:12.381 AM | SRV02.contoso.com | Úspěch |
| 4/20/2017 02:13:12.381 AM | srv03.contoso.com | Selhání |

`Type = Hearbeat`(Jenom podmnožinu zobrazená pole)

| TimeGenerated | Počítač | ComputerIP |
|:---|:---|:---|
| 4/21/2017 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 4/21/2017 12:02:21.916 PM | SRV02.contoso.com | 10.10.100.2 |
| 4/21/2017 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>vnitřní spojení

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Vrátí následující záznamy, kde pole počítače odpovídá pro oba datové typy.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Úspěch | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |


#### <a name="outer-join"></a>vnější spojení

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Vrátí následující záznamy pro oba datové typy.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch  | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:14:12.381 AM | Úspěch  | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |
| srv03.contoso.com | 4/20/2017 01:33:35.974 AM | Selhání  | 4/21/2017 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 4/21/2017 12:01:47.373 PM | 10.10.100.2 | Prezenční signál |



#### <a name="left-join"></a>Levé spojení

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Vrátí následující záznamy z MyBackup_CL s všechna odpovídající pole z prezenčního signálu.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Úspěch | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |
| srv03.contoso.com | 4/20/2017 02:13:12.381 AM | Selhání | | | |


### <a name="extend"></a>Rozšíření
Umožňuje vytvořit běhu pole v dotazech. Všimněte si, že spuštění pole nelze použít pomocí příkazu měr k provedení agregace.

**Příklad 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Ukazuje váha skóre doporučení pro doporučení vyhodnocení SQL.

**Příklad 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Zobrazí hodnotu čítače v články znalostní báze místo bajtů.

**Příklad 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Hodnota WireData TotalBytes škáluje tak, aby všechny výsledky jsou v rozmezí od 0 do 100.

**Příklad 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Značky hodnoty čítače výkonu menší než 50 procent jako nízká, jiné jako Vysoká.

**Příklad 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Vypočítá maximální počet zápisů disku za minutu pro každý disk ve vašem počítači.

**Podporované funkce**

| Funkce | Popis | Příklady syntaxe |
| --- | --- | --- |
| Abs |Vrátí absolutní hodnotu zadanou hodnotu nebo funkce. |`abs(x)` <br> `abs(-5)` |
| ACOS |Vrací kosinus oblouk hodnotu, nebo funkci. |`acos(x)` |
| a |Vrátí hodnotu true, pokud všechny jeho operandy vyhodnotit na hodnotu true. |`and(not(exists(popularity)),exists(price))` |
| ASIN |Vrací sinus oblouk hodnotu, nebo funkci. |`asin(x)` |
| Atan |Vrací tangens oblouk hodnotu, nebo funkci. |`atan(x)` |
| ATAN2 |Vrací úhel, vyplývající z převodu obdélníkový souřadnice x, y polární souřadnice. |`atan2(x,y)` |
| cbrt – |Kořenové datové krychle. |`cbrt(x)` |
| ceil |Zaokrouhlí na celé číslo. |`ceil(x)`  <br> `ceil(5.6)`Vrátí hodnotu 6 |
| Cos |Vrátí kosinus úhlu. |`cos(x)` |
| COSH |Vrací hyperbolický kosinus úhlu. |`cosh(x)` |
| DEF |Zkratka pro výchozí. Vrátí hodnotu pole "pole". Pokud pole neexistuje, vrátí výchozí hodnotu zadanou a výsledkem je první hodnota, kde: `exists()==true`. |`def(rating,5)`. Tato funkce def() vrátí hodnocení nebo pokud není zadán žádný hodnocení v dokumentu, vrátí 5. <br> `def(myfield, 1.0)`je ekvivalentní `if(exists(myfield),myfield,1.0)`. |
| stupňů |Převede radiánech stupňů. |`deg(x)` |
| div |`div(x,y)`rozdělí x, y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| DIST |Vrací vzdálenost mezi dvěma vektorů, (body) v n dimenzí místa. Přebírá napájení plus dva nebo více instancí ValueSource a vypočítá vzdálenosti mezi dvěma způsoby. Každý ValueSource musí být číslo. Musí být sudé číslo instancí ValueSource předaná a metoda předpokládá, že první polovinu představují první vektoru a druhou polovinu představují druhý vektoru. |`dist(2, x, y, 0, 0)`Vypočítá Euclidean vzdálenost mezi (0,0) a (x, y) pro každý dokument. <br> `dist(1, x, y, 0, 0)`Vypočítá Manhattan (taxicab) vzdálenost mezi (0,0) a (x, y) pro každý dokument. <br> `dist(2,,x,y,z,0,0,0)`Euclidean vzdálenost mezi (0,0,0) a (x, y, z) pro každý dokument.<br>`dist(1,x,y,z,e,f,g)`Manhattan vzdálenost mezi (x, y, z) a (e, f, g), kde každý znak je název pole. |
| existuje |Vrátí hodnotu TRUE, pokud žádné člen pole existuje. |`exists(author)`Vrátí hodnotu TRUE pro každý dokument, který má hodnotu v poli "Autor".<br>`exists(query(price:5.00))`Vrátí hodnotu TRUE, pokud "cena" odpovídá, "5.00". |
| Exp |Vrátí Eulerova na číslo na mocninu x. |`exp(x)` |
| Floor |Zaokrouhlí číslo dolů na celé číslo. |`floor(x)`  <br> `floor(5.6)`Vrátí hodnotu 5 |
| hypo |Vrátí sqrt(sum(pow(x,2),pow(y,2))) bez zprostředkující přetečení nebo podtečení. |`hypo(x,y)`  <br> ` |
| Pokud |Umožňuje podmíněného funkce dotazy. V `if(test,value1,value2)`, test se nebo odkazuje na logickou hodnotu nebo výraz, který vrací logickou hodnotu (TRUE nebo FALSE). `value1`je vrácena hodnota funkce Pokud test vypočítá hodnotu TRUE. `value2`je vrácena hodnota funkce Pokud test vypočítá hodnotu FALSE. Výraz může být žádné funkce, které výstupy logické hodnoty. Může být také funkce vracení číselných hodnot, ve kterých se interpretuje jako false hodnota 0, nebo vrácení řetězce, ve které případu prázdný řetězec interpretována jako false. |`if(termfreq(cat,'electronics'),popularity,42)`Tato funkce zkontroluje každý dokument zobrazit, pokud obsahuje termín "electronics" v poli cat. Pokud ano, je vrácena hodnota pole oblíbenosti. Jinak je vrácena hodnota 42. |
| lineární |Implementuje `m*x+c`, kde m a c jsou konstanty a x je libovolný funkce. Jde o ekvivalent `sum(product(m,x),c)`, ale poněkud efektivnější, jak jsou implementované jako jedinou funkci. |`linear(x,m,c) linear(x,2,4)`Vrátí`2*x+4` |
| ln |Vrátí přirozené protokol zadanou funkci. |`ln(x)` |
| Protokolu |Vrátí protokol základní 10 zadanou funkci. |`log(x)   log(sum(x,100))` |
| mapy |Mapuje hodnoty vstupní funkce x, které spadají do min a max, včetně k zadané cílové. Argumenty min a max musí být konstanty. Argumenty cíle a výchozí může být konstanty nebo funkce. Pokud hodnota x nespadá mezi min a max, poté je vrácena hodnota x nebo výchozí hodnota je vrácena v případě, že zadaný jako 5. argument. |`map(x,min,max,target) map(x,0,0,1)`Změní všechny hodnoty 0 až 1. To může být užitečné při zpracování 0 výchozí hodnoty.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Změní hodnoty mezi 0 a 100 až 1 a všechny ostatní hodnoty na hodnotu -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Změny žádné hodnoty mezi 0 a 100 x + 599 a všechny ostatní hodnoty frekvence podmínek, solr, v textovém poli. |
| maximální počet |Vrací maximální hodnotu číselného více vnořené funkce nebo konstanty, které jsou zadané jako argumenty: `max(x,y,...)`. Maximální funkce může být také užitečná pro "bottoming out" jinou funkci nebo pole v některé zadaný konstanta.  Použití `field(myfield,max)` syntaxe pro výběr maximální hodnota, která jediné pole s více hodnotami. |`max(myfield,myotherfield,0)` |
| min |Vrací minimální hodnotu číselného více vnořené funkce konstanty, které jsou zadané jako argumenty: `min(x,y,...)`. Funkce min, také může být užitečné pro zajištění "horní mez" na funkce pomocí konstanta. Použití `field(myfield,min)` syntaxe pro výběr minimální hodnota jednoho pole s více hodnotami. |`min(myfield,myotherfield,0)` |
| MOD |Vypočítá zbytek z funkce x, y funkce. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Vrátí rozdíl mezi její argumenty v milisekundách. Data jsou relativní vzhledem k se systémy Unix a POSIX epoch čas, půlnoc, 1. ledna 1970 UTC. Argumenty může být název indexované TrieDateField, nebo na základě konstantní data matematické datum nebo teď. `ms()`je ekvivalentní `ms(NOW)`, počet milisekund od epoch. `ms(a)`Vrátí počet milisekund od epoch, který představuje argument. `ms(a,b)`Vrátí počet milisekund, po tomto b dojde před a, což je `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| není |Logicky posunut hodnota zabalená funkce. |`not(exists(author))`Hodnota TRUE, pouze pokud `exists(author)` je false. |
| nebo |Logická disjunkce. |`or(value1,value2)`Hodnota TRUE, pokud buď value1 nebo value2 platí. |
| Pow |Vyvolá určenou mocninu o zadaném základu. `pow(x,y)`Vyvolá x exponentem y. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Stejné jako sqrt. |
| Produktu |Vrátí součin více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami. `mul(...)`může také použít jako alias pro tuto funkci. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Provede vzájemných funkce s `recip(x,m,a,b)` implementace `a/(m*x+b)`, kde m, a, b jsou konstanty a x je žádné libovolně komplexní funkce. Když a b stejné a je x > = 0, tato funkce má maximální hodnota je 1, který zahodí jako x zvyšuje. Zvýšení hodnoty z a b společně výsledky v Přesun celého funkce plošší část křivky. Tyto vlastnosti můžete vytvořit ideální funkce pro zvyšovat skóre novější dokumenty, když je x `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad |Převede radiánech stupňů. |`rad(x)` |
| Tisknout |Zaokrouhlí číslo na nejbližší celé číslo. |`rint(x)`  <br> `rint(5.6)`Vrátí hodnotu 6 |
| Sin |Vrátí sinus úhlu. |`sin(x)` |
| SINH |Vrací hyperbolický sinus úhlu. |`sinh(x)` |
| Škálování |Udává hodnoty funkce x tak, aby se patří mezi zadaný minTarget a maxTarget (včetně). Aktuální implementace prochází všechny hodnoty funkce získat min a max, takže ho můžete vybrat správné škálování. Aktuální implementace nerozlišuje po odstranění dokumentů, nebo dokumenty, které nemají žádnou hodnotu. Použije 0.0 hodnoty pro tyto případy: To znamená, že pokud jsou obvykle všechny větší hodnotu než 0,0 hodnoty, jeden můžete nadále binárními 0,0 jako minimální hodnota k namapování z. V těchto případech odpovídající `map()` funkce může jako alternativní řešení Chcete-li změnit 0,0 na hodnotu v rozsahu skutečné, jak je vidět tady:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`Hodnoty x, škáluje tak, aby všechny hodnoty jsou mezi 1 a 2 (včetně). |
| Sqrt |Vrátí druhou odmocninu čísla se zadanou hodnotou nebo funkce. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Vypočítá vzdálenost mezi dva řetězce. Používá rozhraní Lucene kontrolu pravopisu StringDistance a podporuje všechny implementace, které jsou k dispozici v tomto balíčku. Také umožňuje aplikacím zařadit vlastní, prostřednictvím prostředků na Solr možnosti načítání. provede strdist `(string1, string2, distance measure)`. Možné hodnoty pro míru vzdálenost jsou:<ul><li>jw: Jaro Winkler</li><li>Upravit: Levenstein nebo upravit vzdálenost</li><li>ngram: The NGramDistance-li zadána, můžete volitelně předávat velikost ngram příliš. Výchozí hodnota je 2.</li><li>FQN: Plně kvalifikovaný název pro implementaci rozhraní StringDistance třídy. Musí mít konstruktor bez arg.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub – |Vrátí x-y z `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Součet |Vrátí součet více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami. `add(...)`může se použít jako alias pro tuto funkci. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Vrátí počet termín se zobrazí v poli pro tento dokument. |termfreq(text,'memory') |
| Tan |Vrátí tangens úhlu. |`tan(x)` |
| TANH |Vrací hyperbolický tangens úhlu. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Odkaz na pole a omezující vlastnost vyhledávání
Při použití hledání protokolů hledání dat, zobrazí výsledky různé pole a omezující vlastnosti. Některé z informací se nemusí zobrazit velmi popisný. Použijte následující informace vám pomohou pochopit výsledky.

| Pole | Typ vyhledávání | Popis |
| --- | --- | --- |
| TenantId |Všechny |Používá se k oddílu dat. |
| TimeGenerated |Všechny |Používá k řízení časové ose timeselectors (ve vyhledávání a na dalších obrazovkách). Reprezentuje při generování část dat (obvykle na agenta). Čas je vyjádřen ve formátu ISO a je vždycky UTC. U typů, které jsou založeny na existující instrumentace (to znamená, události v protokolu) je to obvykle reálném čase, který položka/řádku/záznam protokolu byla zaznamenána. Pro některé typy, které vytváří prostřednictvím sad management Pack nebo v cloudu (například doporučení nebo výstrahy) představuje dobu něco jiného. Toto je čas tato nová data se snímkem konfigurací nějaká nebyla shromážděna, nebo bylo vytvořeno ho podle doporučení nebo výstrahy. |
| ID události |Událost |ID události v protokolu událostí systému Windows. |
| Protokol událostí |Událost |Protokol událostí, kde byla události zaznamenané v systému Windows. |
| EventLevelName |Událost |Kritický nebo upozornění nebo informace nebo úspěch |
| eventLevel |Událost |Číselnou hodnotu pro kritický nebo upozornění nebo informace nebo úspěch (použijte EventLevelName místo pro snazší/srozumitelnější dotazy). |
| SourceSystem |Všechny |Kde data pocházejí z (z hlediska připojení režimu do služby). Mezi příklady patří Microsoft System Center Operations Manager a Azure Storage. |
| Název objektu |PerfHourly |Název objektu výkonu systému Windows. |
| InstanceName |PerfHourly |Název instance čítače výkonu systému Windows. |
| CounteName |PerfHourly |Název čítače výkonu systému Windows. |
| ObjectDisplayName |PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Zobrazovaný název objektu cílem pravidlo kolekce výkonu v nástroji Operations Manager. Mohou být také zobrazovaný název objektu zjištěné Operational Insights, nebo pro který byla výstraha vygenerována. |
| RootObjectName |PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Zobrazovaný název nadřazeného člena nadřazeného (v dvojité hostitelský vztah) objekt cílem pravidlo kolekce výkonu v nástroji Operations Manager. Mohou být také zobrazovaný název objektu zjištěné Operational Insights, nebo pro který byla výstraha vygenerována. |
| Počítač |Většina typů |Název počítače, které patří data. |
| Název zařízení |ProtectionStatus |Název počítače dat patří do (stejný jako "Počítač"). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Pořadí stav Threat je číselné vyjádření stavu hrozeb. Podobně jako u kódy odpovědí protokolu HTTP, má pořadí mezery mezi zadanými čísly (proto žádné hrozeb je 150 a není 100 nebo 0), a místo pro přidání nové stavy. Pro souhrn stavu hrozeb a stav ochrany je záměrem Zobrazit nejhorší stav, který byl počítač v průběhu časového období. Čísla rank různé stavy, takže můžete vyhledat záznam s nejvyšší číslo. |
| ThreatStatus |ProtectionStatus |Popis ThreatStatus, mapování 1:1 s ThreatStatusRank. |
| TypeofProtection |ProtectionStatus |Antimalwarových produktu, který je zjištěn v počítači: none, nástroje pro odebrání Microsoft Malware, Forefront a tak dále. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus RequiredUpdate |ID služby stavu pro tento počítač agenta. |
| HealthServiceId |Většina typů |ID služby stavu pro tento počítač agenta. |
| ManagementGroupName |Většina typů |Název skupiny pro správu pro agenty nástroje Operations Manager připojit. Jinak má hodnotu null nebo prázdný. |
| objectType |ConfigurationObject |Zadejte pro tento objekt zjištěný assessment konfigurace analýzy protokolů (jako sada management pack Operations Manager typu nebo třídy). |
| UpdateTitle |RequiredUpdate |Název aktualizace, která byla nalezena není nainstalován. |
| PublishDate |RequiredUpdate |Pokud byla aktualizace publikována ve službě Microsoft Update. |
| Server |RequiredUpdate |Název počítače dat patří do (stejný jako "Počítač"). |
| Produkt |RequiredUpdate |Produkt, který se aktualizace vztahuje. |
| UpdateClassification |RequiredUpdate |Typ aktualizace (například kumulativní aktualizace nebo service pack). |
| KBID |RequiredUpdate |ID článku KB, který popisuje tento osvědčený postup nebo aktualizace. |
| WorkflowName |ConfigurationAlert |Název pravidla nebo monitorování, které vytváří výstrahy. |
| Závažnost |ConfigurationAlert |Závažnost výstrahy. |
| Priorita |ConfigurationAlert |Priorita výstrahy. |
| IsMonitorAlert |ConfigurationAlert |Je tato výstraha vygeneruje, monitorování (true) nebo pravidlem (false)? |
| AlertParameters |ConfigurationAlert |Soubor XML s parametry výstrahy analýzy protokolů. |
| Kontext |ConfigurationAlert |Soubor XML s kontextem výstrahy analýzy protokolů. |
| Úloha |ConfigurationAlert |Technologie nebo úlohy, které výstrahu odkazuje. |
| AdvisorWorkload |Doporučení |Technologie nebo úlohy, které odkazuje doporučení. |
| Popis |ConfigurationAlert |Popis výstrahy (krátký). |
| DaysSinceLastUpdate |UpdateAgent |Počet dní před (relativní vůči TimeGenerated tento záznam) tohoto agenta nainstalovat v žádné aktualizace ze služby Windows Server Update Service (WSUS) nebo Microsoft Update? |
| DaysSinceLastUpdateBucket |UpdateAgent |Podle DaysSinceLastUpdate kategorizaci v čas kbelíků z dobu počítač poslední nainstalované všechny aktualizace z webu služby WSUS nebo Microsoft Update. |
| AutomaticUpdateEnabled |UpdateAgent |Kontrola automatických aktualizací povolená nebo zakázaná na tento agent? |
| AutomaticUpdateValue |UpdateAgent |Automatické aktualizace kontroluje nastaven na automaticky stahovat a instalovat, stahují jenom nebo jenom zkontrolujte? |
| WindowsUpdateAgentVersion |UpdateAgent |Číslo verze agenta nástroje Microsoft Update. |
| WSUSServer |UpdateAgent |Který WSUS server tohoto agenta aktualizace cílí? |
| OSVersion |UpdateAgent |Verze operačního systému tohoto agenta aktualizace běží na. |
| Name (Název) |Doporučení, ConfigurationObjectProperty |Název nebo název doporučení, nebo název vlastnosti z hodnocení konfigurace analýzy protokolů. |
| Hodnota |ConfigurationObjectProperty |Hodnota vlastnosti z hodnocení konfigurace analýzy protokolů. |
| KBLink |Doporučení |Adresa URL v článku KB, který popisuje tento osvědčený postup nebo aktualizace. |
| RecommendationStatus |Doporučení |Doporučení jsou mezi několik typů, jejichž záznamy aktualizovat, ne jenom přidat do indexu vyhledávání. Tento stav se změní, zda doporučení je aktivní/otevřít, nebo pokud analýzy protokolů zjistí, že ho byl vyřešen. |
| RenderedDescription |Událost |Vykreslené popis (znovu používaný text se vyplněná parametry) událost systému Windows. |
| ParameterXml |Událost |Soubor XML s parametry v datové části události systému Windows (jak je vidět v prohlížeči událostí). |
| EventData |Událost |Soubor XML s celou datová část události systému Windows (jak je vidět v prohlížeči událostí). |
| Zdroj |Událost |Zdroj protokolu událostí, které vygenerovalo událost. |
| EventCategory |Událost |Kategorie události přímo z protokolu událostí systému Windows. |
| Uživatelské jméno |Událost |Uživatelské jméno událostí systému Windows (obvykle AUTHORITY\LOCALSYSTEM NT). |
| SampleValue |PerfHourly |Průměrnou hodnotu pro hodinové agregace čítače výkonu. |
| Min. |PerfHourly |Minimální hodnota v hodinový interval agregace hodinové čítače výkonu. |
| Max. |PerfHourly |Maximální hodnota v hodinový interval agregace hodinové čítače výkonu. |
| Percentile95 |PerfHourly |Hodnota 95. percentil hodinový interval agregace hodinové čítače výkonu. |
| SampleCount |PerfHourly |Kolik nezpracovaná vzorků jste použili k vytvoření tento záznam hodinové agregace. |
| Hrozby |ProtectionStatus |Název malwaru byl nalezen. |
| StorageAccount |W3CIISLog |Účet služby Azure Storage protokol byl načten z. |
| AzureDeploymentID |W3CIISLog |ID Azure nasazení cloudové služby v protokolu patří. |
| Role |W3CIISLog |Role služby Azure cloud protokol patří. |
| RoleInstance |W3CIISLog |RoleInstance Azure role, které patří do protokolu. |
| sSiteName |W3CIISLog |Web služby IIS, které patří protokol (metabáze zápis); pole s-sitename v původní protokolu. |
| sComputerName |W3CIISLog |Pole s-computername v původní protokolu. |
| sIP |W3CIISLog |Požadavek HTTP adresa IP serveru byla určena. Pole s-ip v původní protokolu. |
| csMethod |W3CIISLog |Metoda HTTP (např. GET nebo POST) používaná klientem v požadavku HTTP. Cs metoda v původní protokolu. |
| cIP |W3CIISLog |Požadavek HTTP adresa IP klienta pochází. Pole c-ip v původní protokolu. |
| csUserAgent |W3CIISLog |HTTP User-Agent deklarovaná klienta (prohlížeče nebo jinak). Cs uživatelského agenta v původní protokolu. |
| scStatus |W3CIISLog |Kód stavu HTTP (například 200/403/500) vrácená serverem do klienta. Cs stav v původní protokolu. |
| timeTaken |W3CIISLog |Jak dlouho (v milisekundách), kterou trvalo dokončení požadavku. Pole timetaken v původní protokolu. |
| csUriStem |W3CIISLog |Relativní identifikátor URI (bez adresa hostitele, který je, a vyhledávání), byl požadován. Cs-modul uristem pole v původní protokolu. |
| csUriQuery |W3CIISLog |Dotaz v identifikátoru URI. Identifikátor URI dotazy jsou potřebný jenom u dynamických stránek, například stránky ASP, takže toto pole obvykle obsahuje pomlčka pro statické stránky. |
| sPort |W3CIISLog |Port serveru, který požadavek HTTP byl odeslán do (a zda služba IIS naslouchá, protože je převzata). |
| csUserName |W3CIISLog |Ověřit uživatelské jméno, je-li žádost ověřený a není anonymní. |
| csVersion |W3CIISLog |Verze protokolu HTTP použitý v požadavku (například HTTP/1.1). |
| csCookie |W3CIISLog |Informace o souboru cookie. |
| csReferer |W3CIISLog |Web, který uživatel navštívil jako poslední. Tento web poskytl odkaz na aktuální web. |
| csHost |W3CIISLog |Hlavička hostitele (například www.mysite.com) požadovanou. |
| scSubStatus |W3CIISLog |Druhotný stavový kód chyby. |
| scWin32Status |W3CIISLog |Stavový kód Windows. |
| csBytes |W3CIISLog |Počet bajtů odeslaných v požadavku z klienta na server. |
| scBytes |W3CIISLog |Počet bajtů vrácený zpět v odpovědi ze serveru do klienta. |
| ConfigChangeType |Změnakonfigurace |Typ změny (například WindowsServices nebo softwaru). |
| ChangeCategory |Změnakonfigurace |Kategorie změn (Added/upravené nebo odebrané). |
| SoftwareType |Změnakonfigurace |Typ softwaru (aplikace nebo aktualizace). |
| SoftwareName |Změnakonfigurace |Název softwaru (platí jenom pro změny softwaru). |
| Vydavatel |Změnakonfigurace |Dodavatele, který publikuje softwaru (platí jenom pro změny softwaru). |
| SvcChangeType |Změnakonfigurace |Typ změny, které bylo použito na služby systému Windows (stavu/StartupType/cesta/Účet_služby). Tento krok platí jenom pro změny služby Windows. |
| SvcDisplayName |Změnakonfigurace |Zobrazovaný název služby, která byla změněna. |
| SvcName |Změnakonfigurace |Název služby, která byla změněna. |
| SvcState |Změnakonfigurace |Nový (aktuální) stav služby. |
| SvcPreviousState |Změnakonfigurace |Předchozí známé stavu služby (platí jenom Pokud se změnila stav služby). |
| SvcStartupType |Změnakonfigurace |Typ spuštění služby. |
| SvcPreviousStartupType |Změnakonfigurace |Předchozí typ spuštění v služby (platí pouze je-li změnit typ spuštění služby). |
| SvcAccount |Změnakonfigurace |Účet služby. |
| SvcPreviousAccount |Změnakonfigurace |Účet služby předchozí (platí jenom Pokud změněn účet služby). |
| SvcPath |Změnakonfigurace |Cesta ke spustitelnému souboru služby systému Windows. |
| SvcPreviousPath |Změnakonfigurace |Předchozí cesta ke spustitelnému souboru pro službu systému Windows (platí jenom pokud ho změnit). |
| SvcDescription |Změnakonfigurace |Popis služby. |
| Předchozí |Změnakonfigurace |Předchozí stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo předchozí verze). |
| Aktuální |Změnakonfigurace |Nejnovější stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo aktuální verze). |

## <a name="next-steps"></a>Další kroky
Další informace o protokolu hledání:

* Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).
* Použití [vlastních polí ve analýzy protokolů](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.
