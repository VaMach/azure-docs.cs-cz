---
title: "Pochopení dotazovací jazyk Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popis dotazu jazyka SQL jako IoT Hub používá k načtení informací o úlohách a dvojčata zařízení ze služby IoT hub."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: a7650104eda58923558892f6f0f6666d16dbce28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv

Služba IoT Hub zajišťuje efektivní jazyka SQL jako k načtení informací o [dvojčata zařízení] [ lnk-twins] a [úlohy][lnk-jobs], a [směrování zpráv][lnk-devguide-messaging-routes]. Tento článek představuje:

* Úvod do hlavní funkce jazyka dotazů služby IoT Hub, a
* Podrobný popis jazyka.

## <a name="get-started-with-device-twin-queries"></a>Začínáme s dotazy twin zařízení
[Dvojčata zařízení] [ lnk-twins] může obsahovat libovolné objekty JSON vlastnosti a značky. IoT Hub umožňuje dvojčata zařízení dotaz jako jeden dokument JSON obsahující všechny informace o dvojici zařízení.
Předpokládejme například, že vaše dvojčata zařízení IoT hub mít následující strukturu:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

IoT Hub zpřístupní dvojčata zařízení jako kolekce dokumentů s názvem **zařízení**.
Následující dotaz načte tak celou sadu dvojčata zařízení:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Sady SDK služby Azure IoT] [ lnk-hub-sdks] podporu stránkování výsledků velký.

IoT Hub můžete načíst dvojčata zařízení filtrování pomocí libovolné podmínky. Pro instanci,

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

načte dvojčata zařízení pomocí **location.region** značky nastavit na **USA**.
Logické operátory a aritmetické porovnání jsou podporovány také, například

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

načte všechny dvojčata zařízení nachází v USA, konfigurované k odesílání telemetrie méně často než každou minutu. Pro potřeby je také možné použít konstanty pole s **IN** a **NV** operátory (ne v). Pro instanci,

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

načte všechny dvojčata zařízení, které ohlásil Wi-Fi nebo drátové připojení. Často je potřeba určit všechny dvojčata zařízení, které obsahují určité vlastnosti. IoT Hub podporuje funkce `is_defined()` pro tento účel. Pro instanci,

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

načíst všechny dvojčata zařízení, které definují `connectivity` hlášené vlastnost. Odkazovat [klauzule WHERE] [ lnk-query-where] části pro úplný přehled funkcí filtrování.

Seskupení a agregace jsou také podporovány. Pro instanci,

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Vrátí počet zařízení v každé stav konfigurace telemetrie.

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Předchozí příklad znázorňuje situaci, kdy tři zařízení hlášené úspěšná konfigurace, dva jsou stále aplikování konfigurace a jeden ohlásil chybu.

### <a name="c-example"></a>Příklad jazyka C#
Funkce dotazu je zveřejněna rozhraním [C# sady SDK služby] [ lnk-hub-sdks] v **RegistryManager** třídy.
Tady je příklad jednoduchého dotazu:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

Poznámka: Jak **dotazu** vytvořit instanci objektu s velikostí stránky (až 1 000), a pak může načíst více stránek volání **GetNextAsTwinAsync** metody vícekrát.
Všimněte si, že objektu dotazu vystavuje více **Další\***, v závislosti na možnost deserializaci vyžadované dotazu, například zařízení úlohy nebo dvojici objektů nebo prostý JSON má být použit při použití projekce.

### <a name="nodejs-example"></a>Příklad Node.js
Funkce dotazu je zveřejněna rozhraním [sady SDK služby Azure IoT pro Node.js] [ lnk-hub-sdks] v **registru** objektu.
Tady je příklad jednoduchého dotazu:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

Poznámka: Jak **dotazu** vytvořit instanci objektu s velikostí stránky (až 1 000), a pak může načíst více stránek volání **nextAsTwin** metody vícekrát.
Všimněte si, že objektu dotazu vystavuje více **Další\***, v závislosti na možnost deserializaci vyžadované dotazu, například zařízení úlohy nebo dvojici objektů nebo prostý JSON má být použit při použití projekce.

### <a name="limitations"></a>Omezení
> [!IMPORTANT]
> Výsledky dotazu může mít několik minut zpoždění s ohledem na něj nejnovější hodnoty v dvojčata zařízení. Pokud dotazuje dvojčata jednotlivých zařízení podle id, vždycky je vhodnější použít načtení twin rozhraní API pro zařízení, která vždy obsahuje nejnovější hodnoty a má, vyšší omezení.

V současné době porovnání jsou podporovány pouze mezi primitivní typy (žádné objekty), například `... WHERE properties.desired.config = properties.reported.config` je podporováno pouze v případě, že tyto vlastnosti mají primitivní hodnoty.

## <a name="get-started-with-jobs-queries"></a>Začínáme s dotazy úlohy
[Úlohy] [ lnk-jobs] poskytují způsob k provedení operací na sadu zařízení. Každý dvojče zařízení obsahuje informace o úlohách, které je součástí v kolekci s názvem **úlohy**.
Logicky,

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Tato kolekce je v současné době dotazovatelné jako **devices.jobs** ve službě IoT Hub dotazu jazyka.

> [!IMPORTANT]
> V současné době je nikdy vrácena vlastnost úlohy při dotazování dvojčata zařízení (to znamená, dotazy, které obsahuje, ze zařízení"). Lze přistupovat pouze přímo s dotazy pomocí `FROM devices.jobs`.
>
>

Například k získání všech úloh (posledních a plánované), které mají vliv jedno zařízení, můžete použít následující dotaz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Všimněte si, jak tento dotaz obsahuje stav konkrétní zařízení (a případně přímá metoda odpověď) každé úlohy vrátil.
Je také možné filtrovat pomocí libovolné Boolean podmínek na všechny vlastnosti objektu v **devices.jobs** kolekce.
Například následující dotaz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

načte všechny dokončit zařízení twin aktualizace úlohy pro zařízení **myDeviceId** vytvořených po září 2016.

Je také možné načíst výstupy podle zařízení jednu úlohu.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Omezení
V současné době se dotazuje na **devices.jobs** nepodporují:

* Projekce, proto pouze `SELECT *` je možné.
* Podmínky, které odkazují na dvojče zařízení kromě vlastnosti úlohy (viz předchozí části).
* Provádění agregace, jako je počet, avg, Seskupit podle.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Začínáme s výrazy dotazů trasy zprávu typu zařízení cloud

Pomocí [zařízení cloud trasy][lnk-devguide-messaging-routes], můžete nakonfigurovat Centrum IoT k odesílání zpráv typu zařízení cloud k různým koncovým bodům založené na výrazech vyhodnotit na základě jednotlivých zpráv.

Trasa [podmínku] [ lnk-query-expressions] používá stejný dotaz jazyk IoT Hub jako podmínky v dotazech twin a úlohy. Podmínky trasy se vyhodnocují na hlavičky zpráv a text. Směrování výraz dotazu může zahrnovat pouze hlavičky zpráv, pouze text zprávy, nebo obě zpráva hlavičky a tělo zprávy. IoT Hub předpokládá konkrétní schéma pro záhlaví a text zprávy, aby bylo možné směrovat zprávy a následující části popisují, co je vyžadována pro IoT Hub, která správně trasy:

### <a name="routing-on-message-headers"></a>Směrování v záhlaví zpráv

IoT Hub předpokládá následující reprezentace JSON hlavičky zpráv pro směrování zpráv:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

Vlastnosti zprávu systému mají předponu `'$'` symbol.
Vlastnosti uživatelů jsou vždy přistupovat pomocí jeho názvu. Pokud se stane název vlastnosti uživatele, se shoduje s vlastností systému (například `$to`), bude načten vlastnost uživatele s `$to` výraz.
Vždy přístup k vlastnosti systému pomocí závorek `{}`: například můžete použít ve výrazu `{$to}` pro přístup k vlastnosti systému `to`. Názvy vlastností v závorkách vždy načítají odpovídající vlastnost systému.

Mějte na paměti, že jsou názvy vlastností malá a velká písmena.

> [!NOTE]
> Všechny vlastnosti zprávy jsou řetězce. Vlastnosti systému, jak je popsáno v [Příručka vývojáře][lnk-devguide-messaging-format], nejsou aktuálně k dispozici pro použití v dotazech.
>

Například, pokud používáte `messageType` vlastnost, můžete chtít směrovat všechny telemetrická jeden koncový bod a všechny výstrahy pro jiný koncový bod. Můžete napsat následující výraz směrovat telemetrie:

```sql
messageType = 'telemetry'
```

A následující výraz směrovat výstražných zpráv:

```sql
messageType = 'alert'
```

Logické výrazy a funkce jsou také podporovány. Tato funkce umožňuje rozlišovat mezi úroveň závažnosti, například:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Odkazovat [výraz a podmínky] [ lnk-query-expressions] části úplný seznam podporovaných operátory a funkce.

### <a name="routing-on-message-bodies"></a>Směrování na obsah zpráv

IoT Hub můžete pouze směrování podle tělo zprávy obsah, pokud text zprávy je správně vytvořen JSON kódování UTF-8, UTF-16 nebo UTF-32. Je nutné nastavit typ obsahu zprávy `application/json` a kódování obsahu na jednu z podporovaných kódování UTF v záhlaví zprávy umožňující IoT Hub pro směrování zpráv na základě obsahu textu. Pokud není zadán buď z hlaviček, IoT Hub se nebude pokoušet vyhodnotit jakýkoli výraz dotazu zahrnující těla proti zprávy. Pokud vaše zpráva není zprávu JSON, nebo pokud zpráva neurčuje typu obsahu a kódování obsahu, může stále používáte směrování zpráv směrovat zprávy založené na záhlaví zprávy.

Můžete použít `$body` ve výrazu dotazu pro odesílání zpráv. Jednoduchý text odkazu, odkaz na pole text nebo více odkazů text můžete použít ve výrazu dotazu. Výraz dotazu můžete také kombinovat textu odkaz s odkazem na záhlaví zprávy. Tady jsou například všechny výrazy platný dotaz:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Základní informace o dotaz služby IoT Hub
Každé centrum IoT dotaz sestává s výběrem a klauzulí FROM a volitelné WHERE a GROUP BY – klauzule. Každý dotaz je spuštěn na kolekci dokumentů JSON, například dvojčata zařízení. V klauzuli FROM označuje kolekce dokumentu. Chcete-li být vstupní na (**zařízení** nebo **devices.jobs**). Pak je použit filtr v klauzuli WHERE. S agregace, výsledky tohoto kroku seskupeny jako zadaný v klauzuli GROUP BY a pro každou skupinu, je generována řádek uvedené v klauzuli SELECT.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM – klauzule
**z < from_specification >** klauzule můžete předpokládat pouze dvě hodnoty: **ze zařízení**, aby dotaz dvojčata zařízení nebo **z devices.jobs**, k dotazování úlohy podle zařízení podrobnosti.

## <a name="where-clause"></a>Klauzule WHERE
**Kde < filter_condition >** klauzule je volitelný. Určuje, že jeden nebo více podmínky, že dokumenty JSON v kolekci FROM musí splňovat zahrnuty jako součást výsledku. Dokumentu JSON se musí vyhodnotit na hodnotu true, mají být zahrnuty do výsledku k zadaným podmínkám.

Povolené podmínky jsou popsány v části [výrazy a podmínky][lnk-query-expressions].

## <a name="select-clause"></a>Klauzule SELECT
Klauzule SELECT (**vyberte < select_list >**) je povinná a určuje, jaké hodnoty jsou načteny z dotazu. Určuje hodnoty JSON, které mají být použita ke generování nových objektů JSON.
Pro každý prvek filtrované (a volitelně seskupené) podmnožinu kolekce FROM fázi projekce generuje nový objekt JSON, zkonstruovat s hodnot zadaných v klauzuli SELECT.

Toto je gramatiky klauzuli SELECT:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

kde **attribute_name** odkazuje na žádnou vlastnost dokumentu JSON v kolekci FROM. Některé příklady klauzule FROM lze nalézt v [Začínáme s dotazy twin zařízení] [ lnk-query-getstarted] části.

V současné době výběr klauzule liší od **vyberte \***  jsou podporovány pouze v agregační dotazy na dvojčata zařízení.

## <a name="group-by-clause"></a>klauzule GROUP BY
**GROUP BY < group_specification >** klauzule je volitelný krok, který lze provést po filtr zadaný v klauzuli WHERE a před projekce určená v SELECT. Seskupuje dokumentů na základě hodnoty atributu. Tyto skupiny se používají ke generování agregované hodnoty zadané v klauzuli SELECT.

Příklad dotazu pomocí skupiny je:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Formální syntaxe GROUP BY je:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

kde **attribute_name** odkazuje na žádnou vlastnost dokumentu JSON v kolekci FROM.

V klauzuli GROUP BY je v současné době podporována pouze při dotazování dvojčata zařízení.

## <a name="expressions-and-conditions"></a>Výrazy a podmínky
Na vysoké úrovni *výraz*:

* Vyhodnotí na instanci typu JSON (například logická hodnota, čísla, řetězce, pole nebo objekt), a
* Je definován manipulace s daty z dokumentu JSON zařízení a konstanty pomocí předdefinovaných operátory a funkce.

*Podmínky* jsou výrazy, které vyhodnocena jako logická hodnota. Žádné jiné než logická hodnota konstanta **true** se považuje za **false** (včetně **null**, **nedefinované**, všechny objekt nebo pole instance, libovolného řetězce a jasně logickou hodnotu **false**).

Syntaxe pro výrazy je:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Kde:

| Symbol | Definice |
| --- | --- |
| attribute_name | Všechny vlastnosti v dokumentu JSON **FROM** kolekce. |
| binary_operator | Všechny binární operátor uvedené v [operátory](#operators) části. |
| Název_funkce| Všechny funkce uvedené v [funkce](#functions) části. |
| decimal_literal |Float, vyjádřené v desítkovém zápisu. |
| hexadecimal_literal |Číslo vyjádřená řetězec '0 x, za nímž následuje řetězec šestnáctkových číslic. |
| string_literal |Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode. Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: "). Povolené řídicí sekvence: `\'`, `\"`, `\\`, `\uXXXX` pro znaky znakové sady Unicode, které jsou definované 4 hexadecimální číslice. |

### <a name="operators"></a>Operátory
Podporovány jsou následující operátory:

| Rodina | Operátory |
| --- | --- |
| Aritmetické operace |+,-,*,/,% |
| Logické |A, NEBO NE |
| Porovnání |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Funkce
Při dotazování dvojčata a úlohám, které jediný podporovaný je funkce:

| Funkce | Popis |
| -------- | ----------- |
| IS_DEFINED(Property) | Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu (včetně `null`). |

V podmínkách trasy jsou podporovány následující matematické funkce:

| Funkce | Popis |
| -------- | ----------- |
| Abs(x) | Vrátí absolutní hodnotu (kladné) zadaný číselný výraz. |
| Exp(x) | Vrátí hodnotu exponenciálního zadaný číselný výraz (e ^ x). |
| Power(x,y) | Vrátí hodnotu zadaného výrazu určenou mocninu (x ^ y).|
| Square(x) | Vrátí druhou mocninu Zadaná číselná hodnota. |
| CEILING(x) | Vrátí nejmenší hodnotu, celé číslo větší než nebo rovna hodnotě zadané číselný výraz. |
| Floor(x) | Vrátí největší celé číslo menší než nebo rovna zadané číselný výraz. |
| Sign(x) | Vrátí kladnou (+ 1), nula (0) nebo záporné znaménko (-1) zadaný číselný výraz.|
| Sqrt(x) | Vrátí druhou mocninu Zadaná číselná hodnota. |

V podmínkách trasy jsou podporovány následující kontrola typu a přetypování funkce:

| Funkce | Popis |
| -------- | ----------- |
| AS_NUMBER | Převede vstupní řetězec na číslo. `noop`Pokud vstup je číslo; `Undefined` Pokud řetězec nepředstavuje číslo.|
| IS_ARRAY | Vrátí logickou hodnotu udávající, pokud je typ zadaný výraz pole. |
| IS_BOOL | Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je logická hodnota. |
| IS_DEFINED | Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu. |
| IS_NULL | Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu hodnotu null. |
| IS_NUMBER | Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je číslo. |
| IS_OBJECT | Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je objekt JSON. |
| IS_PRIMITIVE | Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu jednoduchého typu (řetězec, logická hodnota, číselná nebo `null`). |
| IS_STRING | Vrátí logickou hodnotu udávající, pokud typ zadaný výraz obsahuje řetězec. |

V podmínkách trasy jsou podporovány následující funkce řetězce:

| Funkce | Popis |
| -------- | ----------- |
| CONCAT(x,...) | Vrátí řetězec, který je výsledkem zřetězení dvou nebo více řetězcové hodnoty. |
| Length(x) | Vrátí počet znaků ze zadaného řetězcového výrazu.|
| LOWER(x) | Vrací výraz řetězce po převodu dat velké písmeno na malá písmena. |
| UPPER(x) | Vrací výraz řetězce po převodu dat malé písmeno na velká písmena. |
| Dílčí řetězec (string, spuštění [, délka]) | Vrátí část řetězcového výrazu od pozice s nulovým základem zadaný znak a pokračuje na určenou délku nebo na konci řetězce. |
| INDEX_OF (řetězec, fragment) | Vrátí počáteční pozici prvního výskytu druhý řetězec výrazu v rámci první zadaného řetězcového výrazu nebo -1, pokud není nalezen řetězec.|
| STARTS_WITH (x, y) | Vrátí logická hodnota, která určuje zda první řetězec výraz začíná druhý. |
| ENDS_WITH (x, y) | Vrátí logická hodnota, která určuje zda první řetězec výraz končí druhý. |
| CONTAINS(x,y) | Vrátí logická hodnota, která určuje zda první řetězec výraz obsahuje druhý. |

## <a name="next-steps"></a>Další kroky
Zjistěte, jak na provedení dotazů ve svých aplikacích pomocí [SDK služby Azure IoT][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
