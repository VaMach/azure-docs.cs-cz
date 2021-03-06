---
title: "Omezení IP adres služby Azure App Service | Microsoft Docs"
description: "Použití omezení IP adres s Azure App Service"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 551f34436dfeac14bb6e8676dd7c9b10d30a3af3
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="azure-app-service-static-ip-restrictions"></a>Omezení statickou IP adresu služby Azure App Service #

Omezení IP adres umožňují definovat seznam IP adres, které jsou povoleny pro přístup k aplikaci. Povolit seznam může obsahovat jednotlivé IP adresy nebo rozsahu IP adres definované masku podsítě.

Po vygenerování požadavek na aplikaci z klienta, IP adresa je porovnán s seznamu povolených. Pokud IP adresa není v seznamu, s reaguje aplikace [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) stavový kód.

Omezení IP adres jsou definované v souboru web.config, který využívá aplikace za běhu (v sadu povolených IP adres v souboru applicationHost.config, se vkládají více přesně omezení, pokud je také přidat sadu povolených IP adres v souboru web.config, bude trvat Priorita). Za určitých okolností může být některé modulu spuštěny před logiku omezení IP v kanálu HTTP. V takovém případě požadavek selže s jiný kód chyby protokolu HTTP.

Omezení IP adres se vyhodnocují na stejných instancí plánu služby App Service přiřazené vaší aplikaci.

Přidat pravidlo omezení IP do vaší aplikace, použijte v nabídce otevřete **sítě**>**omezení IP adres** a klikněte na **konfigurovat omezení IP adres**

! [Omezení IP adres] (media/app-service-ip-restrictions/ip-restrictions.png)

Tady najdete seznam pravidel omezení IP definované pro vaši aplikaci.

![seznam omezení IP adres](media/app-service-ip-restrictions/browse-ip-restrictions.png)

Kliknutím na **[+] přidat** přidat nové pravidlo omezení IP.

![Přidat omezení IP adres](media/app-service-ip-restrictions/add-ip-restrictions.png)
