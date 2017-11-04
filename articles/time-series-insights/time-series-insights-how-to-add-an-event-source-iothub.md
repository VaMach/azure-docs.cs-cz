---
title: "Postup přidání zdroje událostí služby IoT Hub do prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz vysvětluje postup přidání připojeném do služby IoT Hub pro vaše prostředí časové řady Statistika zdroje událostí"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 3b98728ced81256d05b1bed2c92fc66c5ca61b98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-iot-hub-event-source"></a>Postup přidání zdroje událostí služby IoT Hub

Tento kurz se zaměřuje na tom, jak pomocí portálu Azure přidejte zdroje událostí, který čte ze služby IoT Hub pro vaše prostředí Statistika časové řady.

## <a name="prerequisites"></a>Požadavky

Vytvoření služby IoT Hub a jsou k němu zápisu událostí. Další informace o centra IoT najdete v tématu <https://azure.microsoft.com/services/iot-hub/>

> [Skupiny příjemců] Každý zdroj události Statistika časové řady musí mít svůj vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli. Pokud více čtečky přijímat události z stejnou skupinu uživatelů, budou pravděpodobně najdete v části selhání všechny nástroje pro čtení. Podrobnosti najdete v tématu [Příručka vývojáře pro službu IoT Hub](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Zvolte možnost importovat

Nastavení pro zdroj události lze zadat ručně nebo centrum IoT je možné vybrat z centra IoT, které jsou k dispozici.
V **možnost importu** selektor, vyberte jednu z následujících možností:

* Zadejte nastavení centra IoT ručně
* Pomocí služby IoT Hub z dostupných předplatných

### <a name="select-an-available-iot-hub"></a>Vyberte dostupné služby IoT Hub

Následující tabulka popisuje jednotlivé možnosti na kartě nový zdroj událostí s jeho popis při výběru Centrum IoT k dispozici jako zdroje událostí:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **IoT Hub** k vytvoření zdroje událostí služby IoT Hub.
| Id předplatného | Vyberte předplatné, ve kterém byla vytvořena toto centrum IoT.
| Název centra IoT | Vyberte název služby IoT Hub.
| Název zásady centra IoT | Vyberte zásady sdíleného přístupu, které lze najít na kartě nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Zásada sdíleného přístupu ke zdroji událostí *musí* mít **služba připojit** oprávnění.
| Skupina uživatelů centra IoT | Skupiny příjemců číst události ze služby IoT Hub. Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.

### <a name="provide-iot-hub-settings-manually"></a>Zadejte nastavení centra IoT ručně

Následující tabulka popisuje každou vlastnost v kartě nový zdroj událostí s jeho popis při zadávání nastavení ručně:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **IoT Hub** k vytvoření zdroje událostí služby IoT Hub.
| Id předplatného | Předplatné, ve kterém byla vytvořena toto centrum IoT.
| Skupina prostředků | Předplatné, ve kterém byla vytvořena toto centrum IoT.
| Název centra IoT | Název služby IoT Hub. Když vytvoříte Centrum IoT, dáte mu taky určitý název
| Název zásady centra IoT | Zásady sdíleného přístupu, které se dají vytvořit na kartě nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Zásada sdíleného přístupu ke zdroji událostí *musí* mít **služba připojit** oprávnění.
| Klíč zásad centra IoT | Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus. Primární nebo sekundární klíč zadejte sem.
| Skupina uživatelů centra IoT | Skupiny příjemců číst události ze služby IoT Hub. Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.

## <a name="next-steps"></a>Další kroky

1. Přidat zásadu přístupu dat do vašeho prostředí [data definovat zásady přístupu](time-series-insights-data-access.md)
1. Přístup k prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com)