---
title: Service Bus s .NET a AMQP 1.0 | Microsoft Docs
description: "Pomocí protokolu AMQP Azure Service Bus pomocí technologie .NET"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 58a37c0dd24d54996f517961f3a7f1ec36639cfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-service-bus-from-net-with-amqp-10"></a>Service Bus pomocí technologie .NET pomocí protokolu AMQP 1.0

## <a name="downloading-the-service-bus-sdk"></a>Stahování služby Service Bus SDK

Podpora protokolu AMQP 1.0 je k dispozici v Service Bus SDK verze 2.1 nebo vyšší. Zajistíte tak, že stáhnete bits Service Bus z máte nejnovější verzi [NuGet][NuGet].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurace aplikací .NET pro použití protokolu AMQP 1.0

Ve výchozím nastavení klientské knihovny Service Bus .NET komunikuje se službou Service Bus pomocí vyhrazené protokolu, který je založený na protokolu SOAP. Pro použití protokolu AMQP 1.0 místo výchozí vyžaduje protokol explicitní konfiguraci na připojovací řetězec sběrnice služeb, jak je popsáno v následující části. Než tuto změnu zůstává beze změny kódu aplikace, při použití protokolu AMQP 1.0.

V aktuální verzi existuje několik funkcí rozhraní API, které nejsou podporována při použití protokolu AMQP. Tyto nepodporovaných funkcích jsou uvedené dále v části [nepodporované funkce, omezení a chování rozdíly](#unsupported-features-restrictions-and-behavioral-differences). Některá nastavení pokročilou konfiguraci také mít jiný význam při použití protokolu AMQP.

### <a name="configuration-using-appconfig"></a>Konfigurace pomocí souboru App.config

Je vhodné pro použití konfiguračního souboru App.config k ukládání nastavení aplikacemi. U aplikací, Service Bus můžete použít App.config k uložení připojovací řetězec sběrnice služeb. Příklad souboru App.config vypadá takto:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Hodnota `Microsoft.ServiceBus.ConnectionString` nastavení je připojovací řetězec sběrnice služeb, který slouží ke konfiguraci připojení k Service Bus. Formát vypadá takto:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Kde `namespace` a `SAS key` jsou získávány z [portál Azure] [ Azure portal] při vytváření oboru názvů Service Bus. Další informace najdete v tématu [vytvoření oboru názvů Service Bus pomocí portálu Azure][Create a Service Bus namespace using the Azure portal].

Při použití protokolu AMQP, připojit připojovacího řetězce s `;TransportType=Amqp`. Tento zápis dá pokyn, aby jeho připojení k Service Bus pomocí protokolu AMQP 1.0 klientské knihovny.

## <a name="message-serialization"></a>Zpráva serializace

Pokud používáte protokol výchozí, výchozí chování serializace klientské knihovny .NET je použití [DataContractSerializer] [ DataContractSerializer] typ k serializaci [BrokeredMessage] [ BrokeredMessage] instance pro přenos mezi klientské knihovny a službou Service Bus. Pokud používáte režim přenosu protokolu AMQP, Klientská knihovna používá systém typů AMQP za účelem serializace [zprostředkované zprávy] [ BrokeredMessage] do zprávy protokolu AMQP. Serializace umožňuje zprávy přijímají a interpretovat přijímající aplikace, která potenciálně běží na různých platformách, například aplikaci Java, která používá rozhraní API JMS pro přístup k Service Bus.

Když vytvoříte [BrokeredMessage] [ BrokeredMessage] instance, objekt .NET. můžete zadat jako parametr konstruktoru, která bude sloužit jako text zprávy. Pro objekty, které lze mapovat na primitivní typy AMQP je text serializován do AMQP datových typů. Pokud objekt nelze mapovat přímo do AMQP primitivní typ; který je definován vlastní typ aplikace a pak je objekt serializován pomocí [DataContractSerializer][DataContractSerializer], a odesílání serializovaných bajtů dat zprávy protokolu AMQP.

Pro usnadnění funkční spolupráce s klienty rozhraní .NET, použijte pouze typy .NET, které lze serializovat přímo do AMQP typy pro tělo zprávy. V následující tabulce jsou tyto typy a odpovídající mapování na systém typů AMQP.

| Typ objektu textu rozhraní .NET | Typ namapované AMQP | Typ oddílu AMQP textu |
| --- | --- | --- |
| BOOL |Logická hodnota |Hodnota AMQP |
| Bajtů |ubyte |Hodnota AMQP |
| ushort – |ushort – |Hodnota AMQP |
| uint |uint |Hodnota AMQP |
| ulong – |ulong – |Hodnota AMQP |
| SByte – |Bajtů |Hodnota AMQP |
| krátký |krátký |Hodnota AMQP |
| celá čísla |celá čísla |Hodnota AMQP |
| dlouhá |dlouhá |Hodnota AMQP |
| Plovoucí desetinná čárka |Plovoucí desetinná čárka |Hodnota AMQP |
| Double |Double |Hodnota AMQP |
| Decimal |decimal128 |Hodnota AMQP |
| Char |Char |Hodnota AMQP |
| Data a času |časové razítko |Hodnota AMQP |
| Identifikátor GUID |UUID |Hodnota AMQP |
| Byte] |Binární |Hodnota AMQP |
| Řetězec |Řetězec |Hodnota AMQP |
| System.Collections.IList |seznam |Hodnota AMQP: položek obsažených v kolekci lze pouze ty, které jsou definovány v této tabulce. |
| System.Array |Pole |Hodnota AMQP: položek obsažených v kolekci lze pouze ty, které jsou definovány v této tabulce. |
| System.Collections.IDictionary |mapy |Hodnota AMQP: položek obsažených v kolekci lze pouze ty, které jsou definovány v této tabulce. Poznámka: jsou podporovány pouze řetězcových klíčů. |
| identifikátor URI |Popisuje řetězec (viz následující tabulka) |Hodnota AMQP |
| Datový typ DateTimeOffset |Popisuje dlouho (viz následující tabulka) |Hodnota AMQP |
| Časový interval |Popisuje dlouho (viz následující) |Hodnota AMQP |
| Datový proud |Binární |Data protokolu AMQP (může být více). Datové části obsahují nezpracovaná Bajty čtení z datového proudu objektu. |
| Druhý objekt |Binární |Data protokolu AMQP (může být více). Obsahuje serializovaných binárního souboru objektu, který používá objektu DataContractSerializer nebo serializátor poskytl aplikace. |

| Typ formátu .NET | Mapovat AMQP popisuje typ | Poznámky |
| --- | --- | --- |
| identifikátor URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| Datový typ DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| Časový interval |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nepodporované funkce, omezení a rozdíly v chování

Následující funkce rozhraní API služby Service Bus .NET nejsou aktuálně podporovány při použití protokolu AMQP:

* Transakce
* Odesílání prostřednictvím cíl přenosu

Existují zde také některé malé rozdíly v chování rozhraní API služby Service Bus .NET při použití protokolu AMQP, porovnání s výchozím protokolem:

* [OperationTimeout] [ OperationTimeout] vlastnost je ignorována.
* `MessageReceiver.Receive(TimeSpan.Zero)`je implementovaný jako `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Dokončení zprávy pomocí tokenů zámku lze provést pouze příjemci zprávy, které původně přijaté zprávy.

## <a name="controlling-amqp-protocol-settings"></a>Nastavení protokolu AMQP řízení

[Rozhraní API technologie .NET](/dotnet/api/) vystavit několik nastavení, které řídí chování protokolu AMQP:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Určuje počáteční kredit u odkaz. Výchozí hodnota je 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: ovládací prvky maximální velikost protokolu AMQP rámec nabízená při vyjednávání na připojení otevřete čas. Výchozí hodnota je 65 536 bajtů.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Pokud batchable přenosů se tato hodnota určuje maximální zpoždění pro odesílání potížemi. Ve výchozím nastavení dědí odesílatelé nebo příjemci. Jednotlivé odesílatel/příjemce můžete přepsat výchozí, což je 20 milisekund.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: Určuje, zda jsou připojení AMQP navázat připojení přes protokol SSL. Výchozí hodnota je **true**.

## <a name="next-steps"></a>Další kroky

Připraveni na další informace? Získáte pomocí následujících odkazů:

* [Přehled protokolu AMQP Service Bus]
* [Průvodce protokolem AMQP 1.0]
* [AMQP v Service Bus pro systém Windows Server]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Přehled protokolu AMQP Service Bus]: service-bus-amqp-overview.md
[Průvodce protokolem AMQP 1.0]: service-bus-amqp-protocol-guide.md
[AMQP v Service Bus pro systém Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
