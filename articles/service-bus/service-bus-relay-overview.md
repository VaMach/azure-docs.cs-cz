<properties
    pageTitle="Přehled přenosu zpráv přes předávací službu Service Bus | Microsoft Azure"
    description="Přehled předávání v Service Bus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="05/06/2016"
    ms.author="sethm"/>


# Přenos zpráv přes předávací službu Service Bus

Ústřední součást služby Service Bus je centralizovaná předávací služba (ale s vysoce vyváženou zátěží), která vám umožní sestavovat hybridní aplikace, které poběží v datovém centru Azure i ve vašem lokálním podnikovém prostředí.  Předávací služba podporuje různé přenosové protokoly a standardy webových služeb. Mezi ty patří SOAP, WS-*, a dokonce i REST. Předávání přes Service Bus vašim hybridním službám usnadňuje práci tím, že vám umožní bezpečně vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti, veřejnému cloudu, a to bez nutnosti otevřít spojení ve firewallu nebo udělat nežádoucí změny v infrastruktuře podnikové sítě. 

![Koncepty předávání](./media/service-bus-relay-overview/sb-relay-01.png)

Služba předávání podporuje tradiční jednosměrný přenos zpráv, přenos typu požadavek/odpověď a přenos zpráv typu peer-to-peer. Taky podporuje distribuci událostí na úrovni internetu, která umožňuje scénáře typu publikování+odběr a obousměrnou soketovou komunikací pro zvýšenou účinnost mezi body. 

V přenosu zpráv s předáváním se lokální služba připojí k předávací službě přes odchozí port a vytvoří obousměrný soket pro komunikaci vázanou na konkrétní potkávací adresu. Klient pak může komunikovat s lokální službou tak, že odešle zprávy do předávací služby s tím, že cílem je potkávací adresa. Předávací služba pak zprávy „předá“ lokální službě přes obousměrný soket, který je už vytvořený. Klient nepotřebuje přímé spojení s lokální službou, nemusí ani vědět, kde se služba nachází, a lokální služba nepotřebuje mít ve firewallu otevřené žádné příchozí porty.

Spojení mezi lokální službou a předávací službou vytvoříte pomocí skupiny „předávacích“ vazeb WCF. Na pozadí se děje to, že předávací vazby mapují do nových elementů přenosové vazby určené k vytvoření komponentů kanálu WCF, které se integrují se službou Service Bus v cloudu. 

## Další kroky

Pokud se o předávání přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

- [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Jak používat službu předávání přes Service Bus](service-bus-dotnet-how-to-use-relay.md)

 


<!--HONumber=Jun16_HO2-->


