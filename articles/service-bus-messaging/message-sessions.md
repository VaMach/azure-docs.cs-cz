---
title: "Azure Service Bus zprávu relací | Microsoft Docs"
description: "Zpracování zpráv Azure Service Bus s relací pořadí."
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: sethm
ms.openlocfilehash: c6441d2119518e89a869ee52e5f0b80450ae2bbe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="message-sessions--first-in-first-out-fifo"></a>Zprávy relací / nejprve, nejprve out (FIFO) 

Service Bus relací povolte společné a seřazené zpracování bez vazby pořadí související zprávy. Pokud chcete záruku FIFO v Service Bus, budete muset použít relací. Service Bus není doporučený o povaze vztah mezi zprávy a také nedefinuje konkrétním modelu pro určení, kde sekvenci zpráv začíná nebo končí.

Všechny odesílatele můžete vytvořit relaci při odeslání zprávy do tématu nebo fronty nastavením [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) zprostředkovatel vlastnost na identifikátor některé definované aplikací, které jsou jedinečné pro relace. Na úrovni protokolu AMQP 1.0, tato hodnota mapuje *id skupiny* vlastnost.

Podporující relace fronty nebo předplatných, relace začalo existence po minimálně jednu zprávu s relace [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Jakmile existuje relace, neexistuje žádné definované časové nebo rozhraní API pro při vypršení platnosti relace, nebo zmizí. Teoreticky, můžete být přijata zpráva pro relaci v současné době další zprávy v průběhu času v roce a pokud **SessionId** odpovídá, relace je stejný z hlediska Service Bus.

Obvykle ale má aplikace zrušte představu o kterém sada související zprávy spustí a končí; ale sběrnice není nastavena žádná konkrétní pravidla.

Příklad toho, jak zobrazit pořadí pro přenos souboru je nastavit **popisek** vlastnost pro první zprávu, která se **spustit**, pro zprostředkující zprávy a pokuste se **obsah**, a pro poslední zprávy **end**. Relativní umístění obsahu zprávy můžete vypočítat jako aktuální zprávu *SequenceNumber* rozdílů z **spustit** zprávy *SequenceNumber*.

Funkci relace v Service Bus umožňuje konkrétní přijímat operace, ve formě [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) v C# a rozhraní API Java. Povolit funkci nastavením [vlastnost requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) vlastnost fronty nebo předplatné prostřednictvím Správce Azure Resource Manager, nebo nastavením příznaku na portálu. To je potřeba, před dalším pokusem o použití související operace rozhraní API.

Na portálu je nastavený příznak s následující zaškrtávací políčko:

![][2]

Rozhraní API pro relace existovat na klientských počítačích fronty a předplatné. Je model imperativní, kde můžete řídit při příjmu zprávy a relací a model na základě obslužná rutina podobná *OnMessage*, že skryje složitost správy je přijetí cykly.

## <a name="session-features"></a>Funkce relace

Relace zadejte souběžných demultiplexování datové proudy prokládaná zprávy při zachování a zaručující seřazené doručení.

![][1]

A [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) příjemce je vytvoření klient přijetí relaci. Imperativní, volání klienta [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) nebo [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) v jazyce C#. V modelu reaktivní zpětného volání Registruje obslužnou rutinu relace, jak je popsáno později.

Když [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) byla přijata a při jeho trvá klientem, že klient má výhradní zámek na všechny zprávy s danou relaci [SessionId](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) existující fronty nebo předplatného a také na všechny zprávy s třídou **SessionId** , stále dorazí během relace trvá.

Vydání zámek při **Zavřít** nebo **CloseAsync** se nazývají, nebo když vyprší platnost zámek v případech, ve kterých je aplikace nelze to učinit. Zámek relace zacházeno jako výhradní zámek na soubor, což znamená, že aplikace má ukončení relace Jakmile již musí ho a neočekává žádné další zprávy.

Když více souběžných příjemci načítat z fronty, jsou odesílány zprávy, které patří do konkrétní relace do konkrétní příjemce, který má v současnosti zámek pro tuto relaci. V této operaci datového proudu prokládaná zpráv, které se nacházejí v jednom fronty nebo předplatné je řádně zrušte multiplexní k různým příjemcům a tyto příjemci můžete také za provozu na různých klientských počítačů, protože správa zámku se straně služby, stane uvnitř Service Bus.

Fronta je, ale stále fronty: není k dispozici náhodný přístup. Pokud je více souběžných příjemci čekat na přijmout konkrétní relací nebo počkejte na zprávy z konkrétní relace a zpráva v horní části fronty patřící do relace, který nemá žádný příjemce ještě požadován, dodávky podržte, dokud relace příjemce deklarace identity, která relace.

Tři souběžných relací příjemců, což vyžaduje aktivně zprávy z fronty pro každý příjemce pro poskytnutí přehledu o pokroku na předchozím obrázku. Předchozí relace s *SessionId*= 4 má žádné active vlastnícím klienta, což znamená, že žádné doručování zpráv všem uživatelům, dokud zpráva byla přijata. podle nově vytvořený, který vlastní relace příjemce.

Při, může se zdát být chovaly, proces jednoho příjemce může zpracovávat velký počet souběžných relací snadno, zejména v případě, že jsou zapsány s výhradně asynchronní kódu; této několik desítek souběžných relací je efektivně automatické s modelem zpětného volání.

Strategie pro zpracování velký počet souběžných relací, kterým každou relaci pouze zřídka přijímá zprávy, je pro obslužnou rutinu vyřadit relace po určité době nečinnosti a pokračovat ve zpracování, když relace byla přijata jako dorazí další relace.

Relace uzamčení relace příjemce je zastřešující pro zámek zprávy používané *funkce Náhled zámku* vyrovnání režimu. Příjemce nemůže současně mít dvě zprávy "v cestě", ale zprávy musí být zpracovány v pořadí. Nové zprávy se dá získat jenom při předchozí zpráva byla dokončena nebo lettered zpráv. Zrušení zpráva příčiny, stejná zpráva obsluhovat znovu s další operace příjmu.

## <a name="message-session-state"></a>Stav relace zprávy

Když pracovní postupy jsou zpracovány v velkého rozsahu, vysokou dostupnost cloudové systémy, pracovní postup obslužná rutina přidružená konkrétní relace musí být schopna obnovit z neočekávaných chyb a také být schopná obnovení částečně dokončené práce na jiné proces nebo počítač z kde začal práce.

Zařízení stavu relace umožňuje anotaci definované aplikací relace zpráva uvnitř broker, tak, aby stav zaznamenaná zpracování relativně k této relaci bude okamžitě dostupná, když relace se získávají pomocí nové procesoru.

Z hlediska Service Bus je stav relace zpráva neprůhledného binární objekt, který může obsahovat data velikosti jednu zprávu, která je 256 KB pro Service Bus Standard a 1 MB pro Service Bus Premium. Stav zpracování relativně k relaci může uchovávat uvnitř stavu relace nebo stavu relace může ukazovat na některé umístění úložiště nebo záznam databáze, která obsahuje tyto informace.

Rozhraní API pro správu stavu relace, [setstate –](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) a [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), naleznete na [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objekt v C# i rozhraní API Java. Relace, který měl předtím žádný stav relace nastavit vrátí **null** reference pro **GetState**. Vymazání stavu dříve nastavené relace se provádí s [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Všechny existující relace v fronty nebo předplatného, mohou být uvedené s **SessionBrowser** metoda v rozhraní API Java a [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) na [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) a [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) v rozhraní .NET klienta.

Stav relace uchovávat v frontu nebo v, že předplatné, se započítává kvótu úložiště dané entity. Po dokončení aplikace s relací se proto doporučuje pro aplikace, vyčistěte stav zachované vyhnout náklady na externí správu.

## <a name="next-steps"></a>Další kroky

- [Úplný příklad](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/BasicSessionSendReceiveUsingQueueClient) odesílat a přijímat na základě relace zprávy ze sběrnice fronty pomocí rozhraní .NET standardní knihovny.
- [Ukázka](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) používající klienta rozhraní .NET Framework pro zpracování deklaracemi relace zprávy. 

Další informace o zasílání zpráv Service Bus, najdete v následujících tématech:

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png