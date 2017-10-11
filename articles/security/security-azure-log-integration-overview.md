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
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6c3a2ac18fdb7a7a722447af720b9dee28adef08
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Úvod do integrace protokolu Microsoft Azure
Další informace o integraci Azure protokolu, jejích klíčových funkcích a jak to funguje.

## <a name="overview"></a>Přehled

Integrace protokolů Azure je volné řešení, které umožňuje integraci nezpracovaná protokoly z vašich prostředků Azure v systémech místní informace o zabezpečení a událostí správy (SIEM).

Integrace Azure protokolu událostí systému Windows shromažďuje z protokoly Prohlížeče událostí systému Windows, [protokoly aktivity Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [výstrahy Azure Security Center](../security-center/security-center-intro.md), a [Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z Prostředky Azure. Tato integrace pomáhá řešení SIEM poskytnout jednotný řídicí panel pro všechny prostředky, místně nebo v cloudu, takže můžete agregovat, korelovat, analyzovat a výstrahy pro události zabezpečení.

>[!NOTE]
V tomto okamžiku jsou pouze podporované cloudy komerční Azure a Azure Government. Ostatních cloudů nejsou podporovány.

![Integrace protokolů Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jaké protokoly můžete integrovat?
Azure vytvoří rozsáhlé protokolování pro všechny služby Azure. Tyto protokoly představují tři typy protokolů:

* **Řízení nebo protokoly** poskytují přehled o jednotlivých [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operace vytvoření, aktualizace a odstranění. Azure protokoly aktivity je příklad tohoto typu protokolu.
* **Data roviny protokoly** poskytují přehled o události vyvolané jako součást využití prostředek služby Azure. Prohlížeč událostí systému Windows je například tento typ protokolu **systému**, **zabezpečení**, a **aplikace** kanálů v virtuálního počítače s Windows. Dalším příkladem je protokolování diagnostiky nakonfigurovaný pomocí Azure monitorování
* **Zpracování události** zadejte analyzovaných událostí a informace o výstrahách zpracovat vaším jménem. Příkladem tohoto typu události je Azure Security Center výstrahy, pokud je zpracovat a analyzovat vaše předplatné poskytují výstrahy, které jsou relevantní pro aktuální lepšímu zabezpečení Azure Security Center.

Integrace se službou Azure protokolu podporuje ArcSight, QRadar a Splunk. Za všech okolností poraďte s dodavatelem SIEM k vyhodnocení, zda mají nativní konektor. V některých případech nebude budete muset použít integrace se službou Azure protokolu, když nativní konektory jsou k dispozici. Další informace o podporované typy protokolu prosím najdete v části Nejčastější dotazy.

>[!NOTE]
Sice se integrace protokolu Azure bezplatné řešení, existují náklady na úložiště Azure vyplývající z úložiště informace souboru protokolu.

Komunita pomoc je k dispozici prostřednictvím [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Na fóru poskytuje komunitou AzLog možnost podpory navzájem otázky, odpovědi, tipy a triky o tom, jak využít integrace se službou Azure protokolu maximum. Kromě toho týmem protokolu integrace se službou Azure sleduje toto fórum a pomůže vždy, když můžeme.

Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md). Chcete-li to provést, vyberte **integrace protokolu** jako službu, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste se seznámili s integrace protokolů Azure. Další informace o integraci Azure protokolu a typy protokolů, které jsou podporovány, naleznete v následujících tématech:

* [Integrace se službou protokolu Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, požadavky na systém a instalovat pokyny týkající se integrace protokolů Azure.
* [Začínáme s Azure protokolu integrace](security-azure-log-integration-get-started.md) – tento kurz vás provede procesem instalace integrace protokolů Azure a integrace protokoly z úložiště Azure WAD, protokoly aktivity Azure, Azure Security Center výstrahy a Azure Active Directory protokoly auditu.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar. Tomto blogu představuje naše aktuální pozici na konfiguraci partnerských řešení. Ve všech případech naleznete v dokumentaci partnerských řešení nejdřív.
* [Aktivita a ASC výstrahy nad syslog na QRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – tento příspěvek blogu obsahuje pokyny pro odeslání aktivity a Azure Security Center výstrahy syslog na QRadar
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Integrace Security Center výstrahy s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tento dokument ukazuje, jak synchronizovat výstrahy Azure Security Center pomocí protokolu integrace se službou Azure.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
