---
title: "Postup přidání zdroje událostí centra událostí do prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz vysvětluje postup přidání zdroje událostí, která je připojena k Centru událostí pro vaše prostředí časové řady Statistika"
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
ms.openlocfilehash: f6a993b3858cfb94dd9795f5e55f15fa6ec7dcb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-event-hub-event-source"></a>Postup přidání zdroje událostí centra událostí

Tento kurz se zaměřuje na tom, jak pomocí portálu Azure přidejte zdroje událostí, který čte z centra událostí pro vaše prostředí Statistika časové řady.

## <a name="prerequisites"></a>Požadavky

Vytvoření centra událostí a jsou k němu zápisu událostí. Další informace o službě Event Hubs naleznete v tématu <https://azure.microsoft.com/services/event-hubs/>

> [Skupiny příjemců] Každý zdroj události Statistika časové řady musí mít svůj vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli. Pokud více čtečky přijímat události z stejnou skupinu uživatelů, budou pravděpodobně najdete v části selhání všechny nástroje pro čtení. Všimněte si, že také maximální počet 20 skupiny příjemců za centra událostí. Podrobnosti najdete v tématu [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Zvolte možnost importovat

Nastavení pro zdroj události lze zadat ručně nebo centra událostí je možné vybrat z centra událostí, které jsou k dispozici.
V **možnost importu** selektor, vyberte jednu z následujících možností:

* Určit nastavení centra událostí ručně
* Použít Centrum událostí z dostupných předplatných

### <a name="select-an-available-event-hub"></a>Vyberte dostupný centra událostí

Následující tabulka popisuje jednotlivé možnosti na kartě nový zdroj událostí s jeho popis při výběru dostupná centra událostí jako zdroje událostí:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **centra událostí** k vytvoření zdroje událostí centra událostí.
| Id předplatného | Vyberte předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Obor názvů sběrnice | Vyberte obor názvů Service Bus, který obsahuje centra událostí.
| Název centra událostí | Vyberte název centra událostí.
| Název zásady centra událostí | Vyberte zásady sdíleného přístupu, které se dají vytvořit na kartě Konfigurace centra událostí. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Zásada sdíleného přístupu ke zdroji událostí *musí* mít **číst** oprávnění.
| Skupina uživatelů centra událostí | Skupina uživatelů mají události načítat z centra událostí. Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.

### <a name="provide-event-hub-settings-manually"></a>Určit nastavení centra událostí ručně

Následující tabulka popisuje každou vlastnost v kartě nový zdroj událostí s jeho popis při zadávání nastavení ručně:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **centra událostí** k vytvoření zdroje událostí centra událostí.
| Id předplatného | Předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Skupina prostředků | Předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Obor názvů sběrnice | Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí, vytvoříte tím taky obor názvů sběrnice.
| Název centra událostí | Název vašeho centra událostí. Když vytvoříte Centrum událostí, dáte mu taky určitý název
| Název zásady centra událostí | Zásady sdíleného přístupu, které se dají vytvořit na kartě Konfigurace centra událostí. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Zásada sdíleného přístupu ke zdroji událostí *musí* mít **číst** oprávnění.
| Klíč události rozbočovače zásad | Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus. Primární nebo sekundární klíč zadejte sem.
| Skupina uživatelů centra událostí | Skupina uživatelů mají události načítat z centra událostí. Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.

## <a name="next-steps"></a>Další kroky

1. Přidat zásadu přístupu dat do vašeho prostředí [data definovat zásady přístupu](time-series-insights-data-access.md)
1. Přístup k prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com)