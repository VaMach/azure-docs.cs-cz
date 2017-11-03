---
title: "Azure Service Fabric rozhraní příkazového řádku - sfctl cluster | Microsoft Docs"
description: "Popisuje příkazy sfctl clusteru Service Fabric rozhraní příkazového řádku."
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
ms.openlocfilehash: dc2ed59d6adaca97b23dddcb7ec968d90171b483
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-cluster"></a>sfctl clusteru
Vyberte, Správa a provoz clusterů Service Fabric.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    verze kódu| Získá seznam prostředků infrastruktury verze kódu, které jsou zřízené v clusteru Service Fabric.|
|    Konfigurace verze | Získá seznam prostředků infrastruktury verze konfigurace, které jsou zřízené v clusteru Service Fabric.|
|    Stav       | Získá stav clusteru Service Fabric.|
|    Manifest     | Získáte manifestu clusteru Service Fabric.|
|    operace zrušení| Selhání vyvolané uživatel operaci zruší.|
|    operationgit | Získá seznam operací vyvolané uživatele selhání filtrovaná podle zadaného vstupu.|
|    Zřizování     | Zřídit balíčky kód nebo konfigurace clusteru Service Fabric.|
|    obnovení systému  | Do clusteru Service Fabric označuje, že mají pokusit o obnovení systémových služeb, které jsou aktuálně zablokované ve ztrátě kvora.|
|Sestava stavu   | Odešle zprávu o stavu na cluster Service Fabric.|
|    Vyberte       | Připojí se ke clusteru koncový bod Service Fabric.|
| Zrušení zajišťování     | Zrušit zřízení balíčky kód nebo konfigurace clusteru Service Fabric.|
|    upgrade         | Spusťte upgrade verze kódu nebo konfigurace clusteru Service Fabric.|
|    obnovení upgradu  | Proveďte upgrade clusteru přejde k další upgradovací doméně.|
|    vrácení upgradu| Upgrade clusteru Service Fabric se vrátit zpět.|
|    Stav upgradu  | Získá průběh aktuální upgrade clusteru.|
|upgrade aktualizace  | Aktualizujte upgradu parametry upgradu clusteru Service Fabric.|


## <a name="sfctl-cluster-health"></a>sfctl stavu clusteru
Získá stav clusteru Service Fabric.

Získá stav clusteru Service Fabric. EventsHealthStateFilter použijte k filtrování kolekce událostí stavu hlášených v clusteru podle stavu. Podobně, použijte NodesHealthStateFilter a ApplicationsHealthStateFilter k filtrování kolekce uzlů a aplikací vrátil na základě jejich agregovaného stavu. 

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --aplikace stavu stavu filtru| Umožňuje filtrování o stavu aplikace
                                                    objects returned in the result of cluster health
                                                    query based on their health state. The possible
                                                    values for this parameter include integer value
                                                    obtained from members or bitwise operations on
                                                    members of HealthStateFilter enumeration. Only
                                                    applications that match the filter are returned.
                                                    All applications are used to evaluate the
                                                    aggregated health state. If not specified, all
                                                    entries are returned. The state values are flag
                                                    based enumeration, so the value could be a
                                                    combination of these values obtained using
                                                    bitwise 'OR' operator. For example, if the
                                                    provided value is 6 then health state of
                                                    applications with HealthState value of OK (2)
                                                    and Warning (4) are returned. - Default -
                                                    Default value. Matches any HealthState. The
                                                    value is zero. - None - Filter that doesn't
                                                    match any HealthState value. Used in order to
                                                    return no results on a given collection of
                                                    states. The value is 1. - Ok - Filter that
                                                    matches input with HealthState value Ok. The
                                                    value is 2. - Warning - Filter that matches
                                                    input with HealthState value Warning. The value
                                                    is 4. - Error - Filter that matches input with
                                                    HealthState value Error. The value is 8. - All -
                                                    Filter that matches input with any HealthState value. The value is 65535.|
| --události stavu stavu filtru | Umožňuje filtrování vrácených objektů HealthEvent kolekce na základě stavu. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Se vrátí jenom události, které odpovídají filtru. Všechny události se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou příznak - založené na výčtu, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je 6 všechny události s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění.
Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535. | | – statistiky stavu vyloučení | Určuje, zda má být vrácen stav statistiky jako součást výsledků dotazu. Chcete-li hodnotu false ve výchozím nastavení. Statistiku zobrazit počet podřízených entit ve stavu Ok, upozornění a chyby. | |   --zahrnují system--stavu statistik aplikace | Určuje, zda statistiky stavu by měla obsahovat topologie fabric: / statistik stavu aplikace systému. Chcete-li hodnotu false ve výchozím nastavení. Pokud IncludeSystemApplicationHealthStatistics nastavena na hodnotu true, stav statistiky zahrnují entitami, které patří do topologie fabric: / aplikaci systému. Výsledek dotazu obsahuje, jinak hodnota stavu statistiky pouze pro uživatele aplikace. Statistiky stavu musí být součástí výsledek dotazu pro tento parametr má být použita. | | --uzly stavu stavu filtru | Umožňuje filtrování vrácených ve výsledku dotazu stavu clusteru na základě jejich stavu uzlu stavu stavu objektů. Možné hodnoty pro tento parametr patří celočíselná hodnota jednoho z následujících stavů. Vrátí se pouze uzly, které odpovídají filtru. Všechny uzly se používají k vyhodnocení agregovaný stav v pořádku. Pokud není zadaný, jsou vráceny všechny položky. Hodnoty stavu jsou výčet založený na příznak, takže hodnotou může být kombinací tyto hodnoty získané pomocí bitový operátor 'OR'. Například pokud zadaná hodnota je "6" stav uzlů s hodnotou elementu HealthState OK (2) a upozornění (4), jsou vráceny. -Výchozí – výchozí hodnota. Vyhledá všechny stav HealthState. Hodnota je nula. -None - filtr, který se neshoduje se žádnou hodnotu stavu HealthState. Použít cílem vrátit žádné výsledky v dané kolekci stavů. Hodnota je 1. -Ok - filtrujte, aby odpovídá vstup s hodnotou elementu HealthState Ok. Hodnota je 2. -Upozornění - filtr, hodnota odpovídá vstup k elementu HealthState upozornění.
Hodnota je 4. -Chyba – filtr, který odpovídá vstup s hodnotou elementu HealthState chyby. Hodnota je 8. -Všechny - filtr, který odpovídá vstup s libovolnou hodnotou elementu HealthState. Hodnota je 65535. | | časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění                        | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h                      | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o                    | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.                    Výchozí: json.|
| --dotazu                        | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose                      | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-cluster-manifest"></a>sfctl manifestu clusteru
Získáte manifestu clusteru Service Fabric.

Získáte manifestu clusteru Service Fabric. V manifestu clusteru obsahuje vlastnosti clusteru, které obsahují různé typy uzlů v clusteru, konfigurace zabezpečení, odolnost a topologií domény upgradu atd. Tyto vlastnosti jsou zadaný jako součást souboru ClusterConfig.JSON soubor při nasazování clusteru s podporou samostatné. Ale většinu informací v manifestu clusteru je generované interně službou service fabric během nasazení clusteru v jiných scénářích nasazení (například při použití [portál Azure](https://portal.azure.com)). Obsah manifestu clusteru je pouze informativní charakter a uživatelé se nepředpokládá trvat závislost na formát obsah souboru nebo jeho interpretace. 

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| časový limit – -t| Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění  | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h| Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu  | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose| Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-cluster-provision"></a>zřízení clusteru sfctl
Zřídit balíčky kód nebo konfigurace clusteru Service Fabric.

        Validate and provision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
|--clusteru manifest souboru cesta| Cesta k souboru manifestu clusteru.|
|    --cestu souboru kódu            | Cesta souboru clusteru kód balíčku.|
|    časový limit – -t                | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h  | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o| Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose  | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-cluster-select"></a>Vyberte sfctl clusteru
Připojí se ke clusteru koncový bod Service Fabric.

Pokud připojení k zabezpečení clusteru, zadejte certifikát (CRT) a soubor klíče (s příponou Key) nebo jeden soubor s obou (.pem). Nezadávejte i. Volitelně Pokud připojení k zabezpečení clusteru, zadejte také cestu k souboru s kompletem sady certifikační Autority nebo adresáře certifikátů důvěryhodné certifikační Autority.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| --koncový bod [vyžaduje]| Clusteru adresu URL koncového bodu, včetně port a předponu HTTP nebo HTTPS.|
| --aad             | Pro ověřování pomocí služby Azure Active Directory.|
| --certifikační autority              | Cestu k adresáři certifikátů certifikační Autority zacházet s jako platná nebo soubor kompletu certifikační Autority.|
| --certifikátu.            | Cesta k souboru certifikátu klienta.|
| – klíč             | Cesta k souboru klíče certifikátu klienta.|
| – Ověřte ne       | Zakázat ověřování pro certifikáty při použití protokolu HTTPS, Všimněte si: Toto je nezabezpečené možnost a by se neměla používat pro provozní prostředí.|
| --pem             | Cesta k klientský certifikát jako soubor .pem.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění           | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h         | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o       | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu           | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose         | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-cluster-unprovision"></a>unprovision sfctl clusteru
Zrušit zřízení balíčky kód nebo konfigurace clusteru Service Fabric.

        Unprovision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|--verze kódu  | Verze balíčku kódu clusteru.|
|    – Konfigurace verze| Verze manifestu clusteru.|
|    časový limit – -t    | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty
|Argument|Popis|
| --- | --- |
|– ladění         | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
 |   – Nápověda -h       | Zobrazte tuto zprávu nápovědy a ukončení.|
 |   --výstup -o     | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
 |   --dotazu         | Řetězec dotazu JMESPath. Http://jmespath.org/ Další informace najdete v části a
                      Příklady.|
 |   -verbose       | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|


## <a name="sfctl-cluster-upgrade"></a>upgrade clusteru sfctl
Spusťte upgrade verze kódu nebo konfigurace clusteru Service Fabric.

        Validate the supplied upgrade parameters and start upgrading the code or configuration
        version of a Service Fabric cluster if the parameters are valid.

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|    --aplikaci. stav mapy                      | Slovník JSON kódovaný dvojic název aplikace a
                                            maximální procento není v pořádku, než se vyvolá chybu.|
 |   – aplikace typu stavu mapy                 | JSON kódovaný slovník párů typu aplikace
                                            name and maximum percentage unhealthy before raising
                                            error.|
 |   --verze kódu | Verze kódu clusteru. | |   – Konfigurace verze | Verze konfigurace clusteru. | |   --vyhodnocení stavu rozdílové | Umožňuje vyhodnocení stavu rozdílové místo zkušební verzi absolutní stavu po dokončení každé upgradované domény. | |   --rozdílů není v pořádku uzly | Maximální povolené procento uzlů snížení stavu během upgradu clusteru povoleny.  Výchozí: 10.
Mezi stav uzlů na začátku upgradu a stav uzlů v době vyhodnocení stavu se měří delta. Kontrola se provádí po dokončení upgradu každé upgradované domény globální stav clusteru je v mezích. povolená. | |   --selhání akce | Možné hodnoty patří: "Neplatná',"Vrácení","Ruční". | |   --Vynutit restartování | Vynutit restartování. | |   --stavu. Zkontrolujte opakování | Časový limit opakování kontroly stavu měřeno v milisekundách. | |   --stavu. Zkontrolujte nestabilním | Kontrola stavu stabilní trvání měřeno v milisekundách. | |  –--Čekání na kontrolu stavu | Doba v milisekundách čekání na kontrolu stavu. | |  --repliky set kontrola-časový limit | Upgrade repliky nastavit časový limit kontroly, které měří v sekundách. | |   --vrácení režimu upgradu | Možné hodnoty patří: "Neplatná', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorované'.  Výchozí hodnota: UnmonitoredAuto. | |  časový limit – -t | Server časový limit v sekundách.  Výchozí: 60. | |  --není v pořádku aplikace | Maximální povolené procento žádostí, není v pořádku, než ohlásí chybu.
Například povolit 10 % aplikací jako chybný, tato hodnota je 10. Procento představuje maximální procento. povolená aplikací, které může být není v pořádku, než clusteru považován za chybu. Pokud je dodržena procento, ale existuje alespoň jedna aplikace není v pořádku, stav budou vyhodnocené jako varování. To se vypočítá jako podíl počtu není v pořádku aplikace přes celkový počet instancí aplikace v clusteru, s výjimkou aplikace typy aplikací, které jsou součástí ApplicationTypeHealthPolicyMap. Výpočet zaokrouhlí na tolerovat selhání jednoho na malý počet aplikací. | |   --není v pořádku uzly | Maximální povolené procento není v pořádku uzlů než ohlásí chybu.
Například povolit 10 % uzlů jako chybný, tato hodnota je 10. Procento představuje maximální procento. povolená uzlů, které může být není v pořádku, než clusteru považován za chybu. Pokud je dodržena procento, ale je alespoň jeden uzel a není v pořádku, stav budou vyhodnocené jako varování. Procentuální se vypočítá jako podíl počtu uzlů není v pořádku přes celkový počet uzlů v clusteru. Výpočet zaokrouhlí na tolerovat jeden selhání na malém počtu uzlů. Ve velkých clusterech, některé uzly bude vždy mimo provoz nebo se pro opravy, takže toto procento by měl být nakonfigurovaný pro tolerovat možnost, která. | |   --upgradu--delta-není v pořádku – uzly domény | Maximální povolené procento uzly domény upgradu snížení stavu během upgradu clusteru povoleny.
Výchozí: 15.
Mezi uzly domény upgradu na začátku upgrade stav a stav uzly domény upgradu v době vyhodnocení stavu se měří delta. Kontrola se provádí po dokončení upgradu každé upgradované domény pro všechny dokončit, ujistěte se, stav upgradu domény upgradu domén. povolená omezení. | |   – časový limit upgradu domény | Časový limit domény upgradu měřeno v milisekundách. | |   – upgrade vypršení časového limitu | Časový limit upgradu měřeno v milisekundách. | |   --upozornění jako chyby | Upozornění jsou zachází se stejnou závažnost jako chyby. |

### <a name="global-arguments"></a>Globální argumenty
    |Argument|Popis|
| --- | --- |
|– ladění                               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
|    – Nápověda -h                             | Zobrazte tuto zprávu nápovědy a ukončení.|
|    --výstup -o                           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.
                                            Výchozí: json.|
|    --dotazu                               | Řetězec dotazu JMESPath. Další informace najdete v části http://jmespath.org/
                                            informace a příklady.|
|    -verbose                             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění
                                            protokoly.|

## <a name="next-steps"></a>Další kroky
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).