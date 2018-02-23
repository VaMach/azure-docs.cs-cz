---
title: "Míra, omezení pro SMS, e-mailů, nabízená oznámení v aplikaci Azure a webhooky | Microsoft Docs"
description: "Pochopte, jak Azure omezuje počet možných SMS, e-mailu, aplikaci Azure nabízené nebo webhooku oznámení ze skupiny pro akce."
author: dkamstra
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
ms.date: 2/16/2018
ms.author: dukek
ms.openlocfilehash: a4f97cc79945d266edd0af577e37fc9da2aa97bf
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Míra, omezení pro zprávy SMS, e-mailů, nabízená oznámení v aplikaci Azure a webhooku příspěvcích
Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají do konkrétní telefonní číslo, e-mailovou adresu nebo zařízení. Omezení rychlosti zajistí výstrahy spravovat a možné použít.

Prahové hodnoty omezení míry jsou:

 - **SMS**: více než 1 SMS každých 5 minut.
 - **E-mailu**: více než 100 e-mailů za hodinu.
 
 Další akce nejsou míra omezené.

## <a name="rate-limit-rules"></a>Míra limit pravidla
- Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota umožňuje omezené.
- Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů. Omezení rychlosti platí ve všech předplatných. Jakmile je dosaženo prahové hodnoty, platí i v případě, že jsou zpráv odesílány z více předplatných.
- Míra omezené po e-mailovou adresu další oznámení se odesílá komunikovat omezení rychlosti. Stavy oznámení, když vyprší platnost omezení rychlosti.

## <a name="next-steps"></a>Další postup ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.  
* Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
