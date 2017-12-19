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
ms.openlocfilehash: 80eb68346dd05c256c60725eb082aa0651fe7cbd
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
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

## <a name="notes-on-service-lifecycle"></a>Poznámky k služby životního cyklu
* Obě `runAsync()` metoda a `createServiceInstanceListeners` volání jsou volitelné. Služby může mít jeden z nich, obě nebo ani jeden z nich. Například pokud je služba svou práci na základě výzvy pro uživatele, není třeba pro něj k implementaci `runAsync()`. Pouze naslouchací procesy komunikace a jejich přidružené kód jsou nezbytné. Podobně vytváření a vrácení naslouchací procesy komunikace je volitelná, jak služba může mít pouze pozadí práci udělat a proto pouze musí implementovat`runAsync()`
* Je platná pro službu k dokončení `runAsync()` úspěšně a zpět z něj. Toto není považováno za selhání podmínku a by představují práce na pozadí od služby. Pro stavové služby spolehlivé `runAsync()` by volán znovu, pokud služba byly degradaci z primární a poté vyzval zpět na primární.
* Pokud služba ukončí z `runAsync()` po vyvolání výjimky neočekávané výjimce, jedná se o chybu a objektu služby se vypne a oznámila Chyba stavu.
* Když žádný časový limit na vrácení z těchto metod, okamžitě nebude možné zapisovat a proto nelze dokončit všechna skutečná práce. Doporučuje se vrátíte co nejrychleji po přijetí žádost o zrušení. Pokud vaše služba neodpoví na těchto volání rozhraní API v přiměřené době Service Fabric může vynucené ukončení služby. Obvykle tato situace nastane pouze během upgradu aplikace nebo když se odstraňuje služby. Tento časový limit je 15 minut ve výchozím nastavení.
* Selhání v `onCloseAsync()` cesta povede `onAbort()` volané, což je možnost best effort poslední příležitosti pro službu pro čištění až a uvolnění prostředků, které budou požadovaly.

> [!NOTE]
> Stavová spolehlivé služby ještě nepodporuje v jazyce java.
>
>

## <a name="next-steps"></a>Další kroky
* [Úvod do spolehlivé služby](service-fabric-reliable-services-introduction.md)
* [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
* [Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)
