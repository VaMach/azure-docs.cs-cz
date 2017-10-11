---
title: "Switch – příkaz pro různé akce v Azure Logic Apps | Microsoft Docs"
description: "Zvolte jiné akce lze provádět v logiku aplikace založené na výrazu hodnoty pomocí příkazu switch"
services: logic-apps
keywords: "Switch – příkaz"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 338b6a5b549d7bf81186550295608438ac4aee32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Provádění různých akcí v aplikacích logiky s příkaz switch

Při vytváření pracovního postupu, je často nutné provést různé akce na základě hodnoty objektu nebo výraz. Například můžete svou aplikaci logiky chovat různě v závislosti na kód stavu požadavku HTTP, nebo možnosti vybrané v e-mailu.

Příkaz přepínač můžete použít k implementaci těchto scénářů. Aplikace logiky můžete vyhodnotit token nebo výraz a zvolte případ se stejnou hodnotou provést zadané akce. Příkaz switch by měl odpovídat pouze jeden případ.

> [!TIP]
> Podobně jako všechny programovacích jazyků příkazech switch podporují pouze operátory rovnosti. Pokud potřebujete další relační operátory, jako je třeba "větší než", použijte příkaz podmínky.
> Aby chování při spuštění deterministický, případech musí obsahovat jedinečný a statické hodnotu namísto dynamického tokeny nebo výraz.

## <a name="prerequisites"></a>Požadavky

- Aktivní předplatné Azure. Pokud nemáte předplatné Azure aktivní [vytvořit bezplatný účet](https://azure.microsoft.com/free/), nebo zkuste [volné aplikace logiky pro](https://tryappservice.azure.com/).
- [Základní znalosti o aplikace logiky](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-to-your-workflow"></a>Do pracovního postupu přidat příkaz switch

Pokud chcete zobrazit, jak funguje příkazu switch, tento příklad vytvoří aplikace logiky, která monitoruje soubory odesílané do Dropboxu. Když jsou odeslány nové soubory, aplikace logiky odešle e-mailu schvalovatel, který rozhodne, zda se má přenést těchto souborů do služby SharePoint. Aplikace používá příkaz přepínač, který provádí různé akce na základě hodnoty, která vybere položku schvalovatel.

1. Vytvoření aplikace logiky a vyberte této aktivační události: **Dropboxu - Pokud dojde k vytvoření souboru**.

   ![Použití Dropboxu - Pokud dojde k vytvoření souboru aktivační události](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. V části aktivační událost, přidejte tuto akci: **Outlook.com - odesílání schválení e-mailu**

   > [!TIP]
   > Aplikace logiky také podporují odesílání e-mailu scénáře schválení z účtu Office 365 Outlook.

   - Pokud nemáte existující připojení, se zobrazí výzva k jeho vytvoření.
   - Vyplňte požadovaná pole. Například v položce **k**, zadejte e-mailovou adresu pro odesílání e-mailu schvalovatel.
   - V části **možností uživatele**, zadejte `Approve, Reject`.

   ![Konfigurace připojení](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Přidejte příkaz, přepínač.

   - Vyberte **+ nový krok** > **... Další** > **přidání přepínače případu**. 
   - Teď chceme vyberte akce k provedení na základě `SelectedOptions` výstup z *odeslání e-mailu schválení* akce. 
   Můžete najít v tomto poli **přidávat dynamický obsah** selektor.
   - Použití *případ 1* zpracování, pokud se vybere schvalovatel `Approve`.
     - Pokud, zkopírujte původní soubor k SharePoint Online pomocí [ **SharePoint Online – vytvoření souboru** akce](../connectors/connectors-create-api-sharepointonline.md).
     - Přidejte další akci v případě uživatelům oznámit, že nový soubor je k dispozici na webu služby SharePoint.
   - Přidání jiného případu zpracování, pokud uživatel vybere `Reject`.
     - Pokud zamítnutí, odesílat e-mailové oznámení, jiné schvalovatelů oznamující, že soubor byl odmítnut a není nutná žádná další akce.
   - `SelectedOptions`poskytuje pouze dvě možnosti, takže jsme můžete nechat **výchozí** případ prázdný.

   ![Switch – příkaz](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Příkaz switch musí mít alespoň jeden případ kromě případu výchozí.

4. Po příkazu switch odstranit původní soubor nahrán do Dropboxu přidáním tuto akci: **Dropbox – odstranit soubor**

5. Uložte svou aplikaci logiky. Testování aplikace s tím, že nahrajete soubor na Dropbox. Měli byste obdržet e-mailu schválení za chvíli. Vyberte možnost a sledovat chování.

   > [!TIP]
   > Podívejte se na postup [sledování funkce logic apps](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-the-code-behind-switch-statements"></a>Pochopení kódu příkazech switch

Teď, když jste úspěšně vytvořili aplikaci logiky, pomocí příkazu switch, podíváme se na definici kód za příkazem switch.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"`představuje název příkazu přepínače, které můžete přejmenovat čitelnější. 
* `"type": "Switch"`Označuje, že akce je příkaz switch. 
* `"expression"`vyhodnotí pro každý případ deklarovaný později v definici je jeho vybranou možnost v tomto příkladu. 
* `"cases"`může obsahovat libovolný počet případů. Pro každý případ `"Case *"` je výchozí název případu, kdy můžete přejmenovat čitelnější. 
`"case"`Určuje případu popisku, který používá přepínač výraz pro porovnání, a musí být jedinečný a konstantní hodnotu. Pokud žádný z případů odpovídat výrazu přepínače, akce v rámci `"default"` provedení.

## <a name="get-help"></a>Podpora

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [přidat podmínky](logic-apps-use-logic-app-features.md)
- Další informace o [chyb a výjimek](logic-apps-exception-handling.md)
- Prozkoumat více [možnosti jazyka pracovního postupu](logic-apps-author-definitions.md)