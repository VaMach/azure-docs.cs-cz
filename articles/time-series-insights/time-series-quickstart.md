---
title: "Rychlý start - explorer statistiky Azure řady čas | Microsoft Docs"
description: "Tento rychlý start se dozvíte, jak začít pracovat s Azure časové řady Statistika explorer ve webovém prohlížeči k vizualizaci velkých objemů dat IoT. Prohlídka klíčové funkce v ukázce prostředí."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: v-mamcge
manager: jhubbard
editor: MarkMcGeeAtAquent, jasonwhowell, kfile, MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 11/15/2017
ms.openlocfilehash: b1f2881da21849c3ac09b008640fc9f72dc158dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Rychlý úvod: Prozkoumat Statistika Azure časové řady
Tento rychlý start se dozvíte, jak začít pracovat s explorer Statistika řady čas Azure v prostředí volné ukázka. Další informace o použití je webový prohlížeč vizualizovat velké objemy dat IoT a prohlídka klíčové funkce Průzkumníku Statistika časové řady. 

Azure Time Series Insights je plně spravovaná služba pro analýzy, ukládání a vizualizace, která usnadňuje zkoumání a analýzy miliard událostí IoT současně. Poskytuje globální zobrazení dat, takže můžete rychle ověřit řešení IoT a vyhnout nákladným odstávkám důležité zařízení tím, že můžete zjistit skrytá trendy, pomáhá přímé anomálií a provedení analýzy příčin skoro v reálném čase.  Pokud vytváříte aplikace, která data řady čas úložiště nebo dotaz, je potřeba, můžete vyvíjet pomocí REST API pro přehledy čas řady.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) před tím, než začnete.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Prozkoumejte explorer časové řady statistiky v ukázce prostředí

1. V prohlížeči přejděte na [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo). 

2. Pokud budete vyzváni, přihlaste se do Průzkumníka časové řady statistika pomocí svých přihlašovacích údajů účtu Azure. 
 
3. Zobrazí se stránka stručný přehled časové řady. Klikněte na tlačítko **Další** zahájíte stručný přehled.

   ![Klikněte na tlačítko Další](media/quickstart/quickstart1.png)

4. **Panel výběru čas** se zobrazí. Pomocí tohoto panelu vyberte časový rámec pro vizualizaci.

   ![Panel výběru čas](media/quickstart/quickstart2.png)

5. Klikněte na tlačítko a přetáhněte ji v oblasti a pak klikněte na **vyhledávání** tlačítko.
 
   ![Vyberte časový rámec](media/quickstart/quickstart3.png) 

   Statistika řady čas zobrazí graf vizualizace pro časový rámec, který jste zadali. Různé akce v rámci s spojnicový graf, jako je například filtrování, můžete provést Připnutí, řazení a skládání. 

   Se vrátíte do **panel výběru čas**, klikněte na šipku dolů, jak je znázorněno:

   ![Graf](media/quickstart/quickstart4.png)

6. Klikněte na tlačítko **přidat** v **podmínky panely** přidat nové hledaný termín.

   ![Přidat položku](media/quickstart/quickstart5.png)

7. V grafu, můžete vybrat oblast, klikněte pravým tlačítkem na oblast a vyberte **prozkoumat události**.
 
   ![Prozkoumejte události](media/quickstart/quickstart6.png)

   Mřížka nezpracovaná data budou zobrazena ze oblast, kterou zkoumáte:

   ![Zobrazení mřížky](media/quickstart/quickstart7.png)

8. Upravit vaše podmínky ke změně hodnot v grafu a přidejte jiný termín k cross porovnejte různé typy hodnot:

   ![Přidejte termín.](media/quickstart/quickstart8.png)

9. Zadejte termín filtru v **filtrovat řady...**  pole pro filtrování ad hoc řady. Pro rychlý start, zadejte **Station5** k korelace mezi teploty a přetížení pro příslušné stanici.
 
   ![Filtr řady](media/quickstart/quickstart9.png)

Po dokončení rychlé spuštění, můžete experimentovat s datovou sadou ukázka vytvořit různé vizualizace. 

### <a name="next-steps"></a>Další kroky
Jste připraveni vytvořit prostředí časové řady statistiky:
> [!div class="nextstepaction"]
> [Plánování prostředí Statistika časové řady](time-series-insights-environment-planning.md)
