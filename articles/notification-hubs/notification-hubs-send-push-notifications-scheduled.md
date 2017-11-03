---
title: "Postup odesílání oznámení naplánované | Microsoft Docs"
description: "Toto téma popisuje naplánované oznámení pomocí Azure Notification Hubs."
services: notification-hubs
documentationcenter: .net
keywords: "nabízená oznámení, nabízených oznámení, plánování nabízená oznámení"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-scheduled-notifications"></a>Postupy: Odesílání naplánované oznámení
## <a name="overview"></a>Přehled
Pokud máte scénář, ve kterém chcete poslat oznámení v určitém okamžiku v budoucnu, ale nemají snadný způsob, jak probuzení si kód back-end pro odeslání oznámení. Úroveň Standard Notification Hubs podporuje funkce, která umožňuje naplánovat oznámení až 7 dní v budoucnu.

Při odesílání oznámení, jednoduše použijte [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) třídy v sadě SDK centra oznámení, jak je znázorněno v následujícím příkladu:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Navíc může zrušit dříve naplánované oznámení pomocí jeho notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Neexistují žádná omezení počtu naplánovaných oznámení, která můžete odeslat.

