---
title: "Postup sestavení komplexních plánů a pokročilé opakování ve službě Azure Scheduler"
description: "Postup sestavení komplexních plánů a pokročilé opakování ve službě Azure Scheduler"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: e1e45d394a4c442a4fb255ed6d838a589e98860e
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="how-to-build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Postup sestavení komplexních plánů a pokročilé opakování ve službě Azure Scheduler
## <a name="overview"></a>Přehled
Jádrem Azure Scheduler je úloha *plán*. Plán Určuje, kdy a jak Plánovač provede úlohu.

Azure Scheduler umožňuje zadat jiný jednorázové nebo opakované plány pro úlohy. *Jednorázové* plány fire jednou v zadanou dobu – efektivně, jsou *opakovaného* plány, které jsou spouštěny pouze jednou. Opakující se plány fire na předem určený frekvenci.

Azure Scheduler umožňuje s tuto flexibilitu potřebují podporuje celou řadu obchodních scénářů:

* Vyčištění periodických dat. – například každý den, odstranit všechny tweetů starší než 3 měsíce
* Archivace – například každý měsíc, nabízenou faktury historie ke službě zálohování
* Požadavky na externí data – například každých 15 minut, načítat nové sestavy počasí identifikátor klíče subjektu z NOAA
* Zpracování – třeba každý den v týdnu, hodiny mimo špičku bitovou kopii, použijte cloud computing kompresi Image nahrát daný den

V tomto článku jsme provede Příklad úloh, které můžete vytvořit s plánovačem Azure. Poskytujeme data JSON, který popisuje každý plán. Pokud použijete [REST API Scheduleru](https://msdn.microsoft.com/library/mt629143.aspx), můžete použít tento stejný formát JSON pro [vytváření úlohu služby Azure Scheduler](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Podporované scénáře
Mnoho příklady v tomto tématu ilustrují šířky scénáře, které podporuje Azure Scheduler. Široce tyto příklady ukazují, jak vytvořit plány pro mnoho způsoby chování, včetně těch níže:

* Spustit jednou v konkrétní datum a čas
* Spuštění a opakovat explicitní několikrát
* Spustit okamžitě a opakovat
* Spuštění a opakovat každých  *n*  minuty, hodiny, dny, týdny nebo měsíce, spouštění v určitém čase
* Spuštění a opakovat týdenní nebo měsíční četností, ale jenom na konkrétní dny, konkrétní dny v týdnu nebo konkrétní dny v měsíci
* Spuštění a opakovat na více než jednou. v době – například poslední pátek a pondělí v každém měsíci, nebo na 5:15:00 a 17:15:00 každý den

## <a name="dates-and-datetimes"></a>Data a času
Postupujte podle dat v Azure plánovače úloh [specifikace formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) a obsahovat pouze datum.

Postupujte podle odkazů na datum a čas ve službě Azure Scheduler úlohy [specifikace formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) a obsahovat části data a času. Datum a čas, která neurčuje časový posun se považuje za čas UTC.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Postupy: Použití formátu JSON a rozhraní REST API pro vytváření plánů
Vytvoření jednoduchého plánu pomocí [REST API služby Azure Scheduler](https://msdn.microsoft.com/library/mt629143), první [zaregistrovat předplatné zprostředkovatele prostředků](https://msdn.microsoft.com/library/azure/dn790548.aspx) (název zprostředkovatele pro Scheduler je *Microsoft.Scheduler* ), pak [vytvořit kolekci úloh](https://msdn.microsoft.com/library/mt629159.aspx)a v neposlední řadě [vytvořit úlohu](https://msdn.microsoft.com/library/mt629145.aspx). Když vytvoříte úlohu, můžete určit plánování a opakování pomocí JSON stejný, jako je převzat webových stránek níže:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // how often to fire
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Přehled: Úloha schématu základy
Následující tabulka obsahuje přehled důležité elementy související s opakování a plánování v rámci úlohy:

| **Název JSON** | **Popis** |
|:--- |:--- |
| ***čas spuštění*** |*startTime* je datum a čas. Pro jednoduché plány *startTime* je první výskyt a pro komplexní plány úloha spustí dřív než *startTime*. |
| ***opakování*** |*Opakování* objektu určuje pravidla opakování úlohy a bude vykonán opakování úlohy. Objekt opakování podporuje elementy *frekvenci, interval, endTime, count,* a *plán*. Pokud *opakování* je definován *frekvence* je vyžadován; další prvky *opakování* jsou volitelné. |
| ***frekvence*** |*Frekvence* řetězec představující frekvence jednotka, kdy úloha opakovat. Podporované hodnoty jsou *"minutu", "hodina", "dne", "týden"* nebo *"měsíc".* |
| ***interval*** |*Interval* je kladné celé číslo a označuje je interval *frekvence* který určuje, jak často má být úloha spuštěna. Například pokud *interval* 3 a *frekvence* je "týden" úloha opakuje každé 3 týdny. Azure Scheduler podporuje maximálně *interval* 18 měsíců měsíční frekvence 78 týdny frekvence týdenní nebo 548 dnů pro denní četnost. Pro hodinu a minutu četnost podporovaný rozsah je 1 < = *interval* < = 1 000. |
| ***čas ukončení*** |*EndTime* řetězec Určuje minulosti datum a čas, který by neměl spustit úlohu. Není platné. Chcete-li mít *endTime* v minulosti. Pokud žádné *endTime* nebo je zadaný počet, úloha běží nekonečnou. Obě *endTime* a *počet* nelze vložit pro stejnou úlohu. |
| ***počet*** |<p>*Počet* je kladné celé (větší než nula) určuje, kolikrát má tato úloha spuštěna před dokončením.</p><p>*Počet* představuje počet, kolikrát má být úloha spuštěna před určuje byla dokončena. Například pro úlohu, která se spouští každý den *počet* 5 a počáteční datum pondělí, dokončení úlohy po spuštění v pátek. Pokud počáteční datum v minulosti, první spuštění se počítá z čas vytvoření.</p><p>Pokud žádné *endTime* nebo *počet* není zadaný, úloha běží nekonečnou. Obě *endTime* a *počet* nelze vložit pro stejnou úlohu.</p> |
| ***plán*** |Úlohy v zadaném intervalu mění jeho opakování podle plánu opakování. A *plán* obsahuje změny na základě minuty, hodiny, dny v týdnu, dny v měsíci a číslo týdne. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Přehled: Úloha schématu výchozí hodnoty, omezení a příklady
Po tomto přehledu probereme každý z těchto prvků podrobně.

| **Název JSON** | **Typ hodnoty** | **Vyžaduje?** | **Výchozí hodnota** | **Platné hodnoty** | **Příklad** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***čas spuštění*** |Řetězec |Ne |Žádné |Data a časy podle normy ISO 8601 |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***opakování*** |Objekt |Ne |Žádné |Objekt opakování |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***frekvence*** |Řetězec |Ano |Žádné |"minutu", "hodina", "dne", "týden", "měsíc" |<code>"frequency" : "hour"</code> |
| ***interval*** |Číslo |Ano |Žádné |1 až 1000 |<code>"interval":10</code> |
| ***čas ukončení*** |Řetězec |Ne |Žádný |Hodnota data a času představující čas v budoucnosti |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***počet*** |Číslo |Ne |Žádné |>= 1 |<code>"count": 5</code> |
| ***plán*** |Objekt |Ne |Žádné |Objekt plánu |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Podrobné informace: *startTime*
V následující tabulce zachycení jak *startTime* Určuje, jak spustit úlohu.

| **hodnoty startTime** | **Bez opakování** | **Opakování. Žádný plán** | **Opakování s plánem** |
|:--- |:--- |:--- |:--- |
| **Žádný čas spuštění** |Spustit jednou okamžitě |Spusťte jednou okamžitě. Spouštění dalších spuštěních podle výpočet z čas posledního spuštění |<p>Spustit jednou okamžitě</p><p>Zahájí další spuštění na základě plánu opakování.</p> |
| **Počáteční čas v minulosti.** |Spustit jednou okamžitě |<p>Výpočet budoucí prvním spuštění po spuštění a v daném čase spustit</p><p>Spouštění dalších spuštěních na základě oncalculating z čas posledního spuštění</p><p>Najdete v příkladu za touto tabulkou další vysvětlení</p> |<p>Úloha spustí *ne sooner než* zadaným časem spuštění. První výskyt je na základě plánu vypočítaných z čas spuštění</p><p>Zahájí další spuštění na základě plánu opakování.</p> |
| **Počáteční čas v budoucnosti nebo v současné době** |Spustit jednou v zadaný čas spuštění |<p>Spustit jednou v zadaný čas spuštění</p><p>Spouštění dalších spuštěních podle výpočet z čas posledního spuštění</p> |<p>Úloha spustí *ne sooner než* zadaným časem spuštění. První výskyt je na základě plánu vypočítaných z čas spuštění</p><p>Zahájí další spuštění na základě plánu opakování.</p> |

Umožňuje zobrazit příklad co se stane, kde *startTime* je v minulosti s *opakování* , ale žádné *plán*.  Předpokládejme, že je aktuální čas 2015-04-08 13:00, *startTime* je 2015-04-07 14:00, a *opakování* je 2 dní (definované s *frekvence*: den a *interval*: 2.) Všimněte si, že *startTime* je v minulosti a dojde k před aktuálním časem.

Za těchto podmínek *první spuštění* bude 2015-04-09 v 14:00\. Modul plánovače vypočítá výskyty spuštění na základě času začátku.  Všechny instance v minulosti se zahodí. Modul použije další instanci, která nastane v budoucnosti.  Proto v tomto případě *startTime* je 2015-04-07 na 2:00 pm, další instance je dvou dnů od té doby je 2015-04-09 na 2:00 pm.

Všimněte si, že první provádění by být stejné i když startTime 2015-04-05 14:00 nebo 14:00\ 2015-04-01. Po prvním spuštění dalších spuštěních jsou vypočítávány pomocí naplánované – tak nebudou v 2015-04-11 na 2:00 pm, pak 2015-04-13 na 2:00 pm, pak 2015-04-15 na 2:00 pm, atd.

Nakonec pokud úlohu plánu, pokud nejsou v plánu nastavit hodin a minut, jejich výchozí hodin a minut první spuštění, v uvedeném pořadí.

## <a name="deep-dive-schedule"></a>Podrobné informace: *plán*
Na jedné straně *plán* můžete *limit* počet spuštění úlohy.  Například pokud úlohy s četností "měsíc" má *plán* , spustí dne pouze 31, úloha běží pouze měsících, které mají 31<sup>st</sup> den.

Na druhé straně *plán* může taky *rozbalte* počet spuštění úlohy. Například pokud úlohy s četností "měsíc" má *plán* , spustí na dny v měsíci 1 a 2, úloha běží v 1<sup>st</sup> a 2<sup>nd</sup> dny v měsíci místo jenom jednou za měsíc.

Pokud jsou zadány více elementů plán, pořadí vyhodnocení je od největších po nejmenší – číslo týden, měsíc, den, den v týdnu, hodin a minut.

Následující tabulka popisuje *plán* elementy podrobně.

| **Název JSON** | **Popis** | **Platné hodnoty** |
|:--- |:--- |:--- |
| **minut** |Počet minut za hodinu, ve kterém má být úloha spuštěna |<ul><li>Pole celých čísel</li></ul> |
| **hodiny** |Hodin dne, kdy má být úloha spuštěna |<ul><li>Pole celých čísel</li></ul> |
| **dny v týdnu** |Dny v týdnu, úloha se bude spouštět. Tuto položku je možné zadat jenom při týdenní frekvenci. |<ul><li>Pole všech nižší než hodnoty (pole maximální velikost 7)<ul><li>"Pondělí"</li><li>"Úterý"</li><li>"Středa"</li><li>"Čtvrtek"</li><li>"Pátek"</li><li>"Sobota"</li><li>"Neděle"</li></ul></li></ul>*Není* malá a velká písmena |
| **monthlyOccurrences** |Určuje, které dny v měsíci má být úloha spuštěna. Tuto položku je možné zadat jenom při měsíční frekvenci. |<ul><li>Pole objektů monthlyOccurrence:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *den* je den v týdnu, úloha se spustí, například {neděle} je každou neděli v měsíci. Povinná hodnota.</p><p>Výskyt je *výskyt* dne v měsíci, například {neděle, -1} je poslední neděle v měsíci. Volitelné.</p> |
| **Prescribed** |Den v měsíci, který má být úloha spuštěna. Tuto položku je možné zadat jenom při měsíční frekvenci. |<ul><li>Pole níže hodnoty</li><ul><li>Libovolná hodnota < = -1 a > =-31.</li><li>Libovolná hodnota > = 1 a < = 31.</li></ul></ul> |

## <a name="examples-recurrence-schedules"></a>Příklady: Opakování plány
Následují příklady různých plány opakování – zaměřením na objekt plánu a jeho dílčí prvky.

Plány níže všechny předpokládat, že *interval* je nastaven na hodnotu 1\. Navíc jeden musí předpokládat správné frekvence v co je v souladu *plán* – například nelze použít frekvence "dne" a "Prescribed" Změna máte v plánu. Takové omezení jsou popsané výše.

| **Příklad** | **Popis** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |Spustit na 5: 00 každý den. Azure Scheduler odpovídá až každé hodnoty v "hours" s každou hodnotu ve "minutes", po jednom, můžete vytvořit seznam všechny časy, kdy úloha je potřeba spustit. |
| <code>{"minutes":[15], "hours":[5]}</code> |Spustit na 5:15:00 každý den |
| <code>{"minutes":[15], "hours":[5,17]}</code> |Spustit na 5:15:00 a 17:15:00 každý den |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 každý den |
| <code>{"minutes":[0,15,30,45]}</code> |Spustit každých 15 minut |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Spouštět každou hodinu. Tato úloha spouští každou hodinu. Minutu řídí *startTime*, pokud je zadaná nebo pokud není zadaný žádný, podle času vytvoření. Například pokud je čas spuštění nebo čas vytvoření (podle toho, co platí) 12:25 hodin, úloha se budou spouštět v 00:25, 01:25 02:25,..., 23:25. Plán je ekvivalentní s úlohu s *frekvence* "hodina" *interval* 1, a ne *plán*. Rozdíl je, že tento plán může používat jiné *frekvence* a *interval* příliš vytvořit jiné úlohy. Například pokud *frekvence* byly "měsíc", plán by spustit jenom jednou za měsíc místo každý den Pokud *frekvence* byly "den" |
| <code>{minutes:[0]}</code> |Spouštět každou hodinu hodině. Tato úloha také spouští každou hodinu, ale na hodinu (například 12: 00, 1: 00, 2: 00, atd.) Jde o ekvivalent úlohy s četností "hodina", čas spuštění s nulový počet minut a žádný plán Pokud frekvence byly "dne", ale pokud je četnost byly "týden" nebo "měsíc", plán by spustit pouze jeden den, týden nebo jednoho dne a měsíce, v uvedeném pořadí. |
| <code>{"minutes":[15]}</code> |Spustit na 15 minut po hodině každou hodinu. Spouští se každou hodinu od 00:15, tedy v 1:15, 2:15 atd., a končí ve 22:15 a 23:15. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Spustit v 17: 00 v sobotu každý týden |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit v 17: 00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit v 17:15:00 a 17:45:00 v pondělí, středu a pátek každý týden |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit na 5: 00 a 17: 00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Spustí se ve všední dny každých 15 minut. |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Spustit každých 15 minut mezi 9: 00 a 16:45:00 ve všední dny |
| <code>{"weekDays":["sunday"]}</code> |Spustit v neděli při spuštění |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Na úterý a každý čtvrtek při spuštění |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Spustit v 6: 00 na 28th den z každých měsíc (za předpokladu frekvenci měsíc) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Spusťte v 6: 00 poslední den v měsíci. Pokud chcete spustit úlohu v poslední den v měsíci, použijte místo den 28, 29, 30 a 31 -1. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Spustit v 6: 00 na první a poslední den v měsíci |
| <code>{monthDays":[1,-1]}</code> |Spusťte na první a poslední den v měsíci při spuštění |
| <code>{monthDays":[1,14]}</code> |Na první a čtrnáctá den každý měsíc při spuštění |
| <code>{monthDays":[2]}</code> |Spusťte na druhý den v měsíci při spuštění |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |Spustit na prvním každý pátek v měsíci v 5: 00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Spusťte na první každý pátek v měsíci při spuštění |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Spustit na třetí pátek od konce měsíce, každý měsíc, při spuštění |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Spustit na první a poslední pátek v každém měsíci v 5:15:00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Spustit na první a poslední pátek v každém měsíci v čas spuštění |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Spusťte na páté každý pátek v měsíci v čase zahájení. Pokud neexistuje žádné páté pátek v měsíci, to nejde spustit, protože je naplánována na spuštění v pouze páté pátek. Zvažte použití -1 namísto 5 pro výskyt, pokud chcete spustit úlohu v posledního výskytu pátek v měsíci. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Spustit každých 15 minut na poslední pátek v měsíci |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 ve 3. středu každého měsíce |

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Úvod do používání Scheduleru na portálu Azure Portal](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

