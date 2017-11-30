---
title: "Co jsou oznámení o stavu služby | Microsoft Docs"
description: "Oznámení o stavu služby umožňují zobrazit stav služby, které publikování zpráv Microsoft Azure."
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
ms.openlocfilehash: efdd42d244710b27fc33154b708cfbe40312e3b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="service-health-notifications"></a>Oznámení o stavu služby
## <a name="overview"></a>Přehled

Tento článek ukazuje, jak zobrazit oznámení o stavu služby pomocí portálu Azure.

Oznámení o stavu služby umožňují zobrazit zprávy o stavu služby, které zveřejnil tým Azure, které mohou ovlivňovat prostředky v rámci svého předplatného. Tato oznámení jsou podtřídou třídy aktivity protokolu události a možné také najít v protokolu aktivit. Oznámení o stavu služby může být informační nebo níž lze provést akci v závislosti na třídě.

Existují pět třídy oznámení o stavu služby:  

- **Je vyžadována akce:** z času na čas Azure může dojít k nějakou neobvyklou akci na vašem účtu. Azure pravděpodobně nutné spolupracovat s vámi, chcete-li to opravit. Azure vám pošleme oznámení buď s podrobnostmi o akce je nutné vzít nebo s podrobnostmi o tom, jak vám poskytne Azure inženýrství nebo podporu.  
- **Odbornou obnovení:** došlo k události a techniky potvrdili, že se stále setkáváte dopad. Azure inženýrství musí spolupracovat s vámi přímo k obnovení úplné stavu vašich služeb.  
- **Incident:** služby ovlivňující událostí aktuálně ovlivňuje jednu nebo více prostředků ve vašem předplatném.  
- **Údržba:** Toto je oznámení, která vás informuje o plánované údržbě aktivity, která může mít vliv na jeden nebo více prostředků v rámci svého předplatného.  
- **Informace o:** od času Azure zasílat oznámení, které vás informují o potenciální optimalizace, které mohou pomoci zlepšovat vaše využití prostředků.  
- **Zabezpečení:** naléhavé zabezpečení související informace týkající se vaší solution(s) spuštěné v Azure.

Každý oznámení o stavu služby obsahuje informace o oboru a vliv na vaše prostředky. Podrobnosti o patří:

Název vlastnosti | Popis
-------- | -----------
Kanály | Je jedním z následujících hodnot: "Admin", "Operace"
correlationId | Obvykle je identifikátor GUID ve formátu řetězce. Události s, který patří do stejné akce uber obvykle sdílet stejnou correlationId.
eventDataId | Je jedinečný identifikátor události
EventName | Je název události
úroveň | Úroveň události. Jeden z následujících hodnot: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"
resourceProviderName | Název zprostředkovatele prostředků pro prostředek dopad
Typ prostředku| Typ prostředku ovlivněné prostředku
Podřízený stav | Obvykle stavový kód HTTP REST odpovídající volání, ale můžou taky patřit jiných řetězců popisující podřízeného stavu, jako jsou tyto hodnoty běžné: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (kód stavu HTTP : 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504).
eventTimestamp | Časové razítko při zpracování požadavku odpovídající události služby Azure vygenerovalo událost.
submissionTimestamp |   Časové razítko, když jsou události dostupné pro zadávání dotazů.
subscriptionId | Předplatné Azure, ve kterém se tato událost byla zaznamenána
status | Řetězec popisující stav operace. Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno.
operationName | Název operace.
category | "ServiceHealth"
resourceId | ID prostředku ovlivněné prostředku.
Properties.Title | Lokalizovaný název pro tuto komunikaci. Výchozím jazykem je angličtina.
Properties.Communication | Lokalizované podrobnosti o komunikaci se službou značka jazyka HTML. Výchozí hodnota je angličtina.
Properties.incidentType | Možné hodnoty: AssistedRecovery, ActionRequired, informace, incidentů, údržby, zabezpečení
Properties.trackingId | Identifikuje incident, který je přidružen této události. Použijte ke korelaci událostí souvisejících s incidentem.
Properties.impactedServices | Uvozený blob JSON, která popisuje služby a oblasti, které jsou ovlivněny incidentem. Seznam služeb, z nichž každá má ServiceName a seznam ImpactedRegions, z nichž každá má RegionName.
Properties.defaultLanguageTitle | Komunikace v angličtině
Properties.defaultLanguageContent | Komunikace v angličtině jako značka jazyka html nebo prostý text
Properties.Stage | Možné hodnoty pro AssistedRecovery, ActionRequired, informace, incidentů, zabezpečení: jsou aktivní, vyřešeno. Za účelem údržby jsou: aktivní, plánovaná, InProgress, zrušení, Rescheduled, vyřešeno, dokončeno
Properties.communicationId | Komunikace Tato událost souvisí.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Zobrazení oznámení o stavu služby v portálu Azure
1.  V [portál](https://portal.azure.com), přejděte na **monitorování** služby

    ![Monitorování](./media/monitoring-service-notifications/home-monitor.png)
2.  Klikněte **monitorování** možnost otevře prostředí monitorování. Azure monitorování spojuje veškeré monitorování nastavení a data do jednoho konsolidované zobrazení. Nejprve se otevře část **Protokol aktivit**.

3.  Nyní klikněte na **výstrahy** části

    ![Monitorování](./media/monitoring-service-notifications/service-health-summary.png)
4. Klikněte na **+ přidat výstraha aktivity protokolu** a nakonfigurovat výstrahu zajistit upozornění pro budoucí služby oznámení. Další informace o konfiguraci výstrah na oznámení o službách [naleznete na stránce aktivity protokolu výstrahy a oznámení o službách](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Další kroky:
Přijímat [výstrahy oznámení pokaždé, když oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md) odeslání  
Další informace o [aktivity protokolu výstrahy](monitoring-activity-log-alerts.md)
