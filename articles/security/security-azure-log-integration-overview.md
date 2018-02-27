---
title: "Protokoly z prostředků Azure integrovat do vašeho systému SIEM systémy | Microsoft Docs"
description: "Další informace o integraci Azure protokolu, jejích klíčových funkcích a jak to funguje."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Úvod do integrace protokolu Microsoft Azure

Integrace protokolů Azure umožňuje integraci s místní informace o zabezpečení a událostí správy (SIEM) systémy nezpracovaná protokoly z vašich prostředků Azure v případě, že konektor k [Azure monitorování](../monitoring-and-diagnostics/monitoring-get-started.md) dosud nejsou k dispozici od dodavatele systému SIEM.

Upřednostňovanou metodou pro integraci Azure protokoly se pomocí svého dodavatele SIEM Azure monitorování konektoru a následující tyto [pokyny](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). Ale pokud dodavatele SIEM neposkytuje konektor k monitorování Azure, bude pravděpodobně možné použít protokol integrace se službou Azure jako dočasné řešení (je-li vašeho systému SIEM podporován protokol integrace se službou Azure) dokud takové connector je k dispozici.

>[!IMPORTANT]
>Pokud váš primární zájem v shromažďování protokolů virtuálního počítače, většina dodavatelů SIEM zahrnují v jejich řešení. Pomocí systému SIEM dodavatele konektor by měla být vždy upřednostňované alternativní.

Integrace Azure protokolu událostí systému Windows shromažďuje z protokoly Prohlížeče událostí systému Windows, [protokoly aktivity Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [výstrahy Azure Security Center](../security-center/security-center-intro.md), a [Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z Prostředky Azure. Tato integrace pomáhá řešení SIEM poskytnout jednotný řídicí panel pro všechny prostředky, místně nebo v cloudu, takže můžete agregovat, korelovat, analyzovat a výstrahy pro události zabezpečení.

>[!NOTE]
V tomto okamžiku jsou pouze podporované cloudy komerční Azure a Azure Government. Ostatních cloudů nejsou podporovány.

![Integrace protokolů Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jaké protokoly můžete integrovat?

Azure vytvoří rozsáhlé protokolování pro všechny služby Azure. Tyto protokoly představují tři typy protokolů:

* **Řízení nebo protokoly** poskytují přehled o jednotlivých [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operace vytvoření, aktualizace a odstranění. Azure protokoly aktivity je příklad tohoto typu protokolu.
* **Data roviny protokoly** poskytují přehled o události vyvolané jako součást využití prostředek služby Azure. Prohlížeč událostí systému Windows je například tento typ protokolu **systému**, **zabezpečení**, a **aplikace** kanálů v virtuálního počítače s Windows. Dalším příkladem je protokolování diagnostiky nakonfigurovaný pomocí Azure monitorování
* **Zpracování události** zadejte analyzovaných událostí a informace o výstrahách zpracovat vaším jménem. Příkladem tohoto typu události je Azure Security Center výstrahy, pokud je zpracovat a analyzovat vaše předplatné poskytují výstrahy, které jsou relevantní pro aktuální lepšímu zabezpečení Azure Security Center.

Integrace se službou Azure protokolu podporuje ArcSight, QRadar a Splunk. Za všech okolností poraďte s dodavatelem SIEM k vyhodnocení, zda mají nativní konektor. Integrace se službou Azure protokolu byste neměli používat, když nativní konektory jsou k dispozici.

Pokud nejsou k dispozici žádné další možnosti, lze považovat protokolu integrace se službou Azure. Následující tabulka obsahuje Naše doporučení.

|**SIEM** | **Zákazník již pomocí protokolu integrátor** | **Zákazník příčin možností integrace systému SIEM**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Začít s migrací do [rozšíření Azure monitorování pro Splunk](https://splunkbase.splunk.com/app/3534/) | Použití [SPLUNK konektoru](https://splunkbase.splunk.com/app/3534/) |
|**IBM QRADAR** | Migrace na nebo začít používat konektor QRadar popsané na konci http://aka.ms/azmoneventhub | Použití konektoru QRadar popsané na konci http://aka.ms/azmoneventhub  |
|**ARCSIGHT** | Pokračovat v používání protokolu integrátor, dokud nebude k dispozici konektor a potom migrovat do řešení na základě konektor.  | Jako alternativu zvažte analýzy protokolů Azure. Proveďte není připojit k protokolu integrace se službou Azure Pokud chcete absolvovat proces migrace, až konektor k dispozici. |

>[!NOTE]
>Sice se integrace protokolu Azure bezplatné řešení, existují náklady na úložiště Azure vyplývající z úložiště informace souboru protokolu.

Pokud potřebujete pomoc, můžete otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md). Chcete-li to provést, vyberte **integrace protokolu** jako službu, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další postup

V tomto dokumentu jste se seznámili s integrace protokolů Azure. Další informace o integraci Azure protokolu a typy protokolů, které jsou podporovány, naleznete v následujících tématech:

* [Začínáme s Azure protokolu integrace](security-azure-log-integration-get-started.md) – tento kurz vás provede procesem instalace integrace protokolů Azure a integrace protokoly z úložiště Azure WAD, protokoly aktivity Azure, Azure Security Center výstrahy a Azure Active Directory protokoly auditu.
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Monitorování data do centra událostí pro používání pomocí externího nástroje Azure datového proudu](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
