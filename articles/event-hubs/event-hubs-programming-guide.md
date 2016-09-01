<properties 
    pageTitle="Průvodce programováním pro Azure Event Hubs | Microsoft Azure"
    description="Popisuje programování pro Azure Event Hubs pomocí sady Azure .NET SDK."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sethm" />

# Průvodce programováním pro službu Event Hubs

Toto téma popisuje programování pro Azure Event Hubs pomocí sady Azure .NET SDK. Předpokládá se předběžná znalost služby Event Hubs. Koncepční přehled služby Event Hubs naleznete v tématu [Přehled služby Event Hubs](event-hubs-overview.md).

## Zdroje událostí

Události se do centra událostí odesílají buď pomocí metody POST protokolu HTTP, nebo prostřednictvím připojení protokolu AMQP 1.0. Možnost, která se použije, závisí na konkrétním adresovaném scénáři. Připojení protokolu AMQP 1.0 se měří jako zprostředkovaná připojení ve službě Service Bus. Díky tomu, že poskytují trvalý kanál pro zasílání zpráv, jsou vhodnější ve scénářích, kde se počítá s častými vysokými objemy zpráv a vyžaduje se nižší latence.

Centra událostí Event Hubs se vytváří a spravují pomocí třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Pokud používáte rozhraní API spravované pomocí .NET, budou primárními konstrukcemi pro publikování dat ve službě Event Hubs třídy [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) a [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx). Třída [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) poskytuje komunikační kanál AMQP, přes který se události posílají do centra událostí. Třída [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) představuje událost a slouží k publikování zpráv v centru událostí. Tato třída obsahuje tělo, některá metadata a záhlaví s informacemi o události. Další vlastnosti se do objektu [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) přidají při průchodu centrem událostí.

## Začínáme

Třídy rozhraní .NET, které podporují službu Event Hubs, poskytuje knihovna Microsoft.ServiceBus.dll. Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svou aplikaci se všemi závislostmi služby Service Bus, je stáhnout si balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Můžete případně použít i [konzolu Správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ve Visual Studiu. V tom případě je potřeba zadat v okně [konzoly Správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) následující příkaz:

```
Install-Package WindowsAzure.ServiceBus
```

## Vytvoření centra událostí

K vytváření center událostí můžete použít třídu [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Příklad:

```
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

Ve většině případů se doporučuje používat metody [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx), aby nedošlo ke generování výjimek v případě restartu služby. Příklad:

```
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Všechny operace vytváření pro službu Event Hubs, včetně [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx), vyžadují oprávnění pro **správu** v daném oboru názvů. Pokud chcete omezit oprávnění svých publikujících (zdrojových) nebo přijímajících aplikací, můžete zamezit volání operací vytváření v produkčním kódu použitím přihlašovacích údajů s omezenými oprávněními.

Třída [EventHubDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubdescription.aspx) obsahuje podrobnosti o centru událostí, včetně pravidel autorizace, intervalu uchování zpráv, stavu, cesty a ID oddílů. Tuto třídu můžete použít k aktualizaci metadat centra událostí.

## Vytvoření klienta pro centra událostí (Event Hubs)

Primární třídou pro interakci s centry událostí je [Microsoft.ServiceBus.Messaging.EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx). Tato třída poskytuje možnosti jak pro odesílatele, tak pro příjemce. Instanci této třídy můžete vytvořit pomocí metody [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) (Vytvořit), jak je znázorněno v následujícím příkladu.

```
var client = EventHubClient.Create(description.Path);
```

Tato metoda používá informace o připojení služby Service Bus v souboru App.config v sekci `appSettings`. Pokud se chcete podívat na příklad XML `appSettings`, které se používá k ukládání informací o připojení služby Service Bus, přečtěte si dokumentaci k metodě [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx).

Další možností je vytvoření klienta z připojovacího řetězce. Tato možnost dobře funguje při použití rolí pracovního procesu systému Azure, protože je možné řetězec uložit ve vlastnostech konfigurace pracovního procesu. Příklad:

```
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Připojovací řetězec bude mít stejný formát jako v souboru App.config u předchozích metod:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=Manage;SharedAccessKey=[key]
```

Nakonec jde taky vytvořit objekt [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) z instance [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx), jak je znázorněno v následujícím příkladu.

```
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Pozor, všimněte si, že další objekty [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) vytvořené z instance MessagingFactory znova používají stejné základní připojení protokolu TCP. Tyto objekty tudíž mají omezení propustnosti na straně klienta. Metoda [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) opětovně používá jedinou instanci MessagingFactory. Pokud u jednoho odesílatele potřebujete vysokou propustnost, tak můžete vytvořit více instancí MessagingFactory a jeden objekt [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) z každé z nich.

## Odesílání událostí do centra událostí

Události do centra událostí odesíláte tak, že vytvoříte instanci [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) a události odešlete pomocí metody [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx). Tato metoda přebírá jediný parametr instance [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) a synchronně ho odesílá do centra událostí.

## Serializace událostí

Třída [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) má [čtyři přetížené konstruktory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.eventdata.aspx), které přebírají různé parametry, například objekt a serializátor, pole bajtů nebo datový proud. Taky je možné vytvořit instanci třídy [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) a nastavit datový proud těla potom. Pokud při práci s třídou [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) používáte JSON, můžete načíst pole bajtů řetězce kódovaného ve formátu JSON pomocí metody **Encoding.UTF8.GetBytes()**.

## Klíč oddílu

Třída [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) má vlastnost [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) (Klíč oddílu), která umožňuje odesílateli zadat hodnotu, která se po zahašování použije k vytvoření přiřazení k oddílu. Použitím klíče oddílu zajistíte, že se všechny události se stejným klíčem odešlou do stejného oddílu v centru událostí. Běžné klíče oddílů zahrnují ID relace uživatele a jedinečné ID odesílatele. Vlastnost [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) je volitelná. K dispozici je při použití metody [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) nebo metody [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx). Pokud hodnotu vlastnosti [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) nezadáte, budou odeslané události distribuovány do oddílů pomocí modelu kruhového dotazování.

## Dávkové operace odesílání událostí

Odesílání událostí v dávkách (batch) může výrazně zvýšit propustnost. Metoda [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) přebírá parametr **IEnumerable** typu [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) a odesílá celý batch do centra událostí jako atomickou operaci.

```
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Všimněte si, že jeden batch nesmí překročit velikost 256 KB, tedy standardní omezení pro událost. Kromě toho každá zpráva v batchi používá stejnou identitu zdroje. Dodržení maximálního limitu velikosti události u batche musí zajistit odesílatel. V případě překročení se u klienta vygeneruje chyba odeslání (**Send**).

## Asynchronní odesílání a škálované odesílání

Události můžete do centra událostí posílat i asynchronně. Asynchronní odesílání může zvýšit frekvenci, s jakou je klient schopný odesílat události. Metody [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) i [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) jsou k dispozici v asynchronních verzích, které vrací objekt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). I když tato technika může zvýšit propustnost, taky může způsobit, že klient bude odesílat události i po omezení ze strany služby Event Hubs, což může v případě nesprávné implementace vést k selhání klienta nebo ztrátě zpráv. K ovládání možností opětovných pokusů klienta můžete navíc u klienta použít vlastnost [RetryPolicy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.cliententity.retrypolicy.aspx).

## Vytvoření odesílatele na oddíl

I když se události nejčastěji odesílají do centra událostí s klíčem oddílu, může být v některých případech potřeba odesílat události přímo na konkrétní oddíl. Příklad:

```
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.createpartitionedsender.aspx) vrací objekt [EventHubSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubsender.aspx), který můžete použít k publikování událostí do určitého oddílu centra událostí.

## Příjemci událostí

Služba Event Hubs má dva primární modely příjmu událostí: přímé příjemce a abstrakce vyšší úrovně, jako je například [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx). Přímí příjemci si za koordinaci přístupu k oddílům v rámci skupiny příjemců zodpovídají sami.

### Přímý příjemce

Nejpřímějším způsobem, jak číst z oddílu v rámci skupiny příjemců, je použít třídu [EventHubReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.aspx). Pokud chcete vytvořit instanci této třídy, musíte použít instanci třídy [EventHubConsumerGroup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.aspx). V následujícím příkladu musí být při vytváření příjemce pro skupinu příjemců určeno ID oddílu.

```
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Metoda [CreateReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.createreceiver.aspx) má několik přetížení, která usnadňují ovládání vytvářeného čtenáře. Tyto metody zahrnují určení posunu jako řetězce nebo časového razítka a možnost určit, jestli má být zadaný posun ve vráceném datovém proudu, nebo se má začít až po něm. Jakmile vytvoříte příjemce, můžete začít přijímat události prostřednictvím vráceného objektu. Metoda [Receive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.receive.aspx) má čtyři přetížení, která řídí parametry operace přijímání, jako je například velikost batche (dávky) nebo doba čekání. Pokud chcete zvýšit propustnost příjemce, můžete použít asynchronní verze těchto metod. Příklad:

```
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

Zprávy jsou s ohledem na konkrétní oddíl přijímány v pořadí, v jakém byly do centra událostí odeslány. Posun je token v podobě řetězce, který se používá k identifikaci zprávy v oddílu.

Všimněte si, že oddíl v rámci skupiny příjemců neumožňuje v žádné chvíli připojení více než pěti souběžných čtenářů. S tím, jak se příjemci různě připojují a odpojují, mohou jejich relace zůstat ještě několik minut aktivní, než služba rozpozná, že se odpojili. Během této doby může pokus o opětovné připojení k oddílu selhat. Pokud chcete vidět kompletní příklad, jak napsat přímého příjemce pro službu Event Hubs, podívejte se na ukázku [přímých příjemců služeb Service Bus a Event Hubs](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6).

### EventProcessorHost

Třída [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) zpracovává data z center událostí (Event Hubs). Tuto implementaci byste měli používat při vytváření čtenářů událostí na platformě .NET. Třída [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) poskytuje pro implementace zpracovatelů událostí bezpečné prostředí runtime, které umožňuje bezpečné použití vláken a více procesů. Taky poskytuje možnost vytváření kontrolních bodů a správy „půjčování“ oddílu.

K použití třídy [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) může být potřeba implementovat rozhraní [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx). Toto rozhraní obsahuje tři metody:

- [OpenAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.openasync.aspx)

- [CloseAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.closeasync.aspx)

- [ProcessEventsAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.processeventsasync.aspx)

Pokud chcete zahájit zpracování událostí, vytvořte instanci [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) s poskytnutím příslušných parametrů pro vaše centrum událostí. Potom zavolejte [RegisterEventProcessorAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.registereventprocessorasync.aspx), čímž registrujete svou implementaci [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) s modulem runtime. V tomto okamžiku se hostitel pokusí „zapůjčit“ každý oddíl v centru událostí s použitím „chamtivého“ algoritmu. Tato zapůjčení vydrží po stanovenou dobu a následně musí být obnovena. V tomto případě se instance pracovních procesů připojí jako nové uzly a umístí své rezervace zapůjčení. Každý uzel se snaží získat další zapůjčení, a tak se v průběhu času zatížení posune.

![Event Processor Host](./media/event-hubs-programming-guide/IC759863.png)

Postupem času se dosáhne rovnováhy. Tato dynamická funkce umožňuje použití automatického škálování na bázi procesoru u příjemců jak pro vertikální navýšení, tak pro vertikální snížení kapacity. Vzhledem k tomu, že služba Event Hubs nedisponuje přímým konceptem počítání zpráv, bývá průměrné využití procesoru často nejlepším mechanismem, jak měřit back end nebo škálování příjemců. Když začnou zdroje publikovat více událostí, než mohou příjemci zpracovat, může být zvýšená aktivita procesoru využita k automatickému škálování počtu instancí pracovních procesů.

Třída [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) taky implementuje mechanismus vytváření kontrolních bodů Azure na bázi úložiště. Tento mechanismus ukládá posun na bázi oddílu, každý příjemce tedy může určit poslední kontrolní bod předchozího příjemce. Vzhledem k tomu, že oddíly prostřednictvím zapůjčení přechází mezi uzly, usnadňuje tento synchronizační mechanismus přesun zátěže.

## Odvolání zdroje

Jako bonus k pokročilým funkcím runtime třídy [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) umožňuje služba Event Hubs odvolání zdroje, aby bylo možné blokovat u konkrétních zdrojů možnost odesílat události do centra událostí. Tyto funkce jsou zvláště užitečné, pokud jsou pochybnosti ohledně pravosti tokenu zdroje nebo se zdroj v důsledku aktualizace softwaru začne chovat nevhodně. V těchto situacích může být pro identitu zdroje, která je součástí jeho tokenu SAS, zablokováno publikování událostí.

Další informace o odvolání zdroje a o tom, jak jako zdroj odesílat do centra událostí, naleznete v ukázce [zabezpečeného publikování ve velkém rozsahu u služeb Service Bus a Event Hubs](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab).

## Další kroky

Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:

- [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)
- [Přehled služby Event Hubs](event-hubs-overview.md)
- [Ukázky kódu služby Event Hubs](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hub&f[0].Type=SearchText&ac=5)
- [Referenční dokumentace rozhraní API třídy EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)



<!---HONumber=Aug16_HO4-->


