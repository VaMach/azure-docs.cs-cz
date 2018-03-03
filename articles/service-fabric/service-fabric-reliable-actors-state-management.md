---
title: "Stav správy Reliable Actors | Microsoft Docs"
description: "Popisuje způsob správy, jako trvalý a pro zajištění vysoké dostupnosti replikují Reliable Actors stavu."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: f196b2e54efc5ecbbd93e48e1f115edb99e5c858
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="reliable-actors-state-management"></a>Spolehlivé aktéři řízení stavu
Reliable Actors jsou jedním podprocesem objekty, které může zapouzdřit logiku a stavu. Protože aktéři spustit na spolehlivé služby, jejich stavu udržovat spolehlivé pomocí stejné trvalosti a mechanismech replikace. Tímto způsobem Neztraťte aktéři jejich stav po selhání při opětovné aktivaci po uvolnění paměti, nebo při jejich přesunu mezi uzly v clusteru s podporou kvůli vyrovnávání prostředků nebo upgradu.

## <a name="state-persistence-and-replication"></a>Trvalost stavu a replikace
Jsou považovány za všechny Reliable Actors *stateful* vzhledem k tomu, že každá instance objektu actor se mapuje na jedinečný identifikátor. To znamená, že opakovaná volání stejné ID objektu actor jsou směrované na stejnou instanci objektu actor. V systému bezstavové naopak volání klienta není zaručena bezpečnost pro směrované na stejný server pokaždé, když. Z tohoto důvodu služby objektu actor jsou vždy stavové služby.

I když aktéři jsou považovány za stateful, který neznamená, že musí spolehlivě uložení stavu. Aktéři můžete vybrat úroveň trvalost stavu a replikace na základě jejich dat požadavky na úložiště:

* **Trvalý stav**: je uložen stav disku a se replikují do tři nebo více replik. Trvalého stavu je možnost nejvíce odolná úložiště stavu, kde můžete zachovat stav prostřednictvím clusterů výpadku.
* **Volatile stavu**: stav se replikovat na tři nebo více replik a pouze uchovává se v paměti. Volatile stavu poskytuje odolnost proti selhání uzlu a selhání objektu actor a během upgradu a vyrovnávání prostředků. Však není trvalý stav na disk. Proto všechny repliky byly ztraceny najednou, stav dojde ke ztrátě také.
* **Žádné trvalého stavu**: stav není replikovat nebo zapsaných na disk, používají jenom pro objekty actor nemusíte spolehlivě Udržovat stav.

Každou úroveň trvalost je jednoduše jiné *zprostředkovatele stavu* a *replikace* konfigurace služby. Zda je stav zapsán do disku závisí na poskytovateli stavu--součástí spolehlivá služba, která ukládá stav. Replikace závisí na tom, kolik repliky může služba nasazený s. Stejně jako u spolehlivé služby zprostředkovatele stavu i počet replik jde snadno nastavit ručně. Rozhraní objektu actor poskytuje atribut, který, když použije na objekt actor, automaticky vybere výchozí poskytovatel stavu a automaticky vygeneruje nastavení pro repliky počet dosáhnout jednoho z těchto tří nastavení trvalosti. Atribut StatePersistence není zdědí odvozené třídy, každý typ objektu Actor musíte zadat jeho StatePersistence úroveň.

### <a name="persisted-state"></a>Trvalého stavu
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Toto nastavení využívá zprostředkovatele stavu, která ukládá data na disku a automaticky nastaví počet replik služby 3.

### <a name="volatile-state"></a>Volatile stavu
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Toto nastavení používá poskytovatele v paměti jen stavu a nastaví počet replik na 3.

### <a name="no-persisted-state"></a>Žádné trvalého stavu
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Toto nastavení používá poskytovatele v paměti jen stavu a nastaví počet replik na 1.

### <a name="defaults-and-generated-settings"></a>Výchozí hodnoty a vygenerované nastavení
Pokud používáte `StatePersistence` atribut zprostředkovatele stavu je automaticky vybrána pro za běhu při spuštění služby objektu actor. Počet replik, ale je nastavena v době kompilace pomocí nástroje sestavení sady Visual Studio objektu actor. Nástroje pro sestavení automaticky generovat *výchozí služba* služby objektu actor v ApplicationManifest.xml. Vytvoří se pro parametry **sady replik minimální velikost** a **cíl repliky nastavit velikost**.

Tyto parametry můžete ručně změnit. Ale pokaždé, když `StatePersistence` změně atributu, parametry jsou nastaveny na výchozí hodnoty velikosti sady replik pro vybranou `StatePersistence` atribut přepsání žádné předchozí hodnoty. Jinými slovy, jsou hodnoty, které se nastavují v ServiceManifest.xml *pouze* přepsat v čase vytvoření buildu, když změníte `StatePersistence` hodnota atributu.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Správce stavu
Všechny instance objektu actor má svou vlastní správce stavu: jako slovník datová struktura, která spolehlivě uchová dvojice klíč/hodnota. Správce stavu je obálku kolem zprostředkovatele stavu. Můžete ho použít k ukládání dat bez ohledu na to, který se používá nastavení trvalosti. Neposkytuje žádné záruky, spuštěné služby objektu actor můžete změnit z nastavení volatile (v paměti jen) stavu tak, aby nastavení trvalého stavu prostřednictvím postupného upgradu při zachování dat. Je však možné změnit počet replik pro spuštěnou službu.

Správce stavu klíče musí být řetězce. Hodnoty jsou obecné a mohou být jakéhokoli typu, včetně vlastních typů. Hodnotami uloženými v správce stavu musí být serializovatelný kontrakt dat, protože může přenést přes síť do dalších uzlů během replikace a může zapsat na disk, v závislosti na nastavení trvalost stavu objektu actor.

Správce stavu zpřístupní běžné metody slovník pro správu stavu, podobné těm, které jsou obsaženy ve slovníku pro spolehlivé.

## <a name="accessing-state"></a>Přístup ke stavu
Stav je přístupná prostřednictvím Správce stavu podle klíče. Metody správce stavu jsou všechny asynchronní, protože v/v disku může vyžadují při aktéři držena formou stavu. Při prvním přístupu jsou uložená v mezipaměti objektů stavu. Opakujte přístup operations přístup k objektům přímo z paměti a vrátí synchronně, aniž by docházelo k vstupně-výstupní diskové nebo asynchronní režií přepínání kontextu. Stav objektu se odebere z mezipaměti v následujících případech:

* Po ho načte objekt z správce stavu, vyvolá metoda objektu actor k neošetřené výjimce.
* Objekt actor je znovu aktivovat, po právě deaktivována nebo po selhání.
* Zprostředkovatel stavu stránky stavu na disk. Tento postup závisí na implementaci zprostředkovatele stavu. Výchozí zprostředkovatel stavu pro `Persisted` nastavení je toto chování.

Stav můžete načíst pomocí standardního *získat* operace, která vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java), pokud položka neexistuje pro klíč:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Můžete také načíst stav pomocí *TryGet* metoda, která nevyvolá výjimku pokud záznam neexistuje pro klíč:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

## <a name="saving-state"></a>Ukládání stavu
Metody načtení stavu manager vrátí odkaz na objekt v místní paměti. Úprava tento objekt v místní paměti samostatně nezpůsobí, je bezpečně uložit. Pokud objekt je načtena z správce stavu a upravit, musíte znovu vložit do Správce stavu bezpečně uložit.

Můžete vložit stavu pomocí nepodmíněné *nastavit*, což je ekvivalentem `dictionary["key"] = value` syntaxe:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Stav můžete přidat pomocí *přidat* metoda. Tato metoda vyvolá `InvalidOperationException`(C#) nebo `IllegalStateException`(Java) při pokusu o přidání klíče, která již existuje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Můžete také přidat stavu pomocí *TryAdd* metoda. Tato metoda nevyvolá výjimku při pokusu o přidání klíče, který již existuje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Na konci metodu objektu actor správce stavu automaticky uloží všechny hodnoty, které byly přidány nebo upraveném operace insert nebo update. "Uložení" může obsahovat uložením na disk a replikace, v závislosti na nastavení použít. Hodnoty, které nebyly upraveny nejsou jako trvalý, nebo replikovat. Pokud byly změněny žádné hodnoty, uložení operace se nic nestane. Pokud ukládání selže, budou zahozeny upravený stav a je znovu původního stavu.

Můžete také uložit stav ručně voláním `SaveStateAsync` metodu objektu actor základní:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

## <a name="removing-state"></a>Odebrání stavu
Můžete odebrat stavu trvale ze Správce stavu objektu actor voláním *odebrat* metoda. Tato metoda vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java) při pokusu o odstranění klíče, který neexistuje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Rovněž můžete odebrat stavu trvale pomocí *TryRemove* metoda. Tato metoda nevyvolá výjimku při pokusu o odebrání klíče, který neexistuje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="best-practices"></a>Doporučené postupy
Tady jsou některé navrhované postupy a tipy k řešení potíží pro správu vašeho stavu objektu actor.

### <a name="make-the-actor-state-as-granular-as-possible"></a>Zkontrolujte jako podrobné nejblíže stavu objektu actor
To je důležité pro výkon a využití prostředků aplikace. Vždy, když je všechny aktualizace nebo zápisu "s názvem stavu" objektu actor, celou hodnotu odpovídající této "s názvem stavu" se serializovat a odesílá přes síť na sekundárních replikách.  Sekundárních zapisují na místní disk a odpověď zpět na primární repliku. Když primární obdrží potvrzení z kvora sekundárních replikách, zapíše stav jeho místní disk. Předpokládejme například, zda že je hodnota třídu, která má 20 členy a jejich velikost je 1 MB. I když mezi členy třídy, které je pouze změnit velikost 1 KB, end až platícího náklady na serializaci a sítě a disku zápisy pro úplné 1 MB. Podobně pokud je hodnota kolekce (například seznam, pole nebo slovník), platíte náklady na úplnou kolekci i v případě, že upravíte jednoho z jeho členů. Rozhraní StateManager třídy objektu actor je jako slovník. Vždy by měl model datovou strukturu představující stavu objektu actor nad tohoto slovníku.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Správně spravovat životní cyklus objektu actor
Měli byste mít jasné zásady o správě velikosti stavu v každém oddílu služby objektu actor. Služby objektu actor by měl mít pevný počet aktéři a znovu použít, je velmi míře. Pokud vytvoříte nepřetržitě nové aktéři, musíte je odstranit, jakmile se provádějí v práci. Rozhraní objektu actor ukládá některé metadata o každém objektu actor, který již existuje. Odstranění všech stavu objektu actor metadata o tomto objektu actor neodebere. Je nutné odstranit objektu actor (viz [odstraňování aktéři a jejich stavu](service-fabric-reliable-actors-lifecycle.md#deleting-actors-and-their-state)) k odebrání všech informací o je uložená v systému. Jako další kontrolu, musí zadat dotaz na službu objektu actor (najdete v části [vytváření výčtu aktéři](service-fabric-reliable-actors-platform.md)) v k zkontrolujte, zda jsou čísla aktéři očekávaný rozsah.
 
Pokud někdy uvidíte, že velikost souboru databáze služby objektu Actor roste překračuje očekávanou velikost, ujistěte se, že jsou následující výše uvedených pokynů. Pokud postupujete podle těchto pokynů a jsou stále databáze problémům s velikostí souboru, měli byste [otevřete lístek podpory](service-fabric-support.md) s produktový tým jak získat nápovědu.

## <a name="next-steps"></a>Další postup

Musí být serializované stavu, který je uložen v Reliable Actors před jeho zapsaný na disk a replikované pro vysokou dostupnost. Další informace o [serializace typu objektu Actor](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

V dalším kroku Další informace o [objektu Actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md).
