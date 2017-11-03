---
title: "Přehled Azure Event Hubs vyhrazené kapacity | Microsoft Docs"
description: "Přehled Microsoft Azure Event Hubs vyhrazené kapacity."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sethm;babanisa
ms.openlocfilehash: db8b119178de0e565b2064e9a52d5e9989d60d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Přehled služby Event Hubs vyhrazené

*Vyhrazené centra událostí* kapacity nabízí nasazení jednoho klienta pro zákazníky s nejnáročnější požadavky. Úplné škálované Azure Event Hubs můžete příjem příchozích dat než 2 miliony událostí za sekundu nebo až 2 GB za sekundu telemetrie s plně odolná úložiště a dílčí sekundu latencí. To také umožňuje integrované řešení zpracování v reálném čase a batch ve stejném systému. S [zaznamenat centra událostí](event-hubs-capture-overview.md) součástí nabídku, můžete snížit složitost řešení tak, že jeden datový proud v reálném čase i na základě batch kanály podpory.

Následující tabulka porovnává úrovní služby k dispozici služby Event Hubs. Nabídka vyhrazené centra událostí je pevný měsíční poplatek, ve srovnání s využití ceny pro většinu funkcí Standard. Vyhrazené úroveň nabízí funkce standardní plán, ale s kapacitou škálování enterprise pro zákazníky s náročné úlohy. 

| Funkce | Standard | Vyhrazený |
| --- |:---:|:---:|:---:|
| Události příchozího přenosu dat | Platba za mil. události | Zahrnuje |
| Jednotky propustnosti (1 MB za sekundu vstupní, výstupní 2 MB za sekundu) | Platit za hodinu | Zahrnuje |
| Velikost zprávy | 256 kB | 1 MB |
| Zásady vydavatele | Ano | Ano |   
| Skupiny příjemců | 20 | 20 |
| Přehrání zprávy | Ano | Ano |
| Maximální počet jednotek propustnosti | 20 (flexibilní do 100)   | 1 kapacitní jednotka ≈ 200 |
| Zprostředkovaná připojení | 1000 zahrnuté | 100 tisíc zahrnuté |
| Další zprostředkovaná připojení | Ano | Ano |
| Uchovávání zpráv | 1 den v základu | Až 7 dnů v ceně |
| Zachycování | Platit za hodinu | Zahrnuje |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Výhody kapacity vyhrazené centra událostí

Při použití vyhrazené centra událostí jsou k dispozici následující výhody:

* Hostování s žádné šumu od ostatních klientů jednoho klienta.
* Velikost zprávy zvyšuje 1 MB porovnání s 256 KB pro Standard.
* Pokaždé, když Opakovatelný výkon.
* Zaručit kapacitu podle vašich potřeb shluků.
* Škálovatelná mezi 1 a 8 kapacitu jednotky (CU) – poskytuje až 2 milionů příjem příchozích dat událostí za sekundu.
  * Vlas spravovat měřítko pro vyhrazené centra událostí, kde každý Cu: můžete zadat přibližně ekvivalentem 200 jednotky propustnosti (TU).
* Nula údržby: jsme spravovat vyrovnávání zatížení, OS aktualizace, opravy zabezpečení a rozdělení do oddílů.
* Pevné ceny měsíčně.

Vyhrazené centra událostí taky odebere některá omezení propustnosti standardní nabídky. Jednotky propustnosti na vrstvě Standard získat oprávnění k 1000 událostí za sekundu nebo 1 MB za sekundu příjem příchozích dat za TU a double toto množství odchozí. Nabídka vyhrazené škálování nemá žádné omezení na příjem příchozích dat a počty odchozí události. Těchto mezních hodnot se řídí pouze zpracování kapacitu zakoupené event hubs.

Tato služba je zaměřený na největší uživatelé telemetrie a je k dispozici pro zákazníky s smlouvu enterprise agreement.

## <a name="how-to-onboard"></a>Jak se budou registrovat

Platforma vyhrazené centra událostí je nabízen smlouvu enterprise agreement s různou velikost vlas. Každý Cu: poskytuje přibližně ekvivalentem 200 jednotek propustnosti. Vaše kapacita je možné škálovat nahoru nebo dolů v průběhu tohoto měsíce přidáním nebo odebráním vlas podle svých potřeb. Vyhrazené plán je jedinečný, v tom, že si všimnete víc praktických registrace z produktového týmu služby Event Hubs se získat flexibilní nasazení, který je pro vás nejvhodnější. 

## <a name="next-steps"></a>Další kroky
Obraťte se na obchodním zástupcem společnosti Microsoft nebo Microsoft Support a získejte další informace o kapacitě vyhrazené centra událostí. Také další informace o službě Event Hubs když přejdete na následujících odkazech:

Podrobné informace o cenách získáte pomocí následujících odkazů:

- [Vyhrazené centra událostí ceny](https://azure.microsoft.com/pricing/details/event-hubs/). Můžete také kontaktovat na obchodním zástupcem společnosti Microsoft nebo Microsoft Support a získejte další informace o kapacitě vyhrazené centra událostí.
- [– Nejčastější dotazy centra událostí](event-hubs-faq.md) obsahuje informace o cenách a odpovídá na některé nejčastější dotazy týkající se služby Event Hubs. 
