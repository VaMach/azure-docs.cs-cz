---
title: "Přehled životního cyklu Azure Service Fabric spolehlivé služby | Microsoft Docs"
description: "Další informace o různých životního cyklu události v Service Fabric spolehlivé služby"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Přehled životního cyklu spolehlivé služby
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java v Linuxu](service-fabric-reliable-services-lifecycle-java.md)
>
>

Pokud přemýšlíte o životních spolehlivé služeb, jsou nejdůležitější základní informace o životním cyklu. Obecně platí:

* Při spuštění
  * Služby se vytvářejí.
  * Mají možnost pro vytvoření a návrat nula nebo více – moduly naslouchání
  * Jsou otevřené žádné vrácený naslouchací procesy, povolení komunikace se službou
  * Služby runAsync metoda je volána, což službu, kterou chcete provést dlouho spuštěná nebo práce na pozadí
* Při vypnutí
  * Token zrušení předaný runAsync se zruší, a naslouchací procesy jsou uzavřeny.
  * Po který je dokončena, je destructed samotného objektu služby

Nejsou podrobnosti kolem řazení tyto události. Konkrétně pořadí událostí se může v závislosti na tom, jestli je spolehlivá služba Stateless nebo Stateful mírně změnit. Kromě toho pro stavové služby, máme scénář primární záměny řešit. Během tohoto pořadí roli primární se přenese do jiné repliky (nebo, je vrácen) bez vypnutí služby. Nakonec máme vezměte v úvahu podmínek chyb nebo selhání.

## <a name="stateless-service-startup"></a>Spuštění bezstavové služby
Životní cyklus bezstavové služby je poměrně jednoduché. Tady je pořadí událostí:

1. Služba je vytvořená.
2. Potom v paralelní dvě věci situace:
    - `StatelessService.createServiceInstanceListeners()`je voláno, a jsou otevřené žádné vrácený naslouchací procesy (`CommunicationListener.openAsync()` se volá na každý naslouchací proces)
    - Metodě runAsync služby (`StatelessService.runAsync()`) se nazývá
3. Pokud je k dispozici, je volána metoda onOpenAsync služby vlastní (konkrétně `StatelessService.onOpenAsync()` je volána. Toto je neobvyklé přepsání ale je k dispozici).

Je důležité si uvědomit, že neexistuje žádná řazení mezi volání k vytváření a otevírání naslouchací procesy a runAsync. Před zahájením runAsync, může se otevřít naslouchací procesy. Podobně runAsync může stát volána před naslouchací procesy komunikace jsou otevřené nebo i byla vytvořená. Pokud žádné synchronizace je vyžadována, je ponechán jako cvičení na implementátor. Běžná řešení:

* Někdy naslouchací procesy nemůže fungovat, dokud některé další informace se vytvoří nebo fungovat Hotovo. Pro bezstavové služby, které pracovní obvykle stačí v konstruktoru služby během `createServiceInstanceListeners()` volat, nebo jako součást konstrukce naslouchací proces sám sebe.
* Kód v runAsync někdy není chcete spustit, dokud posluchače jsou otevřené. V takovém případě je nutné další spolupráce. Běžným řešením je některé příznak v rámci naslouchací procesy, která udává, kdy nebude dokončena, který se změnami runAsync před pokračováním na samotnou práci.

## <a name="stateless-service-shutdown"></a>Vypnutí bezstavové služby
Při vypínání bezstavové služby, je pak stejného vzoru, právě obráceným:

1. Paralelní
    - Zavřeny všechny otevřené naslouchací procesy (`CommunicationListener.closeAsync()` se volá na každý naslouchací proces)
    - Token zrušení předaný `runAsync()` zrušeny (Kontrola token zrušení `isCancelled` vlastnost vrací hodnotu true a pokud je token volána `throwIfCancellationRequested` metoda vrátí `CancellationException`)
2. Jednou `closeAsync()` dokončení na každý naslouchací proces a `runAsync()` také dokončení služby `StatelessService.onCloseAsync()` metoda je volána, pokud je k dispozici (znovu jde neobvyklé přepsání).
3. Po `StatelessService.onCloseAsync()` dokončí, je destructed objektu služby

## <a name="stateful-service-startup"></a>Spuštění stavové služby
Stavová služba mají podobný Princip pro bezstavové služby s několik změn. Pořadí událostí pro spuštění stavové služby, vypadá takto:

1. Služba je vytvořená.
2. `StatefulServiceBase.onOpenAsync()`je volána. Toto volání není ve službě běžně přepsat.
3. Paralelní dojít v následujících akcí:
    - `StatefulServiceBase.createServiceReplicaListeners()`je volána. 
      - Pokud služba je primární služba, jsou otevřené všechny vrácený naslouchací procesy. `CommunicationListener.openAsync()`je volána v každé naslouchací proces.
      - Pokud služba je sekundární službu, pouze tyto moduly pro naslouchání označen jako `listenOnSecondary = true` jsou otevřené. Naslouchací procesy, které jsou otevřené na sekundárních hodnot je méně častých.
    - Pokud služba je aktuálně primárního serveru, službu na `StatefulServiceBase.runAsync()` metoda je volána.
4. Po všechny repliky naslouchací proces pro `openAsync()` volá dokončit a `runAsync()` je volána, `StatefulServiceBase.onChangeRoleAsync()` je volána. Toto volání není ve službě běžně přepsat.

Podobně jako bezstavové služby, neexistuje žádný koordinovat pořadí, ve kterém se vytváří a otevřít naslouchací procesy a kdy **runAsync** je volána. Pokud potřebujete koordinaci, řešení jsou podobné. Neexistuje jeden další případ pro stavové služby. Řekněme, že volání, které přicházejí na naslouchací procesy komunikace vyžadovat informace uložené v produktu některé [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md). Protože naslouchací procesy komunikace by bylo možné otevřít před spolehlivé kolekce jsou z něj číst nebo zapisovat a před **runAsync** může spustit, je nutné některé další spolupráce. Nejjednodušší a nejběžnější řešení je pro naslouchací procesy komunikace vrátit chybový kód, který klient používá vědět, opakujte žádost.

## <a name="stateful-service-shutdown"></a>Vypnutí stavové služby
Podobně jako bezstavové služby události životního cyklu během vypnutí jsou stejné jako při spuštění, ale vrátit zpět. Když se právě vypíná stavové služby, dojde k následujícím událostem:

1. Paralelní:
    - Jsou zavřeny všechny otevřené naslouchací procesy. `CommunicationListener.closeAsync()`je volána v každé naslouchací proces.
    - Token zrušení předaný `runAsync()` byla zrušena. Token zrušení volání `isCancelled()` metoda vrátí hodnotu true a pokud je volána, je token `throwIfCancellationRequested()` metoda vrátí `OperationCanceledException`.
2. Po `closeAsync()` skončí u každé naslouchací proces a `runAsync()` také dokončení služby `StatefulServiceBase.onChangeRoleAsync()` je volána. Toto volání není ve službě běžně přepsat.

   > [!NOTE]  
   > Potřeba čekat na **runAsync** ukončíte je nutné, pokud je tato replika primární repliky.

3. Po `StatefulServiceBase.onChangeRoleAsync()` metoda skončí, `StatefulServiceBase.onCloseAsync()` metoda je volána. Toto volání je neobvyklé přepsání, ale je k dispozici.
3. Po `StatefulServiceBase.onCloseAsync()` je destructed dokončení objektu služby.

## <a name="stateful-service-primary-swaps"></a>Primární záměna stavové služby
Je-li spuštěn stavové služby, je pouze primární repliky této stavové služby mají jejich naslouchací procesy komunikace otevřít a jejich **runAsync** metodu s názvem. Sekundární repliky se vytvářejí, ale zobrazit žádná další volání. Při spuštěné stavové služby, můžete repliky, který je aktuálně primární změnit. Co to znamená v podmínkách události životního cyklu, které můžete zobrazit repliku? Chování, které se zobrazí stavová repliky závisí na tom, jestli je replika se degradaci nebo povýší během prohození.

### <a name="for-the-primary-thats-demoted"></a>Pro primární, kterého je snížení úrovně
Pro primární repliku, která je k degradaci Service Fabric musí tato replika zastavit zpracování zpráv a ukončete všechny pozadí práce, kterou její probíhající činnosti. V důsledku toho tento krok vypadá to, když je služba vypnuta. Jedním rozdílem je, že služba není destructed nebo zavřená, protože zůstane jako sekundární. Následující rozhraní API se označují jako:

1. Paralelní:
    - Jsou zavřeny všechny otevřené naslouchací procesy. `CommunicationListener.closeAsync()`je volána v každé naslouchací proces.
    - Token zrušení předaný `runAsync()` je zrušená. Kontrolu token zrušení `isCancelled()` metoda vrátí hodnotu true a pokud je volána, je token `throwIfCancellationRequested()` metoda vrátí `OperationCanceledException`.
2. Po `closeAsync()` skončí u každé naslouchací proces a `runAsync()` také dokončení služby `StatefulServiceBase.onChangeRoleAsync()` je volána. Toto volání není ve službě běžně přepsat.

### <a name="for-the-secondary-thats-promoted"></a>Pro sekundární která vyzval
Podobně Service Fabric musí sekundární repliky, který je propagována do zahájit naslouchání pro zprávy v drátové síti a spustit úlohy na pozadí, které potřebuje k dokončení. V důsledku toho tento proces vypadá stejně při vytváření služby s tím rozdílem, že replika samotné již existuje jako. Následující rozhraní API se označují jako:

1. Paralelní:
    - `StatefulServiceBase.createServiceReplicaListeners()`je voláno, a všechny vrátí, moduly pro naslouchání jsou otevřené. `CommunicationListener.openAsync()`je volána v každé naslouchací proces.
    - Služby `StatefulServiceBase.runAsync()` metoda je volána.
2. Po všechny repliky naslouchací proces pro `openAsync()` volá dokončit a `runAsync()` je volána, `StatefulServiceBase.onChangeRoleAsync()` je volána. Toto volání není ve službě běžně přepsat.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Běžné problémy při vypnutí stavové služby a primární snížení úrovně
Service Fabric změní primární stavové služby z různých důvodů. Nejběžnější jsou [clusteru vyrovnává](service-fabric-cluster-resource-manager-balancing.md) a [upgradu aplikace](service-fabric-application-upgrade.md). Během těchto operací (i během vypnutí normální služby, jako by se zobrazí, pokud byla služba odstraněna), je důležité, že služba respektují `cancellationToken`. 

Služby, které zpracovávají zrušení řádně může mít několik problémy. Tyto operace jsou pomalé, protože Service Fabric čeká na služby, aby řádně ukončena. Nakonec to může vést k selhání upgrady tohoto časového limitu a vrátit zpět. Selhání respektovat token zrušení může také způsobit imbalanced clustery. Clustery stát nevyváženou, protože uzly získat aktivní, ale služby nemůže být znovu vyrovnána protože trvá příliš dlouho přesunout na jiné místo. 

Protože se stavové služby, je také pravděpodobné, že se používají [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md). V Service Fabric při snížení úrovně primární jeden z nejdůležitějších věcí, které se stane, se zruší se oprávnění k zápisu do základní stav. To vede ke druhé sadě problémy, které můžou ovlivnit služby životního cyklu. Kolekce návratový výjimky založené na načasování a jestli je přesouvání repliky nebo vypnutí. Tyto výjimky by měl být správně zpracovat. Výjimky vyvolané Service Fabric spadají do trvalého [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) a přechodný [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) kategorií. Trvalé výjimky by měl zaznamenána a vyvolána při přechodné výjimky můžete zkusit znovu na základě logiky některé opakování.

Zpracování výjimek, které pocházejí z použití `ReliableCollections` ve spojení s událostmi služby životního cyklu, je důležitou součástí testování a ověření spolehlivě. Doporučujeme vždy spustit služby zatížení při provádění upgradu a [chaos testování](service-fabric-controlled-chaos.md) před nasazením do produkčního prostředí. Tyto základní postup pomůže, zajistěte, aby služby je implementovaná správně a zpracovává události životního cyklu správně.

## <a name="notes-on-service-lifecycle"></a>Poznámky k služby životního cyklu
* Obě `runAsync()` metoda a `createServiceInstanceListeners/createServiceReplicaListeners` volání jsou volitelné. Služby může mít jeden z nich, obě nebo ani jeden z nich. Například pokud je služba svou práci na základě výzvy pro uživatele, není třeba pro něj k implementaci `runAsync()`. Pouze naslouchací procesy komunikace a jejich přidružené kód jsou nezbytné. Podobně vytváření a vrácení naslouchací procesy komunikace je volitelná, jak služba může mít pouze pozadí práci udělat a proto pouze musí implementovat`runAsync()`
* Je platná pro službu k dokončení `runAsync()` úspěšně a zpět z něj. Toto není považováno za selhání podmínku a by představují práce na pozadí od služby. Pro stavové služby spolehlivé `runAsync()` by volán znovu, pokud služba byly degradaci z primární a poté vyzval zpět na primární.
* Pokud služba ukončí z `runAsync()` po vyvolání výjimky neočekávané výjimce, jedná se o chybu a objektu služby se vypne a oznámila Chyba stavu.
* Když žádný časový limit na vrácení z těchto metod, okamžitě nebude možné zapisovat a proto nelze dokončit všechna skutečná práce. Doporučuje se vrátíte co nejrychleji po přijetí žádost o zrušení. Pokud vaše služba neodpoví na těchto volání rozhraní API v přiměřené době Service Fabric může vynucené ukončení služby. Obvykle tato situace nastane pouze během upgradu aplikace nebo když se odstraňuje služby. Tento časový limit je 15 minut ve výchozím nastavení.
* Selhání v `onCloseAsync()` cesta povede `onAbort()` volané, což je možnost best effort poslední příležitosti pro službu pro čištění až a uvolnění prostředků, které budou požadovaly.

## <a name="next-steps"></a>Další postup
* [Úvod do spolehlivé služby](service-fabric-reliable-services-introduction.md)
* [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start-java.md)
* [Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)
