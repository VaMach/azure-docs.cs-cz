---
title: "Vytvořte front-end webové aplikace Azure Service Fabric pomocí ASP.NET Core | Microsoft Docs"
description: "Zveřejnit aplikaci Service Fabric na webu pomocí služby projektu ASP.NET Core a komunikace mezi službami prostřednictvím vzdálené komunikace služby."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: b19aaa652f2c15573ded632ca1348e1a6752f080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Vytvoření služby front-end webové aplikace pomocí ASP.NET Core
Ve výchozím nastavení neposkytují služby Azure Service Fabric veřejné rozhraní na webu. Vystavit funkcionalitu vaší aplikace do klientů protokolu HTTP, budete muset vytvořit webového projektu fungovat jako vstupní bod a potom z ní sdělit jednotlivých služeb.

V tomto kurzu budeme pokračovat tam kde jsme skončil v [vytvoření vaší první aplikace v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) kurzu a přidání webové služby ASP.NET Core před službu stavová čítače. Pokud jste tak již neučinili, měli přejděte zpět a první krok prostřednictvím tohoto kurzu.

## <a name="set-up-your-environment-for-aspnet-core"></a>Nastavení prostředí pro ASP.NET Core

Budete potřebovat Visual Studio 2017 podle spolu se v tomto kurzu. Všechny edice provede. 

 - [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/)

K vývoji aplikací ASP.NET Core Service Fabric, musí mít nainstalované následující úlohy:
 - **Vývoj pro Azure** (v části *Web a Cloud*)
 - **Vývoj pro ASP.NET a webové** (v části *Web a Cloud*)
 - **Vývoj pro různé platformy .NET core** (v části *ostatní modulové*)

>[!Note] 
>.NET Core nástrojů pro Visual Studio 2015 se už aktualizují, ale pokud stále používáte Visual Studio 2015, budete muset mít [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) nainstalována.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Přidání služby ASP.NET Core do aplikace
ASP.NET Core je odlehčený, napříč platformami webové rozhraní vývoj, které vám pomůže vytvořit moderní webové uživatelské rozhraní a webovým rozhraním API. 

Umožňuje přidat projekt webového rozhraní API ASP.NET naše stávající aplikace.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **služby** v aplikaci projektu a zvolte **Přidat > Nový Service Fabric Service**.
   
    ![Přidání nové služby do existující aplikace][vs-add-new-service]
2. Na **vytvoření služby** vyberte **ASP.NET Core** a pojmenujte ho.
   
    ![Výběr v dialogovém okně Nový služby webovou službu ASP.NET.][vs-new-service-dialog]

3. Další stránka obsahuje sadu ASP.NET Core šablony projektů. Všimněte si, že jsou stejné možnosti, které by vidíte, pokud jste vytvořili projekt ASP.NET Core mimo aplikace Service Fabric pomocí malé množství další kód pro registraci služby modulu runtime Service Fabric. V tomto kurzu zvolte **webového rozhraní API**. Koncepty však můžete použít k vytvoření úplné webové aplikace.
   
    ![Výběr typu projektu ASP.NET][vs-new-aspnet-project-dialog]
   
    Po vytvoření projektu webového rozhraní API byste měli mít dvě služby ve vaší aplikaci. Při dalším sestavit aplikaci, můžete přidat další služby stejným způsobem. Každý může být nezávisle verzí a upgradovaný.

## <a name="run-the-application"></a>Spuštění aplikace
Chcete-li získat představu o co máme jste Hotovo, umožňuje nasazení nové aplikace a podívejte se na výchozí chování, které poskytuje webové rozhraní API ASP.NET Core šablony.

1. Stisknutím klávesy F5 v sadě Visual Studio k ladění aplikace.
2. Po dokončení nasazení sady Visual Studio spustí prohlížeč na kořenový server služby webového rozhraní API ASP.NET. Šablona webového rozhraní API ASP.NET Core neposkytuje výchozí chování pro kořenový adresář, takže byste měli vidět chybu 404 v prohlížeči.
3. Přidat `/api/values` do umístění v prohlížeči. Tím se spustí `Get` metodu ValuesController v šabloně webového rozhraní API. Vrátí výchozí odpovědi, které poskytuje šablony – pole JSON, který obsahuje dva řetězce:
   
    ![Výchozí hodnot vrácených z webového rozhraní API ASP.NET Core šablony][browser-aspnet-template-values]
   
    Na konci tohoto kurzu Tato stránka se zobrazí nejnovější hodnotu čítače z našich stavové služby namísto výchozí řetězců.

## <a name="connect-the-services"></a>Připojení služby
Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services. V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP, jiných služeb, které jsou přístupné přes rozhraní HTTP REST API a stále jiných služeb, které jsou přístupné přes webové sokety. Pro informace o dostupných možnostech a kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md). V tomto kurzu používáme [vzdálené komunikace služby Service Fabric](service-fabric-reliable-services-communication-remoting.md), zadaný v sadě SDK.

V metodě vzdálené komunikace služby (modelován na vzdálených volání procedur nebo RPC) definujete rozhraní tak, aby fungoval jako veřejného kontraktu služby. Pak použijte tohoto rozhraní k vygenerování třídu proxy pro interakci se službou.

### <a name="create-the-remoting-interface"></a>Vytvořit rozhraní vzdálené komunikace
Začněme vytvořením rozhraní fungovat jako kontrakt mezi stavové služby a další služby, v tomto případě ASP.NET Core webového projektu. Toto rozhraní musí být sdílená všech služeb, které ho používají volání RPC, takže ho vytvoříme v jeho vlastní projektu knihovny tříd.

1. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a zvolte **přidat** > **nový projekt**.

2. Vyberte **Visual C#** v levém navigačním podokně a potom vyberte položku **knihovny tříd** šablony. Ujistěte se, že verze rozhraní .NET Framework je nastavená na **4.5.2**.
   
    ![Vytvoření projektu rozhraní pro stavové služby][vs-add-class-library-project]

3. Nainstalujte **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet. Vyhledejte **Microsoft.ServiceFabric.Services.Remoting** nuget balíček správce a instalace pro všechny projekty v řešení, které používají vzdálené komunikace služby, včetně:
   - Knihovna tříd projekt, který obsahuje rozhraní služby
   - Projekt stavové služby
   - Projektu ASP.NET Core webové služby
   
    ![Probíhá přidávání balíčku NuGet služby][vs-services-nuget-package]

4. V knihovně tříd vytvořit rozhraní s jedinou metodu `GetCountAsync`, a rozšířit rozhraní z `Microsoft.ServiceFabric.Services.Remoting.IService`. Vzdálené komunikace rozhraní musí být odvozeny od tohoto rozhraní to znamená, že je Služba vzdálené komunikace rozhraní.
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>Toto rozhraní implementovat v stavové služby
Teď, když jsme definovali rozhraní, musíme implementovat v stavové služby.

1. Stavové služby přidejte odkaz na projekt knihovny tříd, který obsahuje rozhraní.
   
    ![Přidat odkaz na projekt knihovny tříd v stavové služby][vs-add-class-library-reference]
2. Najít třídu, která dědí z `StatefulService`, jako například `MyStatefulService`, a rozšířit ji k implementaci `ICounter` rozhraní.
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Nyní implementovat jednu metodu, která je definována v `ICounter` rozhraní `GetCountAsync`.
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Vystavení stavové služby pomocí služby vzdálené komunikace naslouchací proces
Pomocí `ICounter` rozhraní implementované, posledním krokem je otevřít vzdálenou komunikaci služby komunikační kanál. Pro stavové služby, Service Fabric nabízí přepisovatelné metodu s názvem `CreateServiceReplicaListeners`. Pomocí této metody můžete určit jeden nebo více naslouchací procesy komunikace, na základě typu komunikaci, která chcete povolit pro vaši službu.

> [!NOTE]
> Volání metody ekvivalentní pro otevření komunikační kanál pro bezstavové služby `CreateServiceInstanceListeners`.

V takovém případě jsme nahradit existující `CreateServiceReplicaListeners` metoda a zadejte instanci `ServiceRemotingListener`, která vytvoří koncový bod protokolu RPC, lze volat z klientů prostřednictvím `ServiceProxy`.  

`CreateServiceRemotingListener` Rozšiřující metody na `IService` rozhraní umožňuje snadno vytvářet `ServiceRemotingListener` s všechna výchozí nastavení. Při použití této metody rozšíření, zajistěte, abyste měli `Microsoft.ServiceFabric.Services.Remoting.Runtime` importovat obor názvů. 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Použití třídy ServiceProxy komunikovat se službou
Naše stavové služby je nyní připravena přijímat provoz z jiných služeb prostřednictvím protokolu RPC. Takže zůstane je přidání kód ke komunikaci s ním z webovou službu ASP.NET.

1. V projektu ASP.NET přidejte odkaz na knihovnu tříd, který obsahuje `ICounter` rozhraní.

2. Dříve jste přidali **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet do projektu ASP.NET. Tento balíček poskytuje `ServiceProxy` třídy, které můžete provádět RPC volá, aby se stavové služby. Zkontrolujte, jestli že tento balíček NuGet je nainstalované v projektu webové služby ASP.NET Core.

4. V **řadiče** složku, otevřete `ValuesController` třídy. Všimněte si, že `Get` metoda aktuálně právě vrátí pole pevně řetězce "hodnota1" a "hodnota2"--který by odpovídal co jsme viděli dříve v prohlížeči. Tato implementace nahraďte následujícím kódem:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    První řádek kódu je klíče. Vytvořit proxy ICounter pro stavové služby, musíte zadat dva kusy informace: název služby a ID oddílu.
   
    Pomocí rozdělení do oddílů stavové služby porušením jejich stav do různých intervalů, na základě klíče, které definujete, jako je ID zákazníka nebo PSČ. V naší aplikaci trivial má stavové služby jenom jeden oddíl, tak klíč není důležité. Všechny klíče, který zadáte přejde do stejného oddílu. Další informace o vytváření oddílů služby najdete v tématu [postup oddíl spolehlivé služby Service Fabric](service-fabric-concepts-partitioning.md).
   
    Název služby je identifikátor URI formuláře fabric: /&lt;název_aplikace&gt;/&lt;service_name&gt;.
   
    Pomocí těchto dvou položek příslušné Service Fabric jednoznačně identifikovat na počítač, který by měl být odeslány požadavky. `ServiceProxy` Třída také bezproblémově zpracovává případ, kdy je počítač, který je hostitelem oddílu stavové služby selže a jiný počítač musí být povýšen na jeho proběhnout. Tato abstrakce usnadňuje psaní kódu klienta, jak nakládat s jinými službami výrazně jednodušší.
   
    Jakmile jsme proxy server, můžeme jednoduše vyvolání `GetCountAsync` metodu a vrátí její výsledek.

5. Stisknutím klávesy F5 spusťte upravené aplikaci. Jako dříve, Visual Studio automaticky spustí prohlížeč do kořenového adresáře webového projektu. Přidejte cestu "rozhraní api nebo hodnoty" a měli byste vidět aktuální vrátila hodnotu čítače.
   
    ![Stavová čítače hodnota zobrazená v prohlížeči][browser-aspnet-counter-value]
   
    Aktualizujte stránku prohlížeče pravidelně zobrazíte hodnota čítače aktualizovat.

## <a name="kestrel-and-weblistener"></a>Kestrel a WebListener

Webový server výchozí ASP.NET Core, označuje jako Kestrel, je [není podporován pro zpracování přímé přenosy z Internetu](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel). V důsledku toho šablonu ASP.NET Core bezstavové služby Service Fabric používá [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) ve výchozím nastavení. 

Další informace o Kestrel a WebListener v Service Fabric služby, naleznete v [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="connecting-to-a-reliable-actor-service"></a>Připojení ke službě spolehlivé objektu Actor
Tento kurz se zaměřuje na přidání webový front-end, který oznamovat se stavovou službou. Můžete však provést velmi podobný modelu, aby komunikoval s aktéři. Při vytváření spolehlivé objektu Actor projektu sady Visual Studio automaticky generuje projektu rozhraní za vás. Toto rozhraní můžete použít ke generování proxy služby objektu actor v webového projektu ke komunikaci s objektu actor. Komunikační kanál je zadán automaticky. Proto není potřeba dělat nic, který je ekvivalentní pro vytvoření `ServiceRemotingListener` stejně, jako jste pro stavové služby v tomto kurzu.

## <a name="how-web-services-work-on-your-local-cluster"></a>Jak fungují webové služby v místním clusteru
Obecně platí můžete nasadit přesně stejnou aplikaci Service Fabric na clusteru s více počítači, který jste nasadili v místním clusteru a být vysoce jistý, že funguje podle očekávání. Je to proto, že místní cluster je jednoduše konfigurace pěti uzly sbalena na jednom počítači.

Pokud jde o webové služby, je však jeden nuance klíče. Při clusteru se nachází za službou Vyrovnávání zatížení, jako je tomu v Azure, musíte zajistit, aby webové služby jsou nasazeny na každý počítač od nástroje pro vyrovnávání zatížení jednoduše kruhové dotazování provoz mezi počítači. To provedete nastavením `InstanceCount` pro službu na zvláštní hodnotu-1.

Naopak když jste místní spuštění webové služby, musíte zajistit, že pouze jedna instance je služba spuštěná. Spustíte, jinak hodnota do konfliktu z více procesy, které jsou na stejné cesty a portu naslouchá. Počet instancí služby webové v důsledku toho musí být nastavená na 1, pro místní nasazení.

Naučte se konfigurovat různé hodnoty pro různé prostředí, najdete v tématu [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Další kroky
Teď, když máte webové přední skončili sadu pro vaši aplikaci s ASP.NET Core, další informace o [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) pro podrobné pochopení fungování ASP.NET Core s Service Fabric.

Dále [Další informace o komunikaci se službou](service-fabric-connect-and-communicate-with-services.md) v obecné získat úplná obrázek o tom, jak služba funguje komunikace v Service Fabric.

Až budete mít dostatečné povědomí o tom, jak funguje komunikace služby, [vytvořte cluster v Azure a nasazení aplikace do cloudu](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
