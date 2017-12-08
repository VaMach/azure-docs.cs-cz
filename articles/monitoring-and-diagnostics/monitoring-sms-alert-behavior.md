---
title: "SMS výstrahy chování ve skupinách akce | Microsoft Docs"
description: "Formát zprávy SMS a odpovídat na zprávy SMS k odhlášení odběru, obnovit předplatné nebo požádat o pomoc."
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
ms.openlocfilehash: 3e4eca174209eeb9cbce1d45111d1e5cc30af8b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS výstrahy chování ve skupinách, akce
## <a name="overview"></a>Přehled ##
Akce skupiny umožňují konfigurovat seznam příjemců. Tyto skupiny můžete využít pak při definování aktivity protokolu výstrah; zajištění, že konkrétní akce skupiny zasláno oznámení, když se aktivuje výstraha aktivity protokolu. Jeden z výstrahy mechanismů podporována je SMS; výstrahy podporovat jednosměrnou komunikaci. Uživatel může reagovat na výstrahu:

- **Odběr upozornění:** uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.  
- **Obnovit předplatné výstrah:** uživatele můžete obnovit předplatné na všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.  
- **Požádat o pomoc:** uživatel může požádat o další informace o serveru SMS. Bude přesměrován na tento článek

Tento článek se zabývá chování výstrahy SMS a akce odpovědi uživatele můžete provádět na základě národního prostředí uživatele:

## <a name="usacanada-sms-behavior"></a>Chování USA nebo Kanady SMS
### <a name="receiving-an-sms-alert"></a>Příjem oznámení SMS
SMS příjemce, který je nakonfigurovaný jako součást skupiny akce, obdrží serveru služby SMS při aktivuje výstrahu. Serveru SMS, bude mít následující informace:
* Shortname skupiny akce, které tato výstraha byla odeslána
- Název výstrahy

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Odhlášení z SMS výstrahy pro jednu skupinu akce
Uživatele můžete zrušit odběr služby SMS pro výstrahy pro jednu akci skupinu podle neodpovídá na požadavky shortcode 20873 s klíčovými slovy: "Zakázat &lt;Shortname akce skupiny&gt;".

Příklad: Uživatel chtějí odhlásit z výstrah pro skupinu akce s shortname "Azure" by odeslat zprávu SMS shortcode 20873 s informacemi o tom "Zakázat Azure"

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Odhlášení z SMS výstrahy pro všechny skupiny akce
Uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce odpovědi na shortcode 20873 s žádným z následujících klíčových slov:
* STOP

Příklad: Uživatel chtějí odběr všech upozornění služby SMS pro všechny skupiny akce by odeslat zprávu SMS shortcode 20873 s informacemi o tom "STOP"

>[!NOTE]
>Pokud uživatel odhlásil(a) odběr z výstrah služby SMS, ale se pak přidá do nové skupiny akce; BUDE přijímat výstrahy služby SMS pro tuto novou skupinu akce ale zůstávají odhlásit ze všech skupin, předchozí akce.
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a>Resubscribing SMS výstrah pro jednu skupinu akce
Uživatele můžete obnovit předplatné do služby SMS pro výstrahy pro jednu akci skupinu podle neodpovídá na požadavky shortcode 20873 s klíčová slova: "Povolit &lt;Shortname akce skupiny&gt;".

Příklad: Uživatel chtějí obnovit předplatné výstrah pro skupinu akce s shortname "Azure" by odeslat zprávu SMS shortcode 20873 s informacemi o tom "Povolit Azure"

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a>Resubscribing SMS výstrah pro všechny skupiny akce
Uživatele můžete obnovit předplatné pro všechny služby SMS pro výstrahy pro všechny skupiny akce odpovědi na shortcode 20873 s žádným z následujících klíčových slov:

* SPUŠTĚNÍ

Příklad: Uživatel chtějí odběr všech upozornění služby SMS pro všechny skupiny akce by odeslat zprávu SMS shortcode 20873 s informacemi o tom "START"

### <a name="requesting-help-via-sms"></a>Žádost o nápovědu prostřednictvím serveru SMS
Uživatel může požádat o další informace o serveru SMS, že obdržely podle neodpovídá na požadavky shortcode 20873 s žádným z následujících klíčových slov:
* POMOC

Odpověď zašle uživateli se zobrazí odkaz na tohoto článku.

## <a name="next-steps"></a>Další kroky
Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md) a zjistěte, jak získat výstrahy  
Další informace o [omezení rychlosti SMS](monitoring-alerts-rate-limiting.md)  
Další informace o [skupiny akcí](monitoring-action-groups.md)
