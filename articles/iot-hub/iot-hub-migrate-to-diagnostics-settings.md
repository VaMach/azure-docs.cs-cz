---
title: "Azure IoT Hub migrovat nastavení diagnostiky | Microsoft Docs"
description: "Postup aktualizace Azure IoT Hub pro použití nastavení diagnostiky Azure místo operations monitorování ke sledování stavu operací ve službě IoT hub v reálném čase."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 2716f397ad0e7abfdcd397340da8fa8116a172db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migrace služby IoT Hub operacích monitorování k nastavení diagnostiky

Zákazníci, kteří používají [operations monitorování] [ lnk-opsmon] ke sledování stavu operací v IoT Hub můžete migrovat tento pracovní postup [nastavení Azure diagnostiky] [ lnk-diagnostics-settings], funkce monitorování Azure. Nastavení diagnostiky zadat úrovni prostředků diagnostických informací pro řadou služeb Azure.

Operace monitorování funkcí služby IoT Hub je zastaralá a bude v budoucnu odeberou. Tento článek popisuje kroky pro přesun úlohy z operations monitorování k nastavení diagnostiky. Další informace o vyřazení časovou osu najdete v tématu [monitorovat řešení Azure IoT s monitorování Azure a Azure Resource Health][lnk-blog-announcement].

## <a name="update-iot-hub"></a>Aktualizace služby IoT Hub

Aktualizace služby IoT Hub v portálu Azure, nejprve zapnout nastavení diagnostiky a potom vypnout operations monitorování.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Vypnout operations monitorování

Po testování nového nastavení diagnostiky na pracovní postup, můžete vypnout funkci monitorování operace. 

1. V nabídce centra IoT, vyberte **Operations monitorování**.
1. V rámci každé monitorování kategorie, vyberte **žádné**.
1. Uložte operace sledování změn.

## <a name="update-applications-that-use-operations-monitoring"></a>Aktualizace aplikací, které používají operations monitorování

Schémata pro operace sledování a nastavení diagnostiky se mírně lišit. Je důležité, že můžete aktualizovat aplikace, které používají operations monitorování dnes k mapování na schéma používá nastavení diagnostiky. 

Navíc diagnostiky nastavení nabízí sledování pět nových kategorií. Po aktualizaci aplikací pro existující schéma, přidáte nové kategorie:

- Operace dvojče cloud zařízení
- Operace twin zařízení cloud
- Dotazy Twin
- Operace úloh
- Přímé metody

Struktury konkrétní schématu, najdete v části [pochopit schéma pro nastavení diagnostiky][lnk-diagnostics-schema].

## <a name="next-steps"></a>Další kroky

- [Monitorování stavu služby Azure IoT Hub a rychle diagnostikovat problémy][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
