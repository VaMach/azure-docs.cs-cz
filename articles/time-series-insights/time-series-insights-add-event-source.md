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
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 9dc394c0da34a8ee2796c6114e7f2f647c5345a1
ms.lasthandoff: 04/25/2017

---

# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-azure-portal"></a>Vytvoření zdroje událostí pro prostředí Time Series Insights pomocí webu Azure Portal

Zdroj událostí Time Series Insights je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs. Time Series Insights se připojuje přímo ke zdrojům událostí a ingestuje datový proud, aniž by uživatelé museli psát jediný řádek kódu. V současné době Time Series Insights podporuje služby Azure Event Hubs a Azure IoT Hub a v budoucnu budou přidány další zdroje událostí.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Postup přidání zdroje událostí do prostředí

1.    Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2.    V nabídce na levé straně webu Azure Portal klikněte na Všechny prostředky.
3.    Vyberte vaše prostředí Time Series Insights.

  ![Vytvoření zdroje událostí Time Series Insights](media/add-event-source/getstarted-create-eventsource1.png)

4.    Vyberte „Zdroje událostí“ a klikněte na „+ Přidat“.

  ![Vytvoření zdroje událostí Time Series Insights – podrobnosti](media/add-event-source/getstarted-create-eventsource2.png)

5.    Zadejte název zdroje událostí. Tento název je přidružený ke všem událostem, které přichází z tohoto zdroje událostí, a je dostupný v době zpracování dotazu.
6.    Vyberte centrum událostí ze seznamu prostředků centra událostí v rámci aktuálního předplatného nebo vyberte možnost importu „Určit nastavení centra událostí ručně“, pokud chcete zadat centrum událostí umístěné v jiném předplatném. Události musí být publikovány ve formátu JSON.
7.    Vyberte zásadu, která má v centru událostí oprávnění ke čtení.
8.    Zadejte skupinu příjemců centra událostí.

  > [!IMPORTANT]
  > Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights). Pokud skupinu příjemců používají další služby, bude negativně ovlivněna operace čtení pro toto prostředí i ostatní služby. Pokud jako skupinu příjemců používáte $Default, může potenciálně dojít k jejímu opakovanému použití jinými čtenáři.

9.    Klikněte na Vytvořit.

Po vytvoření zdroje událostí začne Time Series Insights automaticky streamovat data do vašeho prostředí.

## <a name="next-steps"></a>Další kroky

* [Odesílání událostí](time-series-insights-send-events.md) do zdroje událostí
* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)

