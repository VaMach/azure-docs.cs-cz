<properties 
    pageTitle="Kurz přenosu zpráv přes předávací službu Service Bus | Microsoft Azure"
    description="Sestavte službu a klientskou aplikaci služby Service Bus, která používá přenos zpráv přes předávací službu Service Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="05/17/2016"
    ms.author="sethm" />

# Kurz přenosu zpráv přes předávací službu v Service Bus

Tento kurz ukazuje, jak sestavit jednoduchou službu a klientskou aplikaci služby Service Bus, která používá funkce „předávací službu“ Service Bus. Odpovídající kurz, který používá [zprostředkované zasílání zpráv](service-bus-messaging-overview.md#Brokered-messaging) přes Service Bus, najdete v článku [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus](service-bus-brokered-tutorial-dotnet.md).

Při absolvování tohoto kurzu pochopíte kroky, které jsou potřeba pro vytvoření klienta a aplikaci služby Service Bus. Podobně jako v případě služby WCF je služba Service Bus konstrukt, který vystavuje jeden nebo víc koncových bodů, a každý z nich vystavuje jednu nebo víc operací služeb. Koncový bod služby specifikuje adresu, kde se dá služba najít, vazbu, která obsahuje informaci, že klient musí komunikovat se službou, a kontrakt, který definuje funkci, kterou služba klientovi poskytuje. Hlavní rozdíl mezi službou WCF a službou Service Bus je v tom, že je koncový bod vystavený v cloudu a ne místně na vašem počítači.

Po dokončení řady témat v tomto kurzu budete mít funkční službu a klienta, který může vyvolat operace této služby. První téma popisuje, jak vytvořit účet. V dalších krocích se popisuje, jak definovat službu, která používá kontrakt, jak tuto službu implementovat a jak službu konfigurovat pomocí kódu. Taky se v nich popisuje, jak hostovat a spustit službu. Vytvořená služba se hostuje sama a klient a služba běží na stejném počítači. Službu můžete konfigurovat pomocí kódu nebo konfiguračního souboru.

Poslední tři kroky popisují, jak vytvořit klientskou aplikaci, nakonfigurovat klientskou aplikaci a vytvořit a použít klienta, který může přistupovat k funkcím hostitele.

Všechna témata v této části předpokládají, že jako vývojové prostředí používáte Visual Studio.

## Registrace účtu

Prvním krokem je vytvoření oboru názvů služby a získání klíče sdíleného přístupového podpisu (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Kombinace oboru názvů a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

1. Pokud chcete vytvořit obor názvů, přejděte na [klasický portál Azure][]. V levé čísti klikněte na **Service Bus** a pak na **Vytvořit**. Zadejte název oboru názvů, přijměte výchozí nastavení ostatních hodnot, a pak klikněte na zatržítko OK.

    >[AZURE.NOTE] Pro klientskou aolikaci a aplikaci služby nemusíte používat stejný obor názvů.

    ![][4]

1. V hlavním okně portálu klikněte na název oboru názvů, který jste vytvořili v předchozím kroku.

2. Klikněte na kartu **Konfigurovat** a zobrazí se výchozí zásady sdíleného přístupu pro váš obor názvů.

    ![][1]

3. Poznamenejte si primární klíč pro zásady **RootManagerSharedAccessKey** nebo ho zkopírujte do schránky. Tuto hodnotu použijete později v tomto kurzu.

## Definování kontraktu služby WCF pro použití se službou Service Bus

Kontrakt služby specifikuje, jaké operace (termín webových služeb pro metody nebo funkce) služba podporuje. Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic. Každá metoda v rozhraní odpovídá konkrétní operaci služby. Na každé rozhraní musí mít aplikovaný atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) a na každou operace musí byt aplikovaný atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx). Pokud metoda v rozhraní, které má atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), nemá atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví. Kód k těmto úlohám najdete v příkladu za postupem. Podrobnější diskuzi o kontraktech a službách najdete v dokumentaci WCF v části [Návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx).

### Vytvoření kontraktu Service Bus s rozhraním

1. Otevřete Visual Studio jako správce tak, že v nabídce **Start** kliknete na program pravým tlačítkem a vyberete možnost **Spustit jako správce**.

2. Vytvořte nový projekt konzolové aplikace. Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**. V dialogu **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**). Klikněte na šablonu **Konzolová aplikace** a pojmenujte ji jako **EchoService**. Klikněte na **OK**, tím vytvoříte projekt.

    ![][2]

3. Nainstalujte balíček Service Bus NuGet. Tento balíček automaticky přidá reference na knihovny Service Bus a WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který vám umožňuje programový přístup k základním funkcím WCF. Service Bus používá mnoho objektů a atributů WCF k definování kontraktů služby.

    V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet pro řešení**. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Zkontrolujte, že je v části **Verze** označený název projektu. Klikněte na **Instalovat** a přijměte podmínky použití.

    ![][3]

3. V Průzkumníku řešení poklikejte na soubor Program.cs a pokud ještě není otevřený, otevře se v editoru Visual Studio.

4. Na začátek souboru přidejte následující příkazy:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Změňte název oboru názvů z výchozího názvu **EchoService** na **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Tento kurz používá obor názvů C# **Microsoft.ServiceBus.Samples**, to je obor názvů typu spravovaného kontraktem, která se použije v konfiguračním souboru v kroku [Konfigurace klienta WCF](#configure-the-wcf-client). Při sestavování této ukázky můžete specifikovat jakýkoli obor názvů, který chcete – tento kurz ale bude fungovat jen tehdy, když odpovídajícím způsobem v konfiguračním souboru aplikace upravíte obor názvů kontraktu a služby. Obor názvů specifikovaný v souboru App.config musí být stejný jako obor názvů zadaný ve vašich souborech C#.

1. Přímo po deklaraci oboru názvů `Microsoft.ServiceBus.Samples`, ale uvnitř oboru názvů, definujte nové rozhraní s názvem `IEchoContract` a aplikujte atribut `ServiceContractAttribute` a rozhraní s hodnotou oboru názvů **http://samples.microsoft.com/ServiceModel/Relay/**. Hodnota oboru názvů se liší od oboru názvů, které používáte v celém svém kódu. Místo toho se obor názvů používá jako jedinečný identifikátor pro tento kontrakt. Když explicitně zadáte obor názvů, zabráníte tím přidání výchozí hodnoty oboru názvů do názvu kontraktu.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Obor názvů kontraktu služby obvykle obsahuje schéma pojmenování s informacemi o verzi. Informace o verzi, které jsou v oboru názvů kontraktu služby, službám umožňuje službám izolovat výrazné změny pomocí definice nové služby s novým oborem názvů, která bude vystavená na novém koncovém bodu. Tímto způsobem můžou klienti pokračovat v používání starého kontraktu služby bez nutnosti ho aktualizovat. Informace o verzi může mít podobu data nebo čísla sestavení. Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498). Pro účely tohoto kurzu nebude mít schéma pojmenování oboru názvů kontraktu služby žádné informace o verzi.

1. V rozhraní `IEchoContract` deklarujte metodu pro jednu operaci, kterou kontrakt `IEchoContract` vystaví v rozhraní, a aplikujte atribut `OperationContractAttribute` na metodu, kterou chcete vystavit v rámci veřejného kontraktu Service Bus.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Přímo po definici rozhraní `IEchoContract` deklarujte kanál, který zdědí vlastnosti z obou `IEchoContract` a také do rozhraní `IClientChannel`, jak je vidět tady:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanál je objekt WCF, kterým si hostitel a klient navzájem posílají informace. Později napíšete kód na kanál, aby se informace zrcadlily mezi oběma aplikacemi.

1. V nabídce **Sestavení** můžete kliknout na **Sestavit řešení** nebo stisknout **Ctrl+Shift+B** a potvrdit přesnost své dosavadní práce.

### Příklad

Následující kód ukazuje základní rozhraní, které definuje kontrakt Service Bus.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Když je teď vytvořené rozhraní, můžete ho implementovat.

## Implementujte kontrakt WFG pro využívání služby Service Bus.

K vytvoření předávací službu Service Bus je potřeba, abyste nejdřív vytvořili kontrakt, který se definuje pomocí rozhraní. Další informace o vytváření rozhraní najdete v předchozím kroku. Dalším krokem je implementace rozhraní. K tomu patří vytvoření třídy s názvem `EchoService`, která implementuje uživatelsky definované rozhraní `IEchoContract`. Po implementaci rozhraní nakonfigurujete rozhraní pomocí souboru App.config. Tento konfigurační soubor obsahuje informace, které aplikace potřebuje, jako je třeba název služby, název kontraktu a typ protokolu použitého pro komunikaci se Service Bus. Kód použitý k těmto úlohám najdete v příkladu za postupem. Obecnější diskuzi o způsobu implementace kontraktu služby najdete v dokumentaci WCF [Implementace kontraktů služby](https://msdn.microsoft.com/library/ms733764.aspx).

1. Vytvořte novou třídu s názvem `EchoService` přímo po definování rozhraní `IEchoContract`. Třída `EchoService` implementuje rozhraní `IEchoContract`. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Podobně jako u implementace jiných rozhraní můžete definici implementovat v jiném souboru. V tomto kurzu je ale implementace ve stejném souboru jako definice rozhraní a metoda `Main`.

1. Na rozhraní `IEchoContract` aplikujte atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx). Atribut specifikuje název služby a obor názvů. Když to dokončíte, třída `EchoService` bude vypadat takto:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementujte metodu `Echo` definovanou v rozhraní `IEchoContract` ve třídě `EchoService`. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Klikněte na **Sestavení**, klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.

### Definice konfigurace hostitele služby

1. Konfigurační soubor je velmi podobný konfiguračnímu souboru WCF. Obsahuje název služby, koncový bod (umístění, které služba Service Bus vystaví pro klienty a hostitele, aby přes něj mohli komunikovat) a vazbu (typ protokolu používaný pro komunikaci). Hlavní rozdíl je v tom, že nakonfigurovaný koncový bod služby odkazuje na vazbu [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx), která není součástí architektury .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) je jedna z vazeb definovaných prostředím Service Bus.

1. V **Průzkumníku řešení** poklikejte na soubor App.config a otevře se v editoru Visual Studio.

2. V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků. 

1. Ve značkách `<system.serviceModel>` přidejte element `<services>`. V jednom konfiguračním souboru můžete definovat několik aplikací Service Bus. V tomto kurzu se ale definuje jen jedna.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. V elementu `<services>` přidejte element `<service>`, který definuje název služby.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. V elementu `<service>` definujte umístění koncového bodu kontraktu a také vazbu koncového bodu.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Koncový bod definuje, kde bude klient hledat hostitelskou aplikaci. Tento kurz později použije tento krok k vytvoření adresu URI, která plně vystavuje hostitele přes Service Bus. Vazba deklaruje, že používáme protokol TCP ke komunikaci se Service Bus.

1. V nabídce **Sestavení** klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.

### Příklad

Následující kód ukazuje implementaci kontraktu služby.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Následující kód ukazuje základní formát souboru App.config přidruženého k hostiteli služby.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Hostování a spuštění základní webové služby pro registraci se Service Bus

Tento krok popisuje, jak spustit základní službu Service Bus.

### Vytvoření pověření Service Bus

1. V `Main()` vytvořte dvě proměnné, do kterých se uloží obor názvů a klíč SAS načtené z okna konzoly.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Klíč SAS se později použije ve vašem projektu Service Bus. Obor názvů se předá do `CreateServiceUri` jako parametr a vytvoří se URI služby.

4. Pomocí objektu [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) deklarujte, že jako typ pověření budete používat klíč SAS. Následující kód přidejte přímo za kód, který jste přidali v předchozím kroku.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### Vytvoření bázové adresy pro tuto službu

1. Za kód, který jste přidali v posledním kroku, vytvořte instanci `Uri` pro bázovou adresu služby. Toto URI specifikuje schéma Service Bus, obor názvů a cestu rozhraní služby.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    „sb“ je zkratka schématu Service Bus a indikuje, že používáme protokol TCP. To jsme předtím indikovali v konfiguračním souboru, když jsme specifikovali [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jako vazbu.
    
    V tomto kurzu je URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### Vytvoření a konfigurace hostitele služby

1. Nastavte režim připojení na `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Režim připojení popisuje, protokol, který služba používá ke komunikaci se Service Bus – a to buď HTTP nebo TCP. Pomocí výchozího nastavení `AutoDetect` se služba pokusí připojit k Service Bus přes TCP, pokud je dostupné, a přes HTTP, pokud TCP není dostupné. Všimněte si, že tu je rozdíl oproti protokolu, který služba specifikuje pro komunikaci klienta. Jeho protokol se určuje podle požité vazby. Služba může například použít vazbu [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx), která specifikuje, že její koncový bod (vystavený v Service Bus) s klienty komunikuje přes HTTP. Stejná služba by mohla specifikovat **ConnectivityMode.AutoDetect**, aby služba komunikovala se Service Bus přes TCP.

1. Vytvořte hostitele služby pomocí URI, které jste předtím vytvořili v této části.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hostitel služby je objekt WCF, který instancuje službu. Tady jí předáte typ služby, kterou chcete vytvořit (typ `EchoService`) a taky adresu, na které chcete službu vystavit.

1. Na začátku souboru Program.cs přidejte odkazy na [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Zpátky v `Main()` nakonfigurujte koncový bod, a povolte tak veřejný přístup.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Tento krok informuje Service Bus, že se vaše aplikace dá veřejně najít pomocí kanálu ATOM služby Service Bus vašeho projektu. Pokud **DiscoveryType** nastavíte na **private**, služba by pro klienta pořád byla dostupná. Neobjevila by se ale při prohledávání oboru názvů Service Bus. Místo toho by klient musel předem znát cestu ke koncovému bodu.

1. Použijte pověření služby na koncové body služby definované v souboru App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak jsme uvedli v předchozím kroku, mohli jste v konfiguračním souboru deklarovat několik služeb a koncových bodů. Pokud byste to udělali, tento kód by prošel konfigurační soubor a vyhledal by všechny koncové body, na které by měl vaše pověření použít. V tomto kurzu má ale konfigurační soubor jen jeden koncový bod.

### Otevření hostitele služby

1. Otevřete službu.

    ```
    host.Open();
    ```

1. Informujte uživatele, že zpráva běží, a vysvětlete mu, jak službu ukončit.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Po dokončení zavřete hostitele služby.

    ```
    host.Close();
    ```

1. Stisknutím kláves **CTRL+SHIFT+B** sestavte projekt.

### Příklad

Následující příklad obsahuje kontrakt a implementaci služby z předchozích kroků tohoto kurzu a hostuje službu v konzolové aplikaci. Zkompilujte následující kód do spustitelného souboru s názvem EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## Vytvoření klienta WCF pro kontrakt služby

Dalším krokem je vytvoření základní klientské aplikace Service Bus a definování kontraktu služby, který implementujete v pozdějších krocích. Všimněte si, že hodně těchto kroků připomíná kroky pro vytvoření služby: definování kontraktu, upravení souboru App.config, připojení k Service Bus pomocí pověření a tak dál. Kód použitý k těmto úlohám najdete v příkladu za postupem.

1. Vytvořte pro klienta nový projekt v aktuálním řešení ve Visual Studiu, a to tímto postupem:
    1. V Průzkumníku řešení ve stejném řešení, které obsahuje službu, klikněte pravým tlačítkem na aktuální řešení (nikoli projekt) a klikněte na **Přidat**. Pak klikněte na **Nový projekt**.
    2. V dialogovém okně **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**), klikněte na šablonu **Konzolová aplikace** a pojmenujte ji jako **EchoClient**.
    3. Klikněte na tlačítko **OK**.
<br />

1. V Průzkumníku řešení poklikejte na soubor Program.cs v projektu **EchoClient** a pokud ještě není otevřený, otevře se v editoru Visual Studio.

1. Změňte název oboru názvů z výchozího názvu `EchoClient` na `Microsoft.ServiceBus.Samples`.

1. Nainstalujte [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). V Průzkumníku řešení klikněte pravým tlačítkem na projekt **EchoClient** a pak klikněte na **Správa balíčků NuGet**. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Klikněte na **Instalovat** a přijměte podmínky použití.

    ![][3]

1. V souboru Program.cs přidejte příkaz `using` pro obor názvů [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx). 

    ```
    using System.ServiceModel;
    ```

1. Přidejte definici kontraktu služby do oboru názvů, jak je vidět v následujícím příkladu. Všimněte si, že je tato definice skoro stejná jako definice použitá v projektu **Service**. Tento kód byste měli přidat na začátek oboru názvů `Microsoft.ServiceBus.Samples`.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Stisknutím kláves **CTRL+SHIFT+B** sestavte klienta.

### Příklad

Následující kód ukazuje aktuální stav souboru Program.cs v projektu EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## Konfigurace klienta WCF

V tomto kroku vytvoříte soubor App.config pro základní klientskou aplikaci, která bude mít přístup ke službě vytvořené v jednom z předchozích kroků tohoto kurzu. Tento soubor App.config definuje kontrakt, vazbu a název koncového bodu. Kód použitý k těmto úlohám najdete v příkladu za postupem.

1. V Průzkumníku řešení v projektu **EchoClient** poklikejte na **App.config.cs** a otevře se v editoru Visual Studio.

2. V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků.

1. V elementu system.serviceModel element přidejte element `<client>`.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tento krok deklaruje, že definujete klientskou aplikaci ve stylu WCF.

1. V elementu `client` definujte název, kontrakt a typ vazby koncového bodu.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Tento krok definuje název koncového bodu, kontrakt definovaný ve službě, a fakt, že klientská aplikace používá TCP ke komunikaci se Service Bus. Název koncového bodu se použije v následujícím kroku k propojení této konfigurace koncového bodu s URI služby.

1. Klikněte na **Soubor** a pak na **Uložit vše**.

## Příklad

Následující kód ukazuje soubor App.config pro klienta Echo.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Implementace klienta WFG pro volání služby Service Bus

V tomto kroku implementujete základní klientskou aplikaci, která bude mít přístup ke službě, kterou jste vytvořili v jednom z předchozích kroků tohoto kurzu. Klient při přístupu k Service Bus klient provádí spoustu stejných operací jako služba:

1. Nastaví režim připojení.
1. Vytvoří URI, které vyhledá hostitelskou službu.
1. Definuje bezpečnostní pověření.
1. Aplikuje pověření na připojení.
1. Otevře připojení.
1. Provádí úlohy specifické pro aplikace.
1. Ukončí připojení.

Jedním z hlavních rozdílů je ale, že klientská aplikace pro připojení k Service Bus používá kanál, zatímco služba používá volání na **ServiceHost**. Kód použitý k těmto úlohám najdete v příkladu za postupem.

### Implementace klientské aplikace

1. Nastavte režim připojení na **AutoDetect**. Do metody `Main()` aplikace **EchoClient** přidejte následující kód.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definujte proměnné, do kterých se uloží hodnoty obor názvů služby a klíče SAS načtené z konzoly.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Vytvořte URI, která definuje umístění hostitele ve vašem projektu Service Bus.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Vytvořte objekt pověření pro koncový bod vašeho oboru názvů.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Vytvořte objekt kanálu pro vytváření, který načte konfiguraci popsanou v souboru App.config.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Objekt kanálu pro vytváření je objekt WCF, který vytvoří kanál, přes který může služba komunikovat s klientskými aplikacemi.

1. Použijte pověření Service Bus.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Vytvořte a otevřete kanál pro službu.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Napište základní uživatelské prostředí a funkci pro echo.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Všimněte si, že kód používá instanci objektu kanálu jako proxy pro službu.

1. Zavřete kanál a zavřete objekt pro vytváření.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## Spuštění aplikací

1. Stisknutím kláves **CTRL+SHIFT+B** sestavte řešení. Sestavíte tím projekt klienta a projekt služby, které jste vytvořili v předchozích krocích.

2. Než spustíte klientskou aplikaci, musíte se ujistit, že aplikace služby běží. V Průzkumníku řešení ve Visual Studiu klikněte pravým tlačítkem na řešení **EchoService**, pak klikněte na **Vlastnosti**.

3. V dialogovém okně vlastností řešení klikněte na **Spouštěný projekt** a pak klikněte na tlačítko **Více projektů po spuštění**. Ujistěte se, že se **EchoService** v seznamu objeví jako první. 

4. V poli **Akce** u projektů **EchoService** i **EchoClient** nastavte **Start**.

    ![][5]

5. Klikněte na **Závislosti projektu**. V poli **Projekty** klikněte na **EchoClient**. Ujistěte se, že je v poli **Závisí na** označené **EchoService**.

    ![][6]

6. Klikněte na **OK**, a dialog **Vlastnosti** se zavře.

7. Stiskněte klávesu **F5** a oba projekty se spustí.

8. Obě okna konzoly se otevřou a požádají vás o zadání oboru názvů. Nejdřív se musí spustit služba, takže v okně konzoly **EchoService** zadejte obor názvů, a pak stiskněte **Enter**.

9. Pak se zobrazí výzva k zadání klíče SAS. Zadejte klíč SAS a stiskněte Enter.

    Tady je příklad výstupu z okna konzoly. Nezapomeňte, tyto zadané hodnoty platí jen pro tento příklad.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Aplikace služby do okna konzoly vypíše adresu, na které naslouchá, jak je vidět na následujícím příkladu.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. V okně konzoly **EchoClient** zadejte stejný údaj, který jste zadali pro aplikaci služby. Stejným postupem jako v předchozích krocích zadejte stejný obor názvů a klíč SAS pro klientskou aplikaci.

11. Po zadání těchto hodnot klient otevře kanál ke službě a zobrazí se výzva k zadání nějakého textu, jak je vidět v následujícím příkladu výstupu konzoly.

    `Enter text to echo (or [Enter] to exit):` 

    Zadejte nějaký text, který se má odeslat do aplikace služby, a stiskněte Enter. Tento text se odešle do služby pomocí operace služby Echo a objeví se v okně konzoly služby, jak je vidět v následujícím příkladu výstupu.

    `Echoing: My sample text`

    Klientská aplikace obdrží hodnotu vrácenou z operace `Echo`. Tou je původní text, který se vypíše do okna konzoly. Následující příklad ukazuje výstup okna konzoly klienta.

    `Server echoed: My sample text`

12. Tímto způsobem můžete dál posílat textové zprávy z klienta do služby. Když skončíte, stiskněte Enter v oknech konzoly klienta a služby a obě aplikace se ukončí.

## Příklad

Následující příklad ukazuje, jak vytvořit klientskou aplikaci, jak volat operace služby a jak zavřít klienta po dokončení volání operace.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## Další kroky

Tento kurz vám ukázal, jak sestavit službu a klientskou aplikaci služby Service Bus, které používají funkce „předávací služby“ Service Bus. Podobný kurz týkající se [zprostředkovaného zasílání zpráv](service-bus-messaging-overview.md#Brokered-messaging) přes Service Bus najdete v článku [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus](service-bus-brokered-tutorial-dotnet.md).

Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující témata.

- [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
- [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Architektura služby Service Bus](service-bus-architecture.md)

[klasický portál Azure]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png



<!--HONumber=Jun16_HO2-->


