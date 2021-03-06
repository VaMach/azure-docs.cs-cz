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
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 34e4d47b1de509c2053996d9d1078733d7055447
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Spuštění, zastavení a sestav ve službě chaos testu.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    sestava| Získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu.|
|    start | Spustí Chaos v clusteru.|
|    Stop  | Zastaví Chaos v clusteru Pokud je již spuštěna, jinak se neprovede žádnou akci.|


## <a name="sfctl-chaos-report"></a>Sestava chaos sfctl
Získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu.

Můžete buď zadat ContinuationToken získat další segment Chaos sestavy nebo můžete zadat časový rozsah prostřednictvím StartTimeUtc a EndTimeUtc, ale ContinuationToken a časový rozsah nelze zadat ve stejném volání. Pokud nejsou k dispozici více než 100 Chaos události, Chaos sestavy je vrácený v segmenty, kde segment obsahuje více než 100 Chaos událostí. 

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --token pokračování| Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| --end-time-utc   | Windows souboru čas reprezentující koncový čas časové rozmezí, pro kterou má být vytvořen Chaos sestavy. Přečtěte si [DateTime.ToFileTimeUtc metoda](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) podrobnosti.|
| --start-time-utc | Windows souboru čas reprezentující čas zahájení časové rozmezí, pro kterou má být vytvořen Chaos sestavy. Přečtěte si [DateTime.ToFileTimeUtc metoda](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) podrobnosti.|
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
Spustí Chaos v clusteru.

Pokud ještě není spuštěná Chaos v clusteru, začíná Chaos předaný v Chaos parametry. Pokud Chaos je již spuštěn, když toto volání se provádí, volání selže s kódem chyby FABRIC_E_CHAOS_ALREADY_RUNNING.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --app-type-health-policy-map  | JSON kódovaný seznam s maximální procento není v pořádku aplikacemi pro konkrétní aplikaci typy. Každý záznam určuje jako klíč název typu aplikace a jako hodnotu celé číslo, které představuje procento MaxPercentUnhealthyApplications používá k vyhodnocení aplikace typu zadané aplikace. Definuje mapu s maximální procento není v pořádku aplikacemi pro konkrétní aplikaci typy. Každý záznam určuje jako klíč název typu aplikace a jako hodnota celé číslo, které představuje procento MaxPercentUnhealthyApplications používá k vyhodnocení aplikace typu zadané aplikace. Mapy zásady stavu typ aplikace lze během vyhodnocení stavu clusteru popisují typy speciální aplikací. Typy aplikací, které jsou součástí mapy vyhodnocují proti procento zadané v mapě a nikoli s globální MaxPercentUnhealthyApplications definované v zásadách stavu clusteru. Aplikace zadaná v mapování typů aplikací nejsou započítané globální fond aplikací. Například pokud některé aplikace a typ jsou důležité, Správce clusteru můžete přidat položku do mapy pro tento typ aplikace a přiřaďte ho hodnotu 0 % (to znamená, není tolerovat případných selhání). Všechny ostatní aplikace můžete vyhodnotit s MaxPercentUnhealthyApplications nastavit na hodnotu 20 % na tolerovat několika selháními mimo tisíců instancí aplikace. Mapy zásady stavu typ aplikace se používá pouze v případě, že v manifestu clusteru umožňuje vyhodnocení stavu typu aplikace pomocí položky konfigurace pro HealthManager/EnableApplicationTypeHealthEvaluation.|
|--chaos filtr cíle         | JSON zakódovaných pomocí dva řetězce typu klíče slovníku. Dva klíče jsou NodeTypeInclusionList a ApplicationInclusionList. Hodnoty pro obě tyto klíče jsou seznam řetězců. chaos_target_filter definuje všechny filtry pro cílové Chaos chyb, například chybující pouze určité typy uzlů nebo chybující jenom určité aplikace. Pokud se nepoužívá chaos_target_filter, Chaos závady všechny entity clusteru. Pokud se používá chaos_target_filter Chaos závady jenom entity, které splňují specifikace chaos_target_filter. NodeTypeInclusionList a ApplicationInclusionList povolit pouze union sémantiku. Není možné zadat průnik NodeTypeInclusionList a ApplicationInclusionList. Například není možné zadat "poruch tuto aplikaci, pouze pokud je na daný typ uzlu." Jakmile entity je součástí NodeTypeInclusionList nebo ApplicationInclusionList, nelze dané entity vyloučit pomocí ChaosTargetFilter. I v případě, že applicationX se nezobrazí v ApplicationInclusionList, v některých Chaos iteraci applicationX může být došlo k chybě protože Odehrává se na uzlu nodeTypeY, který je součástí NodeTypeInclusionList. Pokud NodeTypeInclusionList a ApplicationInclusionList jsou prázdná, je vyvolána ArgumentException. Všechny typy chyb (restartovat uzel, restartujte balíček kódu, odeberte repliky, restartujte repliky, přesunutí primární a sekundární) jsou povolené pro uzly z těchto typů uzlu. Pokud typ uzlu (vyslovení NodeTypeX) nejsou uvedené v NodeTypeInclusionList, pak uzlu úrovni chyb (např. NodeRestart) budou nikdy povolené pro uzly NodeTypeX, ale kód balíčku a repliky chyb lze povolit stále pro NodeTypeX, pokud aplikace Jsou umístěny na uzlu NodeTypeX ApplicationInclusionList se stane. V tomto seznamu, tento počet zvýšit mohou obsahovat maximálně 100 názvy typu uzlu, se vyžaduje ke konfiguraci MaxNumberOfNodeTypesInChaosEntityFilter konfigurace upgradu. Všechny repliky, které patří do služby tyto aplikace se přenášejí pomocí Chaos repliky chyb (restartování replika, replika odebrat, přesunutí primární a sekundární přesunutí). Chaos restartovat balíček kódu jenom v případě, že balíček kódu je hostitelem repliky jenom tyto aplikace. Pokud aplikace v tomto seznamu nezobrazí, ho můžete stále být došlo k chybě v některé Chaos iteraci Pokud ukončení aplikace v uzlu typu uzlu, který je součástí NodeTypeInclusionList. Ale pokud applicationX je vázaný na nodeTypeY prostřednictvím omezení umístění a applicationX chybí z ApplicationInclusionList a nodeTypeY chybí z NodeTypeInclusionList, pak applicationX nikdy selže. Maximálně 1000 názvy aplikací mohou být součástí tohoto seznamu, tento počet zvýšit, se vyžaduje ke konfiguraci MaxNumberOfApplicationsInChaosEntityFilter konfigurace upgradu.|
|--context                     | JSON kódovaný mapu (řetězec, řetězec) zadejte páry klíč hodnota. Mapy slouží k zaznamenání informací o Chaos spustit. Nemůže být více než 100 tyto dvojice a každý řetězec (klíč nebo hodnota) může mít maximálně povolených 4095 znaků dlouhé. Tato mapa je nastavena podle starter zmatku spustit, aby se volitelně ukládat kontext o konkrétní spustit.|
| --disable-move-replica-faults | Zakáže přesunutí primární a sekundární chyb přesunout.|
| --max-cluster ustálení| Maximální množství času čekání pro všechny clusteru entity k stabilní a v pořádku.  Výchozí: 60. Chaos spustí iterací a na začátku každé iteraci ověřuje stav clusteru entit. Během ověřování Pokud clusteru entity není stabilní a nezotavila během MaxClusterStabilizationTimeoutInSeconds, Chaos vygeneruje událost selhání ověření.|
| --max-concurrent-faults    | Maximální počet souběžných chyb vyvolané za iteraci.           Výchozí: 1. Chaos spustí iterací a dvě po sobě jdoucích iterací jsou odděleny fázi ověřování. Čím souběžnosti, agresivnější vkládání chyb – vyvolat složitější řadu stavy k odhalení chyby. Doporučuje se k spuštění s hodnotou 2 nebo 3 a při přesouvání postupujte opatrně.|
| --max-percent-unhealthy-apps  | Při vyhodnocování stavu clusteru během Chaos, maximální povolené procento žádostí, není v pořádku, než ohlásí chybu. Maximální povolené procento žádostí, není v pořádku, než ohlásí chybu. Například povolit 10 % aplikací jako chybný, tato hodnota je 10. Procento představuje maximální procento. povolená aplikací, které může být není v pořádku, než clusteru považován za chybu. Pokud je dodržena procento, ale existuje alespoň jedna aplikace není v pořádku, stav budou vyhodnocené jako varování. To se vypočítá jako podíl počtu není v pořádku aplikace přes celkový počet instancí aplikace v clusteru, s výjimkou aplikace typy aplikací, které jsou součástí ApplicationTypeHealthPolicyMap. Výpočet zaokrouhlí na tolerovat selhání jednoho na malý počet aplikací. Výchozí procentuální hodnotu rovná nule.|
| --max-percent-unhealthy-nodes | Při vyhodnocování stavu clusteru během Chaos, maximální povolené procento není v pořádku uzlů než ohlásí chybu. Maximální povolené procento není v pořádku uzlů než ohlásí chybu. Například povolit 10 % uzlů jako chybný, tato hodnota je 10. Procento představuje maximální procento. povolená uzlů, které může být není v pořádku, než clusteru považován za chybu. Pokud je dodržena procento, ale je alespoň jeden uzel a není v pořádku, stav budou vyhodnocené jako varování. Procentuální se vypočítá jako podíl počtu uzlů není v pořádku přes celkový počet uzlů v clusteru. Výpočet zaokrouhlí na tolerovat jeden selhání na malém počtu uzlů. Výchozí procentuální hodnotu rovná nule. Ve velkých clusterech některé uzly bude vždy mimo provoz nebo se pro opravy, takže toto procento by měl být nakonfigurovaný pro tolerovat možnost, která.|
| --time-to-run              | Celková doba (v sekundách) pro který Chaos spustí před zastavením automaticky. Maximální povolená hodnota je 4 294 967 295 (System.UInt32.MaxValue).  Výchozí: 4294967295.|
| časový limit – -t               | Server časový limit v sekundách.  Výchozí: 60.|
| --Čekací doba mezi chyb | Čekací doba (v sekundách) mezi po sobě jdoucích chyb v rámci jedné iterací.  Výchozí: 20. Čím větší je hodnota, čím nižší překrývání mezi chyb a jednodušší pořadí stavu přechází procházející clusteru. Doporučuje se má začít s hodnotu mezi 1 a 5 a cvičení opatrní při přesunutí nahoru.|
| --Čekací doba mezi iterací| Čas – oddělení (v sekundách) mezi dvě po sobě jdoucích opakování Chaos. Větší hodnota, tím nižší rychlost chyby vkládání. Výchozí: 30.|
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