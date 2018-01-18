---
title: "Azure Service Fabric rozhraní příkazového řádku - sfctl choas | Microsoft Docs"
description: "Popisuje příkazy chaos sfctl Service Fabric rozhraní příkazového řádku."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: dbea84511c37cf52c3d98f0247e5ce3c0b2a05c3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Spuštění, zastavení a sestav ve službě chaos testu.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    sestava| Získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu.|
|    start | Pokud Chaos ještě není spuštěná v clusteru, spustí se Chaos se zadaným v Chaos parametry.|
|    Stop  | Zastaví Chaos v clusteru Pokud je již spuštěna, jinak se neprovede žádnou akci.|


## <a name="sfctl-chaos-report"></a>Sestava chaos sfctl
Získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu.

Můžete buď zadat ContinuationToken získat další segment Chaos sestavy nebo můžete zadat časový rozsah prostřednictvím StartTimeUtc a EndTimeUtc, ale ContinuationToken a časový rozsah nelze zadat ve stejném volání. Pokud nejsou k dispozici více než 100 Chaos události, Chaos sestavy je vrácený v segmenty, kde segment obsahuje více než 100 Chaos událostí. 

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --token pokračování| Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| --end-time-utc   | Počet rysky reprezentující koncový čas časové rozmezí, pro kterou má být vytvořen Chaos sestavy. Přečtěte si [DateTime.Ticks vlastnost](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) podrobnosti o značek.|
| --start-time-utc | Počet rysky reprezentující čas zahájení časové rozmezí, pro kterou má být vytvořen Chaos sestavy. Přečtěte si [DateTime.Ticks vlastnost](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) podrobnosti o značek.|
| časový limit – -t     | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug          | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h        | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o      | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu          | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose        | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-chaos-start"></a>spuštění chaos sfctl
Pokud Chaos ještě není spuštěná v clusteru, spustí se Chaos se zadaným v Chaos parametry.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --app-type-health-policy-map  | JSON kódovaný seznam s maximální procento není v pořádku aplikacemi pro konkrétní aplikaci typy. Každý záznam určuje jako klíč název typu aplikace a jako hodnotu celé číslo, které představuje procento MaxPercentUnhealthyApplications používá k vyhodnocení aplikace typu zadané aplikace.|
| --disable-move-replica-faults | Zakáže přesunutí primární a sekundární chyb přesunout.|
| --max-cluster ustálení| Maximální množství času čekání pro všechny clusteru entity k stabilní a v pořádku.  Výchozí: 60.|
| --max-concurrent-faults    | Maximální počet souběžných chyb vyvolané za iteraci.           Výchozí: 1.|
| --max-percent-unhealthy-apps  | Při vyhodnocování stavu clusteru během Chaos, maximální povolené procento žádostí, není v pořádku, než ohlásí chybu.|
| --max-percent-unhealthy-nodes | Při vyhodnocování stavu clusteru během Chaos, maximální povolené procento není v pořádku uzlů než ohlásí chybu.|
| --time-to-run              | Celková doba (v sekundách) pro který Chaos spustí před zastavením automaticky. Maximální povolená hodnota je 4 294 967 295 (System.UInt32.MaxValue).  Výchozí: 4294967295.|
| časový limit – -t               | Server časový limit v sekundách.  Výchozí: 60.|
| --Čekací doba mezi chyb | Čekací doba (v sekundách) mezi po sobě jdoucích chyb v rámci jedné iterací.  Výchozí: 20.|
| --Čekací doba mezi iterací| Čas – oddělení (v sekundách) mezi dvě po sobě jdoucích opakování Chaos.  Výchozí: 30.|
| --warning-as-error         | Při vyhodnocování stavu clusteru během Chaos, považovat za upozornění s stejné závažnosti chyby.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                    | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                  | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.           Výchozí: json.|
| --dotazu                    | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                  | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-chaos-stop"></a>zastavení chaos sfctl
Zastaví Chaos v clusteru Pokud je již spuštěna, jinak se neprovede žádnou akci.

Zastaví Chaos z plánování další chyb. ale neovlivní chyb během letu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| časový limit – -t| Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug  | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h| Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu  | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose| Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další postup
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).