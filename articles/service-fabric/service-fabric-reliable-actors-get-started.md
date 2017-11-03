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
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
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
K tomu, abyste mohli začít s Reliable Actors, vám stačí pochopit pár základních konceptů:

* **Služba objektu actor**. Služby Reliable Actors jsou zabalené ve službách Reliable Services, které je možné nasadit v infrastruktuře Service Fabric. Instance objektu actor se aktivují v pojmenované instanci služby.
* **Registrace objektu actor**. Stejně jako Reliable Services musí být i služba Reliable Actors zaregistrovaná v modulu runtime Service Fabric. Kromě toho musí být typ actor zaregistrovaný v modulu runtime Actor.
* **Rozhraní objektu actor**. Rozhraní objektu actor se používá k definování veřejného rozhraní objektu actor silného typu. V terminologii modelu Reliable Actors rozhraní objektu actor definuje typ zpráv, kterým objekt actor rozumí a může je zpracovat. Rozhraní objektu actor používají ostatní objekty actor a klientské aplikace k (asynchronnímu) odesílání zpráv do objektu actor. Reliable Actors můžou implementovat více rozhraní.
* **Třída ActorProxy**. Třídu ActorProxy používají klientské aplikace k vyvolání metod zveřejněných přes rozhraní objektu actor. Třída ActorProxy zajišťuje dvě důležité funkce:
  
  * Překlad názvů: Dokáže vyhledat objekt actor v clusteru (najít uzel clusteru, který je jeho hostitelem).
  * Zpracování selhání: Dokáže opakovat vyvolání metod a znovu přeložit umístění objektu actor například po selhání, které vyžaduje přemístění objektu actor do jiného uzlu v clusteru.

Za zmínku stojí následující pravidla týkající se rozhraní objektů actor:

* Metody rozhraní objektu actor není možné přetížit.
* Metody rozhraní objektu actor nesmí obsahovat vnější, referenční ani volitelné parametry.
* Obecná rozhraní se nepodporují.

## <a name="create-a-new-project-in-visual-studio"></a>Vytvořte nový projekt v sadě Visual Studio
Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric:

![Service Fabric nástrojů pro Visual Studio – nový projekt][1]

V dialogovém okně Další můžete si zvolit typ projektu, který chcete vytvořit.

![Šablony projektů Service Fabric][5]

Pro projekt HelloWorld použijeme služby Service Fabric Reliable Actors.

Po vytvoření řešení, byste měli vidět následující strukturou:

![Struktura projektu Service Fabric][2]

## <a name="reliable-actors-basic-building-blocks"></a>Základní stavební bloky Reliable Actors
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

Služba objektu actor musí být zaregistrovaná s typem služby v modulu runtime Service Fabric. Aby služba objektu actor mohla spouštět instance objektu actor, musí v ní být zaregistrovaný také typ actor. Pro objekty actor tuto práci provádí metoda registrace `ActorRuntime`.

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
