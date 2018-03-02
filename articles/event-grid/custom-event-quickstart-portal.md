---
title: "Vlastní události pro Azure Event Grid pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Pomocí Azure Event Gridu a PowerShellu můžete publikovat téma a přihlásit se k odběru příslušné události."
services: event-grid
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 01/30/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: f37d496d43bb24c51d6e1c11b77d9ceba48b7b23
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-and-route-custom-events-with-the-azure-portal-and-event-grid"></a>Vytvoření a směrování vlastních událostí pomocí webu Azure Portal a Event Gridu

Azure Event Grid je služba zpracování událostí pro cloud. V tomto článku pomocí webu Azure Portal vytvoříte vlastní téma, přihlásíte se k jeho odběru a aktivujete událost, abyste viděli výsledek. Obvykle odesíláte události do koncového bodu, který na událost reaguje například webhookem nebo funkcí Azure Functions. Pro zjednodušení tohoto článku však budete události odesílat na adresu URL, která jenom shromažďuje zprávy. Tuto adresu URL vytvoříte pomocí nástrojů třetích stran [RequestBin](https://requestb.in/) nebo [Hookbin](https://hookbin.com/).

>[!NOTE]
>**RequestBin** a **Hookbin** nejsou určené pro použití s vysokou propustností. Použití těchto nástrojů je čistě demonstrativní. Pokud najednou nabídnete více než jednu událost, možná se v nástroji nezobrazí všechny.

Až budete hotovi, uvidíte, že se data událostí odeslala do koncového bodu.

![Data událostí](./media/custom-event-quickstart-portal/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Témata služby Event Grid jsou prostředky Azure a musí být umístěné ve skupině prostředků Azure. Skupina prostředků je logická kolekce, ve které se nasazují a spravují prostředky Azure.

1. Na levém navigačním panelu vyberte **Skupiny prostředků**. Pak vyberte **Přidat**.

   ![Vytvoření skupiny prostředků](./media/custom-event-quickstart-portal/create-resource-group.png)

1. Nastavte název skupiny prostředků na *gridResourceGroup* a umístění na *westus2*. Vyberte **Vytvořit**.

   ![Zadání hodnot pro skupinu prostředků](./media/custom-event-quickstart-portal/provide-resource-group-values.png)

## <a name="create-a-custom-topic"></a>Vytvoření vlastního tématu

Téma poskytuje uživatelsky definovaný koncový bod, do kterého odesíláte události. 

1. Pokud chcete vytvořit téma ve své skupině prostředků, vyberte **Všechny služby** a vyhledejte *event grid*. Z dostupných možností vyberte **Témata Event Gridu**.

   ![Vytvoření tématu Event Gridu](./media/custom-event-quickstart-portal/create-event-grid-topic.png)

1. Vyberte **Přidat**.

   ![Přidání tématu Event Gridu](./media/custom-event-quickstart-portal/add-topic.png)

1. Zadejte název tématu. Název tématu musí být jedinečný, protože je reprezentován položkou DNS. Vyberte jednu z [podporovaných oblastí](overview.md). Vyberte skupinu prostředků, kterou jste vytvořili dříve. Vyberte **Vytvořit**.

   ![Zadání hodnot pro téma Event Gridu](./media/custom-event-quickstart-portal/provide-topic-values.png)

1. Po vytvoření tématu vyberte **Aktualizovat** a zobrazte toto téma.

   ![Zobrazení tématu Event Gridu](./media/custom-event-quickstart-portal/see-topic.png)

## <a name="create-a-message-endpoint"></a>Vytvoření koncového bodu zpráv

Před přihlášením k odběru tématu vytvoříme koncový bod pro zprávy události. Místo psaní kódu, který by na událost reagoval, vytvoříme koncový bod, který bude shromažďovat zprávy, abyste je mohli zobrazit. RequestBin a Hookbin jsou nástroje třetích stran, které umožňují vytvořit koncový bod a zobrazit požadavky, které se do nich odesílají. Přejděte na nástroj [RequestBin](https://requestb.in/) a klikněte na **Create a RequestBin** (Vytvořit RequestBin) nebo přejděte na nástroj [Hookbin](https://hookbin.com/) a klikněte na **Create New Endpoint** (Vytvořit nový koncový bod).  Zkopírujte adresu URL přihrádky, protože ji budete potřebovat při přihlašování k odběru tématu.

## <a name="subscribe-to-a-topic"></a>Přihlášení k odběru tématu

K odběru tématu se přihlašujete, aby služba Event Grid věděla, které události chcete sledovat. 

1. Pokud chcete vytvořit odběr služby Event Grid, znovu vyberte **Všechny služby** a vyhledejte *event grid*. Z dostupných možností vyberte **Odběry Event Gridu**.

   ![Vytvoření odběru Event Gridu](./media/custom-event-quickstart-portal/create-subscription.png)

1. Vyberte **+ Odběr události**.

   ![Přidání odběru Event Gridu](./media/custom-event-quickstart-portal/add-subscription.png)

1. Zadejte jedinečný název odběru události. Jako typ tématu vyberte **Témata Event Gridu**. Jako instanci vyberte vlastní téma, které jste vytvořili. Zadejte adresu URL z nástroje RequestBin nebo Hookbin jako koncový bod pro oznámení události. Až budete hotovi se zadáváním hodnot, vyberte **Vytvořit**.

   ![Zadání hodnot pro odběr Event Gridu](./media/custom-event-quickstart-portal/provide-subscription-values.png)

Nyní aktivujeme událost, abychom viděli, jak služba Event Grid distribuuje zprávu do vašeho koncového bodu. Pro zjednodušení tohoto článku odešlete ukázková data události do tématu pomocí služby Cloud Shell. Obvykle by aplikace nebo služba Azure odesílala data události.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="send-an-event-to-your-topic"></a>Odeslání události do tématu

Nejprve získáme adresu URL a klíč tématu. Místo `<topic_name>` použijte název vašeho tématu.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Následující příklad získá ukázková data události:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Pokud použijete příkaz `echo "$body"`, zobrazí se celá událost. Element JSON `data` je datová část vaší události. V tomto poli může být libovolný JSON ve správném formátu. Můžete také použít pole subject (předmět) pro pokročilé směrování a filtrování.

CURL je nástroj, který provádí požadavky HTTP. V tomto článku používáme nástroj CURL k odeslání události do tématu. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Právě jste aktivovali událost a služba Event Grid odeslala zprávu do koncového bodu, který jste nakonfigurovali při přihlášení k odběru. Přejděte na adresu URL koncového bodu, kterou jste vytvořili dříve. Nebo v otevřeném prohlížeči klikněte na tlačítko pro obnovení. Zobrazí se událost, kterou jste právě odeslali.

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
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete pokračovat v práci s touto událostí, nevyčišťujte prostředky vytvořené v rámci tohoto článku. Pokud pokračovat nechcete, odstraňte prostředky, které jste v rámci tohoto článku vytvořili.

Vyberte skupinu prostředků a pak vyberte **Odstranit skupinu prostředků**.

## <a name="next-steps"></a>Další kroky

Když teď víte, jak vytvářet témata a odběry událostí, zjistěte, s čím vám služba Event Grid ještě může pomoct:

- [Informace o službě Event Grid](overview.md)
- [Směrování událostí služby Blob Storage do vlastního webového koncového bodu](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorování změn virtuálního počítače pomocí služeb Azure Event Grid a Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Streamování velkých objemů dat do datového skladu](event-grid-event-hubs-integration.md)
