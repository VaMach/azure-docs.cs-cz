---
title: "Azure mřížky události doručení a zkuste to znovu"
description: "Popisuje, jak Azure událostí mřížky doručí události a jak zpracovává nedoručených zpráv."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: darosa
ms.openlocfilehash: 4eacb37d6e19b4b69d604aa84fd404479dead1ea
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Doručení zpráv událostí mřížky a zkuste to znovu 

Tento článek popisuje, jak Azure událostí mřížky zpracovává události, když není potvrdí doručení.

Událost mřížky poskytuje trvanlivý doručení. Zajišťuje každou zprávu alespoň jednou pro každé předplatné. Události se posílají okamžitě registrované webhooku každého předplatného. Webhook, jehož není potvrdil přijetí události do 60 sekund od první pokus o doručení, události mřížky opakuje doručení události.

## <a name="message-delivery-status"></a>Stav doručení zpráv

Událost mřížky používá kódy odpovědi HTTP potvrdit k přijetí události. 

### <a name="success-codes"></a>Kódy úspěch

Následující kódy odpovědi HTTP znamenat, že událost byla doručena úspěšně vaší webhooku. Událost mřížky zvažuje doručení dokončení.

- 200 OK
- 202 platných

### <a name="failure-codes"></a>Selhání kódy

Následující kódy odpovědi HTTP znamenat, že události doručení pokus se nezdařil. Mřížky událostí se pokusí znovu odeslat událost. 

- 400 – Chybný požadavek
- 401 unauthorized
- 404 – Nenalezeno
- 408 časový limit požadavku.
- 414 URI příliš dlouhý
- 500 vnitřní chybu serveru
- 503 Služba nedostupná
- Vypršel časový limit 504 brány

Další kód odpovědi nebo nedostatek odpověď znamená chybu. Událost mřížky opakuje doručení. 

## <a name="retry-intervals"></a>Opakujte intervaly

Událost mřížky používá zásady opakování exponenciálního omezení rychlosti pro odeslání události. Pokud vaše webhooku neodpovídá nebo vrací kód chyby, opakování mřížky události doručení podle plánu, následující:

1. 10 sekund.
2. 30 sekund
3. 1 minuta
4. 5 minut
5. 10 minut
6. 30 minut
7. 1 hodina

Událost mřížky přidá malé náhodné přeskupování všechny intervalech zkuste to znovu.

## <a name="retry-duration"></a>Opakujte doba trvání

Ve verzi Preview vyprší mřížky událostí Azure všechny události, které nejsou doručeny během dvou hodin.

## <a name="monitoring"></a>Monitorování

Na portálu můžete zobrazit stav události doručení.

Metriky pro odběr událostí najdete vyhledejte **odběry událostí** v dostupné služby a vyberte ho.

![Vyhledejte odběry událostí](./media/delivery-and-retry/select-event-subscriptions.png)

Filtrovat podle typu události, předplatné a umístění. Vyberte **metriky** pro předplatné, které chcete zobrazit.

![Filtr událostí odběrů](./media/delivery-and-retry/filter-events.png)

Zobrazte metriky pro událost téma a odběr.

![Zobrazit události metriky](./media/delivery-and-retry/subscription-metrics.png)

Pokud jste publikovali vlastní téma, můžete zobrazit metriky pro ni. Vyberte skupinu prostředků obsahující tématu a vyberte tématu.

![Vyberte vlastní tématu](./media/delivery-and-retry/select-custom-topic.png)

Zobrazte metriky pro vlastní události tématu.

![Zobrazit události metriky](./media/delivery-and-retry/custom-topic-metrics.png)

## <a name="next-steps"></a>Další postup

* Úvod k mřížce událostí, naleznete v části [o mřížky událostí](overview.md).
* Chcete-li rychle začít používat událostí mřížky, přečtěte si téma [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).