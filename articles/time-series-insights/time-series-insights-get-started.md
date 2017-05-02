---
title: "Vytvoření prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte, jak vytvořit prostředí Time Series Insights, jak ho propojit se zdrojem událostí a připravit pro analýzu dat událostí během pár minut."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.lasthandoff: 04/25/2017

---

# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Vytvoření nového prostředí Time Series Insights na webu Azure Portal

Prostředí Time Series Insights je prostředek Azure s kapacitou úložiště a příchozího přenosu dat. Zákazníci zřizují prostředí s požadovanou kapacitou přes Azure Portal.

## <a name="steps-to-create-the-environment"></a>Postup vytvoření prostředí

Prostředí vytvoříte podle těchto pokynů:

1.    Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2.    Klikněte na symbol plus (+) v levém horním rohu.
3.    Do vyhledávacího pole zadejte „Time Series Insights“.

  ![Vytvoření prostředí Time Series Insights](media/get-started/getstarted-create-environment1.png)

4.    Vyberte Time Series Insights a klikněte na Vytvořit.

  ![Vytvoření skupiny prostředků Time Series Insights](media/get-started/getstarted-create-environment2.png)

5.    Zadejte název prostředí. Tento název bude reprezentovat prostředí v [průzkumníku Time Series Insights](https://insights.timeseries.azure.com).
6.    Vyberte předplatné. Vyberte to, které obsahuje váš zdroj událostí. Time Series Insights dokáže automaticky rozpoznat prostředky služby Azure IoT Hub a centra událostí, které existují v rámci stejného předplatného.
7.    Vyberte nebo vytvořte skupinu prostředků. Skupina prostředků je kolekce společně používaných prostředků Azure.
8.    Vyberte umístění pro hostování. Abyste se vyhnuli přenášení dat mezi datovými centry, zvolte umístění, které obsahuje váš zdroj událostí.
9.    Vyberte cenovou úroveň.
10.    Vyberte kapacitu. Kapacitu prostředí můžete po vytvoření změnit.
11.    Vytvořte prostředí. Můžete také připnout prostředí na řídicí panel pro usnadnění přístupu po přihlášení.

  ![Vytvoření služby Time Series Insights – připnutí na řídicí panel](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Další kroky

* [Definování zásad přístupu k datům](time-series-insights-data-access.md) pro přístup k prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)
* [Vytvoření zdroje událostí](time-series-insights-add-event-source.md)
* [Odesílání událostí](time-series-insights-send-events.md) do zdroje událostí

