---
title: "Vytvoření prvního automatizovaného pracovního postupu spojujícího systémy a cloudové služby | Dokumentace Microsoftu"
description: "Automatizujte firemní procesy a pracovní postupy pro scénáře systémové integrace a integrace podnikových aplikací (EAI) pomocí vytváření a spouštění aplikací logiky."
author: ecfan
manager: anneta
editor: 
services: logic-apps
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: LADocs; estfan
ms.openlocfilehash: d62255ba6e3d5bdfbd792a47f3a92d4c88876742
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-logic-app-for-automating-workflows-and-processes-through-the-azure-portal"></a>Vytvoření první aplikace logiky pro automatizaci pracovních postupů a procesů prostřednictvím webu Azure Portal

Systémy a služby můžete integrovat bez psaní kódu sestavováním a spouštěním automatizovaných pracovních postupů pomocí [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). V tomto kurzu se pro ukázku, jak snadno automatizovat úlohy pomocí pracovních postupů, vytvoří základní aplikace logiky, která v informačním kanálu RSS kontroluje nový obsah na webu a za každou novou položku v informačním kanálu odešle e-mail. 

![Přehled – příklad první aplikace logiky](./media/logic-apps-create-a-logic-app/logic-app-overview.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření prázdné aplikace logiky
> * Přidání triggeru pro spuštění aplikace logiky při publikování položky informačního kanálu RSS
> * Přidání akce pro odesílání e-mailů s podrobnostmi o položce informačního kanálu RSS
> * Spuštění a testování aplikace logiky

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* E-mailový účet od [jakéhokoli poskytovatele e-mailu, kterého podporuje Azure Logic Apps](../connectors/apis-list.md), pro odesílání oznámení. Můžete použít například Office 365 Outlook, Outlook.com, Gmail nebo jiného podporovaného poskytovatele. Tento kurz používá Office 365 Outlook.

  > [!TIP]
  > Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com. Pokud máte pracovní nebo školní účet Azure, máte účet Office 365 Outlook.

* Odkaz na informační kanál RSS nějakého webu. V tomto příkladu se používá [informační kanál RSS pro hlavní zprávy z webu CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="1-create-a-blank-logic-app"></a>1. Vytvoření prázdné aplikace logiky 

1. Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").

2. V hlavní nabídce Azure zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**.

   ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Vytvořte aplikaci logiky s použitím nastavení uvedeného v tabulce.

   ![Zadání podrobností o aplikaci logiky](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *název_vaší_aplikace_logiky* | Zadejte jedinečný název aplikace logiky. | 
   | **Předplatné** | *vaše_předplatné_Azure* | Vyberte předplatné Azure, které chcete použít. | 
   | **Skupina prostředků** | *vaše_skupina_prostředků_Azure* | Vytvořte nebo vyberte skupinu prostředků Azure, která vám pomůže organizovat a spravovat související prostředky Azure. | 
   | **Umístění** | *vaše_oblast_Azure* | Vyberte oblast datového centra pro nasazení aplikace logiky. | 
   |||| 

4. Až budete připraveni, vyberte **Připnout na řídicí panel** a pak zvolte **Vytvořit**.

   Právě jste vytvořili prostředek Azure pro vaši aplikaci logiky. 
   Až Azure nasadí aplikaci logiky, v Návrháři pro Logic Apps se zobrazí šablony pro obvyklé scénáře, abyste mohli začít rychleji.

   > [!NOTE] 
   > Když vyberete **Připnout na řídicí panel**, vaše aplikace logiky se po nasazení zobrazí na řídicím panelu Azure a automaticky se otevře v Návrháři pro Logic Apps. Pokud ne, může svou aplikaci logiky vyhledat a otevřít ručně.

5. Prozatím v části **Šablony** zvolte **Prázdná aplikace logiky**, abyste mohli sestavit zcela novou aplikaci logiky.

   ![Výběr šablony aplikace logiky](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   V Návrháři pro Logic Apps se teď zobrazí dostupné [*konektory*](../connectors/apis-list.md) a jejich [*triggery*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), pomocí kterých spouštíte pracovní postup aplikace logiky.

   ![Triggery aplikace logiky](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="2-add-a-trigger-for-starting-the-workflow"></a>2. Přidání triggeru pro spuštění pracovního postupu

Každá aplikace logiky se musí spouštět pomocí [*triggeru*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Trigger se aktivuje, když dojde ke konkrétní události nebo když nová data splní podmínku, kterou jste nastavili. Modul Logic Apps pak vytvoří instanci aplikace logiky pro spuštění vašeho pracovního postupu. Při každé aktivaci triggeru modul vytvoří další samostatnou instanci, ve které se spustí pracovní postup vaší aplikace logiky.

1. Při filtrování do pole hledání zadejte „rss“. Vyberte trigger **RSS – Při publikování položky informačního kanálu**. 

   ![Výběr triggeru RSS – Při publikování položky informačního kanálu](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Zadejte odkaz na informační kanál RSS webu, který chcete sledovat, například `http://rss.cnn.com/rss/cnn_topstories.rss`. Nastavte interval a frekvenci opakování. V tomto příkladu nastavte vlastnosti tak, aby se informační kanál kontroloval každý den. 

   ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

3. Teď svoji práci uložte. Na panelu nástrojů návrháře zvolte **Uložit**.
Pokud chcete sbalit a skrýt podrobnosti triggeru, zvolte záhlaví triggeru.

   ![Uložení aplikace logiky](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Vaše aplikace logiky je teď v provozu, ale dokud do pracovního postupu nepřidáte akce, nebude dělat nic kromě kontroly nových položek v informačním kanálu RSS. 

## <a name="3-add-an-action-that-responds-to-the-trigger"></a>3. Přidání akce, která reaguje na trigger

Teď přidejte [*akci*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), což je úloha, kterou provádí pracovní postup vaší aplikace logiky. V tomto příkladu přidejte akci, která odešle e-mail, když se v informačním kanálu RSS objeví nová položka.

1. V Návrháři pro Logic Apps pod triggerem zvolte **+ Nový krok** > **Přidat akci**.

   ![Přidání akce](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Návrhář zobrazí [dostupné konektory](../connectors/apis-list.md), ze kterých můžete vybrat akci, která se má spustit v reakci na trigger.

   ![Výběr ze seznamu akcí](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Při filtrování do pole hledání zadejte „odeslat e-mail“. Vyhledejte a vyberte konektor odpovídající vašemu poskytovateli e-mailu. Pak pro konektor vyberte akci „odeslat e-mail“. Například: 

   * Pro pracovní nebo školní účet Azure vyberte konektor Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte konektor Outlook.com. 
   * Pro účty Gmail vyberte konektor Gmail. 

   My budeme pokračovat s konektorem Office 365 Outlook. 
   Pokud používáte jiného poskytovatele, postup je stejný, ale vaše uživatelské rozhraní může vypadat jinak. 

   ![Výběr akce Office 365 Outlook – Odeslat e-mail](./media/logic-apps-create-a-logic-app/actions.png)

3. Když se zobrazí výzva k zadání přihlašovacích údajů, zadejte uživatelské jméno a heslo k e-mailovému účtu. 

4. Zadejte podrobnosti uvedené v tabulce a zvolte pole, která chcete do e-mailu zahrnout.

   | Akce | Kroky | 
   | -- | ----- | 
   | Výběr dostupných polí pro pracovní postup | Klikněte do textového pole, aby se otevřel seznam **Dynamický obsah**, nebo zvolte **Přidat dynamický obsah**. | 
   | Zobrazení dalších dostupných polí | V seznamu **Dynamický obsah** u každé části zvolte **Zobrazit další**.  | 
   | Přidání prázdných řádků do textového pole | Stiskněte Shift + Enter. | 
   | Zavření seznamu **Dynamický obsah** | Znovu zvolte **Přidat dynamický obsah**. | 
   ||| 

   ![Výběr dat pro zahrnutí do e-mailu](./media/logic-apps-create-a-logic-app/rss-action-setup.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Komu** | *e-mailová_adresa_příjemce* | Zadejte e-mailovou adresu příjemce. Pro účely testování můžete použít svou vlastní e-mailovou adresu. | 
   | **Předmět** | Nový příspěvek na CNN: **Název informačního kanálu** | Zadejte obsah předmětu e-mailu. <p>Pro účely tohoto kurzu zadejte navrhovaný text a vyberte pole **Název informačního kanálu** triggeru, které zobrazí název položky informačního kanálu. | 
   | **Text** | Název: **Název informačního kanálu** <p>Datum publikování: **Datum publikování informačního kanálu** <p>Odkaz: **Odkaz na primární informační kanál** | Zadejte obsah e-mailu. <p>Pro účely tohoto kurzu zadejte navrhovaný text a pak vyberte následující pole triggeru: <p>- **Název informačního kanálu:** znovu zobrazí název položky informačního kanálu </br>- **Datum publikování informačního kanálu:** zobrazí datum a čas publikování položky </br>- **Odkaz na primární informační kanál:** zobrazí adresu URL položky informačního kanálu | 
   |||| 

   > [!NOTE] 
   > Pokud vyberete pole, které obsahuje pole položek, návrhář kolem akce odkazující na toto pole položek automaticky přidá smyčku For each. Aplikace logiky tak provede příslušnou akci pro každou položku pole.

5. Jakmile budete hotovi, uložte změny. Na panelu nástrojů návrháře zvolte **Uložit**.

   ![Hotová aplikace logiky](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

   Pokud teď chcete aplikaci logiky otestovat, pokračujte k další části.

## <a name="4-run-and-test-your-workflow"></a>4. Spuštění a testování pracovního postupu

1. Pokud chcete aplikaci logiky ručně spustit pro účely testování, na panelu nástrojů návrháře zvolte **Spustit**. Případně můžete nechat aplikaci logiky zkontrolovat zadaný informační kanál RSS podle nastaveného plánu.

   ![Spuštění aplikace logiky](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

   Pokud aplikace logiky najde nové položky, odešle e-mail obsahující požadovaná data, například:

   ![E-mail odeslaný za novou položku informačního kanálu RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

   Pokud aplikace logiky nenajde žádné nové položky, přeskočí akci, která odesílá e-mail, a s další kontrolou počká na další interval. 

2. Pokud chcete zkontrolovat historii spouštění a triggerů vaší aplikace logiky, v nabídce aplikace logiky zvolte možnost **Přehled**.
Pokud chcete zobrazit další podrobnosti o spuštění, zvolte řádek příslušného spuštění.

   ![Sledování a kontrola činnosti a historie spouštění aplikaci logiky](./media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Pokud nenajdete očekávaná data, na panelu příkazů zkuste zvolit možnost **Aktualizovat**.

   Bez ohledu na úspěch nebo selhání spuštění se v zobrazení Podrobnosti o spuštění zobrazí kroky, které byly úspěšné nebo selhaly. 

   ![Zobrazení podrobností o spuštění aplikace logiky](./media/logic-apps-create-a-logic-app/logic-app-run-details.png)

   Další informace o stavu, historii spouštění a historii triggerů vaší aplikace logiky a o její diagnostice najdete v článku [Odstraňovaní potíží s aplikací logiky](../logic-apps/logic-apps-diagnosing-failures.md).

3. Pokud chcete zobrazit vstupy a výstupy jednotlivých kroků, rozbalte krok, který chcete zkontrolovat. Tyto informace vám můžou pomoct s diagnostikou a laděním problémů ve vaší aplikaci logiky. Například:

   ![Zobrazení podrobností o kroku](./media/logic-apps-create-a-logic-app/logic-app-run-details-expanded.png)

   Další informace najdete v tématu [Monitorování aplikace logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gratulujeme, právě jste vytvořili a spustili svou první základní aplikaci logiky. Tento příklad ukazuje, jak snadno vytvořit pracovní postupy, které automatizují procesy pro integraci systémů a služeb – a všechno bez kódu.

> [!NOTE]
> Aplikace logiky se bude spouštět dál, dokud ji nevypnete. Pokud chcete svou aplikaci dočasně vypnout, pokračujte k další části.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Tento kurz používá prostředky a provádí akce, za které se na vaše předplatné Azure můžou účtovat poplatky. Jakmile budete s kurzem a testováním hotovi, nezapomeňte vypnout nebo odstranit všechny prostředky, za které nechcete, aby se vám účtovaly poplatky.

Aplikaci logiky můžete zakázat spouštění a odesílání e-mailů, aniž byste ji odstranili. V nabídce aplikace logiky zvolte **Přehled**. Na panelu nástrojů zvolte **Vypnout**.

![Vypnutí aplikace logiky](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Nejčastější dotazy

**Otázka:** Co dalšího můžu dělat se svou aplikací logiky? </br>
**Odpověď:** Můžete provádět i další úlohy, například zobrazit a upravit definici JSON, zkontrolovat protokol aktivit nebo aplikaci logiky odstranit.

Další úlohy správy aplikace logiky najdete pod těmito příkazy v nabídce aplikace logiky:

| Úkol | Kroky | 
| ---- | ----- | 
| Zobrazení stavu aplikace, historie spouštění a triggerů a obecných informací | Zvolte **Přehled**. | 
| Úprava aplikace | Zvolte **Návrhář aplikace logiky**. | 
| Zobrazení definice JSON pracovního postupu vaší aplikace | Zvolte **Zobrazení kódu aplikace logiky**. | 
| Zobrazení operací, které aplikace logiky provedla | Zvolte **Protokol aktivit**. | 
| Zobrazení předchozích verzí aplikace logiky | Zvolte **Verze**. | 
| Dočasné vypnutí aplikace | Zvolte **Přehled** a pak na panelu nástrojů zvolte **Vypnout**. | 
| Odstranění aplikace | Zvolte **Přehled** a pak na panelu nástrojů zvolte **Odstranit**. Zadejte název aplikace logiky a zvolte **Odstranit**. | 
||| 

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy ohledně Azure Logic Apps, navštivte [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Chcete pomoci vylepšit Azure Logic Apps a konektory? Hlasujte nebo zanechte své nápady na [webu User Voice pro Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

* [Vytvoření aplikace logiky pomocí sady Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Přidání podmínek a spuštění pracovních postupů](../logic-apps/logic-apps-use-logic-app-features.md)
*   [Šablony pro aplikace logiky](../logic-apps/logic-apps-use-logic-app-templates.md)
* [Vytváření aplikací logiky ze šablon Azure Resource Manageru](../logic-apps/logic-apps-arm-provision.md)
* [Měření využití Logic Apps](../logic-apps/logic-apps-pricing.md) 
* [Ceny Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps)
