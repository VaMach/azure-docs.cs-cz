<properties
    pageTitle="Kurz REST pro přenos zpráv přes předávací službu Service Bus | Microsoft Azure"
    description="Vytvořte jednoduchou hostitelskou aplikaci pro předávání přes Service Bus, která vystavuje rozhraní založené na REST."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/03/2016"
    ms.author="sethm" />

# Kurz REST pro Service Bus

Tento kurz popisuje, jak vytvořit jednoduchou hostitelskou aplikaci pro předávání přes Service Bus, která vystavuje rozhraní založené na REST. REST webovému klientovi, jako je třeba webový prohlížeč, umožňuje přístup k API pro Service Bus přes požadavky HTTP.

Teto kurz používá programovací model REST WCF (Windows Communication Foundation) k vytvoření služby v Service Bus. Další informace najdete v dokumentaci [Programovací model Rest WCF](https://msdn.microsoft.com/library/bb412169.aspx) a [Návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx).

## Krok 1: Vytvoření oboru názvů služby

Nejdřív je potřeba vytvořit obor názvů a získat klíč sdíleného přístupového podpisu (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby. Kombinace oboru názvů služby a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

1. Pokud chcete vytvořit obor názvů služby, přejděte na [klasického portálu Azure][]. V levé čísti klikněte na **Service Bus** a pak na **Vytvořit**. Zadejte název oboru názvů a pak klikněte na zatržítko.

2. V hlavním okně [klasického portálu Azure][] klikněte na název oboru názvů, který jste vytvořili v předchozím kroku.

3. Klikněte na kartu **Konfigurovat**.

4. V části **generátor sdíleného přístupového klíče** si poznamenejte **Primární klíč** přiřazený k zásadám **RootManagerSharedAccessKey** nebo ho zkopírujte do schránky. Tuto hodnotu použijete později v tomto kurzu.

## Krok 2: Definování kontraktu služby WCF na bázi REST pro použití se službou Service Bus

Podobně jako v případě ostatních služeb Service Bus musíte při vytvoření služby ve stylu REST definovat kontrakt. Kontrakt určuje, které operace hostitel podporuje. Operaci služby se můžeme představit jako metodu webové služby. Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic. Každá metoda v rozhraní odpovídá konkrétní operaci služby. Atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) se musí použít na každé rozhraní a atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) se musí použít na každou operaci. Pokud metoda v rozhraní, které má atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), nemá atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví. Kód použitý pro tyto úlohy je v následujícím příkladu za postupem.

Hlavní rozdíl mezi základním kontraktem Service Bus a kontraktem ve stylu REST je v přidání vlastnosti do atributu [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Tato vlastnost vám umožní mapovat metodu ve svém rozhraní k metodě na druhé straně rozhraní. V tomto případě použijeme [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) k propojení metody na HTTP GET. Službě Service Bus tak umožníme přesně získat a interpretovat příkazy odeslané na rozhraní.

### Vytvoření kontraktu Service Bus s rozhraním

1. Spusťte Visual Studio jako správce: v nabídce **Start** klikněte na program pravým tlačítkem a vyberte možnost **Spustit jako správce**.

2. Vytvořte nový projekt konzolové aplikace. Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**. V dialogovém okně **Nový projekt** klikněte na **Visual C#**, vyberte šablonu **Konzolová aplikace** ta zadejte jí název **ImageListener**. Použijte výchozí **Umístění**. Klikněte na **OK**, tím vytvoříte projekt.

3. Visual Studio pro projekt C# vytvoří soubor `Program.cs`. Tato třída obsahuje prázdnou metodu `Main()` potřebnou ke správnému sestavení projektu konzolové aplikace.

4. Nainstalujte balíček Service Bus NuGet, tím do projektu přidáte odkazy na Service Bus a **System.ServiceModel.dll**. Tento balíček automaticky přidá reference na knihovny Service Bus a WCF **System.ServiceModel**. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ImageListener** a pak klikněte na **Správa balíčků NuGet**. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Klikněte na **Instalovat** a přijměte podmínky použití.

5. Do projektu musíte explicitně přidat odkaz na **System.ServiceModel.Web.dll**:

    a. V Průzkumníku řešení klikněte ve složce projektu pravým tlačítkem na složku **References**, pak klikněte na **Přidat odkaz**.

    b. V dialogovém okně **Přidat odkaz** klikněte vlevo na kartu **Architektura** a do pole **Hledat** zadejte **System.ServiceModel.Web**. Označte zatržítko **System.ServiceModel.Web** a klikněte na **OK**.

6. Na začátek souboru Program.cs přidejte následující příkazy `using`.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který umožňuje programový přístup k základním funkcím WCF. Service Bus používá mnoho objektů a atributů WCF k definování kontraktů služby. Tento obor názvů budete používat ve většině svých aplikacích s předáváním Service Bus. Stejně tak [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomáhá definovat kanál – objekt, přes který komunikujete se Service Bus a klientským webovým prohlížečem. Nakonec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) obsahuje typy, které vám umožní vytvořit webové aplikace.

7. Přejmenujte obor názvů `ImageListener` na **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Přímo po otevření složené závorky deklarace oboru názvů definujte nové rozhraní s názvem **IImageContract** a aplikujte atribut **ServiceContractAttribute** na rozhraní s hodnotou `http://samples.microsoft.com/ServiceModel/Relay/`. Hodnota oboru názvů se liší od oboru názvů, které používáte v celém svém kódu. Hodnota oboru názvů se používá jako jedinečný identifikátor pro tento kontrakt a měla by mít informaci o verzi. Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498). Když explicitně zadáte obor názvů, zabráníte tím přidání výchozí hodnoty oboru názvů do názvu kontraktu.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. V rozhraní `IImageContract` deklarujte metodu pro jednu operaci, kterou kontrakt `IImageContract` vystaví v rozhraní, a aplikujte atribut `OperationContractAttribute` na metodu, kterou chcete vystavit v rámci veřejného kontraktu Service Bus.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. V atributu **OperationContract** přidejte hodnotu **WebGet**.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    To službě Service Bus umožní směrovat požadavky HTTP GET do `GetImage` a překládat vrácené hodnoty`GetImage` do odpovědi HTTP GETRESPONSE. Později v tomto kurzu pomocí webového prohlížeče získáte přístup k této metodě a zobrazíte v prohlížeči obrázek.

11. Přímo po definici `IImageContract` deklarujte kanál, který zdědí vlastnosti z rozhraní `IImageContract` i `IClientChannel`.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanál je objekt WCF, kterým si služba a klient navzájem posílají informace. Později vytvoříte kanál ve své hostitelské aplikaci. Service Bus pak pomocí tohoto kanálu předá požadavky HTTP GET z prohlížeče do vaší implementace **GetImage**. Service Bus taky pomocí tohoto kanálu vezme vrácenou hodnotu **GetImage** a přeloží ji do HTTP GETRESPONSE pro klientský webový prohlížeč.

12. V nabídce **Sestavení** klikněte na **Sestavit řešení** a zkontrolujte přesnost své dosavadní práce.

### Příklad

Následující kód ukazuje základní rozhraní, které definuje kontrakt Service Bus.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## Krok 3: Implementace kontraktu služby WCF na bázi REST pro použití služby Service Bus

K vytvoření služby Service Bus ve stylu REST je potřeba, abyste nejdřív vytvořili kontrakt, který se definuje pomocí rozhraní. Dalším krokem je implementace rozhraní. K tomu patří vytvoření třídy s názvem **ImageService**, která implementuje uživatelsky definované rozhraní **IImageContract**. Po implementaci kontraktu nakonfigurujete rozhraní pomocí souboru App.config. Tento konfigurační soubor obsahuje informace, které aplikace potřebuje, jako je třeba název služby, název kontraktu a typ protokolu použitého pro komunikaci se Service Bus. Kód použitý k těmto úlohám najdete v příkladu za postupem.

Podobně jako v předchozích krocích je jen malý rozdíl mezi implementací kontraktu ve stylu REST a základního kontraktu Service Bus.

### Implementace kontraktu Service Bus ve stylu REST

1. Vytvořte novou třídu s názvem **ImageService** přímo po definování rozhraní **IImageContract**. Třída **ImageService** implementuje rozhraní **IImageContract**.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Podobně jako u implementace jiných rozhraní můžete definici implementovat v jiném souboru. V tomto kurzu se ale implementace objeví ve stejném souboru jako definice rozhraní a metoda `Main()`.

2. Aplikujte atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) na třídu **IImageService** na znamení, že třída je implementace kontraktu WCF.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Jak jsme už zmínili, tento obor názvů není obvyklý obor názvů. Místo toho je součástí architektury WCF, která identifikuje kontrakt. Další informace najdete v tématu [Názvy datových kontraktů](https://msdn.microsoft.com/library/ms731045.aspx) v dokumentaci WCF.

3. Přidejte do svého projekt obrázek ve formátu .jpg.  

    To je obrázek, který služba zobrazí ve webovém prohlížeči příjemce. Klikněte pravým tlačítkem na projekt a klikněte na **Přidat**. Pak klikněte na **Existující položka**. V dialogovém okně **Přidat existující položku** vyberte vhodný soubor .jpg a klikněte na **Přidat**.

    Když přidáváte soubor, zkontrolujte, že je v rozbalovacím seznamu vedle pole**Název souboru:** vybraná možnost **Všechny soubory**. Zbytek tohoto kurzu předpokládá, že je název obrázku „image.jpg“. Pokud máte jiný soubor, budete ho muset přejmenovat nebo odpovídajícím způsobem upravit svůj kód.

4. Pokud chcete zkontrolovat, že běžící služba dokáže najít soubor obrázku, v **Průzkumníku řešení** klikněte pravým tlačítkem na soubor obrázku, pak klikněte na **Vlastnosti**. V podokně **Vlastnosti** nastavte **Kopírovat do výstupního adresáře** na **Kopírovat, pokud je novější**.

5. Přidejte do projektu odkaz na sestavení **System.Drawing.dll** a taky přidejte následující přidružené příkazy `using`.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Ve třídě **ImageService** přidejte následující konstruktor, který načte bitovou mapu a odešle ji do klientského prohlížeče.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Přímo za předchozí kód přidejte následující metodu **GetImage** ve třídě **ImageService**, aby se vrátila zpráva HTTP,která bude obsahovat obrázek.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Tato implementace používá **MemoryStream** k získání obrázku a jeho přípravu pro streamování do prohlížeče. Spustí pozici přenosu na nule, deklaruje obsah přenosu jako jpeg a přenese informaci.

8. V nabídce **Sestavení** klikněte na **Sestavit řešení**.

### Definování konfigurace pro spuštění webové služby v Service Bus

1. V **Průzkumníku řešení** poklikejte na **App.config** a otevře se v editoru Visual Studio.

    Soubor **App.config** připomíná konfigurační soubor WCF a obsahuje název služby, koncový bod (umístění, které služba Service Bus vystaví pro klienty a hostitele, aby přes něj mohli komunikovat) a vazbu (typ protokolu používaný pro komunikaci). Hlavní rozdíl je tu v tom, že nakonfigurovaný koncový bod služby odkazuje na vazbu [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx), která není součástí architektury .NET Framework.

2. XML element `<system.serviceModel>` je element WCF, který definuje jednu nebo víc služeb. Tady se používá k definování názvu služby a koncového bodu. Na konci elementu `<system.serviceModel>` (ale pořád ještě uvnitř `<system.serviceModel>`) přidejte element `<bindings>`, který má následující obsah. To definuje vazbu použitou v aplikaci. Můžete definovat víc vazeb, ale v tomto kurzu se definuje jen jedna.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Tento krok definuje vazbu Service Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) s typem **relayClientAuthenticationType** nastaveným na **None**. Toto nastavení naznačuje, že koncový bod, který používá tuto vazbu, nepotřebuje pověření klienta.

3. Za element `<bindings>` přidejte element `<services>`. Podobně jako u vazeb můžete v jednom konfiguračním souboru definovat několik služeb. V tomto kurzu ale definujete jen jednu.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Tento krok konfiguruje službu, která použije předtím nastavenou výchozí vazbu **webHttpRelayBinding**. Taky používá výchozí ho poskytovatele **sbTokenProvider**, který se definuje v dalším kroku.

4. Po elementu `<services>` vytvořte element `<behaviors>` s následujícím obsahem, který nahradí „SAS_KEY“ klíčem *Sdíleného přístupového podpisu*, který jste získali z [klasického portálu Azure][] v kroku 1.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Ještě v souboru App.config v elementu `<appSettings>` nahraďte celou hodnotu připojovacího řetězce element připojovacím řetězcem, který jste předtím získali z portálu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. V nabídce **Sestavení** klikněte na **Sestavit řešení** a sestavte celé řešení.

### Příklad

Následující kód ukazuje implementaci kontraktu a služby pro službu založenou na REST, která běží ve službě Service Bus pomocí vazby **WebHttpRelayBinding**.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Následující příklad ukazuje soubor App.config přidružený ke službě.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## Krok 4: Hostování služby WCF na bázi REST pro použití služby Service Bus

Tento krok popisuje, jak spustit webovou službu pomocí konzolové aplikace ve službě Service Bus. Úplný kód napsaný v tomto kroku najdete v příkladu za postupem.

### Vytvoření bázové adresy pro tuto službu

1. V deklaraci funkce `Main()` vytvořte proměnnou pro uložení oboru názvů vašeho projektu Service Bus. Ujistěte se, že jste `yourNamespace` nahradili názvem oboru názvů služby, který jste předtím vytvořili.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus použije název vašeho oboru názvů k vytvoření jedinečné URI.

2. Vytvořte instanci `Uri` pro bázovou adresu služby, která je založená na tomto oboru názvů.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### Vytvoření a konfigurace hostitele webové služby

- Vytvořte hostitele webové služby pomocí adresy URI, kterou jste předtím vytvořili v této části.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hostitel služby je objekt WCF, který instancuje hostitelskou aplikaci. Tento příklad jí předá typ hostitele, kterého chcete vytvořit (**ImageService**) a taky adresu, na které chcete hostitelskou aplikaci vystavit.

### Spuštění hostitele webové služby

1. Otevřete službu.

    ```
    host.Open();
    ```
    Služba teď běží.

2. Zobrazte zprávu oznamující, že zpráva běží, a jak službu zastavit.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Po dokončení zavřete hostitele služby.

    ```
    host.Close();
    ```

## Příklad

Následující příklad obsahuje kontrakt a implementaci služby z předchozích kroků tohoto kurzu a hostuje službu v konzolové aplikaci. Zkompilujte následující kód do spustitelného souboru s názvem ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### Zkompilování kódu

Po sestavení řešení proveďte následující kroky pro spuštění aplikace:

1. Spusťte službu stisknutím klávesy**F5** nebo přejděte k umístění spustitelného souboru (ImageListener\bin\Debug\ImageListener.exe) a spusťte ho. Nechte aplikaci spuštěnou, protože je potřeba při dalším kroku.

2. Zkopírujte a vložte adresu z příkazového řádku do prohlížeče, zobrazí se obrázek.

3. Když skončíte, v okně příkazovém řádku stiskněte **Enter** a aplikace se zavře.

## Další kroky

Teď, když jste sestavili aplikaci, která používá předávací službu Service Bus, se podívejte na následující články, kde se dozvíte další informace o přenosu zpráv přes předávací službu.

- [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md#relays)

- [Jak používat službu předávání přes Service Bus](service-bus-dotnet-how-to-use-relay.md)

[klasického portálu Azure]: http://manage.windowsazure.com


<!--HONumber=Jun16_HO2-->


