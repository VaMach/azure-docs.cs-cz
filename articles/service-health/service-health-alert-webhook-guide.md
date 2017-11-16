---
title: "Konfigurace oznámení o stavu pro existující systémy správy problém pomocí webhook, jehož | Microsoft Docs"
description: "Získáte přizpůsobený oznámení o události stavu služby pro váš stávající systém správy problém."
author: shawntabrizi
manager: scotthit
editor: 
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: b6a5f61f61675b825dcfe9c706c80944f5890538
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Konfigurace oznámení o stavu pro existující systémy správy problém pomocí webhook, jehož

Tento článek ukazuje, jak nakonfigurovat stav služby upozornění k odesílání dat prostřednictvím Webhooky do existující systém oznámení.

Výstrahy stavu služby v současné době můžete nakonfigurovat tak, aby při incidentu služby Azure setkáte, zobrazí se upozornění prostřednictvím textové zprávy nebo e-mailu.
Však může už máte existující externí oznámení systému na místo, které chcete použít.
Tento dokument ukazuje nejdůležitější části datové části webhooku a jak můžete vytvořit vlastní upozornění, pokud problémů služby můžete ovlivnit chcete dostat upozornění.

Pokud chcete použít předkonfigurované integrace, najdete v části Postup:
* [Konfigurace výstrah s ServiceNow](service-health-alert-webhook-servicenow.md)
* [Konfigurace výstrah s PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Konfigurace výstrah s OpsGenie](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Konfigurace vlastní oznámení pomocí datové webhook služby stavu
Pokud chcete nastavit vlastní vlastní webhooku integraci, budete muset analyzovat datovou část JSON, který je odeslán během oznámení o stavu služby.

Podívejte se [sem zobrazíte příklad](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md) co `Service Health` vypadá webhooku datové části.

Toto je upozornění na stav služby podle, můžete zjistit `context.eventSource == "ServiceHealth"`. Odtud jsou vlastnosti, které nejvíce odpovídají ingestování:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-azure-service-health-for-an-incident"></a>Vytváření přímý odkaz na stav služby Azure pro incident
Přímý odkaz můžete vytvořit vaše přizpůsobené incidentu stavu služby Azure na ploše nebo mobilní vygenerováním specializované adresy URL. Použití `trackingId`, a také první a poslední tři znaky z vaší `subscriptionId`, k vytvoření:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Například pokud vaše `subscriptionId` je `bba14129-e895-429b-8809-278e836ecdb3` a `trackingId` je `0DET-URB`, pak je přizpůsobené adresu URL služby stavu Azure:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Použití úrovně ke zjištění závažnosti problému
Od nejnižší závažnost na nejvyšší závažnost `level` vlastnost v datové části může být libovolná z `Informational`, `Warning`, `Error`, a `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Analýza ovlivněné služby pochopit plný rozsah incidentu
Výstrahy stavu služeb může informovat o problémech napříč více oblastí a službami. Chcete-li získat všechny podrobnosti, je potřeba analyzovat hodnotu `impactedServices`.
Je obsah uvnitř [JSON uvozené](http://json.org/) řetězce, když znaků bez řídících, obsahuje jiný objekt JSON, který lze analyzovat pravidelně.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Změní na:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Ukazuje to, že jsou problémy s "Výstrahy a metrik" v Austrálie – východ a a Tichomoří – jihovýchod, jakož i problémy službou"aplikace" v Austrálie – jihovýchod.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testování svoji integraci webhooku prostřednictvím požadavku HTTP POST
1. Vytvoření datové části stav služby, které chcete odeslat. Můžete najít datovou část příklad stav služby webhooku na [Webhooky Azure aktivity protokolu výstrahy](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Vytvořte požadavek HTTP POST takto:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. Měli byste obdržet `2XX - Successful` odpovědi.

4. Přejděte na [PagerDuty](https://www.pagerduty.com/) potvrďte, že byl úspěšně nastaven svoji integraci.

## <a name="next-steps"></a>Další kroky
- Zkontrolujte [schéma výstrahy webhooku protokolu činnosti](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Další informace o [oznámení o stavu služby](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Další informace o [skupiny akcí](../monitoring-and-diagnostics/monitoring-action-groups.md).