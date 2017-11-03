---
title: "Přidání podmínek a spustit pracovní postupy - Azure Logic Apps | Microsoft Docs"
description: "Řídí, jak spouštět pracovní postupy v Azure Logic Apps přidáním podmíněnou logiku, aktivační události, akce a parametrů."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-logic-apps-features"></a>Pomocí funkcí Logic Apps

V [předchozí téma](../logic-apps/logic-apps-create-a-logic-app.md), jste vytvořili svou první aplikaci logiky. Chcete-li řídit svou aplikaci logiky pracovního postupu, můžete zadat různé cesty pro svou aplikaci logiky ke spuštění a postupy zpracování dat v polích, kolekce a dávek. V pracovním postupu aplikace logiky můžete zahrnout tyto prvky:

* Podmínky a [přepínač příkazy](../logic-apps/logic-apps-switch-case.md) umožní aplikaci logiky spustit různé akce, které jsou založené na tom, zda jsou splněny určité podmínky.

* [Smyčky](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky opakovaně spustit kroky. Například můžete opakovat akce přes pole při použití **for_each –** smyčky. Nebo akce můžete opakovat, dokud je splněna podmínka, pokud použijete **dokud** smyčky.

* [Obory](../logic-apps/logic-apps-loops-and-scopes.md) umožňují můžete seskupit sérii akcí, například k implementaci výjimek.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky spuštění samostatné pracovní postupy pro položky v poli, při použití **SplitOn** příkaz.

Toto téma představuje další koncepty pro vytvoření aplikace logiky:

* Zobrazení kódu, chcete-li upravit existující aplikaci logiky
* Možnosti pro spuštění pracovního postupu

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Podmínky: Spustit kroky až po splnění podmínku

Pokud chcete, aby aplikace logiky spustit kroky jenom v případě, že data splňuje určitá kritéria, můžete přidat podmínku, která porovná data v pracovním postupu proti konkrétní pole nebo hodnoty.

Předpokládejme například, že máte aplikaci logiky, který odesílá příliš mnoho e-mailům v příspěvcích na informačního kanálu RSS na web. Podmínku můžete přidat tak, aby aplikace logiky odešle e-mailu jenom v případě, že nového příspěvku patří do určité kategorie.

1. V [portál Azure](https://portal.azure.com), najít a otevřít aplikaci logiky v návrháři aplikace logiky.

2. Do umístění pracovního postupu, který chcete přidáte podmínku. 

   Chcete-li přidat podmínku mezi existující kroky v pracovním postupu aplikace logiky, přesuňte ukazatel přes na šipku, ve které chcete přidat podmínku. 
   Vyberte **znaménko plus** (**+**), pak zvolte **přidat podmínku**. Například:

   ![Přidejte podmínku do aplikace logiky](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Pokud chcete přidat podmínku na konci aktuálního pracovního postupu, přejděte k dolnímu okraji svou aplikaci logiky a zvolte **+ nový krok**.

3. Nyní můžete Definujte podmínku. Zadejte zdrojové pole, které chcete vyhodnotit, operaci provést a cílová hodnota nebo pole. Chcete-li přidat existující pole na vaše podmínky, zvolte z **dynamického obsahu seznamu přidat**.

   Například:

   ![Upravit podmínky v základní režimu](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Tady je Dokončená podmínka:

   ![Dokončení podmínky](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Chcete-li definovat podmínku v kódu, zvolte **upravit v rozšířeném režimu**. Například:
   > 
   > ![Upravit podmínky v kódu](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. V části **Pokud Ano** a **ne v případě**, přidávat postupy založené na tom, zda je splněna podmínka.

   Například:

   ![Podmínky se Ano a žádné cesty](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Můžete přetáhnout existující akce do **Pokud Ano** a **ne v případě** cesty.

5. Když jste hotovi, uložte svou aplikaci logiky.

Pouze v případě, že v příspěvcích splňují vaše podmínky se teď se e-mailů.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Opakování akce pro seznam pomocí příkazu forEach

Smyčka typu forEach určuje pole přes opakování akce. Pokud není pole, tok se nezdaří. Například pokud máte action1, který jako výstup pole zprávy a chcete odeslat každou zprávu, můžete zahrnout tento příkaz forEach ve vlastnostech akci:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>Upravit kód definici aplikace logiky

I když máte návrháře aplikace logiky, lze přímo upravit kód, který definuje logiku aplikace.

1. Na panelu příkazů vyberte **Code zobrazení**.

    Úplné editor otevře a zobrazuje definici, kterou jste upravili.

    ![Zobrazení kódu](media/logic-apps-use-logic-app-features/codeview.png)

    V textovém editoru můžete zkopírovat a vložit libovolného počtu akcí v rámci stejné aplikaci logiky nebo mezi aplikace logiky. 
    Můžete také snadno přidávat nebo odebírat celé oddíly z definice a definice můžete také sdílet s ostatními.

2. Pokud chcete uložit provedené úpravy, zvolte **Uložit**.

## <a name="parameters"></a>Parametry

Některé funkce Logic Apps jsou k dispozici pouze v zobrazení kódu, například parametry. Parametry usnadňují znovu použít hodnoty v celé aplikaci logiky. Například pokud máte e-mailovou adresu, kterou chcete použít v několik akcí, měli byste tuto e-mailovou adresu jako parametr.

Parametry jsou vhodné pro stahování na hodnoty, které budete chtít nejspíš se měnit hodně. Jsou užitečné zejména v případě, že je nutné přepsat parametry v různých prostředích. Další postupy k přepsání parametry podle prostředí najdete v tématu [vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md) a [dokumentace k REST API](https://docs.microsoft.com/rest/api/logic).

Tento příklad ukazuje, jak se bude aktualizovat stávající aplikaci logiky tak, aby parametry lze použít pro termín dotazu.

1. V zobrazení kódu najdete `parameters : {}` objektu a přidejte `currentFeedUrl` objektu:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Přejděte na `When_a_feed-item_is_published` akce, Najít `queries` části a nahradit hodnotu dotazu:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    Pro připojení dvě nebo více řetězce, můžete také použít `concat` funkce. 
    Například `"@concat('#',parameters('currentFeedUrl'))"` funguje stejně jako výše.

3.  Až budete hotoví, zvolte **Uložit**. 

    Teď můžete změnit předáním jinou adresu URL prostřednictvím informačního kanálu RSS webu `currentFeedURL` objektu.

Další informace o [postupy k vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Spusťte aplikaci logiky pracovních postupů

Existuje několik možností pro spuštění pracovního postupu definované v aplikaci logiky. Pracovního postupu na vyžádání můžete vždy spustit v [portál Azure].

### <a name="recurrence-triggers"></a>Aktivuje opakování

Opakování aktivační událost se spustí na časový interval, který zadáte. Pokud je aktivační událost podmíněnou logiku, aktivační události Určuje, jestli je potřeba spustit pracovní postup. Aktivační událost určuje pracovního postupu se budou spouštět vrácením `200` stavový kód. Když pracovní postup není nutné spustit, vrátí aktivační události `202` stavový kód.

### <a name="callback-using-rest-apis"></a>Zpětné volání pomocí rozhraní REST API

Chcete-li spustit pracovní postup, služby zavolat koncový bod logiku aplikace. Chcete-li spustit tento druh logiku aplikace na vyžádání, zvolte **spustit nyní** na panelu příkazů. V tématu [spustit pracovní postupy voláním logiku aplikace koncové body jako triggery](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[portál Azure]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky

* [Příkazech Switch](../logic-apps/logic-apps-switch-case.md) 
* [Smyčky, obory a rozdělení dávek](../logic-apps/logic-apps-loops-and-scopes.md)
* [Vytváření definic aplikací logiky](../logic-apps/logic-apps-author-definitions.md)