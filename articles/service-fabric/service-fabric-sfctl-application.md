---
title: "Aplikace Azure Service Fabric rozhraní příkazového řádku - sfctl | Microsoft Docs"
description: "Popisuje příkazy sfctl aplikace Service Fabric rozhraní příkazového řádku."
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
ms.date: 12/22/2018
ms.author: ryanwi
ms.openlocfilehash: 345717e76097931f52354369e822af41133b34f0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="sfctl-application"></a>sfctl application
Vytvoření, odstranění a spravovali aplikace a typy aplikací.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
| vytvoření       | Vytvoří aplikace Service Fabric pomocí zadaný popis.|
| odstraňovat       | Odstraní stávající aplikace Service Fabric.|
| Nasazení     | Získá informace o aplikace nasazené na uzlu Service Fabric.|
| nasazení stavu | Získá informace o stavu aplikace nasazené na uzlu Service Fabric.|
| nasazení seznamu| Získá seznam aplikace nasazené na uzlu Service Fabric.|
| stav       | Získá stav aplikace service fabric.|
| Informace o         | Získá informace o aplikace Service Fabric.|
| Seznam         | Získá seznam aplikace vytvořené v clusteru Service Fabric, které odpovídají filtru zadaném jako parametr.|
| načítání | Získá načíst informace o aplikaci Service Fabric. |
| Manifest     | Získá manifest popisující typ aplikace.|
| Zřizování    | Zřizuje nebo zaregistruje typ aplikace Service Fabric s clusterem.|
| report-health| Odešle zprávu o stavu na aplikace Service Fabric.|
| type         | Získá seznam typů aplikací v clusteru Service Fabric odpovídající zadanému názvu.|
| seznam typů    | Získá seznam typů aplikací v clusteru Service Fabric.|
| Zrušení zajišťování  | Odebere nebo zrušení registrace typu aplikace Service Fabric z clusteru.|
| upgrade      | Spustí se upgrade aplikace v clusteru Service Fabric.|
| upgrade-resume  | Obnoví upgrade aplikace v clusteru Service Fabric.|
| upgrade-rollback| Spustí se vracení zpět aktuálně probíhající upgradu aplikace v clusteru Service Fabric.|
| upgrade-status  | Získá informace pro upgrade na nejnovější provést na tuto aplikaci.|
| Nahrávání       | Zkopírujte balíček aplikace Service Fabric do úložiště bitové kopie.|

## <a name="sfctl-application-create"></a>Vytvoření aplikace sfctl
Vytvoří aplikace Service Fabric pomocí zadaný popis.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| název – aplikace [vyžaduje]| Název aplikace, včetně "fabric:' schéma identifikátoru URI.|
| Typ – aplikace [vyžaduje]| Název typu aplikace najít v manifestu aplikace.|
| verze – aplikace [vyžaduje]| Verze typu aplikace, jak jsou definovány v manifestu aplikace.|
| – maximální počet uzlů     | Maximální počet uzlů, že Service Fabric rezervuje kapacity pro tuto aplikaci. To neznamená, že služby tato aplikace se umístí na všechny tyto uzly.|
| --metriky            | JSON kódovaný seznam popisů aplikace kapacity metriky. Metrika je definován jako název, související se sadou kapacity pro každý uzel, který aplikace existuje v.|
| – minimální počet uzlů     | Minimální počet uzlů, že Service Fabric rezervuje kapacity pro tuto aplikaci. To neznamená, že služby tato aplikace se umístí na všechny tyto uzly.|
| – Parametry         | Seznam JSON kódovaný aplikace parametr přepsání má být použita při vytváření aplikace.|
| časový limit – -t         | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug              | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h            | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o          | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu              | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose            | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-delete"></a>odstranění aplikace sfctl
Odstraní stávající aplikace Service Fabric.

Odstraní stávající aplikace Service Fabric. Aplikace musí být vytvořeny, než mohl být odstraněn. Odstranění aplikace dojde k odstranění všech služeb, které jsou součástí této aplikace. Ve výchozím nastavení pokusí Service Fabric v řádně ukončit službu repliky a pak odstraňte službu. Ale pokud služba je s problémy řádně zavření repliky, může trvat dlouhou dobu operace odstranění nebo zablokuje. Použijte volitelné příznak ForceRemove přeskočit řádné zavření sekvence a vynuceně odstranit aplikaci a všechny jeho služby.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například pokud je název aplikace "fabric://myapp/app1", identita aplikace by být "Moje aplikace ~ app1" v 6.0 + a "myapp/app1" v předchozích verzích.|
| --force-remove          | Odebrání aplikace Service Fabric nebo služby vynuceně bez průchodu přes pořadí řádné vypnutí. Tento parametr slouží k vynuceně odstranit aplikace nebo služby, pro které odstranění je řádně vypršení časového limitu z důvodu problémů v kódu služby, která zabraňuje ukončení repliky.|
| časový limit – -t            | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h               | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o             | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                 | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose               | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-deployed"></a>sfctl aplikace nasazená
Získá informace o aplikace nasazené na uzlu Service Fabric.
     
### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například pokud je název aplikace "fabric://myapp/app1", identita aplikace by být "Moje aplikace ~ app1" v 6.0 + a "myapp/app1" v předchozích verzích.|
| – Název uzlu [vyžaduje]| Název uzlu.|
| časový limit – -t            | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h               | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o             | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                 | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose               | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-health"></a>Stav sfctl aplikací
Získá stav aplikace service fabric.

Vrátí stav stavu aplikace service fabric. Odpověď hlásí stav Ok, chyby nebo upozornění. Pokud entita nebyla nalezena v health store, vrátí chybu.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například pokud je název aplikace "fabric://myapp/app1", identita aplikace by být "Moje aplikace ~ app1" v 6.0 + a "myapp/app1" v předchozích verzích.|
| --deployed-applications-health-state-filter| Umožňuje filtrování stavu objektů nasazené aplikace stavu vrátil ve výsledku dotazu stavu aplikace na základě jejich stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Pouze nasazené aplikace, které odpovídají filtru, bude vrácen. Všechny nasazené aplikace se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 stav nasazených aplikací s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535.|
| --events-health-state-filter            | Umožňuje filtrování vrácených objektů HealthEvent kolekce na základě stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Se vrátí jenom události, které odpovídají filtru. Všechny události se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535.|
| --exclude-health-statistics | Určuje, zda má být vrácen stav statistiky jako součást výsledků dotazu. Chcete-li hodnotu false ve výchozím nastavení. Statistiku zobrazují počet podřízených entit ve stavu Ok, upozornění a chyby.|
| --services-health-state-filter          | Umožňuje filtrování stavu objektů služby stavu vrátil ve výsledku dotazu stavu služeb na základě jejich stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Vrátí se pouze služby, které odpovídají filtru. Všechny služby se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 pak stavu služeb s hodnotou elementu HealthState OK (2) a upozornění (4), bude vrácen. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535.|
| časový limit – -t                            | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                               | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                             | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                                 | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                               | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-info"></a>informace o aplikaci sfctl
Získá informace o aplikace Service Fabric.

Vrací informace o aplikaci, která byla vytvořena nebo právě probíhá vytváření clusteru Service Fabric a jejíž název odpovídá zadanému jako parametr. Odpověď obsahuje název, typ, stav, parametry a další podrobnosti o aplikaci.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například pokud je název aplikace "fabric://myapp/app1", identita aplikace by být "Moje aplikace ~ app1" v 6.0 + a "myapp/app1" v předchozích verzích.|
| --exclude-application-parameters| Příznak, který určuje, zda aplikace parametry budou vyloučeny z výsledek.|
| časový limit – -t                 | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                      | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                    | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                  | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.             Výchozí: json.|
| --dotazu                      | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                    | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-list"></a>seznam aplikací sfctl
Získá seznam aplikace vytvořené v clusteru Service Fabric, které odpovídají filtru zadaném jako parametr.

Získá informace o aplikacích, které byly vytvořeny nebo právě probíhá vytváření v Service Fabric clusteru a splňují filtrů zadána jako parametr. Odpověď obsahuje název, typ, stav, parametry a další podrobnosti o aplikaci. Pokud aplikace na stránce nebudou vyhovovat, vrátí se jeden stránky s výsledky a také pokračovací token, který můžete použít k získání na další stránku.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
|--aplikace definice typ filtru| Použít pro filtrování ApplicationDefinitionKind pro operace dotazů aplikace. -Výchozí – výchozí hodnota. Filtr, který odpovídá vstup s libovolnou hodnotou ApplicationDefinitionKind. Hodnota je 0. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou ApplicationDefinitionKind. Hodnota je 65535. -ServiceFabricApplicationDescription - filtr, který odpovídá vstup s hodnotou ApplicationDefinitionKind ServiceFabricApplicationDescription. Hodnota je 1. -Vytvářené - filtr, který odpovídá vstup s hodnotou ApplicationDefinitionKind vytvářené. Hodnota je 2. Výchozí: 65535.|
| --application-type-name      | Název typu aplikace použít k filtrování aplikace, které chcete vyhledat. Tato hodnota by neměla obsahovat verze typu aplikace.|
| --token pokračování         | Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky, pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| --exclude-application-parameters| Příznak, který určuje, zda aplikace parametry jsou vyloučeny z výsledek.|
| časový limit – -t                 | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                      | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                    | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                  | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.             Výchozí: json.|
| --dotazu                      | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                    | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-load"></a>zatížení sfctl aplikace
Získá načíst informace o aplikaci Service Fabric.

Vrací zatížení informace o aplikaci, která byla vytvořena nebo právě probíhá vytváření clusteru Service Fabric a jejíž název odpovídá zadanému jako parametr. Odpověď obsahuje název, uzly minimální, maximální počet uzlů, počet uzlů, ke kterému je aktuálně zabírá aplikace a aplikace zatížení metriky informace o aplikaci.

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|– id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s "~" znak. Například pokud je název aplikace "fabric://myapp/app1", identita aplikace by být "Moje aplikace ~ app1" v 6.0 + a "myapp/app1" v předchozích verzích. |
| časový limit – -t               | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty
|Argument|Popis|
| --- | --- |
|--debug                    | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
    – Nápověda -h                  | Zobrazte tuto zprávu nápovědy a ukončení.|
    --výstup -o                | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
    --dotazu                    | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
    -verbose                  | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-manifest"></a>manifest aplikace sfctl
Získá manifest popisující typ aplikace.
        
Získá manifest popisující typ aplikace. Odpověď obsahuje manifest aplikace XML jako řetězec.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --– Název typu aplikace – [vyžaduje]| Název typu aplikace.|
| --– verze typu aplikace – [vyžaduje]| Verze typu aplikace.|
| časový limit – -t                      | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                         | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                       | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                  Výchozí: json.|
| --dotazu                           | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                         | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-provision"></a>zřídit aplikace sfctl
Zřizuje nebo zaregistruje typ aplikace Service Fabric s clusterem.
        
Zřizuje nebo zaregistruje typ aplikace Service Fabric s clusterem. To je potřeba, než může být vytvořena instance všechny nové aplikace.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --aplikace typ sestavení path [vyžaduje]| Cesta relativní image store balíčku aplikace.|
| časový limit – -t                         | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                              | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                            | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                          | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                              | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                            | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-type"></a>Typ aplikace sfctl

Získá seznam typů aplikací v clusteru Service Fabric odpovídající zadanému názvu.

Vrací informace o typech aplikací, které jsou zřízené nebo právě probíhá zřizování v clusteru Service Fabric. Tyto výsledky se typy aplikací, jejíž název odpovídá přesně jeden zadaný jako parametr, a které jsou v souladu s parametry daný dotaz. Každá verze vrácena jako jeden typ aplikace jsou vráceny všechny verze aplikace typ odpovídající názvu typu aplikace. Odpověď obsahuje název, verzi, stav a další podrobnosti o typu aplikace. Toto je stránkové dotaz, což znamená, není-li všechny typy aplikací nevejde na stránce se vrátí jednu stránku výsledků a také pokračovací token, který můžete použít k získání na další stránku. Například pokud existují 10 typy aplikací, ale na stránce odpovídá pouze první typy aplikací 3 nebo maximální počet výsledků je nastaven na 3, 3 je vracen. Pro přístup k zbytek výsledky se načtěte následující stránky s použitím token vrácený pokračování v dalším dotazu. Token prázdný pokračování je vrácena, pokud nejsou žádné další stránky.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --– Název typu aplikace – [vyžaduje]| Název typu aplikace.|
| --token pokračování           | Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky, pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL.|
| --exclude-application-parameters  | Příznak, který určuje, zda aplikace parametry budou vyloučeny z výsledek.|
| --max-results                  | Maximální počet výsledků, které má být vrácen jako součást stránkové dotazy. Tento parametr určuje horní mez počtu výsledků vrácených. Vráceny výsledky mohou být menší než zadaná maximální výsledky, pokud nebudou vyhovovat ve zprávě podle omezení velikosti maximální počet zpráv definované v konfiguraci. Pokud tento parametr je nulová nebo není zadaný, stránkové dotaz obsahuje tolik výsledky jako možné, že se nevešla návratové zprávy.|
| časový limit – -t                   | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                        | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                      | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                    | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.               Výchozí: json.|
| --dotazu                        | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                      | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-unprovision"></a>unprovision sfctl aplikace
Odebere nebo zrušení registrace typu aplikace Service Fabric z clusteru.

Odebere nebo zrušení registrace typu aplikace Service Fabric z clusteru. Tuto operaci může provést pouze v případě všechny instance aplikace typu aplikace byla odstraněna. Po zrušení registrace typu aplikace vytvářet žádné nové instance aplikace pro tento typ konkrétní aplikace.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --– Název typu aplikace – [vyžaduje]| Název typu aplikace.|
| --– verze typu aplikace – [vyžaduje]| Verze typu aplikace.|
| časový limit – -t                      | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                         | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                       | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                  Výchozí: json.|
| --dotazu                           | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                         | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-upgrade"></a>upgrade aplikace sfctl
Spustí se upgrade aplikace v clusteru Service Fabric.

Ověří upgradu parametry zadané aplikace a spustí upgradu aplikace, pokud jsou parametry platné. Upozorňujeme, že upgrade popis nahradí existující popis aplikace. To znamená, že pokud nejsou zadané parametry, parametrech existující aplikace, budou přepsány seznamu prázdné parametry. Výsledkem je aplikace pomocí výchozí hodnoty parametrů z manifestu aplikace.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| id – aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s ' ~' znak. Například pokud je název aplikace, fabric://myapp/app1', identita aplikace by být "Moje aplikace ~ app1 se u 6.0 + a ' myapp/app1' v předchozích verzích.|
| verze – aplikace [vyžaduje]| Cílová verze aplikace.|
| --parametrů [požadovaných]| Seznam JSON kódovaný aplikace parametr přepsání použijí při upgradu aplikace.|
| --default-service-health-policy| JSON kódovaný specifikace zásad stavu, ve výchozím nastavení použit k vyhodnocení stavu typu služby.|
| --selhání akce            | Akce se provede při upgradu monitorované zaznamená monitorování zásad nebo stavu porušení zásad.|
| --Vynutit restartování             | Vynuceně restartování procesů během upgradu, i když verze kódu nebylo změněno.|
| --health-check-retry-timeout| Množství času opakovat hodnocení stavu, pokud je není v pořádku, než akce selhání aplikace nebo clusteru je spustit. Měří v milisekundách.  Výchozí: PT0H10M0S.|
| --stavu kontrola stabilní trvání | Množství času, aby aplikace nebo clusteru musí zůstat v pořádku před upgradem k další upgradovací doméně.            Měří v milisekundách.  Výchozí: PT0H2M0S.|
| --health-check-wait-duration| Množství času čekání po dokončení upgradu domény před použitím zásad stavu. Měří v milisekundách.            Výchozí: 0.|
| --max-unhealthy-apps        | Maximální povolené procento není v pořádku nasazené aplikace. Reprezentován jako číslo mezi 0 a 100.|
| --režimu                      | Režim použitý pro sledování stavu během postupného upgradu.            Výchozí: UnmonitoredAuto.|
| --replica-set-check-timeout | Maximální množství času blokovat zpracování upgradu domény a zabránit ztrátě dostupnosti po neočekávaným problémům. Měří v sekundách.|
| --service-health-policy     | JSON kódovaný mapa s zásady stavu typu služby za název typu služby. Mapy je prázdný být výchozí.|
| časový limit – -t                | Server časový limit v sekundách.  Výchozí: 60.|
| --upgrade-domain-timeout    | Množství času každé upgradované domény musí dokončit před provedením FailureAction. Měří v milisekundách.  Výchozí: P10675199DT02H48M05.4775807S.|
| – upgrade vypršení časového limitu           | Množství času celkové upgrade musí dokončit před provedením FailureAction. Měří v milisekundách.  Výchozí: P10675199DT02H48M05.4775807S.|
| --warning-as-error          | Vyhodnocení upozornění stavu s stejné závažnost považovat za chyby.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug                     | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                   | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                 | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.            Výchozí: json.|
| --dotazu                     | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                   | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-upload"></a>Nahrání aplikace sfctl
Zkopírujte balíček aplikace Service Fabric do úložiště bitové kopie.

Volitelně můžete zobrazte průběhu odesílání pro každý soubor v balíčku. Nahrát posílá průběh `stderr`.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Cesta [vyžaduje]| Cesta k balíčku místní aplikace.|
|úložiště bitových kopií – řetězec| Cílové image úložiště pro nahrání balíčku aplikace.  Výchozí hodnota: úložiště bitových kopií: prostředků infrastruktury.|
| – Zobrazí průběh  | Ukázat průběh nahrávání souboru pro velké balíčky.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --debug       | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h     | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o   | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu       | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose     | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další postup
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).
