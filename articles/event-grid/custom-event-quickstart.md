---
title: "Vlastní události pro Azure Event Grid | Dokumentace Microsoftu"
description: "Pomocí služby Azure Event Grid můžete publikovat téma a přihlásit se k odběru příslušné události."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 0290836bebadb20085a3ce84dddc088c3af385da
ms.contentlocale: cs-cz
ms.lasthandoff: 08/28/2017

---

# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Vytvoření a směrování vlastních událostí pomocí služby Azure Event Grid

Azure Event Grid je služba zpracování událostí pro cloud. V tomto článku pomocí Azure CLI vytvoříte vlastní téma, přihlásíte se k jeho odběru a aktivujete událost, abyste viděli výsledek. Obvykle odesíláte události do koncového bodu, který na událost reaguje například webhookem nebo funkcí Azure Functions. Pro zjednodušení tohoto článku však budete události odesílat na adresu URL, která jenom shromažďuje zprávy. Tuto adresu URL vytvoříte pomocí open source nástroje třetí strany [RequestBin](https://requestb.in/).

>[!NOTE]
>**RequestBin** je open source nástroj, který není určený pro použití vyžadující vysokou propustnost. Zde uvedené použití tohoto nástroje je čistě demonstrativní. Pokud najednou nabídnete více než jednu událost, možná se v nástroji nezobrazí všechny.

Až budete hotovi, uvidíte, že se data událostí odeslala do koncového bodu.

![Data událostí](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, pro účely tohoto článku musíte používat nejnovější verzi Azure CLI (2.0.14 nebo novější). Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Témata služby Event Grid jsou prostředky Azure a musí být umístěné ve skupině prostředků Azure. Skupina prostředků je logická kolekce, ve které se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). 

Následující příklad vytvoří skupinu prostředků *gridResourceGroup* v umístění *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Vytvoření vlastního tématu

Téma poskytuje uživatelsky definovaný koncový bod, do kterého odesíláte události. Následující příklad vytvoří téma ve vaší skupině prostředků. Nahraďte `<topic_name>` jedinečným názvem vašeho tématu. Název tématu musí být jedinečný, protože je reprezentován položkou DNS. Ve verzi Preview podporuje služba Event Grid umístění **westus2** a **westcentralus**.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Vytvoření koncového bodu zpráv

Před přihlášením k odběru tématu vytvoříme koncový bod pro zprávy události. Místo psaní kódu, který by na událost reagoval, vytvoříme koncový bod, který bude shromažďovat zprávy, abyste je mohli zobrazit. RequestBin je open source nástroj třetí strany, který umožňuje vytvořit koncový bod a zobrazit požadavky, které se do něj odesílají. Přejděte na web [RequestBin](https://requestb.in/) a klikněte na **Create a RequestBin** (Vytvořit přihrádku žádostí).  Zkopírujte adresu URL přihrádky, protože ji budete potřebovat při přihlašování k odběru tématu.

## <a name="subscribe-to-a-topic"></a>Přihlášení k odběru tématu

K odběru tématu se přihlašujete, aby služba Event Grid věděla, které události chcete sledovat. Následující příklad se přihlásí k odběru tématu, které jste vytvořili, a předá adresu URL z nástroje RequestBin jako koncový bod pro oznámení události. Nahraďte `<event_subscription_name>` jedinečným názvem vašeho odběru a `<URL_from_RequestBin>` hodnotou adresy URL z nástroje RequestBin z předchozí části. Díky zadání koncového bodu při přihlašování k odběru bude služba Event Grid zpracovávat směrování událostí do tohoto koncového bodu. Místo `<topic_name>` použijte hodnotu názvu tématu, který jste vytvořili dříve. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Odeslání události do tématu

Nyní aktivujeme událost, abychom viděli, jak služba Event Grid distribuuje zprávu do vašeho koncového bodu. Nejprve získáme adresu URL a klíč tématu. Znovu místo `<topic_name>` použijte název vašeho tématu.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Pro zjednodušení tohoto článku jsme pro odeslání do tématu nastavili ukázková data události. Obvykle by aplikace nebo služba Azure odesílala data události. Následující příklad získá data události:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Pokud použijete příkaz `echo "$body"`, zobrazí se celá událost. Element JSON `data` je datová část vaší události. V tomto poli může být libovolný JSON ve správném formátu. Můžete také použít pole subject (předmět) pro pokročilé směrování a filtrování.

CURL je nástroj, který provádí požadavky HTTP. V tomto článku používáme nástroj CURL k odeslání události do tématu. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Právě jste aktivovali událost a služba Event Grid odeslala zprávu do koncového bodu, který jste nakonfigurovali při přihlášení k odběru. Přejděte na adresu URL nástroje RequestBin, kterou jste vytvořili dříve. Nebo v prohlížeči klikněte na tlačítko pro obnovení otevřeného okna s webem RequestBin. Zobrazí se událost, kterou jste právě odeslali. 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud chcete pokračovat v práci s touto událostí, nevyčišťujte prostředky vytvořené v rámci tohoto článku. Pokud pokračovat nechcete, pomocí následujícího příkazu odstraňte prostředky, které jste v rámci tohoto článku vytvořili.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Další kroky

Když teď víte, jak vytvářet témata a odběry událostí, zjistěte, s čím vám služba Event Grid ještě může pomoct:

- [Informace o službě Event Grid](overview.md)
- [Monitorování změn virtuálního počítače pomocí služeb Azure Event Grid a Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)

