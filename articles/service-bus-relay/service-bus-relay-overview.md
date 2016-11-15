---
title: "Přehled předávání Service Bus | Dokumentace Microsoftu"
description: "Přehled předávání v Service Bus."
services: service-bus
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1038a2d8-5def-4f48-8703-cb0070fc5f10
ms.service: service-bus
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0482096cbec6a5e4b7b13ea662a180cd9b96e85f


---
# <a name="overview-of-service-bus-relay"></a>Přehled předávání Service Bus
Hlavní součástí služby Service Bus je centralizovaná *předávací* služba (ale s vysokým vyrovnáváním zatížení), která vám umožní sestavovat hybridní aplikace, které poběží v datovém centru Azure i ve vašem místním podnikovém prostředí.  Předávání Service Bus podporuje různé přenosové protokoly a standardy webových služeb. Mezi ty patří SOAP, WS-*, a dokonce i REST. Služba předávání vašim hybridním aplikacím usnadňuje práci tím, že vám umožní bezpečně vystavit veřejnému cloudu služby technologie Windows Communication Foundation (WCF), které se nacházejí v podnikové síti, a to bez nutnosti otevřít připojení brány firewall nebo udělat výraznější změny v infrastruktuře podnikové sítě. 

![Koncepty předávání WCF](./media/service-bus-relay-overview/sb-relay-01.png)

Služba předávání podporuje tradiční jednosměrný přenos zpráv, přenos typu požadavek/odpověď a přenos zpráv typu peer-to-peer. Taky podporuje distribuci událostí na úrovni internetu, která umožňuje scénáře typu publikování+odběr a obousměrnou soketovou komunikací pro zvýšenou účinnost mezi body. 

V přenosu zpráv s předáváním se lokální služba připojí k předávací službě přes odchozí port a vytvoří obousměrný soket pro komunikaci vázanou na konkrétní potkávací adresu. Klient pak může komunikovat s lokální službou tak, že odešle zprávy do předávací služby s tím, že cílem je potkávací adresa. Předávací služba pak zprávy „předá“ lokální službě přes obousměrný soket, který je už vytvořený. Klient nepotřebuje přímé spojení s lokální službou, nemusí ani vědět, kde se služba nachází, a lokální služba nepotřebuje mít ve firewallu otevřené žádné příchozí porty.

Spojení mezi lokální službou a předávací službou vytvoříte pomocí skupiny „předávacích“ vazeb WCF. Na pozadí se děje to, že předávací vazby mapují do nových elementů přenosové vazby určené k vytvoření komponentů kanálu WCF, které se integrují se službou Service Bus v cloudu. 

## <a name="next-steps"></a>Další kroky
Pokud se o předávání přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

* [Přehled architektury služby Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Jak používat předávání přes službu Service Bus WCF Relay](service-bus-dotnet-how-to-use-relay.md)




<!--HONumber=Nov16_HO2-->


