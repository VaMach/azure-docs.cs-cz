---
title: "Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak do protokolu událostí Azure Event Hubs ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: a310236179677046ec49930b07cfdffdadc37974
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management
Vysoce škálovatelná služba Azure Event Hubs slouží ke zpracování příchozích dat. Dokáže přijímat miliony událostí za sekundu a umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených zařízeními a aplikacemi připojenými k vaší síti. Služba Event Hubs slouží jako "přední dveře" pro kanál událostí, a jakmile jsou data shromážděna do centra událostí, lze je transformovat a uložené pomocí kteréhokoli poskytovatele služeb, analýzu v reálném čase nebo adaptérů pro dávkování či ukládání. Event Hubs oddělí vytvoření proudu událostí od spotřeby těchto události, aby spotřebitelé událostí mohli k událostem přistupovat podle svého vlastního plánu.

Tento článek je Pomocníka pro [integrovat Azure API Management službou Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video a popisuje, jak do protokolu událostí správy rozhraní API pomocí Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Vytvoření centra událostí Azure
K vytvoření nového centra událostí, přihlaste se do [portál Azure classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Service Bus**  -> **Centra událostí**->**rychle vytvořit**. Zadejte název centra událostí, oblast, vyberte předplatné a vyberte obor názvů. Pokud jste dosud nevytvořili oboru názvů můžete vytvořit jeden zadáním názvu do **Namespace** textové pole. Po nakonfigurování všech vlastností klikněte na tlačítko **vytvoření nového centra událostí** k vytvoření centra událostí.

![Vytvoření centra událostí][create-event-hub]

Potom přejděte na **konfigurace** kartě nového centra událostí a vytvořte dvě **sdílené zásady přístupu**. Název první **odesílající** a pojmenujte ho **odeslat** oprávnění.

![Odesílání zásad][sending-policy]

Název na druhou **přijetí**, poskytněte **naslouchání** oprávnění a klikněte na tlačítko **Uložit**.

![Přijetí zásad][receiving-policy]

Každá zásada sdíleného přístupu umožňuje aplikacím odesílání a příjmu událostí do a z centra událostí. Chcete-li získat přístup k připojovací řetězce pro tyto zásady, přejděte na **řídicí panel** centra událostí a klikněte na kartu **informace o připojení**.

![Připojovací řetězec][event-hub-dashboard]

**Odesílající** připojovací řetězec se používá při protokolování událostí a **přijetí** připojovací řetězec se používá při stahování události z centra událostí.

![Připojovací řetězec][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Vytvoření protokolovač API Management
Teď, když máte centra událostí, dalším krokem je konfigurace [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) ve službě API Management služby tak, aby mohl zaprotokolovat události do centra událostí.

Protokolovací nástroje API Management jsou konfigurováni pomocí [rozhraní API REST API správy](http://aka.ms/smapi). Před použitím rozhraní REST API poprvé, přečtěte si [požadavky](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) a ujistěte se, že máte [povolen přístup k rozhraní REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Pokud chcete vytvořit protokoly, ujistěte se, požadavek HTTP PUT pomocí následující šablony adresy URL.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Nahraďte `{your service}` s názvem vaší instance služby API Management.
* Nahraďte `{new logger name}` s požadovaným názvem pro vaše nové protokolovacího nástroje. Tento název bude odkazovat, když konfigurujete [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zásad

Přidáte následující hlavičky žádosti.

* Content-Type: application/json
* Autorizace: SharedAccessSignature 58...
  * Pokyny pro generování `SharedAccessSignature` najdete v části [Azure API Management REST API ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Zadejte text žádosti pomocí následující šablony.

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `type`musí být nastavena na `AzureEventHub`.
* `description`poskytuje volitelný popis protokolovacího nástroje a v případě potřeby může být řetězec nulové délky.
* `credentials`obsahuje `name` a `connectionString` centra událostí Azure.

Pokud vytvoříte požadavek, je-li protokolovač vytvořena stavový kód `201 Created` je vrácen.

> [!NOTE]
> Ostatní možné návratové kódy a jejich důvodů najdete v tématu [vytvořit protokoly](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Zobrazíte jak provádět další operace, jako je například seznam, aktualizace a odstranění, najdete [protokolovacího nástroje](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentace entity.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurace zásad protokolu eventhubs
Jakmile vaše protokoly nakonfigurovaný ve službě API Management, můžete nakonfigurovat zásad protokolu eventhubs k požadované událostem protokolu. Zásady protokolu eventhubs lze použít v části zásady příchozí nebo odchozí zásad.

Ke konfiguraci zásad, přihlaste se do [portál Azure](https://portal.azure.com), přejděte do služby API Management a klikněte na tlačítko **portál vydavatele** pro přístup k portálu vydavatele.

![Portál vydavatele][publisher-portal]

Klikněte na tlačítko **zásady** v nabídce API Management na levé straně vyberte požadovaný produkt a rozhraní API a klikněte na tlačítko **přidat zásadu**. V tomto příkladu je právě přidávána zásadu, která **Echo API** v **neomezený** produktu.

![Přidání zásad][add-policy]

Umístěte kurzor v `inbound` zásad a klikněte na tlačítko **protokolu k centru EventHub** zásady a vložit `log-to-eventhub` příkaz šablony zásad.

![Editor zásad][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Nahraďte `logger-id` s názvem protokolovacího nástroje správy rozhraní API, které jste nakonfigurovali v předchozím kroku.

Můžete použít libovolný výraz, který vrací řetězec jako hodnotu `log-to-eventhub` elementu. V tomto příkladu se zaznamená řetězec obsahující datum a čas, název služby, id požadavku, požadavek ip adresu a název operace.

Klikněte na tlačítko **Uložit** aktualizované zásady konfiguraci uložíte. Jakmile je uložen zásady nejsou aktivní a do určené centra událostí jsou zaznamenány události.

## <a name="next-steps"></a>Další kroky
* Další informace o Azure Event Hubs
  * [Začínáme s Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Přijímat zprávy pomocí třídy EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Průvodce programováním pro službu Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Další informace o integraci API Management a služby Event Hubs
  * [Odkaz na entitu protokolovacího nástroje](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [referenční informace o protokolu eventhub zásad](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Podívejte se na video s návodem
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
