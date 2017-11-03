---
title: "Azure Service Fabric rozhraní příkazového řádku sfctl uzlem | Microsoft Docs"
description: "Popisuje příkazy Service Fabric rozhraní příkazového řádku sfctl uzlu."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: 76037c7b4a2f7ada314a9360e3990245e6fbc06c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-node"></a>sfctl uzlu
Spravujte uzly, které vytvoří cluster.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    Zakázat       | Deaktivace uzlu clusteru Service Fabric se záměrem zadaný deaktivace.|
|    Povolit        | Aktivujte uzlem clusteru Service Fabric, která je právě deaktivována.|
|    Stav        | Získá stav uzlu Service Fabric.|
|    Informace o          | Získá seznam uzlů v clusteru Service Fabric.|
|    seznam          | Získá seznam uzlů v clusteru Service Fabric.|
|    načítání          | Získá informace o zatížení Service Fabric uzlu.|
|    odebrat stavu  | Service Fabric upozorní, že trvalého stavu na uzlu trvale odstraněn nebo ztráty.|
|    Sestava stavu | Odešle zprávu o stavu na uzlu Service Fabric.|
|    Restartování       | Restartování uzlu clusteru Service Fabric.|
|    Přechod    | Spuštění nebo zastavení uzlu clusteru.|
|    Přechod stavu| Získá průběh operace pomocí StartNodeTransition spuštěna.|


## <a name="sfctl-node-disable"></a>zakázat sfctl uzlu
Deaktivace uzlu clusteru Service Fabric se záměrem zadaný deaktivace.

Deaktivace uzlu clusteru Service Fabric se záměrem zadaný deaktivace. Po deaktivaci v průběhu záměr deaktivace může být vyšší, ale není snížena (například uzel, který je deaktivována se záměrem pozastavení může být deaktivované další s Restart, ale ne naopak. Uzly může znovu aktivovat pomocí aktivace operaci uzlu kdykoli po jejich deaktivováno. Pokud není dokončena deaktivaci to zruší deaktivaci. Uzel, který přestane fungovat a zobrazí zpět při deaktivováno je ještě potřeba znovu aktivovat, než služeb se může v tomto uzlu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| --záměr deaktivace | Popisuje záměr nebo důvod deaktivace uzlu. Možné hodnoty jsou následující. -Pause - Určuje, že uzlu by měl pozastavena. Hodnota je 1. – Restartování – označuje, že je záměrem je pro uzel restartovat po krátkou dobu. Hodnota je 2. -RemoveData – označuje, že je pro uzel k odebrat data. Hodnota je 3. .|
| časový limit – -t       | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění            | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h          | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o        | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu            | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose          | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-enable"></a>Povolit sfctl uzlu
Aktivujte uzlem clusteru Service Fabric, která je právě deaktivována.

Aktivuje uzlem clusteru Service Fabric, která je právě deaktivována. Po aktivaci uzlu znovu stane přijatelná cíle pro umístění nové repliky, a všechny deaktivované repliky zbývající na uzlu jsou aktivovat.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| časový limit – -t       | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění            | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h          | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o        | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu            | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose          | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-health"></a>stav uzlu sfctl
Získá stav uzlu Service Fabric.

Získá stav uzlu Service Fabric. EventsHealthStateFilter použijte k filtrování kolekce události stavu ohlášeny uzel podle stavu. Pokud uzel, který určíte podle názvu v health store neexistuje, tento příkaz vrátí chybu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| --události stavu stavu filtru| Umožňuje filtrování vrácených objektů HealthEvent kolekce na základě stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Se vrátí jenom události, které odpovídají filtru. Všechny události se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535.|
| časový limit – -t             | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                  | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o              | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                  | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-info"></a>informace o sfctl uzlu
Získá seznam uzlů v clusteru Service Fabric.

Získá informace o konkrétním uzlu v clusteru Service Fabric. Odpověď obsahovat název, stav, ID, stavu, provozu a další podrobnosti o příslušném uzlu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| časový limit – -t       | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění            | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h          | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o        | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu            | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose          | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-list"></a>seznam uzlů sfctl
Získá seznam uzlů v clusteru Service Fabric.

Koncový bod uzly vrací informace o uzly v clusteru Service Fabric. Odpověď obsahuje název, stav, ID, stavu, provozu a další podrobnosti o příslušném uzlu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --token pokračování| Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné.      Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky, pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| --uzlu filtru stavu| Umožňuje filtrování podle NodeStatus uzly. Vrátí se pouze uzly, které jsou odpovídající hodnota zadaného filtru. Hodnota filtru může být jedna z následujících akcí. -Výchozí – Tato hodnota filtru odpovídá všechny uzly, s výjimkou těch stavem jako neznámý nebo odebrané. – všechny – tato hodnota filtru odpovídá všechny uzly. Tento filtr - nahoru – hodnota odpovídá uzly, které jsou zapnuty. Tento filtr - dolů – hodnota odpovídá uzlů, které jsou vypnuté. -povolení - tento filtr hodnota odpovídá uzlů, které jsou právě zapínání stavem jako povolení. -zakazuje - tento filtr hodnota odpovídá uzlů, které jsou během procesu zakázání se stavem jako zakážete. -zakázáno - tento filtr hodnota odpovídá uzlů, které jsou zakázány. -Neznámý – tato hodnota filtru odpovídá uzly, jejichž stav je neznámý. Uzel bude v neznámém stavu, není-li Service Fabric autoritativní informace o tomto uzlu. To může dojít, pokud systém dozví o uzel za běhu. -odebrat - tento filtr hodnota odpovídá uzly jejichž stav je odebráno. Toto jsou uzly, které byly odebrané z clusteru pomocí rozhraní API RemoveNodeState. .      Výchozí: výchozí.|
| časový limit – -t     | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění          | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h        | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o      | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu          | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose        | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-load"></a>sfctl uzlu zatížení
Získá informace o zatížení Service Fabric uzlu.

Získá informace o zatížení Service Fabric uzlu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| časový limit – -t       | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění            | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h          | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o        | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu            | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose          | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-restart"></a>restartování uzlu sfctl
Restartování uzlu clusteru Service Fabric.

Restartování uzlu clusteru Service Fabric, která je již spuštěn.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Název uzlu [vyžaduje]| Název uzlu.|
| – Vytvoření výpisu prostředků infrastruktury  | Zadejte hodnotu PRAVDA, chcete-li vytvořit výpis procesu uzlu prostředků infrastruktury. Toto je malá a velká písmena.  Výchozí: False.|
| – id instance uzlu | ID instance cílový uzel. Pokud je zadané instance ID restartování uzlu pouze v případě, že se shoduje s aktuální instancí třídy uzlu. Výchozí hodnota 0, odpovídá žádné ID instance. Instance ID lze získat pomocí get uzlu dotazu.  Výchozí: 0.|
| časový limit – -t       | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění            | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h          | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o        | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu            | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose          | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-node-transition"></a>Přechod sfctl uzlu
Spuštění nebo zastavení uzlu clusteru.

Spuštění nebo zastavení uzlu clusteru.  Uzel clusteru je proces, není instanci OS sám sebe.
Pro spuštění uzlu, předejte "Start" pro parametr NodeTransitionType. K zastavení uzlu, předejte "Stop" pro parametr NodeTransitionType. Toto rozhraní API spustí operaci – když rozhraní API vrátí že uzlu nemusí dokončení přechodu ještě. Volání GetNodeTransitionProgress s stejné OperationId zobrazíte průběh operace. .

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --– instance-id uzlu [vyžaduje]| ID instance uzlu cílový uzel. To se dá určit prostřednictvím rozhraní API GetNodeInfo.|
| – Název uzlu [vyžaduje]| Název uzlu.|
| --uzlu přechod type [vyžaduje]| Určuje typ přechodu k provedení.                       NodeTransitionType.Start spustí zastaven uzlu.                       NodeTransitionType.Stop zastaví uzel, který je v provozu. -Neplatná - vyhrazené.  Nepředávejte do rozhraní API. -Start - přechod zastaven uzlu na nahoru. -Stop - přechod aktuálním uzlu byla zastavena. .|
| – id operace [vyžaduje]| Identifikátor GUID, který identifikuje volání toto rozhraní API.  To je předán do odpovídající GetProgress API.|
| --stop-doba trvání v sekund, které [vyžadují]| Doba trvání v sekundách, aby uzlu zastavena.  Minimální hodnota je 600, maximální se 14400. Po vypršení této doby uzlu automaticky zobrazí zpět.|
| časový limit – -t                      | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                         | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                       | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                       Výchozí: json.|
| --dotazu                           | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                         | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další kroky
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).