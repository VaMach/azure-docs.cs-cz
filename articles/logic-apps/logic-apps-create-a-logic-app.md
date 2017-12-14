---
title: "Automatizace pracovních postupů spojujících systémy a cloudové služby – Azure Logic Apps | Dokumentace Microsoftu"
description: "Vytváření aplikací logiky pro automatizaci pracovních postupů pro scénáře systémové integrace a integrace podnikových aplikací (EAI)"
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
ms.date: 10/20/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 5906605192f9b03f612e6ca3a445434a23713d7f
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="automate-your-first-workflow-to-process-data-with-a-logic-app"></a>Automatizace prvního pracovního postupu pro zpracování dat pomocí aplikace logiky

Pokud chcete ve své organizaci rychleji integrovat systémy a služby, můžete automatizovat pracovní postupy a obchodní procesy pomocí [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). Tento kurz Rychlý start představuje, jak snadno sestavit a spustit automatizovaný pracovní postup vytvořením aplikace logiky. Ukázková aplikace ukazuje automatizaci pracovního postupu, který v informačním kanálu RSS webu kontroluje nové položky a za každou položku odešle e-mail.

Tato ukázková aplikace logiky odešle e-mail podobný tomuto příkladu:

![E-mail odeslaný za novou položku informačního kanálu RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

A tady je základní pracovní postup aplikace logiky, kterou vytvoříte:

![Přehled – příklad aplikace logiky](./media/logic-apps-create-a-logic-app/logic-app-simple-overview.png)

V tomto rychlém startu se naučíte:

> [!div class="checklist"]
> * Vytvoření prázdné aplikace logiky
> * Přidání triggeru pro spuštění pracovního postupu, když se v informačním kanálu RSS objeví nová položka
> * Přidání akce pro odeslání e-mailu s podrobnostmi o položce informačního kanálu RSS
> * Spuštění pracovního postupu aplikace logiky

Pokud ještě nemáte předplatné Azure, [zaregistrujte si bezplatný účet Azure](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

* E-mailový účet od jakéhokoli poskytovatele e-mailu, kterého podporuje Azure Logic Apps, pro odesílání oznámení. Můžete použít například Office 365 Outlook, Outlook.com nebo Gmail. Další podporované e-mailové konektory [najdete v seznamu konektorů](https://docs.microsoft.com/connectors/). Tento kurz Rychlý start používá Office 365 Outlook.

  > [!TIP]
  > Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com. Pokud máte pracovní nebo školní účet Azure, máte účet Office 365 Outlook.

* Odkaz na informační kanál RSS nějakého webu. V tomto příkladu se používá [informační kanál RSS pro hlavní zprávy z webu Reuters](http://feeds.reuters.com/reuters/topNews): `http://feeds.reuters.com/reuters/topNews`

V tomto kurzu Rychlý start se nevyžaduje psaní kódu, ale Logic Apps podporuje další scénáře využívající kód, například spouštění vlastního kódu z aplikace logiky pomocí služby [Azure Functions](../azure-functions/functions-overview.md).

## <a name="create-a-blank-logic-app"></a>Vytvoření prázdné aplikace logiky 

1. Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal"). 

2. V hlavní nabídce Azure zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**.

   ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](./media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Vytvořte aplikaci logiky s použitím nastavení uvedeného v tabulce pod tímto obrázkem:

   ![Zadání podrobností o aplikaci logiky](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *název_vaší_aplikace_logiky* | Zadejte jedinečný název aplikace logiky. | 
   | **Předplatné** | *název_vašeho_předplatného_Azure* | Vyberte předplatné Azure, které chcete použít. | 
   | **Skupina prostředků** | *název_vaší_skupiny_prostředků_Azure* | Vytvořte pro tuto aplikaci logiky [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md), která zajistí uspořádání všech prostředků přidružených k této aplikaci. | 
   | **Umístění** | *oblast_vašeho_datového_centra_Azure* | Vyberte oblast datového centra pro nasazení aplikace logiky, například USA – západ. | 
   | **Log Analytics** | Vypnuto | Pro svou aplikaci logiky zapněte protokolování diagnostiky, ale pro účely tohoto kurzu Rychlý start ponechte nastavení **Vypnuto**. | 
   |||| 

4. Až budete připraveni, vyberte **Připnout na řídicí panel**. Aplikace logiky se tak automaticky zobrazí na řídicím panelu Azure a po nasazení se automaticky otevře. Zvolte **Vytvořit**.

   > [!NOTE]
   > Pokud nechcete svou aplikaci logiky připnout, po nasazení ji musíte ručně vyhledat a otevřít, abyste mohli pokračovat.

   Jakmile Azure nasadí vaši aplikaci logiky, otevře se Návrhář pro Logic Apps se zobrazenou stránkou s úvodním videem. 
   Pod videem najdete šablony pro běžné vzory aplikací logiky. 
   Tento kurz Rychlý start vytváří zcela novou aplikaci logiky. 

5. Přejděte pod úvodní video a běžné triggery. V části **Šablony** zvolte **Prázdná aplikace logiky**.

   ![Výběr šablony prázdné aplikace logiky](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   V Návrháři pro Logic Apps se zobrazí dostupné konektory a jejich triggery, které slouží ke spouštění pracovních postupů aplikací logiky.

   ![Triggery aplikace logiky](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="add-a-trigger-to-detect-new-items"></a>Přidání triggeru pro rozpoznávání nových položek

Každý pracovní postup aplikace logiky začíná [triggerem](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Trigger se aktivuje, když dojde ke konkrétní události nebo když nová data splní podmínku, kterou nastavíte. Pokaždé, když se trigger aktivuje, vytvoří modul Logic Apps instanci aplikace logiky pro spuštění vašeho pracovního postupu.

1. Při filtrování do pole hledání zadejte „rss“. Vyberte trigger **RSS – Při publikování položky informačního kanálu**. 

   ![Výběr triggeru RSS – Při publikování položky informačního kanálu](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Zadejte odkaz na informační kanál RSS, který chcete monitorovat, například `http://feeds.reuters.com/reuters/topNews`. Nastavte interval a frekvenci opakování. Tento příklad kontroluje informační kanál každých pět minut.

   ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

   Logic Apps vytvoří připojení k informačnímu kanálu RSS.

   > [!TIP]
   > Pro zjednodušení zobrazení v návrháři můžete sbalit a skrýt podrobnosti o tvaru – stačí kliknout na záhlaví tvaru.

3. Uložte svou práci. Na panelu nástrojů návrháře zvolte **Uložit**. 

   ![Uložení aplikace logiky](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Vaše aplikace logiky je teď v provozu, ale kromě kontroly informačního kanálu nic nedělá. Přidejme tedy akci, která bude reagovat na aktivaci triggeru.

## <a name="add-an-action-to-send-email"></a>Přidání akce pro odeslání e-mailu

Teď, když máte trigger, přidejte [akci](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), která odešle e-mail, když se v informačním kanálu RSS objeví nová položka. Váš pracovní postup tuto akci provede po aktivaci triggeru.

1. V Návrháři pro Logic Apps pod triggerem zvolte **+ Nový krok** > **Přidat akci**.

   ![Přidání akce](./media/logic-apps-create-a-logic-app/add-new-action.png)

   V návrháři se zobrazí akce, které vaše aplikace logiky může provést při aktivaci triggeru.

   ![Výběr ze seznamu akcí](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Při filtrování do pole hledání zadejte „odeslat e-mail“. Vyhledejte a vyberte e-mailový konektor, který chcete použít. Pak pro tento konektor vyberte akci „odeslat e-mail“. Například: 

   * Pro pracovní nebo školní účty Azure vyberte Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte Outlook.com. 
   * Pro účty Gmail vyberte Gmail. 

   Tento kurz Rychlý start používá Office 365 Outlook. 
   Pokud používáte jiného poskytovatele e-mailu, postup je stejný, ale vaše uživatelské rozhraní může vypadat jinak. 

   ![Výběr akce Office 365 Outlook – Odeslat e-mail](./media/logic-apps-create-a-logic-app/actions.png)

3. Když se zobrazí výzva k zadání přihlašovacích údajů, zadejte uživatelské jméno a heslo k e-mailovému účtu. 

   Logic Apps vytvoří připojení k e-mailovému účtu.

4. Teď zadejte data, která chcete do e-mailu zahrnout. 

   1. Do pole **Komu** zadejte e-mailovou adresu příjemce. 
   Pro účely testování můžete použít svou vlastní e-mailovou adresu.

   2. Do pole **Předmět** zadejte předmět e-mailu. 
   Pro účely tohoto příkladu zadejte New RSS item (Nová položka RSS), jak je znázorněno níže:

      ![Zadání předmětu e-mailu](./media/logic-apps-create-a-logic-app/logic-app-add-subject.png)

      Když kliknete do textového pole, otevře se **seznam Přidat dynamický obsah** umožňující výběr dostupných datových polí pro zahrnutí do vaší akce. 
      Pokud se seznam dynamického obsahu neotevře, zvolte pod příslušným textovým polem **Přidat dynamický obsah**.

   3. Ze seznamu **Přidat dynamický obsah** vyberte **Název informačního kanálu**, který do e-mailu zahrne název položky.

      ![Zadání předmětu e-mailu](./media/logic-apps-create-a-logic-app/logic-app-select-field.png)

      Jakmile budete hotovi, předmět e-mailu bude vypadat jako v tomto příkladu:

      ![Přidaný název informačního kanálu](./media/logic-apps-create-a-logic-app/added-feed-title.png)

      > [!NOTE] 
      > Pokud vyberete pole, které obsahuje pole, například **categories-item**, návrhář kolem akce odkazující na toto pole automaticky přidá smyčku For each. Aplikace logiky tak může provést příslušnou akci pro každou položku pole. 
      > 
      > Pokud chcete odebrat smyčku, zvolte tři tečky (**...**) v záhlaví smyčky a pak zvolte **Odstranit**.

   4. Do pole **Text** zadejte obsah e-mailu. 
   Pro účely tohoto příkladu zadejte tento text a vyberte tato pole:

      ![Přidání obsahu e-mailu](./media/logic-apps-create-a-logic-app/logic-app-complete.png)

      | Pole | Popis | 
      | ----- | ----------- | 
      | **Název informačního kanálu** | Zobrazí název položky. | 
      | **Datum publikování informačního kanálu** | Zobrazí datum a čas publikování položky. | 
      | **Odkaz na primární informační kanál** | Zobrazí adresu URL položky. | 
      ||| 

      > [!TIP]
      > Pokud chcete do textového pole přidat prázdné řádky, stiskněte Shift + Enter. 
      
5. Uložte svou práci. Na panelu nástrojů návrháře zvolte **Uložit**.

   ![Hotová aplikace logiky](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

## <a name="run-your-logic-app-workflow"></a>Spuštění pracovního postupu aplikace logiky

Pokud chcete aplikaci logiky spustit ručně, na panelu nástrojů návrháře zvolte **Spustit**. Jinak můžete počkat na spuštění aplikace logiky podle plánu, který jste nastavili.

![Spuštění aplikace logiky](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

Pokud informační kanál RSS obsahuje nové položky, aplikace logiky za každou novou položku odešle e-mail. Například tady je ukázka outlookového e-mailu, který tato aplikace logiky odešle:

![E-mail odeslaný za novou položku informačního kanálu RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Pokud informační kanál neobsahuje nové položky, aplikace logiky přeskočí krok, který odesílá e-mail, a s další kontrolou počká na další interval. 

> [!TIP]
> Pokud neobdržíte žádné e-maily, zkontrolujte složku s nevyžádanými e-maily. Pokud si nejste jisti správným spuštěním aplikace logiky, přečtěte si téma [Řešení potíží s aplikací logiky](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulujeme, právě jste vytvořili a spustili svou první aplikaci logiky. Tento kurz Rychlý start vám ukázal, jak můžete snadno a rychle vytvářet automatizované pracovní postupy pro integraci systémů a služeb.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Aplikace logiky zůstane spuštěná a potenciálně se bude účtovat na vaše předplatné Azure, dokud ji nevypnete nebo neodstraníte. Kromě toho, pokud pro svou aplikaci logiky vytvoříte připojení, tato připojení zůstanou zachována i po odstranění aplikace logiky. 

Jakmile budete hotovi, nezapomeňte vypnout nebo odstranit všechny prostředky, za které nechcete, aby se vám účtovaly poplatky, nebo které nechcete zachovat. Pokud chcete odstranit všechny prostředky vytvořené pro účely tohoto kurzu Rychlý start, odstraňte skupinu prostředků Azure, kterou jste pro tuto aplikaci logiky vytvořili. 

### <a name="delete-resource-group"></a>Odstranění skupiny prostředků

Pokud nechcete zachovat nic, co souvisí s vaší aplikací logiky, odstraňte skupinu prostředků, kterou jste vytvořili pro účely tohoto kurzu Rychlý start, a všechny související prostředky. Další informace o správě [skupin prostředků Azure](../azure-resource-manager/resource-group-portal.md#manage-resources).

1. V nabídce Azure zvolte **Skupiny prostředků**.

2. Zvolte skupinu prostředků, kterou chcete odstranit. V nabídce skupiny prostředků zvolte **Přehled**, pokud ještě není vybraný. 

3. Zkontrolujte všechny prostředky ve skupině, kterou chcete odstranit. Jakmile budete připraveni, zvolte **Odstranit skupinu prostředků** na panelu nástrojů skupiny prostředků.

### <a name="turn-off-logic-app"></a>Vypnutí aplikace logiky

Pokud chcete aplikaci logiky zastavit, aniž byste odstranili svou práci, vypněte ji. 

V nabídce aplikace logiky zvolte **Přehled**. Na panelu nástrojů zvolte **Vypnout**.

  ![Vypnutí aplikace logiky](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Pokud se nabídka aplikace logiky nezobrazí, zkuste se vrátit na řídicí panel Azure a otevřít aplikaci logiky znovu.

### <a name="delete-logic-app"></a>Odstranění aplikace logiky

Můžete odstranit pouze aplikaci logiky, ale přitom zachovat všechny ostatní související prostředky, jako například vytvořená připojení.

1. V nabídce aplikace logiky zvolte **Přehled**. Na panelu nástrojů zvolte **Odstranit**. 

   ![Odstranění aplikace logiky](./media/logic-apps-create-a-logic-app/delete-logic-app.png)

   > [!TIP]
   > Pokud se nabídka aplikace logiky nezobrazí, zkuste se vrátit na řídicí panel Azure a otevřít aplikaci logiky znovu.

2. Potvrďte, že chcete odstranit aplikaci logiky, a pak zvolte **Odstranit**.

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy, navštivte [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Pokud chcete zanechat své nápady na funkce nebo hlasovat, navštivte [web zpětné vazby od uživatelů Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytváření pracovních postupů aplikací logiky z předem připravených šablon](../logic-apps/logic-apps-create-logic-apps-from-templates.md)