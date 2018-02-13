---
title: "Akce Webhooku protokolu výstrahy ve výstrahách Azure (Preview) | Microsoft Docs"
description: "Tento článek popisuje, jak pravidlo výstrahy protokolu pomocí protokolu statistice analýzy nebo aplikaci, bude nabízet data jako webhooku protokolu HTTP a další podrobnosti o různých přizpůsobení možné."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 49905638-f9f2-427b-8489-a0bcc7d8b9fe
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: vinagara
ms.openlocfilehash: 4af1bb61888810011ce64fde7931cabfefe76ab6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Akce Webhooku pro pravidla výstrah protokolu
Když [v Azure (Preview) se vytvoří výstraha](monitor-alerts-unified-usage.md), máte možnost [konfigurace pomocí akce skupiny](monitoring-action-groups.md) provést několik akcí.  Tento článek popisuje různé webhooku akce, které jsou k dispozici a podrobnosti o konfiguraci vlastních webhooku založenými na JSON.


## <a name="webhook-actions"></a>Akce Webhooku

Akce Webhooku umožňují vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST.  Služba volané by měla podporovat webhooky a určit, jak se bude používat žádné datové části obdrží.  Může také zavolat REST API, která nepodporuje konkrétně webhooků, pokud je požadavek ve formátu, který funguje s technologií rozhraní API.  Příklady použití webhook, jehož v reakci na výstrahy jsou odesílání zprávy v [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).  Kompletní a podrobný postup vytvoření pravidla výstrahy s webhooku pro volání Slack je k dispozici na [Webhooky ve výstrahách analýzy protokolů](../log-analytics/log-analytics-alerts-webhooks.md).

Akce Webhooku vyžadují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Adresa URL Webhooku |Adresa URL webhooku. |
| Vlastní datovou část JSON |Vlastní datovou část odeslat spolu s webhooku.  Pokud zvolíte možnost během vytváření výstrahy. Podrobnosti jsou dostupné v [Správa výstrah pomocí výstrah Azure (Preview)](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Tlačítko Webhooku souběžně s test *zahrnout vlastní datovou část JSON webhooku* možnost protokolu výstrah, fiktivní volání otestovat adresu URL webhooku se aktivuje. Neobsahuje se skutečná data nebo zástupce schématu JSON se používá pro výstrahy, protokolu. Při jakékoli jeden webhook se vlastní JSON zástupce můžete otestovat, odešle se všechny webhooků, které jsou nakonfigurované ve skupině Akce s vlastní datovou část JSON.

Webhooky zahrnují adresu URL a datovou část ve formátu JSON, který se data odesílají externí služby.  Ve výchozím nastavení budou datové části zahrnují hodnoty v následující tabulce.  Můžete nahradit vlastní jeden vlastní tato datová část.  V takovém případě můžete proměnné v tabulce pro jednotlivé parametry mají být zahrnuty jejich hodnota vaše vlastní datovou část.


| Parametr | Proměnná | Popis |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Název pravidla výstrahy. |
| AlertThresholdOperator |#thresholdoperator |Operátor prahová hodnota pro pravidlo výstrahy.  *Větší než* nebo *menší než*. |
| AlertThresholdValue |#thresholdvalue |Prahová hodnota pro pravidlo výstrahy. |
| LinkToSearchResults |#linktosearchresults |Propojit vyhledávání protokolu analýzy protokolů, který vrátí záznamy v dotazu, který vytvořili výstrahu. |
| Element resultcount element |#searchresultcount |Počet záznamů ve výsledcích hledání. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Koncový čas pro dotaz ve formátu UTC. |
| SearchIntervalInSeconds |#searchinterval |Časový interval pro pravidlo výstrahy. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Počáteční čas pro dotaz ve formátu UTC. |
| SearchQuery |#searchquery |Vyhledávací dotaz protokolu používá pravidlo výstrahy. |
| SearchResults |"IncludeSearchResults":true|Záznamů vrácených dotazem jako tabulku JSON, omezen na prvních 1000 záznamů nebo řádky; Pokud "IncludeSearchResults": true je přidaný do vlastní definici JSON webhooku jako vlastnost nejvyšší úrovně |
| WorkspaceID |#workspaceid |ID pracovního prostoru analýzy protokolů. |
| Závažnost |#severity |Závažnost nastavit výstrahy přímým protokolu. |

Například může určit následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.  Služby, který volá tento webhook by byla očekávána tento parametr.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Tato datová část příkladu by odkazující na něco jako následující odeslání do webhooku.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Zahrnout vlastní datovou část výsledky hledání, ujistěte se, že **IncudeSearchResults** je nastaven jako vlastnost nejvyšší úrovně v datové části json.


Si můžete projít kompletní příklad vytvoření pravidla výstrahy s webhooku zahájíte externí služba v [vytvoří akci, výstrah webhooku v analýzy protokolů k odeslání zprávy na Slack](../log-analytics/log-analytics-alerts-webhooks.md).

## <a name="sample-payload"></a>Datová část ukázky
Tato část uvádí datová část ukázky webhooku protokolu výstrahy, včetně při datová část je standardní a při jeho vlastní.

> [!NOTE]
> Pro zajištění zpětné kompatibility, je stejné jako datové části standardní webhooku pro výstrahy pomocí Azure Log Analytics [OMS výstrahy správy](../log-analytics/log-analytics-solution-alert-management.md). Ale pro výstrahy protokolu pomocí [Application Insights](../application-insights/app-insights-analytics.md), datové části standardní webhooku je založena na schéma akce skupiny

### <a name="standard-webhook-for-log-alerts"></a>Standardní Webhooku protokolu výstrahy
Obě tyto příklady, že jsme uvedli, fiktivní datové části s pouze dva sloupce a dva řádky.

#### <a name="log-alert-for-azure-log-analytics"></a>Výstraha protokolu pro Azure analýzy protokolů
Následuje ukázka datové části pro standardní webhooku akci bez vlastní Json, pokud se používá pro protokol, na základě analýzy protokolů výstrahy.

    {
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }



#### <a name="log-alert-for-azure-application-insights"></a>Výstraha protokolu pro službu Azure Application Insights
Následuje ukázka datové části pro standardní webhooku akci bez vlastní Json, pokud se používá pro aplikace založené na Statistika protokolu – výstrahy.


    {
    "schemaId":"LogAlert","data":
    {
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }
    }


#### <a name="log-alert-with-custom-json-payload"></a>Výstraha protokolu s vlastní datovou část JSON
Například pokud chcete vytvořit vlastní datovou část, která obsahuje jenom název výstrahy a výsledky hledání, můžete použít následující.

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }

Následuje ukázka datové části pro akci vlastní webhooku pro všechny výstrahy protokolu.


    {
    "AlertRuleName":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }




## <a name="next-steps"></a>Další postup
- Vytvoření a správa [akce skupin v Azure](monitoring-action-groups.md)
- Další informace o [protokolu výstrahy ve výstrahách Azure (preview)](monitor-alerts-unified-log.md)
