---
title: "Postup přidání referenční datové sady do prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento článek popisuje postup přidání referenční datové sady do prostředí Azure časové řady statistiky. Referenční data jsou užitečné pro připojení k datům zdroje na posílení hodnoty."
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 7cdcefbd0daec3b7bab59680afa1470624583c74
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Vytvořit odkaz na sadu dat pro vaše prostředí časové řady statistika pomocí portálu Azure

Tento článek popisuje postup přidání referenční datové sady do prostředí Azure časové řady statistiky. Referenční data jsou užitečné pro připojení k datům zdroje na posílení hodnoty.

Referenční sada dat je kolekce položek rozšířená o události ze zdroje událostí. Modul příchozího přenosu dat Time Series Insights se připojí k události ze zdroje událostí s položkou v referenční sadě dat. Tato rozšířená událost je pak k dispozici pro dotaz. Toto připojení je založené na klíčích definovaných v referenční sadě dat.

## <a name="add-a-reference-data-set"></a>Přidat odkaz datové sady

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyhledejte existující časové řady Statistika prostředí. Klikněte na tlačítko **všechny prostředky** v nabídce na levé straně na portálu Azure. Vyberte vaše prostředí Time Series Insights.

3. V části **prostředí topologie** záhlaví, vyberte **referenční datové sady**.

    ![Vytvoření referenční sady dat Time Series Insights](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Vyberte **+ přidat** přidat nový odkaz datové sady.

5. Zadejte jedinečný **název referenční datové sady**.

    ![Vytvoření referenční sady dat Time Series Insights – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

6. Zadejte **název klíče** v prázdné pole a vyberte jeho **typu**. Tento název a typ se používají a vyberte správnou vlastnost z událostí ve zdroji událostí pro připojení k referenční data. 

   Pokud zadáte název klíče, jako například **DeviceId** a zadejte jako **řetězec**, pak modul příjem příchozích dat časové řady Statistika hledá vlastnost s názvem **DeviceId** typu **Řetězec** v každé příchozí události hledat až a spojení s. K události je možné připojit více než jeden klíč. Při hledání shodného názvu klíče se rozlišují velká a malá písmena.

7. Vyberte **Vytvořit**.

## <a name="next-steps"></a>Další kroky
* [Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.
* Úplnou referenční dokumentaci k rozhraní API najdete v dokumentu [Rozhraní API referenčních dat](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
