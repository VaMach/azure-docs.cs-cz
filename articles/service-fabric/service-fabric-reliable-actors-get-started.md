---
title: "Vytvoření vaší první na základě objektu actor Azure mikroslužbu v jazyce C# | Microsoft Docs"
description: "Tento kurz vás provede kroky při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-reliable-actors"></a>Začínáme s Reliable Actors
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
> * [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Tento článek vysvětluje základy Azure Service Fabric Reliable Actors a provede vás vytváření, ladění a nasazování jednoduchou aplikaci spolehlivé objektu Actor v sadě Visual Studio.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, abyste měli vývojového prostředí Service Fabric na váš počítač.
Pokud potřebujete nastavit tak, najdete podrobné pokyny na [postup nastavení vývojového prostředí](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Základní koncepty
Začít s Reliable Actors, potřebujete jenom pochopit několik základní koncepty:

* **Služby objektu actor**. Spolehlivé služby, které můžou být nasazené v Service Fabric infrastruktury jsou součástí Reliable Actors. Instance objektu actor aktivují v instanci služby s názvem.
* **Registrace objektu actor**. Jako se službami Reliable Services spolehlivé objektu Actor služby musí být registrováno v modulu runtime Service Fabric. Typ objektu actor kromě toho musí být registrováno s modulem runtime objektu Actor.
* **Rozhraní objektu actor**. Rozhraní objektu actor se používá k definování silného typu veřejné rozhraní objektu actor. Rozhraní objektu actor v terminologii modelu objektu Actor spolehlivé, definuje typy zprávy, které můžete porozumět objektu actor a proces. Rozhraní objektu actor slouží ostatní aktéři a klientské aplikace "Odeslat" (asynchronně) zpráv do objektu actor. Reliable Actors můžete implementovat více rozhraní.
* **Třída ActorProxy**. Třída ActorProxy se používá klientskými aplikacemi volat metody vystavenou přes rozhraní objektu actor. Třída ActorProxy poskytuje dvě důležité funkce:
  
  * Překlad názvů: je možné najít objektu actor v clusteru (Najít uzlu clusteru, který je hostitelem).
  * Zpracování selhání: mohou zkuste volání metod a znovu přeložit umístění objektu actor po, například selhání vyžadující objektu actor pro přemístit do jiného uzlu v clusteru.

Následující pravidla, které se týkají objektu actor rozhraní jsou důležité zmínit:

* Metody rozhraní objektu actor nemohou být přetíženy.
* Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.
* Obecná rozhraní nejsou podporovány.

## <a name="create-a-new-project-in-visual-studio"></a>Vytvořte nový projekt v sadě Visual Studio
Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric:

![Service Fabric nástrojů pro Visual Studio – nový projekt][1]

V dialogovém okně Další můžete si zvolit typ projektu, který chcete vytvořit.

![Šablony projektů Service Fabric][5]

Pro projekt HelloWorld použijeme služby Service Fabric Reliable Actors.

Po vytvoření řešení, byste měli vidět následující strukturou:

![Struktura projektu Service Fabric][2]

## <a name="reliable-actors-basic-building-blocks"></a>Spolehlivé aktéři základních stavebních bloků
Typické Reliable Actors řešení se skládá ze tří projektů:

* **Projekt aplikace (MyActorApplication)**. Toto je projekt, který balíčky všechny společně služby pro nasazení. Obsahuje *ApplicationManifest.xml* a skriptů prostředí PowerShell pro správu aplikace.
* **Rozhraní projektu (MyActor.Interfaces)**. Toto je projekt, který obsahuje definici rozhraní objektu actor. V projektu MyActor.Interfaces můžete definovat rozhraní, která se použije aktéři v řešení. Lze definovat vašich rozhraní objektu actor v jakékoli projektu s libovolným názvem, ale rozhraní definuje kontrakt objektu actor, který sdílí implementace objektu actor a klienti volání objektu actor, proto obvykle má smysl definovat v sestavení, které jsou oddělené od objektu actor implementaci a může být sdílen více další projekty.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Projekt služby objektu actor (MyActor)**. Toto je používá k definování služba Service Fabric, která bude hostitelem objektu actor projektu. Obsahuje implementace objektu actor. Implementace objektu actor je třída, která je odvozena ze základního typu `Actor` a implementuje alespoň jedno rozhraní, která jsou definována v MyActor.Interfaces projektu. Třídu objektu actor musí také implementovat konstruktor, který přijímá `ActorService` instance a `ActorId` a předává je základní `Actor` třídy. To umožňuje vkládání závislostí konstruktor platformy závislostí.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Služby objektu actor musí být zaregistrován s typem služby v modulu runtime Service Fabric. V pořadí pro službu objektu Actor ke spuštění vaše instance objektu actor musí být typu vašeho objektu actor také zaregistrován u služby objektu Actor. `ActorRuntime` Metoda registrace provede tuto práci pro aktéři.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Pokud spustíte z nový projekt v sadě Visual Studio a máte jenom jednu definici objektu actor, registrace je zahrnuta ve výchozím nastavení v kódu, který generuje Visual Studio. Pokud definujete jiných aktéři ve službě, budete muset přidat objektu actor registraci pomocí:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Modul runtime Service Fabric aktéři vysílá některé [události a čítače výkonu související s metody objektu actor](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Jsou užitečné v Diagnostika a sledování výkonu.
> 
> 

## <a name="debugging"></a>Ladění
Service Fabric tools pro Visual Studio podporují ladění na místním počítači. Můžete začít relaci ladění pomocí stiskne klávesu F5. Visual Studio vytvoří (v případě potřeby) balíčky. Také nasadí aplikaci na místní cluster Service Fabric a připojí ladicí program.

Během procesu nasazení můžete sledovat průběh v **výstup** okno.

![Service Fabric ladicích výstup – okno][3]

## <a name="next-steps"></a>Další kroky
Další informace o [jak Reliable Actors pomocí platformy Service Fabric](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
