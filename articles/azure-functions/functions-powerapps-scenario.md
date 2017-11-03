---
title: "Volání funkce z PowerApps | Microsoft Docs"
description: "Vytvořte vlastní konektor pak volání funkce pomocí tohoto konektoru."
services: functions
keywords: "cloudových aplikací, cloudových služeb, PowerApps, obchodní procesy, obchodní aplikace"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 1e262fde37b68bcfcee3c974deb91bd07965de19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-function-from-powerapps"></a>Volání funkce z PowerApps
[PowerApps](https://powerapps.microsoft.com) platformy je určená pro obchodní specialisté můžete vytvářet aplikace, bez kódu tradiční aplikace. Profesionální vývojáře můžete použít Azure Functions k rozšíření možností PowerApps, při stínění Tvůrce aplikací PowerApps z technické podrobnosti.

Vytvoříte aplikaci v tomto tématu podle scénáři údržby pro turbín větru. Toto téma ukazuje, jak zavolat funkci, která je definována v [vytvořit definici OpenAPI pro funkci](functions-openapi-definition.md). Funkce určuje, zda nouzové opravy na turbínu větru nákladově efektivní.

![Dokončení aplikace v PowerApps](media/functions-powerapps-scenario/finished-app.png)

Informace o volání stejnou funkci z Flow Microsoft najdete v tématu [volání funkce z Microsoft Flow](functions-flow-scenario.md).

V tomto tématu se dozvíte, jak:

> [!div class="checklist"]
> * Příprava ukázkových dat v aplikaci Excel.
> * Exportujte definici rozhraní API.
> * Přidáte připojení k rozhraní API.
> * Vytvořte aplikaci a přidejte datové zdroje.
> * Přidání ovládacích prvků pro zobrazení dat v aplikaci.
> * Přidání ovládacích prvků volání funkce a zobrazovat data.
> * Spusťte aplikaci k určení, zda je nákladově efektivní a opravit.

## <a name="prerequisites"></a>Požadavky

+ Aktivní [PowerApps účet](https://powerapps.microsoft.com/tutorials/signup-for-powerapps.md) s stejné přihlašovací údaje jako účet Azure. 
+ V aplikaci Excel, protože aplikace Excel bude používat jako zdroj dat pro vaši aplikaci.
+ Dokončení tohoto kurzu [vytvořit definici OpenAPI pro funkci](functions-openapi-definition.md).


## <a name="prepare-sample-data-in-excel"></a>Příprava ukázkových dat v aplikaci Excel
Můžete začít tak, že Příprava ukázkových dat, který používáte v aplikaci. Zkopírujte následující tabulku do aplikace Excel. 

| Název      | Zeměpisná šířka  | Longtitude  | LastServiceDate | MaxOutput | ServiceRequired | EstimatedEffort | InspectionNotes                            |
|------------|-----------|-------------|-----------------|-----------|-----------------|-----------------|--------------------------------------------|
| Turbína 1  | 47.438401 | -121.383767 | 2/23/2017       | 2850      | Ano             | 6               | Toto je druhý problém tohoto měsíce.       |
| Turbína 4  | 47.433385 | -121.383767 | 5/8/2017        | 5400      | Ano             | 6               |                                            |
| Turbína 33 | 47.428229 | -121.404641 | 6/20/2017       | 2800      |                 |                 |                                            |
| Turbína 34 | 47.463637 | -121.358824 | 2/19/2017       | 2800      | Ano             | 7               |                                            |
| Turbína 46 | 47.471993 | -121.298949 | 3/2/2017        | 1200      |                 |                 |                                            |
| Turbína 47 | 47.484059 | -121.311171 | 8/2/2016        | 3350      |                 |                 |                                            |
| Turbína 55 | 47.438403 | -121.383767 | 10/2/2016       | 2400      | Ano             | 40               | Máme některé části brzo pro tento z nich. |

1. V aplikaci Excel, vyberte data a na **Domů** , klikněte na **formátovat jako tabulku**.

    ![Formátovat jako tabulku](media/functions-powerapps-scenario/format-table.png)

1. Vyberte libovolný styl a klikněte na **OK**.

1. S tabulkou vybrali, na **návrhu** zadejte `Turbines` pro **název tabulky**.

    ![Název tabulky](media/functions-powerapps-scenario/table-name.png)

1. Uložte sešit aplikace Excel.

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Přidat připojení k rozhraní API
Vlastní rozhraní API (také označované jako vlastní konektor) je k dispozici v PowerApps, ale musíte provést připojení na rozhraní API, abyste mohli používat v aplikaci.

1. V [web.powerapps.com](https://web.powerapps.com), klikněte na tlačítko **připojení**.

    ![Připojení PowerApps](media/functions-powerapps-scenario/powerapps-connections.png)

1. Klikněte na tlačítko **nové připojení**, přejděte dolů k položce **turbína Repair** konektor a klikněte na něj.

    ![Nové připojení](media/functions-powerapps-scenario/new-connection.png)

1. Zadejte klíč rozhraní API a klikněte na tlačítko **vytvořit**.

    ![Vytvoření připojení](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Pokud sdílíte s ostatními uživateli aplikace, musí každý uživatel, který funguje na nebo používá aplikace také zadejte klíč rozhraní API pro připojení k rozhraní API. Toto chování může v budoucnu změnit, a toto téma tak, aby odrážela, který bude aktualizován.

## <a name="create-an-app-and-add-data-sources"></a>Vytvořte aplikaci a přidejte zdroje dat
Nyní jste připraveni vytvořit aplikaci v PowerApps a přidejte data z aplikace Excel a vlastní rozhraní API jako zdroje dat pro aplikaci.

1. V [web.powerapps.com](https://web.powerapps.com), v levém podokně klikněte na **novou aplikaci**.

1. V části **prázdnou aplikaci**, klikněte na tlačítko **rozložení pro telefony**.

    ![Vytvoření aplikace pro tablety](media/functions-powerapps-scenario/create-phone-app.png)

    Aplikace se otevře v PowerApps Studio pro web. Následující obrázek ukazuje různé části PowerApps Studio. Tento image je pro dokončení aplikaci; prázdná obrazovka nejprve v prostředním podokně se zobrazí.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(1) levém navigačním panelu**, ve které zobrazí hierarchické zobrazení všech ovládacích prvků na každý obrazovky

    **(2) prostředním podokně**, který ukazuje na obrazovce pracujete na

    **(3) pravém podokně**, kde nastavíte možnosti například rozložení a zdroje dat.

    **(4) vlastnost** rozevíracího seznamu, kde můžete vybrat vlastnosti, která se týkají vzorce

    **(5) řádek vzorců**, kde můžete přidat vzorce (jako v aplikaci Excel), které definují chování aplikace
    
    **(6) pás karet**, kde můžete přidat ovládací prvky a přizpůsobit prvky návrhu

1. Jako zdroj dat přidáte soubor aplikace Excel.

    1. V pravém podokně klikněte na **Data** , klikněte na **přidat zdroj dat**.

        ![Přidání zdroje dat](media/functions-powerapps-scenario/add-data-source.png)

    1. Klikněte na tlačítko **přidání statických dat do aplikace**.

        ![Přidání zdroje dat](media/functions-powerapps-scenario/add-static-data.png)

        Za normálních okolností by číst a zapisovat data z externího zdroje, ale přidáváte data z aplikace Excel jako statických dat vzhledem k tomu, že toto je vzorový.

    1. Přejděte k souboru aplikace Excel, který jste uložili, vyberte **turbín** tabulky a klikněte na tlačítko **Connect**.

        ![Přidání zdroje dat](media/functions-powerapps-scenario/choose-table.png)

1. Jako zdroj dat přidáte vlastní rozhraní API.

    1. Na **Data** , klikněte na **přidat zdroj dat**.

    1. Klikněte na tlačítko **turbína Repair**.

        ![Konektor opravit turbína](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Přidání ovládacích prvků pro zobrazení dat v aplikaci
Teď, když zdroji dat jsou k dispozici v aplikaci, přidáte obrazovky do vaší aplikace, můžete zobrazit data turbína.

1. Na **Domů** , klikněte na **nové obrazovky** > **obrazovky seznamu**.

    ![Seznam obrazovky](media/functions-powerapps-scenario/list-screen.png)

    Přidá k obrazovce, která obsahuje PowerApps *Galerie* zobrazíte položky a jiných ovládacích prvků, které umožňují vyhledávání, řazení a filtrování.

1. Změnit záhlaví k `Turbine Repair`a změňte velikost galerie, takže je prostor pro další ovládací prvky v něm.

    ![Změnit název a změňte velikost Galerie](media/functions-powerapps-scenario/gallery-title.png)

1. S vybraným v pravém podokně klikněte v galerii **Data** změňte zdroj dat z **CustomGallerySample** k **turbín**.

    ![Zdroj dat změny](media/functions-powerapps-scenario/change-data-source.png)

    Datová sada neobsahuje bitovou kopii, takže teď že můžete změnit rozložení, aby lépe vyhovoval data. 

1. Stále v pravém podokně změňte **rozložení** k **nadpisu, podnadpisu a textu**.

    ![Změna rozložení galerie](media/functions-powerapps-scenario/change-layout.png)

1. Jako poslední krok v pravém podokně změňte pole, které jsou zobrazeny v galerii.

    ![Změňte pole galerie](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** = LastServiceDate
    + **Subtitle2** = ServiceRequired
    + **Title2** = název 

1. V aplikaci gallery vybrána, nastavte **TemplateFill** vlastnost, která má následující vzorec: `If(ThisItem.IsSelected, Orange, White)`.

    ![Vzorec výplně šablony](media/functions-powerapps-scenario/formula-fill.png)

    Nyní je snazší zjistit, která položka Galerie je vybrána.

    ![Vybrané položky](media/functions-powerapps-scenario/selected-item.png)

1. Nepotřebujete původní obrazovky v aplikaci. V levém podokně, najeďte myší na **Screen1**, klikněte na tlačítko **...** , a **odstranit**.

    ![Odstranit obrazovky](media/functions-powerapps-scenario/delete-screen.png)

Existuje mnoho dalších formátování, které by obvykle provést v produkční aplikace, ale přesunete k důležitou roli v tomto scénáři - volání funkce.

## <a name="add-controls-to-call-the-function-and-display-data"></a>Přidání ovládacích prvků volání funkce a zobrazení dat
Můžete mít aplikaci, která zobrazí souhrnná data pro každou turbína, takže nyní že je čas přidat ovládací prvky které volání funkce, kterou jste vytvořili a zobrazit data, která je vrácena. Přístup funkce na základě způsobu pojmenujte ji v definici OpenAPI; v takovém případě má `TurbineRepair.CalculateCosts()`.

1. Na pásu karet na **vložit** , klikněte na **tlačítko**. Na stejné kartě, klikněte na tlačítko **popisek**

    ![Tlačítko Vložit a popisek](media/functions-powerapps-scenario/insert-controls.png)

1. Přetáhněte na tlačítko a popisku níže galerie a změňte velikost popisku. 

1. Vyberte tlačítko text a změňte ji na `Calculate costs`. Aplikace by měl vypadat jako na následujícím obrázku.

    ![Aplikace s tlačítkem](media/functions-powerapps-scenario/move-button-label.png)

1. Vyberte tlačítko a zadejte následující vzorec pro tlačítka **OnSelect** vlastnost.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Tento vzorec provede, když se po kliknutí na tlačítko a provede následující akce, pokud je vybrané položky galerie **ServiceRequired** hodnotu `Yes`:

    + Vymaže *kolekce* `DetermineRepair` k odebrání dat z předchozího volání. Kolekce je tabulkové proměnné.

    + Přiřadí do kolekce s daty vrácenými voláním funkce `TurbineRepair.CalculateCosts()`. 
    
        Hodnoty předaný funkci pocházet z **EstimatedEffort** a **MaxOutput** pole pro vybranou položku v galerii. Tato pole nejsou zobrazeny v galerii, ale jsou stále k dispozici pro použití ve vzorcích.

1. Vyberte štítek a zadejte následující vzorec pro jeho **Text** vlastnost.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Používá tento vzorec `First()` funkce pro přístup k první (a pouze) řádek `DetermineRepair` kolekce. Potom zobrazí tři hodnoty, které funkce vrátí hodnotu: `message`, `costToFix`, a `revenueOpportunity`. Tyto hodnoty jsou prázdné, než při prvním spuštění aplikace.

    Dokončená aplikace by měl vypadat jako na následujícím obrázku.

    ![Dokončení aplikace před spuštěním](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>Spuštění aplikace
Máte kompletní aplikace! Nyní je čas a potom ho spusťte najdete v části funkce volá v akci.

1. V pravém horním rohu PowerApps Studio klikněte na tlačítko spustit: ![Tlačítko spustit aplikaci](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Vyberte turbínu s hodnotou `Yes` pro **ServiceRequired**, klikněte **vypočítat náklady** tlačítko. Měli byste vidět výsledek jako na následujícím obrázku.

    ![Dokončení aplikace v PowerApps](media/functions-powerapps-scenario/finished-app.png)

1. Zkuste jiné turbín chcete zobrazit, co je vrácené funkcí pokaždé, když.

## <a name="next-steps"></a>Další kroky
V tomto tématu, jste zjistili, jak:

> [!div class="checklist"]
> * Příprava ukázkových dat v aplikaci Excel.
> * Exportujte definici rozhraní API.
> * Přidáte připojení k rozhraní API.
> * Vytvořte aplikaci a přidejte datové zdroje.
> * Přidání ovládacích prvků pro zobrazení dat v aplikaci.
> * Přidání ovládacích prvků volání funkce a zobrazení dat
> * Spusťte aplikaci k určení, zda je nákladově efektivní a opravit.

Další informace o PowerApps najdete v tématu [Úvod do PowerApps](https://powerapps.microsoft.com/tutorials/getting-started/).

Další informace o dalších zajímavých scénáře, které používají Azure Functions najdete v tématu [volání funkce z Microsoft Flow](functions-flow-scenario.md) a [vytvoří funkci, která se integruje se službou Azure Logic Apps](functions-twitter-email.md).