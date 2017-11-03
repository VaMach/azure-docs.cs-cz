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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: dc57c813a6aecabc21ac3931b7294bce909778d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-application"></a>sfctl aplikace
Vytvoření, odstranění a spravovali aplikace a typy aplikací.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
| vytvoření       | Vytvoří aplikace Service Fabric pomocí zadaný popis.|
| Odstranit       | Odstraní stávající aplikace Service Fabric.|
| Nasazení     | Získá informace o aplikace nasazené na uzlu Service Fabric.|
| nasazení stavu | Získá informace o stavu aplikace nasazené na služby
                      Uzel topologie Fabric.|
| nasazení seznamu| Získá seznam aplikace nasazené na uzlu Service Fabric.|
| Stav       | Získá stav aplikace service fabric.|
| Informace o         | Získá informace o aplikace Service Fabric.|
| seznam         | Získá seznam aplikace vytvořené v clusteru Service Fabric, které odpovídají
                      filtry zadána jako parametr.|
| načítání | Získá načíst informace o aplikaci Service Fabric. |
| Manifest     | Získá manifest popisující typ aplikace.|
| Zřizování    | Zřizuje nebo zaregistruje typ aplikace Service Fabric s clusterem.|
| Sestava stavu| Odešle zprávu o stavu na aplikace Service Fabric.|
| type         | Získá seznam typů aplikací v odpovídajícím clusteru Service Fabric
                      přesně zadaný název.|
| seznam typů    | Získá seznam typů aplikací v clusteru Service Fabric.|
| Zrušení zajišťování  | Odebere nebo zrušení registrace typu aplikace Service Fabric z clusteru.|
| upgrade      | Spustí se upgrade aplikace v clusteru Service Fabric.|
| obnovení upgradu  | Obnoví upgrade aplikace v clusteru Service Fabric.|
| vrácení upgradu| Spustí vrácení zpět aktuálně probíhající upgradu aplikace
                      Cluster Service Fabric.|
| Stav upgradu  | Získá informace pro upgrade na nejnovější provést na tuto aplikaci.|
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
| – ladění              | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| --remove silou | Odebrání aplikace Service Fabric nebo služby vynuceně bez průchodu přes pořadí řádné vypnutí. Tento parametr slouží k vynuceně odstranit aplikace nebo služby, pro které odstranění je řádně vypršení časového limitu z důvodu problémů v kódu služby, která zabraňuje zavřete replik. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h               | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o             | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu                 | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose               | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-deployed"></a>sfctl aplikace nasazená
Získá informace o aplikace nasazené na uzlu Service Fabric.|
|     
### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| – Název uzlu [vyžaduje] | Název uzlu. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0,
                                                 hierarchical names are delimited with the "~"
                                                 character. For example, if the application name is
                                                 "fabric://myapp/app1", the application identity
                                                 would be "myapp~app1" in 6.0+ and "myapp/app1" in
                                                 previous versions.|
| – nasazení aplikace – stavu stavu filtru | Umožňuje filtrování stavu objektů nasazené aplikace stavu vrátil ve výsledku dotazu stavu aplikace na základě jejich stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Pouze nasazené aplikace, které odpovídají filtru, bude vrácen. Všechny nasazené aplikace se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'.                        Například pokud zadaná hodnota je 6 stav nasazených aplikací s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState.                        Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů.                        Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535. | | --události stavu stavu filtru | Umožňuje filtrování vrácených objektů HealthEvent kolekce na základě stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Se vrátí jenom události, které odpovídají filtru. Všechny události se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky.                        Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535. | | --statistiky stavu vyloučení | Určuje, zda má být vrácen stav statistiky jako součást výsledků dotazu. Chcete-li hodnotu false ve výchozím nastavení. Statistiku zobrazit počet podřízených entit ve stavu Ok, upozornění a chyby. | | --služby stavu stavu filtru | Umožňuje filtrování stavu objektů služby stavu vrátil ve výsledku dotazu stavu služeb na základě jejich stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Vrátí se pouze služby, které odpovídají filtru. Všechny služby se používají k vyhodnocení agregovaný stav v pořádku.                        Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 pak stavu služeb s hodnotou elementu HealthState OK (2) a upozornění (4), bude vrácen. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula.                        -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění. Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                                 | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| – id aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, jsou hierarchické oddělených středníky
                                      with the "~" character. For example, if the application name
                                      is "fabric://myapp/app1", the application identity would be
                                      "myapp~app1" in 6.0+ and "myapp/app1" in previous versions.|
| --Parametry vyloučení aplikace | Příznak, který určuje, zda aplikace parametry budou vyloučeny z výsledku. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                      | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
|--aplikace definice typ filtru| Použít k filtrování na ApplicationDefinitionKind pro
                                          application query operations. - Default - Default value.
                                          Filter that matches input with any
                                          ApplicationDefinitionKind value. The value is 0. - All -
                                          Filter that matches input with any
                                          ApplicationDefinitionKind value. The value is 65535. -
                                          ServiceFabricApplicationDescription - Filter that matches
                                          input with ApplicationDefinitionKind value
                                          ServiceFabricApplicationDescription. The value is 1. -
                                          Compose - Filter that matches input with
                                          ApplicationDefinitionKind value Compose. The value is 2.
                                          Default: 65535.|
| – Název typu aplikace | Název typu aplikace použít k filtrování aplikace, které chcete vyhledat. Tato hodnota by neměla obsahovat verze typu aplikace. | | --token pokračování | Parametr token pokračování slouží k získání další sadu výsledků. Token pokračování s hodnotou neprázdné je zahrnutý v odpovědi rozhraní API, když výsledky ze systému nelze uložit do odpověď o jedné. Pokud je tato hodnota předaná pro další volání rozhraní API, rozhraní API vrátí další sadu výsledků. Pokud nejsou žádné další výsledky, pak token pro pokračování neobsahuje hodnotu. Hodnota tohoto parametru by neměla být kódovaná adresou URL. | | --Parametry vyloučení aplikace | Příznak, který určuje, zda aplikace parametry jsou vyloučeny z výsledku. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                      | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                    | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                  | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.             Výchozí: json.|
| --dotazu                      | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                    | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-application-load"></a>zatížení sfctl aplikace
Získá načíst informace o aplikaci Service Fabric.

        Returns the load information about the application that was created or in the process of
        being created in the Service Fabric cluster and whose name matches the one specified as the
        parameter. The response includes the name, minimum nodes, maximum nodes, the number of nodes
        the app is occupying currently, and application load metric information about the
        application.

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|– id aplikace [vyžaduje]| Identita aplikace. To je obvykle úplný název
                                 the application without the 'fabric:' URI scheme. Starting from
                                 version 6.0, hierarchical names are delimited with the "~"
                                 character. For example, if the application name is
                                 "fabric://myapp/app1", the application identity would be
                                 "myapp~app1" in 6.0+ and "myapp/app1" in previous versions. |
| časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty
|Argument|Popis|
| --- | --- |
|– ladění                    | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
    – Nápověda -h                  | Zobrazte tuto zprávu nápovědy a ukončení.|
    --výstup -o                | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí hodnota:
                                 JSON.|
    --dotazu                    | Řetězec dotazu JMESPath. Další informace najdete v části http://jmespath.org/
                                 informace a příklady.|
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
| – ladění                           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| – ladění                              | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| --vyloučení parametry aplikace  | Příznak, který určuje, zda aplikace parametry budou vyloučeny z výsledek.|
| – maximální počet výsledků                  | Maximální počet výsledků, které má být vrácen jako součást stránkové dotazy. Tento parametr určuje horní mez počtu výsledků vrácených. Vráceny výsledky mohou být menší než zadaná maximální výsledky, pokud nebudou vyhovovat ve zprávě podle omezení velikosti maximální počet zpráv definované v konfiguraci. Pokud tento parametr je nulová nebo není zadaný, stránkové dotaz obsahuje tolik výsledky jako možné, že se nevešla návratové zprávy.|
| časový limit – -t                   | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                        | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| – ladění                           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
| id – aplikace [vyžaduje]| Identita aplikace. Toto je obvykle úplný název aplikace bez ' prostředků infrastruktury:' schéma identifikátoru URI. Od verze 6.0, hierarchické oddělených středníky s ' ~' znak. Pro
        example, if the application name is 'fabric://myapp/app1', the application identity would be
        'myapp~app1' in 6.0+ and 'myapp/app1' in previous versions.|
| verze – aplikace [vyžaduje] | Cílová verze aplikací. | | --parametrů [požadovaných] | Přepsání JSON kódovaný seznam parametr aplikace má být použita při upgradu aplikace. | | --výchozí služby stavu zásady | JSON kódovaný specifikace zásad stavu, ve výchozím nastavení použit k vyhodnocení stavu typu služby. | | --selhání akce | Akce se provede při upgradu monitorované zaznamená monitorování zásad nebo stavu porušení zásad. | | --Vynutit restartování | Vynuceně restartování procesů během upgradu, i když verze kódu nebylo změněno. | | --stavu kontrola opakování limitu | Množství času opakovat hodnocení stavu, pokud je není v pořádku, než akce selhání aplikace nebo clusteru je spustit. Měří v milisekundách.  Výchozí hodnota: PT0H10M0S. | | --stavu kontrola stabilní trvání | Množství času, aby aplikace nebo clusteru musí zůstat v pořádku před upgradem k další upgradovací doméně.            Měří v milisekundách.  Výchozí hodnota: PT0H2M0S. | | --stavu kontrola čekací doba | Množství času čekání po dokončení upgradu domény před použitím zásad stavu. Měří v milisekundách.            Výchozí hodnota: 0. | | --max-není v pořádku apps | Maximální povolené procento není v pořádku nasazené aplikace. Reprezentován jako číslo mezi 0 a 100. | | --režimu | Režim použitý pro sledování stavu během postupného upgradu.            Výchozí hodnota: UnmonitoredAuto. | | --repliky set kontrola-časový limit | Maximální množství času blokovat zpracování upgradu domény a zabránit ztrátě dostupnosti po neočekávaným problémům. Měří v sekundách. | | – zásady stavu služby | JSON kódovaný mapa s zásady stavu typu služby za název typu služby. Mapy je prázdný být výchozí. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. | | – časový limit upgradu domény | Množství času každé upgradované domény musí dokončit před provedením FailureAction. Měří v milisekundách.  Výchozí hodnota: P10675199DT02H48M05.4775807S. | | – upgrade vypršení časového limitu | Množství času celkové upgrade musí dokončit před provedením FailureAction. Měří v milisekundách.  Výchozí hodnota: P10675199DT02H48M05.4775807S. | | --upozornění jako chyby | Vyhodnocení upozornění stavu s stejné závažnost považovat za chyby. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                     | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
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
|úložiště bitových kopií – řetězec| Cílové image úložiště pro nahrání balíčku aplikace.  Výchozí hodnota:
                         úložiště prostředků infrastruktury: s bitových kopií.|
| – Zobrazí průběh  | Ukázat průběh nahrávání souboru pro velké balíčky.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění       | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h     | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o   | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu       | Řetězec dotazu JMESPath. Http://jmespath.org/ Další informace najdete v části a
                       Příklady.|
| -verbose     | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další kroky
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).