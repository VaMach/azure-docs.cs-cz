---
title: Co je Azure Relay? | Dokumentace Microsoftu
description: "Přehled služby Azure Relay"
services: service-bus-relay
documentationcenter: .net
author: banisadr
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 11/16/2016
ms.author: babanisa,sethm
translationtype: Human Translation
ms.sourcegitcommit: 8ba8baa1b6500b3300e0bfc1c6b9f00f214f0179
ms.openlocfilehash: 4329e0f99828f5a0651e6b03be0813d9e68debaa


---
# <a name="what-is-azure-relay"></a>Co je Azure Relay?
Služba Azure Relay hybridním aplikacím usnadňuje práci tím, že vám umožní bezpečně vystavit veřejnému cloudu služby, které se nacházejí v podnikové síti, a to bez nutnosti otevřít připojení brány firewall nebo provést výraznější změny v infrastruktuře podnikové sítě. Služba Relay podporuje různé přenosové protokoly a standardy webových služeb.

Služba Relay podporuje tradiční jednosměrný provoz, provoz typu požadavek/odpověď a provoz peer-to-peer. Taky podporuje distribuci událostí na úrovni internetu, která umožňuje scénáře typu publikování+odběr a obousměrnou soketovou komunikací pro zvýšenou účinnost mezi body. 

V přenosu dat s předáváním se lokální služba připojí k službě Relay přes port pro odchozí spojení a vytvoří obousměrný soket pro komunikaci vázanou na konkrétní potkávací adresu. Klient pak může komunikovat s místní službou odesíláním provozu do služby Relay s tím, že cílem je potkávací adresa. Služba Relay pak data „předá“ místní službě přes obousměrný soket vyhrazený pro jednotlivé klienty. Klient nepotřebuje přímé spojení s lokální službou, nemusí ani vědět, kde se služba nachází, a lokální služba nepotřebuje mít ve firewallu otevřené žádné příchozí porty.

Mezi klíčové schopnosti, které služba Relay nabízí, patří obousměrná komunikace bez vyrovnávací paměti přes hranice sítí s omezováním toku dat (podobně jako u protokolu TCP), zjišťování koncových bodů, stav připojení a překrývající se zabezpečení koncových bodů. Schopnosti služby Relay se liší od integračních technologií na úrovni sítě, jako je například síť VPN, tím, že službu Relay lze omezit na jediný koncový bod aplikace na jednom počítači, zatímco technologie sítě VPN se spoléhá na upravování síťového prostředí a mnohem víc do něj zasahuje.

Azure Relay má dvě funkce:

1. [Hybridní připojení](#hybrid-connections) – Pomocí otevřených webových soketů umožňuje scénáře s podporou více platforem.
2. [Přenosy WCF](#wcf-relays) – Pomocí technologie Windows Communication Foundation (WCF) umožňuje vzdálená volání procedur. WCF Relay je starší verze nabídky služby Relay kterou již mnozí uživatelé používají ve svých programovacích modelech WCF.

Hybridní připojení i přenosy WCF umožňují zabezpečené připojení k prostředkům existujícím v rámci podnikové sítě. Použití jedné nebo druhé funkce závisí na konkrétních požadavcích, jak je popsáno v následující tabulce:

|  | WCF Relay | Hybridní připojení |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |
| **JavaScript/NodeJS*** | |x |
| **Java*** | |x |
| **Otevřený protokol založený na standardech** | |x |
| **Několik programovacích modelů protokolu RPC** | |x |

*Podle všeobecné dostupnosti

## <a name="hybrid-connections"></a>Hybridní připojení
Schopnost zabezpečených [hybridních připojení služby Azure Relay](relay-hybrid-connections-protocol.md) s podporou otevřených protokolů je důsledkem vývoje stávajících funkcí služby Relay. Hybridní připojení můžete nasadit na jakékoli platformě a v libovolném jazyku se základní podporou protokolu WebSocket, konkrétně rozhraní WebSocket API v běžných webových prohlížečích. Hybridní připojení jsou založená na protokolech HTTP a WebSocket.

## <a name="wcf-relays"></a>Přenosy WCF
Služba WCF Relay funguje s celým rozhraním .NET Framework (NETFX) i s technologií WCF. Propojení místní služby se službou Relay vytvoříte pomocí skupiny „předávacích“ vazeb WCF. Na pozadí se děje to, že předávací vazby mapují do nových elementů přenosové vazby určené k vytvoření komponentů kanálu WCF, které se integrují se službou Service Bus v cloudu.

## <a name="service-history"></a>Historie služby
Hybridní připojení nahrazuje starší funkci služby BizTalk Services se stejným názvem, která byla postavená na službě Azure Service Bus WCF Relay. Nová schopnost hybridních připojení doplňuje stávající službu WCF Relay a tyto dvě schopnosti služby v dohledné budoucnosti budou existovat vedle sebe. Sdílejí sice společnou bránu, jinak se ale jedná o rozdílné implementace.

## <a name="next-steps"></a>Další kroky:
* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Nov16_HO3-->


