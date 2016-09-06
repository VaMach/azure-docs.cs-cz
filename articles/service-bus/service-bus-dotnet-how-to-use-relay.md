<properties
    pageTitle="Jak používat předávání Service Bus s .NET | Microsoft Azure"
    description="Naučte se, jak použít službu předávání Azure Service Bus ke spojení dvou aplikací hostovaných v různých umístěních."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="05/06/2016"
    ms.author="sethm"/>


# Jak používat službu předávání přes Azure Service Bus

Tento článek popisuje, jak používat službu předávání přes Service Bus. Ukázky jsou napsané v C# a používají API Windows Communication Foundation (WCF) s rozšířeními, které jsou součástí sestavení Service Bus. Další informace o předávání přes Service Bus najdete v přehledu [Přenos zpráv přes předávací službu Service Bus](service-bus-relay-overview.md).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Co je předávání přes Service Bus?

[Služba *předávání* přes Service Bus](service-bus-relay-overview.md) vám umožní sestavovat hybridní aplikace, které poběží v datovém centru Azure i ve vašem lokálním podnikovém prostředí. Předávání přes Service Bus toho docílí tak, že vám umožní bezpečně vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti, veřejnému cloudu, a to bez nutnosti otevřít spojení ve firewallu nebo udělat nežádoucí změny v infrastruktuře podnikové sítě.

![Koncepty předávání](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Předávání přes Service Bus vám umožní hostovat služby WCF ve vaší existující podnikové síti. Potom můžete delegovat čekání na příchozí relace a požadavky z těchto služeb WCF na službu Service Bus běžící v Azure. To vám umožní vystavit tyto služby aplikačnímu kódu běžícímu v Azure nebo mobilním pracovníkům nebo partnerským prostředím v extranetu. Service Bus vám umožňuje bezpečně řídit, kdo má přístup k těmto službám, na velice přesné úrovni. Nabízí výkonný a bezpečný způsob, jak vystavit data a funkce aplikací z existujících podnikových řešení a využívat jejich výhod z cloudu.

Tento článek ukazuje, jak se předávání přes Service Bus může použít k vytvoření webové služby WCF vystavené pomocí vazeb kanálů TCP, které implementují zabezpečenou konverzaci mezi dvěma účastníky.

## Vytvoření oboru názvů služby

Pokud chcete začít používat předávání přes Service Bus v Azure, musíte nejdřív vytvořit obor názvů. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

Vytvoření oboru názvů služby:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## Získání balíčku Service Bus NuGet

Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svoji aplikaci se všemi závislostmi služby Service Bus, je balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Balíček NuGet můžete do své aplikace nainstalovat takto:

1.  V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.
2.  Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete následující dialogové okno.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)


## Vystavení a spotřebování webové služby SOAP přes Service Bus pomocí TCP

Pokud chcete existující webovou službu WCF SOAP vystavit pro externí spotřebu, musíte udělat několik změn v adresách a vazbách služby. K tomu může být potřeba změnit konfigurační soubor nebo kód, podle toho, jak máte služby WCF vytvořené a nastavené. Nezapomeňte, že vám WCF umožňuje mít několik koncových bodů sítě v jedné službě, abyste mohli zachovat stávající koncové body a zároveň přidat koncové body Service Bus pro externí přístup.

V tomto úkolu sestavíte jednoduchou službu WCF a přidáte k ní naslouchací proces Service Bus. Toto cvičení předpokládá, že umíte do jisté míry pracovat s Visual Studiem, a proto vás neprovede úplně všemi podrobnými kroky pro vytvoření projektu. Naopak se zaměřuje na kód.

Než začneme s kroky uvedenými dole, provedeme následující postup pro nastavení a nastavení prostředí:

1.  Ve Visual Studiu vytvořte konzolovou aplikaci, která v řešení bude obsahovat dva projekty – „Client“ a „Service“.
2.  Do obou projektů přidejte balíček NuGet Microsoft Azure Service Bus. Projekty tak budou mít všechny potřebné reference k sestavení.

### Jak vytvořit službu

Nejdřív vytvořte službu samotnou. Každá služba WCF se skládá nejméně ze tří různých částí:

-   Definice kontraktu, která popisuje, které zprávy se vyměňují a jaké operace se mají vyvolávat.
-   Implementace tohoto kontraktu.
-   Hostitel, které hostuje službu WCF a vystavuje určitý počet koncových bodů.

Příklady kódu v této části se vztahují na všechny části.

Kontrakt definuje jednu operaci `AddNumbers`, která sečte dvě čísla a vrátí výsledek. Rozhraní `IProblemSolverChannel` umožní klientovi snadnější správu doby platnosti proxy. Vytvoření takového rozhraní se obvykle považuje za vhodné řešení. Definici kontraktu se taky doporučuje dát do samostatného souboru, aby se mohla používat jako reference v projektech „Client“ i „Service“, ale kód můžete taky jednoduše zkopírovat do obou projektů.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Když je kontrakt hotový, jeho implementace je jednoduchá.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### Programová konfigurace hostitele služby

Když je kontrakt hotový a implementovaný, můžete hostovat službu. Hostování se provádí v objektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx), který se stará o správu instancí služby a hostuje koncové bodu, které čekají na zprávy. Následující kód konfiguruje službu s běžným místním koncovým bodem a koncovým bodem Service Bus a ilustruje tak rozdíly mezi interními a externími koncovými body. Řetězec *namespace* nahraďte názvem vašeho oboru názvů a *yourKey* klíčem SAS, které jste získali v předchozím kroku.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "yourKey")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

V příkladu vytvoříte dva koncové body, které jsou na stejné implementaci kontraktu. Jeden je místní a druhý je promítnutý přes Service Bus. Zásadním rozdíl mezi nimi jsou vazby: [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) pro místní koncový bod, [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) pro adresy a koncový bod Service Bus. Místní koncový bod má místní síťovou adresu s vlastním portem. Koncový bod Service Bus má adresu koncového bodu složenou z řetězce `sb`, názvu vašeho oboru názvů a cesty „solver“. Výsledkem je URI `sb://[serviceNamespace].servicebus.windows.net/solver`, který identifikuje koncový bod služby jako koncový bod Service Bus TCP s plně kvalifikovaným názvem externí DNS. Pokud do kódu vložíte skutečné názvy místo zástupných názvů, jak se popisuje v předchozím kroku, a vložíte ho do funkce `Main` aplikace **Service**, budete mít funkční službu. Pokud chcete, aby služba naslouchala jen na Service Bus, odstraňte deklaraci místního koncového bodu.

### Konfigurace hostitele služby v souboru App.config

Hostitele taky můžete konfigurovat pomocí souboru App.config. V tomto případě bude kód pro hostování vypadat jako kód v následujícím příkladu.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Definice koncových bodů se přesunou do souboru App.config. Nezapomeňte, že balíček NuGet už do souboru App.config přidal velké množství definic, které jsou potřeba pro rozšíření konfigurace pro Service Bus. Následující příklad je naprosto stejný jako předchozí kód a měl by být hned pod elementem **system.serviceModel**. Tento příklad kódu předpokládá, že se obor názvů C# vašeho projektu jmenuje **Service**.
Zástupné názvy nahraďte klíčem SAS a oborem názvů služby Service Bus.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Když provedete změny, služba se spustí jako předtím, ale tentokrát budou dva živé koncové body: jeden místní a jeden v cloudu.

### Vytvoření klienta

#### Programová konfigurace klienta

Aby se služba mohla spotřebovávat, musíte postavit klienta WCF pomocí objektu [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx). Service Bus používá tokenový model zabezpečení, implementovaný pomocí SAS. Třída [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření, které vracení některé známé poskytovatele tokenů. Dole uvedený příklad používá metodu [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) ke zpracování získání příslušného tokenu SAS. Název a klíč se získá z portálu způsobem popsaným v předchozí části.

Nejdřív zkopírujte nebo odkažte na kód kontraktu `IProblemSolver` ze služby do vašeho klientského projektu.

Pak nahraďte kód v metodě `Main` klienta, znovu nahraďte zástupný text klíčem SAS a názvem vašeho oboru názvů Service Bus.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","yourKey") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Teď můžete sestavit klienta a službu, spustit je (službu jako první) a klient zavolá službu a vypíše **9**. Klient a server můžete spustit na různých počítačích, dokonce i v jiných sítích, a komunikace bude stále fungovat. Kód klienta se taky může spustit v cloudu nebo lokálně.

#### Konfigurace klienta v souboru App.config

Následující kód ukazuje, jak nakonfigurovat klienta pomocí souboru App.config.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Definice koncových bodů se přesunou do souboru App.config. Následující příklad je stejný jako kód, který jsme už uvedli, a měl by být hned pod elementem **system.serviceModel**. Stejně jako v předchozím případě zástupné názvy nahraďte klíčem SAS a oborem názvů Service Bus.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## Další kroky

Naučili jste se základy předávání přes Service Bus, další informace se dozvíte na následujících odkazech.

- [Přehled přenosu zpráv přes předávací službu v Service Bus](service-bus-relay-overview.md)
- [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- Ukázky pro Service Bus si můžete stáhnout z [ukázek Azure][] nebo se můžete podívat na [přehled ukázek pro Service Bus][].

  [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu]: service-bus-shared-access-signature-authentication.md
  [ukázek Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [přehled ukázek pro Service Bus]: service-bus-samples.md


<!--HONumber=ago16_HO5-->


