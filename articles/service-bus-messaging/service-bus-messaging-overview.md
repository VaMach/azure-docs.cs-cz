---
title: "Přehled přenosu zpráv ve službě Azure Service Bus | Dokumentace Microsoftu"
description: "Popis přenosu zpráv ve službě Service Bus a Azure Relay"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 3a4382979dd6e18c0e94b4a989bb8289882eeb89
ms.contentlocale: cs-cz
ms.lasthandoff: 05/26/2017


---
# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Přenos zpráv ve službě Service Bus: flexibilní přenos dat v cloudu
Microsoft Azure Service Bus je spolehlivá služba pro přenos informací. Cílem této služby je usnadnění komunikace. Když si dvě nebo více stran chce vyměňovat informace, potřebují nějakého komunikačního zprostředkovatele. Služba Service Bus je mechanizmus pro komunikaci zprostředkovanou přes třetí stranu. Princip je podobný jako při používání poštovních a zásilkových služeb pro fyzickou poštu a zásilky. Díky poštovním službám můžeme do celého světa posílat nejrůznější dopisy a balíky, s určitými zárukami.

Podobně jako když poštovní služba doručuje dopisy, služba Service Bus nabízí flexibilní doručení informací od odesílatele i příjemce. Služba přenosu zpráv zajišťuje, aby se informace doručily i v případě, že obě strany nejsou online ve stejnou dobu nebo že nejsou dostupné ve stejnou dobu. V tomto ohledu je přenos zpráv podobný jako poslání dopisu, zatímco nezprostředkovaná komunikace je podobná telefonnímu hovoru (resp. tomu, jak telefonování vypadalo dřív – bez identifikace volajícího a bez čekání na to, až bude volané číslo dostupné, to už se totiž podobá spíš zprostředkovanému přenosu zpráv).

Odesílatel zprávy taky může potřebovat nejrůznější charakteristiky dodání, třeba transakce, detekci duplikátů, časově omezené zprávy nebo dávkování. I tyto funkce mají předobraz v poště: opakované dodání, dodejka, změna adresy nebo odvolání.

Service Bus podporuje dva rozdílné způsoby přenosu zpráv: *Azure Relay* a *zasílání zpráv Service Bus*.

## <a name="azure-relay"></a>Azure Relay
Komponenta [WCF Relay](../service-bus-relay/relay-what-is-it.md) služby Azure Relay je centralizovaná služba (ale s vysoce vyváženou zátěží), která podporuje různé přenosové protokoly a standardy webových služeb. Mezi ty patří SOAP, WS-*, a dokonce i REST. [Služba předávání](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) poskytuje množství různých možností předávání a může pomoct vyjednat přímé spojení typu peer-to-peer. Služba Service Bus je optimalizovaná pro vývojáře .NET, kteří používají WCF (Windows Communication Foundation) s ohledem na výkon a použitelnost, a poskytuje plný přístup ke své službě předávání přes rozhraní SOAP a REST. Díky tomu se jakékoli programovací prostředí SOAP nebo REST může integrovat se službou Service Bus.

Služba předávání podporuje tradiční jednosměrný přenos zpráv, přenos typu požadavek/odpověď a přenos zpráv typu peer-to-peer. Taky podporuje distribuci událostí na úrovni internetu, která umožňuje scénáře typu publikování+odběr a obousměrnou soketovou komunikací pro zvýšenou účinnost mezi body. V přenosu zpráv s předáváním se lokální služba připojí k předávací službě přes odchozí port a vytvoří obousměrný soket pro komunikaci vázanou na konkrétní potkávací adresu. Klient pak může komunikovat s lokální službou tak, že odešle zprávy do předávací služby s tím, že cílem je potkávací adresa. Předávací služba pak zprávy „předá“ lokální službě přes obousměrný soket, který je už vytvořený. Klient nepotřebuje přímé spojení s lokální službou, nemusí ani vědět, kde se služba nachází, a lokální služba nepotřebuje mít ve firewallu otevřené žádné příchozí porty.

Propojení místní služby se službou Relay vytvoříte pomocí skupiny „předávacích“ vazeb WCF. Na pozadí se děje to, že předávací vazby mapují do elementů přenosové vazby určené k vytvoření komponentů kanálu WCF, které se integrují se službou Service Bus v cloudu.

WCF Relay má spoustu výhod, ale aby se zprávy mohly posílat a přijímat, musí být server i klient online současně. Tento způsob není ideální pro komunikaci ve stylu HTTP, ve které požadavky nemívají dlouhou životnost, ani pro klienty, kteří se připojují jen občas, jako jsou prohlížeče, mobilní aplikace atd. Zprostředkované zasílání zpráv podporuje oddělenou komunikaci, která má sama o sobě svoje výhody– klienti a servery se například můžou spojit podle potřeby a provádět své operace asynchronním způsobem.

## <a name="brokered-messaging"></a>Zprostředkované zasílání zpráv
Na rozdíl od přenosu zpráv přes službu Relay můžeme o zasílání zpráv prostřednictvím služby Service Bus a [zprostředkovaném zasílání zpráv](service-bus-queues-topics-subscriptions.md) mluvit jako o asynchronním nebo „časově odděleném“. Producenti (odesílatelé) a spotřebitelé (příjemci) nemusí být online ve stejnou dobu. Infrastruktura přenosu zpráv spolehlivě uloží zprávy do „zprostředkovatele“ (například fronty), dokud spotřebitel nebude připravený je přijmout. Díky tomu se součásti distribuované aplikace můžou odpojit, například při údržbě nebo při selhání jedné ze součástí, a přitom to nebude mít vliv na celý systém. Přijímající aplikaci navíc stačí být online jen v určitou dobu – například systém pro správu inventáře stačí spustit až na konci pracovního dne.

Zprostředkované zasílání zpráv přes Service Bus má tři základní součásti: fronty, témata a odběry.  Hlavní rozdíl je v tom, že témata podporují funkce pbulikovat/odebírat, které se dají použít pro komplexní směrování a logiku odesílání obsahu, jako třeba odesílání několika příjemcům. Komponenty umožňují nové scénáře pro zasílání zpráv, jako je časové oddělení, publikování/odběr a vyvažování zátěže. Další informace o těchto entitách zasílání zpráv najdete v tématu [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md).

Stejně jako v případě infrastruktury přenosu zpráv přes službu WCF Relay je funkce zprostředkovaného zasílání zpráv poskytována pro programátory v WCF a .NET Framework a také přes REST.

## <a name="next-steps"></a>Další kroky
Pokud se o přenosu zpráv přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak používat fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)


