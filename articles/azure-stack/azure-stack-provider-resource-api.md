---
title: "Využití prostředků poskytovatele rozhraní API | Microsoft Docs"
description: "Odkaz pro rozhraní API, využití prostředků, který načte informace o využití Azure zásobníku"
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: b6055923-b6a6-45f0-8979-225b713150ae
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: alfredop
ms.openlocfilehash: 0c45ce3bc93945ed8700464beebabcda07e8d77c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="provider-resource-usage-api"></a>Rozhraní API využití prostředků poskytovatele
Termín *zprostředkovatele* platí pro správce služeb a zprostředkovatelů delegovaní. Operátory Azure zásobníku a delegované zprostředkovatelé můžete použít rozhraní API pro využití zprostředkovatele a zobrazte tak použití jejich přímé klientů. Například jak je vidět v diagramu, P0 můžete volat zprostředkovatele rozhraní API se získat informace o využití na na P1 a P2 společnosti přímý využití a P1 můžete volat pro informace o využití na P3 a P4.

![Konceptuální model hierarchie zprostředkovatele](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>Referenční dokumentace rozhraní API volání
### <a name="request"></a>Žádost
Požadavek získá spotřeba podrobnosti pro požadované předplatné a pro požadovaný časový rámec. Neexistuje žádný text žádosti.

Toto použití rozhraní API je poskytovatel rozhraní API, takže volající musí být přiřazena roli vlastník, Přispěvatel nebo Čtenář předplatného poskytovatele.

| **– Metoda** | **Identifikátor URI požadavku** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}& subscriberId = {sub1.1} & verze api-version = 2015-06-01-preview & continuationToken = {{hodnota tokenu} |

### <a name="arguments"></a>Argumenty
| **Argument** | **Popis** |
| --- | --- |
| *armendpoint* |Azure Resource Manager koncový bod vašeho prostředí Azure zásobníku. Konvence zásobník Azure je, že název koncového bodu Azure Resource Manager je ve formátu `https://adminmanagement.{domain-name}`. Například pro development kit, pokud je název domény *local.azurestack.external*, pak je koncový bod správce prostředků `https://adminmanagement.local.azurestack.external`. |
| *subId* |ID předplatného uživatele, který provádí volání. |
| *reportedStartTime* |Spuštění dotazu. Hodnota *data a času* by měl být koordinovaný světový čas (UTC) a na začátku hodinu, například 13:00. Pro denní agregace nastavte tuto hodnotu půlnoci času UTC. Formát je *uvozený* ISO 8601. Například *2015-06-16T18 % 3a53 % 3a11 % 2b00 % 3a00Z*, kde je uvozena dvojtečkou k *% 3a* a plus je uvozena k *% 2b* tak, aby se identifikátor URI popisný. |
| *reportedEndTime* |Koncový čas dotazu. Omezení, která se týkají *reportedStartTime* platí také pro tento argument. Hodnota *reportedEndTime* nemůže být v budoucnu nebo aktuální datum. Pokud se jedná, výsledek je nastaven "zpracování není úplný." |
| *aggregationGranularity* |Volitelný parametr, který má dvě hodnoty diskrétní potenciální: dnů a hodin. Jako hodnoty naznačují, jeden vrátí data ve dnech, a druhá je hodinové řešení. Denní možnost je výchozí. |
| *subscriberId* |ID předplatného. Chcete-li získat filtrovaných dat, je požadováno ID předplatného přímé klienta zprostředkovatele. Pokud není zadán žádný parametr ID předplatného, volání vrátí data o využití pro všechny poskytovatele přímé klienty. |
| *verze rozhraní API.* |Verze protokolu, který slouží k vytvoření této žádosti. Tato hodnota nastavena na *2015-06-01-preview*. |
| *continuationToken* |Token načíst z posledního volání poskytovatele rozhraní API pro využití. Tento token je potřeba, když je větší než 1 000 řádků odpověď a funguje jako záložku v průběhu. Pokud token není k dispozici, od začátku den jsou načtena data nebo předaná hodinu, podle členitost. |

### <a name="response"></a>Odpověď
ZÍSKAT /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00 % 3a00 % 3a00 % 2b00 % 3a00 & aggregationGranularity = denně & subscriberId = sub1.1 & verze api-version = 1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Podrobnosti o odpovědi
| **Argument** | **Popis** |
| --- | --- |
| *ID* |Jedinečné ID využití agregace. |
| *Jméno* |Název využití agregace. |
| *Typ* |Definice prostředků. |
| *ID předplatného* |Identifikátor předplatného Azure zásobník uživatele. |
| *usageStartTime* |UTC počáteční čas využití sady, do které patří tato využití agregace.|
| *usageEndTime* |Koncový čas UTC využití sady, do které patří tato využití agregace. |
| *instanceData* |Páry klíč hodnota podrobností instance (v novém formátu):<br> *resourceUri*: plně kvalifikovaný ID prostředku, který zahrnuje skupiny prostředků a název instance. <br> *umístění*: oblast, ve které byla tato služba spuštěna. <br> *značky*: značky prostředku, které jsou určené uživatele. <br> *additionalInfo*: Další podrobnosti o prostředek, který byl zpracován, například typ verzí nebo bitovou kopii operačního systému. |
| *množství* |Množství spotřeby prostředků, které došlo v této časového rámce. |
| *meterId* |Jedinečné ID pro prostředek, která se spotřebovala (také nazývané *ResourceID*). |

## <a name="next-steps"></a>Další kroky
[Využití prostředků klienta referenční dokumentace rozhraní API](azure-stack-tenant-resource-usage-api.md)

[Nejčastější dotazy souvisí s využitím](azure-stack-usage-related-faq.md)
