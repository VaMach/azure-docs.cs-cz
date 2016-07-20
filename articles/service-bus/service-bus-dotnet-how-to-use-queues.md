<properties
    pageTitle="Jak používat fronty Service Bus (.NET) | Microsoft Azure"
    description="Naučte se používat fronty Service Bus v Azure. Ukázky kódu jsou vytvořené v C# s použitím .NET API."
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
    ms.date="05/09/2016"
    ms.author="sethm"/>

# Jak používat fronty Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak používat fronty Service Bus. Ukázky jsou vytvořené v C# a používají .NET API. Mezi popsané scénáře patří i vytváření front a přijímání zpráv. Další informace o frontách najdete v části [Další kroky](#next-steps).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Přidání balíčku Service Bus NuGet

Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svoji aplikaci se všemi závislostmi služby Service Bus, je balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Balíček NuGet můžete do své aplikace nainstalovat takto:

1.  V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.
2.  Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.

    ![][7]

Teď jste připravení psát kód pro službu Service Bus.

## Vytvoření připojovacího řetězce pro Service Bus

Service Bus používá připojovací řetězec k uložení koncových bodů a pověření. Připojovací řetězec nemusíte kódovat napevno, můžete ho přidat do konfiguračního souboru:

- Pokud používáte Azure Cloud Services, doporučuje se připojovací řetězec uložit pomocí konfiguračního systému služby Azure (soubory .csdef a .cscfg).
- Pokud používáte Weby Azure nebo Azure Virtual Machines, doporučuje se připojovací řetězec uložit pomocí konfiguračního systému .NET (soubory .csdef a .cscfg).

V obou případech můžete načíst váš připojovací řetězec pomocí metody [CloudConfigurationManager.GetSetting][GetSetting] jak uvidíte později v tomto článku.

### Konfigurace připojovacího řetězce

Konfigurační mechanizmus služby vám umožní dynamicky měnit konfiguraci z [klasického portálu Azure][] bez opětovného nasazení aplikace. Například přidejte označení `Setting` do souboru definice služby (.csdef), jak je vidět v dalším příkladu.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```
Potom zadejte hodnoty do souboru definice služby (.csdef), jak je vidět v dalším příkladu.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Použijte název klíče Sdíleného přístupového podpisu (SAS) a hodnoty klíče získané z klasického portálu Azure, jak je popsané v předchozí části.

### Konfigurace připojovacího řetězce při použití webů nebo Azure Virtual Machines

Pokud používáte weby nebo Virtual Machines, doporučuje se použít konfigurační systém .NET (například **Web.config**). Připojovací řetězec uložíte pomocí prvku `<appSettings>`.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Použijte název klíče SAS a hodnoty klíče získané z klasického portálu Azure, jak je popsané v předchozí části.

## Vytvoření fronty

Můžete provádět operace správy front služby Service Bus pomocí třídy [NamespaceManager][]. Tato třída poskytuje metody pro vytvoření, výčet a odstranění front.

Tento příklad vytvoří objekt [NamespaceManager][] pomocí třídy Azure [CloudConfigurationManager][] s připojovacím řetězcem složeným z bázové adresy oboru názvů služby Service Bus a příslušného pověření SAS s oprávněními k její správě. Tento připojovací řetězec má stejnou formu jako je uvedené v následujícím příkladu.

````
Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedSecretValue=yourKey
````

Použijte následující příklad s konfiguračními nastaveními v předchozí části.

```
// Create the queue if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue("TestQueue");
}
```

Existují přetížení metody [CreateQueue](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createqueue.aspx), které vám umožní ladit vlastnosti fronty (například nastavit výchozí čas TTL, který se použije na zprávy odeslané do fronty). Tato nastavení se aplikují pomocí třídy [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx). Následující příklad ukazuje, jak vytvořit frontu s názvem `TestQueue`, maximální velikostí 5 GB a výchozím časem TTL pro zprávu 1 minuta.

```
// Configure queue settings.
QueueDescription qd = new QueueDescription("TestQueue");
qd.MaxSizeInMegabytes = 5120;
qd.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new queue with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue(qd);
}
```

> [AZURE.NOTE] Můžete použít metodu [QueueExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.queueexists.aspx) na objekty [NamespaceManager][] pro kontrolu, jestli už v daném oboru názvů služby existuje fronta se zadaným názvem.

## Zasílání zpráv do fronty

Pokud chcete odeslat zprávu do fronty Service Bus, vaše aplikace vytvoří objekt [QueueClient][] pomocí připojovacího řetězce.

Následující kód ukazuje, jak vytvořit objekt [QueueClient][] pro frontu `TestQueue`, kterou jste právě vytvořili pomocí volání API [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx).

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

Client.Send(new BrokeredMessage());
```

Zprávy odeslané do a přijaté z front Service Bus jsou instance třídy [BrokeredMessage][]. Objekty [BrokeredMessage][] mají sadu standardních vlastností (jako třeba[Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), slovník používaný pro udržení vlastních vlastností aplikace a tělo s libovolnými aplikačními daty. Aplikace může tělo zprávy nastavit tak, že do konstruktoru objektu [BrokeredMessage][] předá jakýkoli serializovatelný objekt, a pak se k serializaci objektu použije vhodný **DataContractSerializer**. Nebo můžete poskytnout objekt **System.IO.Stream**.

Následující příklad ukazuje, jak odeslat pět zkušebních zpráv do objektu `TestQueue` [QueueClient][] získaného v předcházejícím příkladu.

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set some addtional custom app-specific properties.
  message.Properties["TestProperty"] = "TestValue";
  message.Properties["Message number"] = i;

  // Send message to the queue.
  Client.Send(message);
}
```

Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md). Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB. Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je. Velikost fronty se definuje při vytvoření, maximální limit je 5 GB. Pokud je zapnutá segmentace, je maximální limit vyšší. Další informace najdete v tématu [Segmentované entity zasílání zpráv](service-bus-partitioning.md).

## Jak přijmout zprávy z fronty

Doporučený způsob přijímání z fronty je použití objektu [QueueClient][]. Objekty [QueueClient][] můžou pracovat ve dvou různých režimech: [ReceiveAndDelete a PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Při použití režimu **ReceiveAndDelete** je přijetí jednorázová operace – tzn. když Service Bus přijme požadavek na čtení zprávy ve frontě, označí zprávu jako spotřebovanou a vrátí ji do aplikace. **ReceiveAndDelete** je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat možnost, že v případě selhání se zpráva nezpracuje. Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se. Service Bus už ale zprávu označila jako spotřebovanou. Když se aplikace znovu spustí a začne znovu přijímat zprávy, zprávu, která se spotřebovala před pádem aplikace, vynechá.

V režimu **PeekLock** (výchozím režimu) je přijetí dvoufázový proces, který podporuje aplikace, které nemůžou tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace. Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), zavolá na přijatou zprávu [Complete][], a tím potvrdí dokončení druhé fáze přijetí. Když Service Bus uvidí volání [Complete][], označí zprávu jako spotřebovanou a odstraní ji z fronty.

Následující příklad ukazuje, jak se zprávy můžou přijímat a zpracovávat pomocí výchozího režimu **PeekLock**. Pokud chcete nastavit jinou hodnotu [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx), můžete použít další přetížení [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx). Tento příklad používá zpětné volání [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) ke zpracování zpráv, když dorazí do `TestQueue`.

```
string connectionString =
  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
QueueClient Client =
  QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

// Callback to handle received messages.
Client.OnMessage((message) =>
{
    try
    {
        // Process message from queue.
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Test Property: " +
        message.Properties["TestProperty"]);

        // Remove message from queue.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in queue.
        message.Abandon();
    }
}, options);
```

Tento příklad konfiguruje zpětné volání [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) pomocí objektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx). [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) je nastavené na **false**, tím se povolí ruční kontrola nad tím, kdy se má na přijatou zprávu zavolat [Complete][]. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) je nastavené na 1 minutu a to způsobí, že klient může na zprávu čekat až 1 minutu, než časový limit vyprší a klient odešle nové volání na kontrolu nových zpráv. Tato hodnota vlastnosti snižuje počet, kolikrát klient provede fakturovatelná volání, která nepřijmou žádné zprávy.

## Zpracování pádů aplikace a nečitelných zpráv

Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace z nějakého důvodu nedokáže zpracovat zprávu, může na přijatou zprávu zavolat metodu [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) (namísto metody [Complete][]). To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, buďto stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.

Zpráva uzamčená ve frontě má taky časový limit a pokud aplikace zprávu nezpracuje v časovém limitu uzamčení (například pokud aplikace spadne), Service Bus zprávu automaticky odemkne ve frontě a znovu ji zpřístupní k přijetí.

Pokud aplikace spadne po zpracování zprávy, ale před vysláním požadavku [Complete][], zpráva se do aplikace odešle znovu. Tomu se často říká **Zpracování nejméně jednou** – to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích se může doručit víckrát. Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy. To se často opírá o vlastnost zprávy [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx), která je při každém pokusu o doručení stejné zprávy stejná.

## Další kroky

Naučili jste se základy front Service Bus, další informace se dozvíte na následujících odkazech.

-   O entitách přenosu zpráv Service Bus si můžete přečíst v tématu [Fronty, témata a odběry][].
-   Sestavit fungující aplikaci, která odesílá zprávy do fronty Service Bus a přijímá je z ní, se naučíte v tématu [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus][].
-   Ukázky pro Service Bus si můžete stáhnout z [ukázek Azure][] nebo se můžete podívat na [přehled ukázek pro Service Bus][].

  [klasického portálu Azure]: http://manage.windowsazure.com
  [7]: ./media/service-bus-dotnet-how-to-use-queues/getting-started-multi-tier-13.png
  [Fronty, témata a odběry]: service-bus-queues-topics-subscriptions.md
  [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus]: service-bus-brokered-tutorial-dotnet.md
  [ukázek Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [přehled ukázek pro Service Bus]: service-bus-samples.md
  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [Complete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx


<!--HONumber=Jun16_HO2-->


