---
title: "Přehled služby Azure událostí mřížky"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/02/2017
ms.author: babanisa
ms.openlocfilehash: a0bcc9650fda10c099887b74b408551b12cde667
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="an-introduction-to-azure-event-grid"></a>Úvod do Azure událostí mřížky

Azure mřížky událostí umožňuje snadno vytvářet aplikace, které se na základě událostí architektury. Vyberete Azure prostředek, ke kterému se chcete přihlásit k odběru a poskytněte obslužné rutiny události nebo koncový bod Webhooku pro odeslání události. Událost mřížky má integrovanou podporu pro události pocházející ze služby Azure, jako jsou skupiny prostředků a úložiště objektů BLOB. Událost mřížky má také vlastní podporu pro aplikace a událostí třetích stran, pomocí vlastní témata a vlastní webhooky. 

Filtry můžete směrovat určité události do různých koncových bodů, vícesměrového vysílání pro několik koncových bodů a ujistěte se, že události se dodávají spolehlivě. Událost mřížky také obsahuje vestavěnou podporou pro třetí strany a vlastní události.

Ve verzi Preview podporuje služba Event Grid umístění **westus2** a **westcentralus**. Přidá jiných oblastí.

Tento článek obsahuje přehled Azure událostí mřížky. Pokud chcete začít pracovat s událostí mřížky, najdete v části [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).

![Funkční model událostí mřížky](./media/overview/event-grid-functional-model.png)

## <a name="concepts"></a>Koncepty

V mřížce událostí Azure, která umožňují zprovoznění existují pět koncepty:

* **Události** -co se stalo.
* **Zdroje událostí nebo vydavatelů** – kde událost byla provedena.
* **Témata** -koncový bod, kde vydavatelů odesílat události.
* **Odběry událostí** -koncový bod nebo integrované mechanismus trasy události, někdy více obslužných rutin. Odběry jsou také používány obslužné rutiny inteligentně filtrování příchozích událostí.
* **Obslužné rutiny událostí** -aplikace nebo služby reagovat na událost.

Další informace o těchto postupech najdete v tématu [koncepty v mřížce událostí Azure](concepts.md).

## <a name="capabilities"></a>Možnosti

Tady jsou některé klíčové funkce Azure událostí mřížky:

* **Jednoduchost** -bodu a klikněte na směřovat k události z vaší prostředků Azure k žádnému obslužné rutiny události nebo koncový bod.
* **Rozšířené filtrování** -filtru pro událost typu nebo událostí publikování cesta získávaly obslužné rutiny událostí pouze relevantní události.
* **FAN-out** -přihlášení k odběru několik koncových bodů ke stejné události posílat kopie události na míst podle potřeby.
* **Spolehlivost** -využívat znova 24 hodin s exponenciálního omezení rychlosti, aby se události doručují.
* **Platím za událostí** – platíte jenom pro velikost použijete událostí mřížky.
* **Vysoká propustnost** -sestavení vysoký počet úloh v mřížce událostí s podporou miliony událostí za sekundu.
* **Předdefinované události** – zprovoznění a rychlé spuštění s integrovanou událostí definovaných prostředků.
* **Vlastní události** -použít událost mřížky trasy, filtr a spolehlivě vlastních událostí doručit ve vaší aplikaci.

Porovnání událostí mřížky, Event Hubs a Service Bus najdete v tématu [volba mezi služby Azure, které doručování zpráv](compare-messaging-services.md).

## <a name="built-in-publisher-and-handler-integration"></a>Integraci vydavatele a obslužné rutiny

Azure nabízí podporu předdefinované událostí pomocí množství služeb, včetně vydavatele a obslužné rutiny.

### <a name="publishers"></a>Vydavatelé

Následující služby Azure v současné době je integrované vydavatele podpora mřížky událostí:

* Skupiny prostředků (operace správy)
* Předplatná Azure (operace správy)
* Event Hubs
* Objekt Blob úložiště
* Vlastní témata

Jinými službami Azure se přidá tohoto roku.

### <a name="handlers"></a>Obslužné rutiny

Následující služby Azure v současné době je podpora předdefinované obslužné rutiny událostí mřížky: 

* Funkce Azure
* Logic Apps
* Azure Automation
* Webhooky
* Microsoft toku

Jinými službami Azure se přidá tohoto roku.

## <a name="what-can-i-do-with-event-grid"></a>Co můžete dělat s událostí mřížky?

Azure mřížky událostí poskytuje několik možností, které významně zlepšit bez serveru, ops automatizace a integraci pracovních: 

### <a name="serverless-application-architectures"></a>Architektury aplikací bez serveru

![Aplikaci bez serveru](./media/overview/serverless_web_app.png)

Event Grid propojuje zdroje dat a obslužné rutiny událostí. Pomocí služby Event Grid můžete například pokaždé, když se do kontejneru úložiště objektů blob přidá nová fotka, okamžitě aktivovat funkci bez serveru, která spustí analýzu obrázku. 

### <a name="ops-automation"></a>Automatizace OPS

![Automatizace operací](./media/overview/Ops_automation.png)

Event Grid umožňuje urychlit automatizaci a zjednodušit vynucování zásad. Event Grid může například upozornit službu Azure Automation při vytvoření virtuálního počítače nebo aktivaci služby SQL Database. Tyto události můžou sloužit k automatické kontrole odpovídajících konfigurací služeb, vkládání metadat do provozních nástrojů, označování virtuálních počítačů nebo zakládání pracovních položek.

### <a name="application-integration"></a>Integrace aplikací

![Integrace aplikací](./media/overview/app_integration.png)

Event Grid propojuje vaši aplikaci s dalšími službami. Můžete například vytvořit vlastní téma odeslat data události vaší aplikace do mřížky události, a využít její spolehlivé doručení advanced směrování a přímá integrace s Azure. Nebo můžete pomocí služeb Event Grid a Logic Apps zpracovávat data kdekoli bez psaní kódu. 

## <a name="how-much-does-event-grid-cost"></a>Jaké události mřížky náklady?

Azure mřížky událostí používá model tvorby cen platím za událostí, takže platíte jenom se používá.

Událost mřížky stojí 0,60 za mil. operace ($0,30 verzi Preview) a jsou nejprve 100 000 operaci za měsíc zdarma. Operace jsou definovány jako příjem příchozích dat událostí, rozšířená shoda, pokus o doručení a správu volání.  Další podrobnosti naleznete na [stránce s cenami](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Další kroky

* [Události objektu Blob Storage trasy](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Reakce na události úložiště objektů blob pomocí událostí mřížky.
* [Vytvoření a přihlásit se k vlastní události](custom-event-quickstart.md)  
  Přejděte přímo na a spusťte odesílání vlastní události pro libovolný koncový bod pomocí rychlého startu Azure událostí mřížky.
* [Použití aplikace logiky jako obslužné rutiny událostí](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Kurz k sestavení aplikace pomocí Logic Apps reagování na události nabídnutých podle událostí mřížky.
* [Streamování velkých objemů dat do datového skladu](event-grid-event-hubs-integration.md)  
  Kurz, který používá Azure Functions k vysílání dat ze služby Event Hubs do SQL Data Warehouse.
* [Odkazu k REST API mřížky událostí](/rest/api/eventgrid)  
  Poskytuje další technické informace o mřížky událostí Azure a odkazem na správě přihlášení k odběru událostí, směrování a filtrování.