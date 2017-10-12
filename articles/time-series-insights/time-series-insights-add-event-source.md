---
title: "Přidání zdroje událostí do prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu připojíte zdroj událostí k prostředí Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: c247a7abd3d1682f8ccd1940e506ef54f9409263
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Vytvoření zdroje událostí pro prostředí Time Series Insights pomocí portálu Ibiza

Zdroj událostí Time Series Insights je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs. Time Series Insights se připojuje přímo ke zdrojům událostí a ingestuje datový proud, aniž by uživatelé museli psát jediný řádek kódu. V současné době Time Series Insights podporuje Azure Event Hubs a Azure IoT Hubs. Další zdroje událostí budou přidány později.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Postup přidání zdroje událostí do prostředí

1.  Přihlaste se k [portálu Ibiza](https://portal.azure.com).
2.  V nabídce na levé straně portálu Ibiza klikněte na Všechny prostředky.
3.  Vyberte vaše prostředí Time Series Insights.

  ![Vytvoření zdroje událostí Time Series Insights](media/add-event-source/getstarted-create-event-source-1.png)

4.  Vyberte „Zdroje událostí“ a klikněte na „+ Přidat“.

  ![Vytvoření zdroje událostí Time Series Insights – podrobnosti](media/add-event-source/getstarted-create-event-source-2.png)

5.  Zadejte název zdroje událostí. Tento název je přidružený ke všem událostem, které přichází z tohoto zdroje událostí, a je dostupný v době zpracování dotazu.
6.  Ze seznamu prostředků centra událostí v aktuálním předplatném vyberte centrum událostí. Nebo vyberte možnost importu Určit nastavení centra událostí ručně a určete centrum událostí v jiném předplatném. Události musí být publikovány ve formátu JSON.
7.  Vyberte zásadu, která má v centru událostí oprávnění ke čtení.
8.  Zadejte skupinu příjemců centra událostí.

  > [!IMPORTANT]
  > Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights). Pokud skupinu příjemců používají další služby, bude negativně ovlivněna operace čtení pro toto prostředí i ostatní služby. Pokud jako skupinu příjemců používáte $Default, může potenciálně dojít k jejímu opakovanému použití jinými čtenáři.

9.  Klikněte na Vytvořit.

Po vytvoření zdroje událostí začne Time Series Insights automaticky streamovat data do vašeho prostředí.

## <a name="next-steps"></a>Další kroky

* [Odesílání událostí](time-series-insights-send-events.md) do zdroje událostí
* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)
