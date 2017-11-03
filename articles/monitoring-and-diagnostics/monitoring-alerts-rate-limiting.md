---
title: "Míra, omezení pro SMS, e-mailů a webhooky | Microsoft Docs"
description: "Pochopte, jak Azure omezuje počet možných oznámení SMS, e-mailu nebo webhooku ze skupiny pro akce."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Míra, omezení pro zprávy SMS, e-mailů a webhooku příspěvcích
Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají do konkrétní telefonní číslo nebo e-mailovou adresu. Omezení rychlosti zajistí výstrahy spravovat a možné použít.

Pravidla pro SMS a e-mailu jsou stejné. Prahová hodnota omezení míry je:

 - **SMS**: 10 zpráv za hodinu.
 - **E-mailu**: 100 zpráv za hodinu.

## <a name="rate-limit-rules"></a>Míra limit pravidla
- Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota umožňuje omezené.
- Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů. Omezení rychlosti platí ve všech předplatných. Jakmile je dosaženo prahové hodnoty, platí i v případě, že jsou zpráv odesílány z více předplatných.  
- Pokud telefonní číslo nebo e-mailu je míra omezené, další oznámení odesláno komunikovat omezení rychlosti. Stavy oznámení, když vyprší platnost omezení rychlosti.

## <a name="rate-limit-of-webhooks"></a>Míra limit webhooky ##
Neexistuje žádné kurz omezení nastavené pro webhooky.

## <a name="next-steps"></a>Další kroky ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.  
* Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
