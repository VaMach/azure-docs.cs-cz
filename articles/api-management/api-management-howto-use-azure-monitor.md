---
title: "Monitorování správy rozhraní API s Azure monitorování | Microsoft Docs"
description: "Naučte se monitorovat pomocí monitorování Azure služby Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 717e033aa4bbd4dd8ebcc727c3f551aee81770dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Monitorování API Management s Azure monitorování
Azure monitorování je služba Azure, která poskytuje jednoho zdroje pro monitorování všech prostředků Azure. S Azure monitorování můžete vizualizovat, dotaz, směrovat, archivaci a provádět akce na metriky a protokoly pocházejících z prostředků Azure, jako je například Správa rozhraní API. 

Následující video ukazuje, jak monitorovat pomocí monitorování Azure API Management. Další informace o monitorování Azure najdete v tématu [Začínáme s Azure monitorování]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Metriky
API Management aktuálně vysílá pět metriky a plánujeme přidat další v budoucnu. Tyto metriky jsou vygenerované každou minutu, která poskytuje téměř v reálném čase přehled o stavu a stavu vašich rozhraní API. Následuje souhrn metriky:
* Celkový počet požadavků brány: počet rozhraní API požadavků v období. 
* Úspěšné požadavky brány: počet žádostí o rozhraní API, které přijaly kódy úspěšné odpovědi HTTP včetně 304, 307 a nic menší než 301 (například 200). 
* Neúspěšné požadavky brány: počet žádostí o rozhraní API, které obdržel chybné kódy odpovědi HTTP včetně 400 a nic větší než 500.
* Neoprávněné žádosti brány: počet žádostí o rozhraní API, které přijaly, včetně 401, 403 a 429 kódy odpovědi HTTP. 
* Další požadavky na brány: počet žádostí o rozhraní API, které přijaly kódy odpovědí protokolu HTTP, které nepatří do žádné z výše uvedených skupin (například 418).

Můžete přistupovat metriky ve službě API Management, nebo přístup metrik Azure prostředky v Azure monitorování. Chcete-li zobrazit metriky ve službě API Management:
1. Otevřete portál Azure.
2. Přejděte do služby API Management.
3. Klikněte na tlačítko **metriky**.

![Okno metriky][metrics-blade]

Další informace o tom, jak používat metriky najdete v tématu [přehled metriky].

## <a name="activity-logs"></a>Protokoly aktivit
Protokoly aktivity získat přehled o činnosti, které byly provedeny v vaše služby API Management. Se dřív označovala jako "protokoly auditu" nebo "operační protokoly". Pomocí protokolů z aktivity, můžete určit ", kdo a kdy" pro všechny operace (PUT, POST, DELETE) na vaše rozhraní API správy služby zápisu. 

> [!NOTE]
> Protokoly aktivity nezahrnují operace čtení (GET) nebo operace provádět na klasickém portálu vydavatele nebo pomocí původní rozhraní API pro správu.

Lze zobrazit protokoly aktivit ve službě API Management nebo přístup k protokolům všechny prostředky Azure v Azure monitorování. Pokud chcete zobrazit aktivity záznamy ve službě API Management:
1. Otevřete portál Azure.
2. Přejděte do služby API Management.
3. Klikněte na tlačítko **protokol aktivit**.

![Okno protokoly aktivit][activity-logs-blade]

Další informace o tom, jak používat metriky najdete v tématu [protokoly aktivity přehled].

## <a name="alerts"></a>Výstrahy
Můžete nakonfigurovat přijímat výstrahy založené na protokolech metriky a aktivity. Azure monitorování umožňuje nakonfigurovat výstrahu při aktivaci, proveďte následující:

* Odeslat oznámení e-mailu
* Volat webhook, jehož
* Vyvolání aplikace logiky Azure

Pravidla výstrah můžete nakonfigurovat ve službě API Management, nebo v Azure monitorování. Konfigurace je ve službě API Management: 
1. Otevřete portál Azure.
2. Přejděte do služby API Management.
3. Klikněte na tlačítko **výstrah pravidla**.

![Okno pravidla výstrah][alert-rules-blade]

Další informace o používání výstrah najdete v tématu [přehled výstrah].

## <a name="diagnostic-logs"></a>Diagnostické protokoly
Diagnostické protokoly poskytují bohaté informace o operacích a chyby, které jsou důležité pro auditování i pro účely odstraňování potíží. Diagnostické protokoly se liší od protokoly aktivity. Protokoly aktivity poskytují přehled o činnosti, které byly provedeny v vašich prostředků Azure. Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.

API Management aktuálně poskytuje diagnostické protokoly (každou hodinu dávkově) o jednotlivých API žádosti s každou položku s následující strukturou:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

Můžete přístup k diagnostickým protokolům ve službě API Management, nebo přístup k protokolům všechny prostředky Azure v Azure monitorování. Chcete-li zobrazit diagnostické protokoly ve službě API Management:
1. Otevřete portál Azure.
2. Přejděte do služby API Management.
3. Klikněte na tlačítko **protokolů diagnostiky**.

![Okno Diagnostické protokoly][diagnostic-logs-blade]

Další informace o tom, jak používat metriky najdete v tématu [přehled diagnostické protokoly].

## <a name="next-step"></a>Další krok

* [Začínáme s Azure monitorování]
* [přehled metriky]
* [protokoly aktivity přehled]
* [přehled diagnostické protokoly]
* [přehled výstrah]

[Začínáme s Azure monitorování]: ../monitoring-and-diagnostics/monitoring-get-started.md
[přehled metriky]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[protokoly aktivity přehled]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[přehled diagnostické protokoly]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[přehled výstrah]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
