---
title: "Míra, omezení pro SMS, e-mailů, nabízená oznámení v aplikaci Azure a webhooky | Microsoft Docs"
description: "Pochopte, jak Azure omezuje počet možných SMS, e-mailu, aplikaci Azure nabízené nebo webhooku oznámení ze skupiny pro akce."
author: dukek
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/8/2017
ms.author: dukek
ms.openlocfilehash: c76bf5cf51f18a32b33060d528c64d119e31dbbd
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Míra, omezení pro zprávy SMS, e-mailů, nabízená oznámení v aplikaci Azure a webhooku příspěvcích
Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají do konkrétní telefonní číslo, e-mailovou adresu nebo zařízení. Omezení rychlosti zajistí výstrahy spravovat a možné použít.

Prahové hodnoty omezení míry jsou:

 - **SMS**: více než 1 SMS každých 5 minut.
 - **E-mailu**: 100 zpráv za hodinu.
 - **Azure nabízená oznámení v aplikaci**: neexistuje žádné kurz omezení pro nabízená oznámení.
 - **Webhooky**: neexistuje žádné kurz omezení pro webhooky.

## <a name="rate-limit-rules"></a>Míra limit pravidla
- Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota umožňuje omezené.
- Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů. Omezení rychlosti platí ve všech předplatných. Jakmile je dosaženo prahové hodnoty, platí i v případě, že jsou zpráv odesílány z více předplatných.  
- Míra omezené po e-mailovou adresu další oznámení se odesílá komunikovat omezení rychlosti. Stavy oznámení, když vyprší platnost omezení rychlosti.

## <a name="next-steps"></a>Další kroky ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.  
* Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
