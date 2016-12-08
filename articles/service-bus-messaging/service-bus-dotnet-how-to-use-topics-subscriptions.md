---
title: "Používání témat Service Bus s .NET | Dokumentace Microsoftu"
description: "Naučte se, jak používat témata a odběry služby Service Bus s :NET v Azure. Ukázky kódu jsou napsané pro aplikace .NET."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 31d0bc29-6524-4b1b-9c7f-aa15d5a9d3b4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 09/16/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 9ace119de3676bcda45d524961ebea27ab093415
ms.openlocfilehash: 71d0049c831b9bbcdef548bc129d6e15256a25b4


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak používat témata a odběry Service Bus
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak používat témata a odběry služby Service Bus. Ukázky kódu jsou napsané v C# a používají .NET API. Popsané scénáře zahrnují vytváření témat a odběrů, vytváření filtrů odběrů, odesílání zpráv do tématu, přijímání zpráv z odběru a odstranění témat a odběrů. Další informace o tématech a odběrech najdete v části [Další kroky](#next-steps).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurace aplikace přes Service Bus
Když vytvoříte aplikaci, která používá Service Bus, musíte přidat odkaz na sestavení Service Bus a zadat odpovídající obory názvů. Nejjednodušší způsob je stáhnout si odpovídající balíček [NuGet](https://www.nuget.org).

## <a name="get-the-service-bus-nuget-package"></a>Získání balíčku Service Bus NuGet
Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svoji aplikaci se všemi potřebnými závislostmi služby Service Bus, je balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Balíček Service Bus NuGet můžete do svého projektu nainstalovat takto:

1. V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.
2. Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete následující dialogové okno:
   
   ![][7]

Teď jste připravení psát kód pro službu Service Bus.

## <a name="create-a-service-bus-connection-string"></a>Vytvoření připojovacího řetězce pro Service Bus
Service Bus používá připojovací řetězec k uložení koncových bodů a pověření. Připojovací řetězec nemusíte kódovat napevno, můžete ho přidat do konfiguračního souboru:

* Pokud používáte služby Azure, doporučuje se připojovací řetězec uložit pomocí konfiguračního systému služby Azure (soubory .csdef a .cscfg).
* Pokud používáte Weby Azure nebo Azure Virtual Machines, doporučuje se připojovací řetězec uložit pomocí konfiguračního systému .NET (soubory .csdef a .cscfg).

V obou případech můžete načíst váš připojovací řetězec pomocí metody `CloudConfigurationManager.GetSetting` jak uvidíte později v tomto článku.

### <a name="configure-your-connection-string"></a>Konfigurace připojovacího řetězce
Konfigurační mechanizmus služby vám umožní dynamicky měnit konfiguraci z webu [Azure Portal][Azure Portal] bez opětovného nasazení aplikace. Můžete například přidat označení `Setting` do souboru definice služby (**.csdef**), jak je vidět v dalším příkladu.

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

Potom zadejte hodnoty do souboru definice služby (.csdef).

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

Použijte název klíče Sdíleného přístupového podpisu (SAS) a hodnoty klíče získané z portálu, jak je popsáno výše.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurace připojovacího řetězce při použití webů Azure nebo Azure Virtual Machines
Pokud používáte weby nebo Virtual Machines, doporučuje se použít konfigurační systém .NET (například Web.config). Připojovací řetězec uložíte pomocí prvku `<appSettings>`.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Použijte název SAS a hodnoty klíče, které jste získali z webu [Azure Portal][Azure Portal], jak je popsáno výše.

## <a name="create-a-topic"></a>Vytvoření tématu
Můžete provádět operace správy témat a odběrů Service Bus pomocí třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Tato třída poskytuje metody pro vytváření, výčet a odstranění témat.

Následující příklad vytvoří objekt `NamespaceManager` pomocí třídy Azure `CloudConfigurationManager` s připojovacím řetězcem složeným z bázové adresy oboru názvů Service Bus a příslušného pověření SAS s oprávněními k její správě. Tento připojovací řetězec má následující formu:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Použijte následující příklad s konfiguračními nastaveními v předchozí části.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Existují přetížení metody [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx), které vám umožní nastavit vlastnosti tématu – například nastavit výchozí čas TTL, který se použije na zprávy odeslané do tématu. Tato nastavení se aplikují pomocí třídy [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx). Následující příklad ukazuje, jak vytvořit téma s názvem **TestTopic**, maximální velikostí 5 GB a výchozím časem TTL pro zprávu 1 minuta.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [!NOTE]
> Můžete použít metodu [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) na objekty [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) pro kontrolu, jestli už v daném oboru názvů existuje téma se zadaným názvem.
> 
> 

## <a name="create-a-subscription"></a>Vytvoření odběru
Odběry témat taky můžete vytvořit pomocí třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv odesílaných do virtuální fronty odběru.

> [!IMPORTANT]
> Aby předplatné mohlo přijímat zprávy, musíte příslušné předplatné vytvořit před odesláním jakýchkoli zpráv do tématu. Pokud nejsou k dispozici žádná předplatná tématu, příslušné téma takové zprávy zahodí.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření odběru s výchozím filtrem (MatchAll).
V případě, že při vytváření nového odběru nezadáte žádný filtr, použije se jako výchozí filtr **MatchAll**. Když použijete filtr **MatchAll**, všechny zprávy publikované do tématu se umístí do virtuální fronty odběru. Následující příklad vytvoří odběr s názvem „AllMessages“ a použije výchozí filtr **MatchAll**.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření odběru s filtry
Můžete taky vytvořit filtry, které vám umožní zprávy odeslané do tématu zobrazit v konkrétním odběru tématu.

Nejflexibilnější filtr, který odběry podporují, je třída [SqlFilter][SqlFilter], která je podmnožinou SQL92. Filtry SQL pracují s vlastnostmi zpráv publikované do tématu. Další informace, které se dají použít s filtrem SQL, najdete v syntaxi [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Následující příklad vytvoří odběr s názvem **HighMessages** s objektem [SqlFilter][SqlFilter], který vybere jen zprávy s uživatelskou vlastností **MessageNumber** větší než 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Následující příklad vytvoří odběr s názvem **LowMessages** s objektem [SqlFilter][SqlFilter], který vybere jen zprávy s uživatelskou vlastností **MessageNumber** menší nebo rovnou 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Když se teď odešle zpráva do `TestTopic`, vždy se dodá příjemci, který odebírá téma **AllMessages**, a selektivně příjemcům, kteří odebírají témata **HighMessages** a **LowMessages** (podle obsahu zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy do tématu
Pokud chcete odeslat zprávu do tématu Service Bus, vaše aplikace vytvoří objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) pomocí připojovacího řetězce.

Následující kód ukazuje, jak vytvořit objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) téma **TestTopic**, které jste předtím vytvořili pomocí volání API [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx).

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Zprávy odeslané do témat Service Bus jsou instance třídy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Objekty [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) mají sadu standardních vlastností (jako třeba[Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), slovník používaný pro udržení vlastních vlastností aplikace a tělo s libovolnými aplikačními daty. Aplikace může tělo zprávy nastavit tak, že do konstruktoru objektu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) předá jakýkoli serializovatelný objekt, a pak se k serializaci objektu použije vhodný **DataContractSerializer**. Nebo se může poskytnout **System.IO.Stream**.

Následující příklad ukazuje, jak odeslat pět zkušebních zpráv do objektu **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) získaného v předcházejícím příkladu. Nezapomeňte, že hodnota vlastnosti [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) každé zprávy se liší podle iterace smyčky (to určuje, které odběry ji přijmou).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md). Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB. Počet zpráv držených v tématu není omezený, ale celková velikost zpráv držených v tématu omezená je. Velikost tématu se definuje při vytvoření, maximální limit je 5 GB. Pokud je zapnutá segmentace, je maximální limit vyšší. Další informace najdete v tématu [Segmentované entity zasílání zpráv](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak přijmout zprávy z odběru
Doporučený způsob přijímání z fronty je použití objektu [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx). Objekty [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) můžou pracovat ve dvou různých režimech: [*ReceiveAndDelete* a *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Při použití režimu **ReceiveAndDelete** je přijetí jednorázová operace – tzn. když Service Bus přijme požadavek na čtení zprávy v odběru, označí zprávu jako spotřebovávanou a vrátí ji do aplikace. Režim **ReceiveAndDelete** je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat možnost, že v případě selhání se zpráva nezpracuje. Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se. Služba Service Bus už ale zprávu označila jako spotřebovanou. Když se aplikace znovu spustí a začne znovu přijímat zprávy, zprávu, která se spotřebovala před pádem aplikace, vynechá.

V režimu **PeekLock** (výchozím režimu) je přijetí dvoufázový proces. To podporuje aplikace, u kterých nejde tolerovat vynechání zpráv. Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace. Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), zavolá na přijatou zprávu [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx), a tím potvrdí dokončení druhé fáze přijetí. Když Service Bus uvidí volání **Complete**, označí zprávu jako spotřebovávanou a odstraní ji z odběru.

Následující příklad ukazuje, jak se zprávy můžou přijímat a zpracovávat pomocí výchozího režimu **PeekLock**. Pokud chcete nastavit jinou hodnotu [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx), můžete použít další přetížení pro [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Tento příklad používá zpětné volání [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) ke zpracování zpráv, když dorazí do odběru **HighMessages**.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Tento příklad konfiguruje zpětné volání [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) pomocí objektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx). [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) je nastavené na **false**, tím se povolí ruční kontrola nad tím, kdy se má na přijatou zprávu zavolat [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx). [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) je nastavené na 1 minutu a to způsobí, že klient může na zprávu čekat až 1 minutu, než se funkce automatického obnovení ukončí a klient odešle nové volání na kontrolu nových zpráv. Tato hodnota vlastnosti snižuje počet, kolikrát klient provede fakturovatelná volání, která nepřijmou žádné zprávy.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Zpracování pádů aplikace a nečitelných zpráv
Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy. Pokud přijímající aplikace z nějakého důvodu nedokáže zpracovat zprávu, může na přijatou zprávu zavolat metodu [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) (namísto metody [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). To způsobí, že Service Bus zprávu odemkne v odběru a zpřístupní ji pro další přijetí, buďto stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.

Zpráva uzamčená v odběru má taky časový limit a pokus aplikace zprávu nezpracuje v časovém limitu uzamčení (například pokud aplikace spadne), Service Bus zprávu automaticky odemkne ve frontě a znovu ji zpřístupní k přijetí.

Pokud aplikace spadne po zpracování zprávy, ale před vysláním požadavku [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx), zpráva se do aplikace odešle znovu. Tomu se často říká *Zpracování nejméně jednou* – to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích se může doručit víckrát. Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy. To se často opírá o vlastnost zprávy [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx), která je při každém pokusu o doručení stejné zprávy stejná.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témat a odběrů
Následující příklad ukazuje, jak odstranit téma **TestTopic** z oboru názvů služby **HowToSample**.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Pokud se odstraní téma, odstraní se i všechny odběry registrované k tomuto tématu. Odběry se taky dají odstranit samostatně. Následující kód ukazuje, jak odstranit odběr s názvem **HighMessages** z tématu **TestTopic**.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Další kroky
Naučili jste se základy témat a odběrů Service Bus, další informace se dozvíte na následujících odkazech.

* [Fronty, témata a odběry][Fronty, témata a odběry].
* [Ukázka filtrů témat][Ukázka filtrů témat]
* Reference pro API pro [SqlFilter][SqlFilter].
* Sestavení fungující aplikace, která odesílá zprávy do fronty Service Bus a přijímá je z ní: [Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus][Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus].
* Ukázky pro Service Bus: Stáhněte si je z [ukázek Azure][ukázek Azure] nebo se podívejte na [přehled](service-bus-samples.md).

[Azure Portal]: https://portal.azure.com

[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

[Fronty, témata a odběry]: service-bus-queues-topics-subscriptions.md
[Ukázka filtrů témat]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
[SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus]: service-bus-brokered-tutorial-dotnet.md
[ukázek Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2



<!--HONumber=Nov16_HO3-->


