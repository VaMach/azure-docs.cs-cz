---
title: "Azure Service Fabric rozhraní příkazového řádku - sfctl partition | Microsoft Docs"
description: "Popisuje příkazy Service Fabric rozhraní příkazového řádku sfctl oddílu."
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
ms.openlocfilehash: 01dd1900fe765618e5da20bd289b9c3a021ea9a3
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-partition"></a>sfctl partition
Dotazování a správu oddílů pro libovolnou službu.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    ztrátě dat.      | Toto rozhraní API indukuje ztráta dat pro zadaný oddíl.|
|    data-loss-status  | Získá průběh operace ztráty dat oddílu pomocí rozhraní API StartDataLoss spuštěna.|
|    stav         | Získá stav zadaný oddíl Service Fabric.|
|    Informace o           | Získá informace o oddílu Service Fabric.|
|    Seznam           | Získá seznam oddílů služby Service Fabric.|
|    načítání           | Získá zatížení zadaný oddíl Service Fabric.|
|    Resetování zatížení     | Obnoví aktuální zatížení oddílu Service Fabric.|
|    ztrátě kvora    | Indukuje ztrátě kvora pro daný stavové služby oddíl.|
|    kvora. ke ztrátě stavu| Získá průběh operace ztráty kvora v oddílu pomocí rozhraní API StartQuorumLoss spuštěna.|
|    Obnovení        | Do clusteru Service Fabric označuje, že mají pokusit o obnovení na konkrétní oddíl, který je aktuálně zablokované ve ztrátě kvora.|
|    Obnovit všechny    | Do clusteru Service Fabric označuje, že mají pokusit o obnovení služby (včetně systémových služeb), které jsou aktuálně zablokované ve ztrátě kvora.|
|    report-health  | Odešle zprávu o stavu v oddílu Service Fabric.|
|    Restartování        | Toto rozhraní API restartuje některé nebo všechny repliky nebo instancí zadaný oddíl.|
|    restart-status | Získá průběh operace PartitionRestart pomocí StartPartitionRestart spuštěna.|
|    svc-name       | Získá název služby Service Fabric pro oddíl.|


## <a name="sfctl-partition-health"></a>sfctl oddílu stavu
Získá stav zadaný oddíl Service Fabric.

Získá informace o stavu zadaného oddílu. EventsHealthStateFilter použijte k filtrování kolekce události stavu ohlášeny služby založené na stav v pořádku.
ReplicasHealthStateFilter použijte k filtrování kolekce objektů ReplicaHealthState v oddílu. Pokud zadáte oddílu, který neexistuje v úložišti stavů, tato rutina vrátí chybu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id oddílu [vyžaduje]| Identita oddílu.|
| --events-health-state-filter  | Umožňuje filtrování vrácených objektů HealthEvent kolekce na základě stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů.                Se vrátí jenom události, které odpovídají filtru. Všechny události se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8.                -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState.                Hodnota je 65535.|
|--exclude-health-statistics   | Určuje, zda má být vrácen stav statistiky jako součást výsledků dotazu. Chcete-li hodnotu false ve výchozím nastavení. Statistiku zobrazují počet podřízených entit ve stavu Ok, upozornění a chyby.|
| --replicas-health-state-filter| Umožňuje filtrování kolekce objektů ReplicaHealthState v oddílu. Hodnota je možné získat od členů nebo bitové operace u členů HealthStateFilter. Vrátí se pouze repliky, které odpovídají filtru. Všechny repliky se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535.|
| časový limit – -t               | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                    | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                  | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                Výchozí: json.|
| --dotazu                    | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/. |
| -verbose                  | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-partition-info"></a>informace o oddílu sfctl
Získá informace o oddílu Service Fabric.

Koncový bod oddíly vrací informace o zadaný oddíl. Odpověď obsahuje ID oddílu, informace o vytváření oddílů schéma, klíče nepodporuje oddílu, stav, stav a další podrobnosti o oddílu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id oddílu [vyžaduje]| Identita oddílu.|
| časový limit – -t          | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h             | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu               | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-partition-list"></a>seznam oddílů sfctl
Získá seznam oddílů služby Service Fabric.

Získá seznam oddílů služby Service Fabric. S ID oddílu, informace o vytváření oddílů schéma, klíče nepodporuje oddílu, stav, stav a další podrobnosti o oddílu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id služby [vyžaduje]| Identita služby. Toto je obvykle úplný název služby bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například, pokud je název služby "fabric: / myapp/app1/svc1", bude identita služby "Moje aplikace ~ app1 ~ svc1" v 6.0 + a "myapp/app1/svc1" v předchozích verzích.|
| --token pokračování| Parametr token pokračování slouží k získání další sadu výsledků.         Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky, pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| časový limit – -t        | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug             | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h           | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o         | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu             | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose           | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-partition-load"></a>zatížení oddílu sfctl
Získá zatížení zadaný oddíl Service Fabric.

Vrací informace o zadaný oddíl. Odpověď obsahuje seznam načíst informace o. Každý informace zahrnují název metriky zatížení, hodnotu a čas poslední hlášené ve standardu UTC.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id oddílu [vyžaduje]| Identita oddílu.|
| časový limit – -t          | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h             | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu               | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-partition-recover"></a>Obnovit sfctl oddílu
Do clusteru Service Fabric označuje, že mají pokusit o obnovení na konkrétní oddíl, který je aktuálně zablokované ve ztrátě kvora.

Do clusteru Service Fabric označuje, že mají pokusit o obnovení na konkrétní oddíl, který je aktuálně zablokované ve ztrátě kvora. Tuto operaci musí provést pouze v případě se ví, že repliky, které jsou vypnutí nelze obnovit. Nesprávné použití toto rozhraní API může způsobit ztrátu dat.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id oddílu [vyžaduje]| Identita oddílu.|
| časový limit – -t          | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h             | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu               | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-partition-restart"></a>sfctl oddílu restartování
Toto rozhraní API restartuje některé nebo všechny repliky nebo instancí zadaný oddíl.

Toto rozhraní API je užitečné pro testování převzetí služeb při selhání. Pokud se používají k zaměření bezstavové služby oddíl, musí být RestartPartitionMode AllReplicasOrInstances. Volání rozhraní API GetPartitionRestartProgress pomocí stejné OperationId zobrazíte průběh.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id operace [vyžaduje]| Identifikátor GUID, který identifikuje volání toto rozhraní API.  To je předán do odpovídající GetProgress API.|
| – id oddílu [vyžaduje]| Identita oddílu.|
| – restartování oddílu mode [vyžaduje]| Popisují oddílů pro restartování.|
| – id služby [vyžaduje]| Identita služby. Toto je obvykle úplný název služby bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například, pokud je název služby "fabric: / myapp/app1/svc1", identita služby by být "Moje aplikace ~ app1 ~ svc1" v 6.0 + a "myapp/app1/svc1" v předchozí sunout rsions.|
| časový limit – -t                    | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                         | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                       | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                     | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                     Výchozí: json.|
| --dotazu                         | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose                       | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další postup
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).
