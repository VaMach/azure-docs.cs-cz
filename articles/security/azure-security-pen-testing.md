---
title: "Testování pera | Microsoft Docs"
description: "Článek obsahuje přehled průnikům testování procesu (pentest) a jak provádět pentest proti vaší aplikace běžící v Azure infrastruktury."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 070e848f753452953b9e5dfe94799e7c0a314530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="pen-testing"></a>Testování pera
Jedním z užitečných funkcí pomocí Microsoft Azure pro testování aplikací a nasazení je, že nemusíte dávat dohromady místní infrastruktury pro vývoj, testování a nasazení aplikací. Všechny infrastruktury se stará pomocí služeb platformy Microsoft Azure. Nemusíte si dělat starosti o žádanek, získávání a "stáčení a překrývání" vlastní místní hardware.

Toto je skvělým – ale stále nutné a ujistěte se, proveďte běžné zabezpečení kvůli opatrností. Jednou z věcí, musíte udělat je průnikům otestujte nasazení aplikace v Azure.

Možná už víte, že Microsoft provádí [průnikům testování naše prostředí Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). To nám pomůže vylepšovat naše platforma a provede naše akce z hlediska vylepšení kontrolních mechanismů pro zabezpečení, Představujeme nové prvky zabezpečení a zlepšení našeho procesy zabezpečení.

Jsme nemáte pera testování vaší aplikace pro vás, ale Chápeme, že se má a potřeba provést testování na vaše vlastní aplikace pera. Dobrá vlastnost, je to způsobeno můžete zvýšit zabezpečení aplikací, vám pomoci lépe zabezpečit celý ekosystém Azure.

Pokud jste pera testování vaší aplikace, může vypadat například útoku do us. Jsme [neustále monitorovat](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) pro vzorce útoků a zahájí o proces reakcí na incidenty, pokud je potřeba. Není můžete a nemá nám pomoci Pokud odpověď incidentu z důvodu vlastní kvůli opatrností pera testování se spouští.

Co dělat?

Až budete připraveni na pero testování aplikací hostovaných v Azure, máte možnost [dejte nám vědět](https://portal.msrc.microsoft.com/en-us/engage/pentest). Jakmile víme, že se chystáte provedení specifických testů, jsme nebude vypnout nechtěně můžete (třeba blokování IP adresu, která testujete z), a pokud testy v souladu s Azure pera testování podmínky a ujednání popsaných v [ Microsoft Cloud Unified průnikům testování pravidla zapojení](https://technet.microsoft.com/en-us/mt784683).
Standardní testy, které můžete provádět, zahrnují:

* Testy na koncové body k odhalení [aplikace otevřete webový projekt zabezpečení (OWASP) top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Testování argumentu neurčité](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) vaše koncových bodů
* [Kontrola portů](https://en.wikipedia.org/wiki/Port_scanner) vaše koncových bodů

Druh jeden typ testu, který nelze provést žádné je [útok na dostupnost služby (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) útoku. To zahrnuje inicializaci útoku DoS sám sebe nebo provádění souvisejících testy, které může určit, ukazují nebo simulovat jakýkoli typ útoku DoS.

Jste připravení začít s pera testování vaší aplikace hostované ve službě Microsoft Azure? Pokud ano, pak přejděte na přes [přehled testovací průnikům](https://technet.microsoft.com/library/mt784683.aspx) (a klikněte na tlačítko požádat o testování v dolní části stránky vytvořit. Naleznete zde také další informace o pera testování podmínky a ujednání a užitečné odkazy na tom, jak můžete sestavu nedostatky zabezpečení vztahující se k Azure nebo jinou službu Microsoft.
