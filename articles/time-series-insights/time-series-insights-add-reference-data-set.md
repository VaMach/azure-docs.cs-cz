---
title: "Přidání referenční sady dat do prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu přidáte referenční sadu dat k prostředí Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017

---

# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Vytvoření referenční sady dat pro prostředí Time Series Insights pomocí portálu Ibiza

Referenční sada dat je kolekce položek rozšířená o události ze zdroje událostí. Modul příchozího přenosu dat Time Series Insights se připojí k události ze zdroje událostí s položkou v referenční sadě dat. Tato rozšířená událost je pak k dispozici pro dotaz. Toto připojení je založené na klíčích definovaných v referenční sadě dat.

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a>Postup přidání referenční sady dat do prostředí

1. Přihlaste se k [portálu Ibiza](https://portal.azure.com).
2. V nabídce na levé straně portálu Ibiza klikněte na Všechny prostředky.
3. Vyberte vaše prostředí Time Series Insights.

    ![Vytvoření referenční sady dat Time Series Insights](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Vyberte možnost Referenční sady dat a klikněte na Přidat.

    ![Vytvoření referenční sady dat Time Series Insights – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Zadejte název referenční sady dat.
6. Zadejte název klíče a jeho typ. Tento název a typ se použijí k výběr správné vlastnosti z události ve zdroji událostí. Pokud například zadáte „DeviceId“ jako název klíče a „String“ jako typ, pak bude modul příchozího přenosu dat Time Series Insights v příchozí události vyhledávat vlastnost s názvem DeviceId typu String. K události je možné připojit více než jeden klíč. Při hledání shodného názvu vlastnosti se rozlišují velká a malá písmena.

     ![Vytvoření referenční sady dat Time Series Insights – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Klikněte na Vytvořit.

## <a name="next-steps"></a>Další kroky

* [Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.
* Úplnou referenční dokumentaci k rozhraní API najdete v dokumentu [Rozhraní API referenčních dat](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
