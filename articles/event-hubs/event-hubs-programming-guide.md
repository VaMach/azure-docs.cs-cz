---
title: "Průvodce programováním pro Azure Event Hubs | Dokumentace Microsoftu"
description: "Zapište kód pro Azure Event Hubs pomocí sady Azure .NET SDK."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 405ec2b27b488b570c4a5c86e4950ff98233360e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-programming-guide"></a>Průvodce programováním pro službu Event Hubs

Tento článek popisuje některé běžné scénáře v psaní kódu pomocí služby Azure Event Hubs a .NET SDK služby Azure. Předpokládá se předběžná znalost služby Event Hubs. Koncepční přehled služby Event Hubs naleznete v tématu [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md).

## <a name="event-publishers"></a>Zdroje událostí

Můžete odesílat události do centra událostí buď pomocí HTTP POST nebo prostřednictvím připojení protokolu AMQP 1.0. Možnost, která používat a kdy závisí na konkrétním adresovaném scénáři. Připojení protokolu AMQP 1.0 se měří jako zprostředkovaná připojení ve službě Service Bus. Díky tomu, že poskytují trvalý kanál pro zasílání zpráv, jsou vhodnější ve scénářích, kde se počítá s častými vysokými objemy zpráv a vyžaduje se nižší latence.

Centra událostí Event Hubs se vytváří a spravují pomocí třídy [NamespaceManager][]. Pokud používáte rozhraní API spravované pomocí .NET, budou primárními konstrukcemi pro publikování dat ve službě Event Hubs třídy [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) a [EventData][]. [EventHubClient][] poskytuje komunikační kanál AMQP, přes který se události posílají do centra událostí. [EventData][] třída představuje událost a slouží k publikování zpráv do centra událostí. Tato třída obsahuje tělo, některá metadata a záhlaví s informacemi o události. Ostatní vlastnosti jsou přidány do [EventData][] objektu při průchodu centra událostí.

## <a name="get-started"></a>Začínáme

Třídy rozhraní .NET, které podporují službu Event Hubs, poskytuje knihovna Microsoft.ServiceBus.dll. Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svou aplikaci se všemi závislostmi služby Service Bus, je stáhnout si balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Můžete případně použít i [konzolu Správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ve Visual Studiu. V tom případě je potřeba zadat v okně [konzoly Správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) následující příkaz:

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Vytvoření centra událostí
K vytváření center událostí můžete použít třídu [NamespaceManager][]. Příklad:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

Ve většině případů se doporučuje používat metody [CreateEventHubIfNotExists][], aby nedošlo ke generování výjimek v případě restartu služby. Příklad:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Všechny operace vytváření pro službu Event Hubs, včetně [CreateEventHubIfNotExists][], vyžadují oprávnění pro **správu** v daném oboru názvů. Pokud chcete omezit oprávnění svých publikujících (zdrojových) nebo přijímajících aplikací, můžete zamezit volání operací vytváření v produkčním kódu použitím přihlašovacích údajů s omezenými oprávněními.

[EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) třída obsahuje podrobnosti o Centru událostí, včetně pravidla autorizace, intervalu uchování zpráv, ID oddílu, stav a cestu. Tuto třídu můžete použít k aktualizaci metadat centra událostí.

## <a name="create-an-event-hubs-client"></a>Vytvoření klienta pro centra událostí (Event Hubs)
Primární třídou pro interakci s centry událostí je [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Tato třída poskytuje možnosti jak pro odesílatele, tak pro příjemce. Instanci této třídy můžete vytvořit pomocí metody [Create](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create) (Vytvořit), jak je znázorněno v následujícím příkladu.

```csharp
var client = EventHubClient.Create(description.Path);
```

Tato metoda používá informace o připojení služby Service Bus v souboru App.config v sekci `appSettings`. Pokud se chcete podívat na příklad XML `appSettings`, které se používá k ukládání informací o připojení služby Service Bus, přečtěte si dokumentaci k metodě [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_).

Další možností je vytvoření klienta z připojovacího řetězce. Tato možnost dobře funguje při použití rolí pracovního procesu systému Azure, protože je možné řetězec uložit ve vlastnostech konfigurace pracovního procesu. Příklad:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Připojovací řetězec bude mít stejný formát jako v souboru App.config u předchozích metod:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Nakonec jde taky vytvořit objekt [EventHubClient][] z instance [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory), jak je znázorněno v následujícím příkladu.

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Pozor, všimněte si, že další objekty [EventHubClient][] vytvořené z instance MessagingFactory znova používají stejné základní připojení protokolu TCP. Tyto objekty tudíž mají omezení propustnosti na straně klienta. Metoda [Create](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) opětovně používá jedinou instanci MessagingFactory. Pokud u jednoho odesílatele potřebujete vysokou propustnost, tak můžete vytvořit více instancí MessagingFactory a jeden objekt [EventHubClient][] z každé z nich.

## <a name="send-events-to-an-event-hub"></a>Odesílání událostí do centra událostí
Odesílání událostí do centra událostí tak, že vytvoříte [EventData][] instance a odešlete pomocí [odeslat](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) metoda. Tato metoda přebírá jediný [EventData][] parametr instance a synchronně ho odesílá do centra událostí.

## <a name="event-serialization"></a>Serializace událostí
Třída [EventData][] má [čtyři přetížené konstruktory](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_), které přebírají různé parametry, například objekt a serializátor, pole bajtů nebo datový proud. Taky je možné vytvořit instanci třídy [EventData][] a nastavit datový proud těla potom. Pokud při práci s třídou [EventData][] používáte JSON, můžete načíst pole bajtů řetězce kódovaného ve formátu JSON pomocí metody **Encoding.UTF8.GetBytes()**.

## <a name="partition-key"></a>Klíč oddílu
Třída [EventData][] má vlastnost [PartitionKey][] (Klíč oddílu), která umožňuje odesílateli zadat hodnotu, která se po zahašování použije k vytvoření přiřazení k oddílu. Použití klíče oddílu zajišťuje, že všechny události se stejným klíčem odešlou do stejného oddílu události rozbočovače. Běžné klíče oddílů zahrnují ID relace uživatele a jedinečné ID odesílatele. Vlastnost [PartitionKey][] je volitelná. K dispozici je při použití metody [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) nebo metody [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_). Pokud hodnotu vlastnosti [PartitionKey][] nezadáte, budou odeslané události distribuovány do oddílů pomocí modelu kruhového dotazování.

### <a name="availability-considerations"></a>Aspekty dostupnosti

Použitím klíče oddílu je volitelný a měli byste pečlivě zvážit zda se má použít. V mnoha případech použitím klíče oddílu je vhodný, pokud řazení událostí je důležité. Pokud použijete klíč oddílu, tyto oddíly vyžadují dostupnosti v jednom uzlu, a můžete výpadků časem; například se při výpočetní uzly restartování a opravy. Jako takový Pokud nastavíte ID oddílu a oddílu z nějakého důvodu nedostupný, se nezdaří pokus o přístup k datům v tomto oddílu. Pokud je nejdůležitější vysokou dostupnost, nezadávejte klíč oddílu; v takovém případě události odešle do oddílů pomocí modelu kruhového dotazování popsané. V tomto scénáři provádíte explicitní volbu mezi dostupnosti (žádné ID oddílu) a konzistence (Připnutí události pro ID oddílu).

Potřeba vzít v úvahu zpracovává zpoždění při zpracování události. V některých případech může být lepší vyřazení dat a zkuste to znovu než zkuste a přečtěte si o zpracování, které může potenciálně způsobit zpoždění další podřízené zpracování. Například s běžícími je lepší čekání na dokončení aktuální data, ale v živé konverzace nebo VOIP scénář spíše byste měli data rychle, i když není úplný.

Zadané těchto aspektů dostupnosti v těchto scénářích můžete vybrat jednu z následujících chybových strategie zpracování:

- Zastavení (Zastavit čtení ze služby Event Hubs, dokud se nevyřeší věcí)
- Vyřaďte (zprávy nejsou důležité, jejich umístění)
- Opakujte (opakování, které vložit jak můžete vidět, zprávy)
- [Nedoručených zpráv](../service-bus-messaging/service-bus-dead-letter-queues.md) (frontu použít nebo jiné centra událostí do zpráv písmeno pouze zprávy, nelze zpracovat.)

Další informace a diskuzi o kompromis mezi dostupnosti a konzistence najdete v tématu [dostupnosti a konzistence v Event Hubs](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Dávkové operace odesílání událostí
Odesílání událostí v dávkách (batch) může výrazně zvýšit propustnost. [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metoda trvá **rozhraní IEnumerable** parametr typu [EventData][] a odesílá celý batch jako atomickou operaci do centra událostí.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Všimněte si, že jeden batch nesmí překročit velikost 256 KB, tedy standardní omezení pro událost. Kromě toho každá zpráva v batchi používá stejnou identitu zdroje. Dodržení maximálního limitu velikosti události u batche musí zajistit odesílatel. V případě překročení se u klienta vygeneruje chyba odeslání (**Send**).

## <a name="send-asynchronously-and-send-at-scale"></a>Asynchronní odesílání a škálované odesílání
Můžete také odeslat události do centra událostí asynchronně. Asynchronní odesílání může zvýšit frekvenci, s jakou je klient schopný odesílat události. Metody [Send](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) i [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) jsou k dispozici v asynchronních verzích, které vrací objekt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). I když tato technika může zvýšit propustnost, taky může způsobit, že klient bude odesílat události i po omezení ze strany služby Event Hubs, což může v případě nesprávné implementace vést k selhání klienta nebo ztrátě zpráv. K ovládání možností opětovných pokusů klienta můžete navíc u klienta použít vlastnost [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity#Microsoft_ServiceBus_Messaging_ClientEntity_RetryPolicy).

## <a name="create-a-partition-sender"></a>Vytvoření odesílatele na oddíl
Přestože je nejčastěji odesílají události do centra událostí bez klíče oddílu, v některých případech můžete chtít odesílat události přímo na konkrétní oddíl. Například:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) vrátí [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) objekt, můžete použít k publikování událostí do oddílu centra konkrétní události.

## <a name="event-consumers"></a>Příjemci událostí
Služba Event Hubs má dva primární modely příjmu událostí: přímé příjemce a abstrakce vyšší úrovně, jako je například [EventProcessorHost][]. Přímí příjemci si za koordinaci přístupu k oddílům v rámci skupiny příjemců zodpovídají sami.

### <a name="direct-consumer"></a>Přímý příjemce
Nejpřímějším způsobem, jak číst z oddílu v rámci skupiny příjemců, je použít třídu [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver). Pokud chcete vytvořit instanci této třídy, musíte použít instanci třídy [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup). V následujícím příkladu musí být při vytváření příjemce pro skupinu příjemců určeno ID oddílu.

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Metoda [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) má několik přetížení, která usnadňují ovládání vytvářeného čtenáře. Tyto metody zahrnují určení posunu jako řetězce nebo časového razítka a možnost určit, jestli má být zadaný posun ve vráceném datovém proudu, nebo se má začít až po něm. Jakmile vytvoříte příjemce, můžete začít přijímat události prostřednictvím vráceného objektu. Metoda [Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) má čtyři přetížení, která řídí parametry operace přijímání, jako je například velikost batche (dávky) nebo doba čekání. Pokud chcete zvýšit propustnost příjemce, můžete použít asynchronní verze těchto metod. Například:

```csharp
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

Zprávy jsou s ohledem na konkrétní oddíl přijímány v pořadí, ve které byly odeslány do centra událostí. Posun je token v podobě řetězce, který se používá k identifikaci zprávy v oddílu.

Všimněte si, že oddíl v rámci skupiny příjemců neumožňuje v žádné chvíli připojení více než pěti souběžných čtenářů. S tím, jak se příjemci různě připojují a odpojují, mohou jejich relace zůstat ještě několik minut aktivní, než služba rozpozná, že se odpojili. Během této doby může pokus o opětovné připojení k oddílu selhat. Kompletní příklad, jak napsat přímého příjemce pro službu Event Hubs, najdete v článku [přímí příjemci centra událostí](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) ukázka.

### <a name="event-processor-host"></a>EventProcessorHost
Třída [EventProcessorHost][] zpracovává data z center událostí (Event Hubs). Tuto implementaci byste měli používat při vytváření čtenářů událostí na platformě .NET. Třída [EventProcessorHost][] poskytuje pro implementace zpracovatelů událostí bezpečné prostředí runtime, které umožňuje bezpečné použití vláken a více procesů. Taky poskytuje možnost vytváření kontrolních bodů a správy „půjčování“ oddílu.

K použití třídy [EventProcessorHost][] může být potřeba implementovat rozhraní [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor). Toto rozhraní obsahuje tři metody:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

Pokud chcete zahájit zpracování událostí, vytvořte instanci [EventProcessorHost][], poskytnutím příslušných parametrů pro vaše Centrum událostí. Potom zavolejte [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1), čímž registrujete svou implementaci [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) s modulem runtime. V tomto okamžiku hostitel pokusí "zapůjčit" každý oddíl v Centru událostí s použitím "chamtivého" algoritmu. Tato zapůjčení vydrží po stanovenou dobu a následně musí být obnovena. V tomto případě se instance pracovních procesů připojí jako nové uzly a umístí své rezervace zapůjčení. Každý uzel se snaží získat další zapůjčení, a tak se v průběhu času zatížení posune.

![Event Processor Host](./media/event-hubs-programming-guide/IC759863.png)

Postupem času se dosáhne rovnováhy. Tato dynamická funkce umožňuje použití automatického škálování na bázi procesoru u příjemců jak pro vertikální navýšení, tak pro vertikální snížení kapacity. Vzhledem k tomu, že služba Event Hubs nedisponuje přímým konceptem počítání zpráv, bývá průměrné využití procesoru často nejlepším mechanismem, jak měřit back end nebo škálování příjemců. Když začnou zdroje publikovat více událostí, než mohou příjemci zpracovat, může být zvýšená aktivita procesoru využita k automatickému škálování počtu instancí pracovních procesů.

Třída [EventProcessorHost][] taky implementuje mechanismus vytváření kontrolních bodů Azure na bázi úložiště. Tento mechanismus ukládá posun na bázi oddílu, každý příjemce tedy může určit poslední kontrolní bod předchozího příjemce. Vzhledem k tomu, že oddíly prostřednictvím zapůjčení přechází mezi uzly, usnadňuje tento synchronizační mechanismus přesun zátěže.

## <a name="publisher-revocation"></a>Odvolání zdroje
Kromě pokročilých funkcí běhové [EventProcessorHost][], umožňuje Služba Event Hubs odvolání zdroje aby bylo možné blokovat konkrétním zdrojům možnost odesílat události do centra událostí. Tyto funkce jsou zvláště užitečné, pokud jsou pochybnosti ohledně pravosti tokenu zdroje nebo se zdroj v důsledku aktualizace softwaru začne chovat nevhodně. V těchto situacích může být pro identitu zdroje, která je součástí jeho tokenu SAS, zablokováno publikování událostí.

Další informace o odvolání zdroje a o tom, jak jako zdroj odesílat do centra událostí, najdete v ukázce [zabezpečeného publikování ve velkém rozsahu u služby Event Hubs](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab).

## <a name="next-steps"></a>Další kroky
Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:

* [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)
* [Co je Služba Event Hubs](event-hubs-what-is-event-hubs.md)
* [Dostupnost a konzistence ve službě Event Hubs](event-hubs-availability-and-consistency.md)
* [Referenční dokumentace rozhraní API třídy EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
