---
title: "Sestavení na základě scheduler automatizované pracovní postupy - Azure Logic Apps | Microsoft Docs"
description: "Tento kurz ukazuje, jak vytvořit pracovní postup na základě scheduler, opakované, automatizované službou Azure Logic Apps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: deb2572de363ca5d0dec0f78f2e30ad648e9b5f8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="check-traffic-with-a-scheduler-based-logic-app"></a>Zkontrolujte pomocí aplikace logiky na základě plánovače

Aplikace logiky Azure umožňuje automatizovat pracovní postupy, které spouštět podle plánu. Tento kurz ukazuje, jak můžete vytvořit [aplikace logiky](../logic-apps/logic-apps-overview.md) s scheduler aktivační událost, která proběhne každé ráno den v týdnu a zkontroluje cesta času, včetně přenosů mezi dvěma umístí. Pokud by překročila omezení konkrétní aplikaci logiky odešle e-mailu pomocí cesta čas a čas navíc potřebné pro vaše cíle.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření prázdné aplikace logiky 
> * Přidejte aktivační událost, která funguje jako Plánovač pro svou aplikaci logiky.
> * Přidáte akci, která se získá dobu cesta pro trasu.
> * Přidáte akci, která vytvoří proměnnou, převede čas cesta z sekund až minut a uloží tuto výsledků v proměnné.
> * Přidejte podmínku, která porovná čas cesta proti zadané omezení.
> * Přidáte akci, která odešle e-mail, pokud cesta překročila limit.

Když jste hotovi, vypadá svou aplikaci logiky tento pracovní postup na vysoké úrovni:

![Aplikace logiky vysoké úrovně](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Pokud ještě nemáte předplatné Azure, <a href="https://azure.microsoft.com/free/" target="_blank">zaregistrujte si bezplatný účet Azure</a> před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

* E-mailový účet od poskytovatele e-mailu podporuje Logic Apps, jako je například Office 365 Outlook, Outlook.com nebo z Gmailu. U jiných poskytovatelů [kontrolní seznam konektory zde](https://docs.microsoft.com/connectors/). Tento rychlý start používá účet Outlook.com. Pokud používáte jinou e-mailový účet, obecné kroky zůstaly stejné, ale uživatelské rozhraní se může objevit mírně lišit.

* Chcete-li získat čas cesta pro trasu, musíte přístupový klíč pro rozhraní API map Bing. Chcete-li získat tento klíč, postupujte podle kroků pro <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">jak získat klíč mapy Bing</a>. 

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k <a href="https://portal.azure.com" target="_blank">portál Azure</a> pomocí svých přihlašovacích údajů účtu Azure.

## <a name="create-your-logic-app"></a>Vytvoření aplikace logiky

1. V hlavní nabídce Azure zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**.

   ![Vytvoření aplikace logiky](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. V části **vytvořit aplikaci logiky**, zadejte tyto informace o aplikaci logiky jako uvedené a popsané. Až budete hotovi, zvolte **Připnout na řídicí panel** > **Vytvořit**.

   ![Zadejte informace o aplikaci logiky](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název** | LA TravelTime | Název pro svou aplikaci logiky | 
   | **Předplatné** | <*your-Azure-subscription-name*> | Název pro vaše předplatné Azure | 
   | **Skupina prostředků** | LA-TravelTime-RG | Název [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) sloužící k organizování související informační zdroje | 
   | **Umístění** | Východní USA 2 | Oblast kam se mají ukládat informace o aplikaci logiky | 
   | **Log Analytics** | Vypnuto | Zachovat **vypnout** nastavení pro protokolování diagnostiky. | 
   |||| 

3. Po Azure nasadí vaší aplikace, Návrhář aplikace logiky otevře a zobrazuje stránku s video úvod a šablony pro obecné vzory aplikace logiky. V části **Šablony** zvolte **Prázdná aplikace logiky**.

   ![Výběr šablony prázdné aplikace logiky](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Dál přidejte opakování [aktivační událost](../logic-apps/logic-apps-overview.md#logic-app-concepts), která aktivuje podle zadaného plánu. Všechny aplikace logiky musí začínat aktivační událost, která se stane, aktivuje se při určité události, nebo když nová data splňuje určité podmínky. Další informace najdete v tématu [vytvoření první aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Přidat aktivační událost plánovače

1. V designeru zadejte do vyhledávacího pole "recurrence". Vyberte této aktivační události: **plán - opakování**

   ![Najít a přidejte aktivační události "Plán-Recurrence"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. Na **opakování** utvářejí, vyberte **výpustky** (**...** ) tlačítko a zvolte **přejmenovat**. Přejmenování aktivační události s popis tohoto:```Check travel time every weekday morning```

   ![Přejmenování aktivační události](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Uvnitř aktivační událost, zvolte **zobrazit rozšířené možnosti**.

4. Zadejte podrobnosti plánu a opakování jako uvedené a popisuje aktivační události:

   ![Zadejte podrobnosti o plánu a opakování](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Interval** | 1 | Počet intervalů čekat mezi kontrolami | 
   | **Frekvence** | Týden | Jednotka času pro opakování | 
   | **Časové pásmo** | Žádné | Platí jenom v případě, že zadáte čas spuštění. Užitečné pro určení místní časové pásmo. | 
   | **Čas spuštění** | Žádné | Zpoždění opakování až v určité datum a čas. Další informace najdete v tématu [plánování úloh a pracovní postupy, které pravidelně spustit](../connectors/connectors-native-recurrence.md). | 
   | **V těchto dnech** | Pondělí, úterý, středu, čtvrtek, pátek | K dispozici pouze tehdy, když **frekvence** je nastaven na "Týden" | 
   | **V těchto hodinách** | 7,8,9 | K dispozici pouze tehdy, když **frekvence** je nastaven na "Týden" nebo "Dne". Vyberte hodiny dne ke spuštění této opakování. V tomto příkladu se spouští v 7, 8 a označí 9 hodin. | 
   | **V těchto minut** | 0,15,30,45 | K dispozici pouze tehdy, když **frekvence** je nastaven na "Týden" nebo "Dne". Vyberte dobu, po které den ke spuštění této opakování. Tento příklad spouští každých 15 minut od označit hodinu nula. | 
   ||||

   Aktivuje se v této aktivační události každý den v týdnu, každých 15 minut, počínaje 7:00:00 a končí v 9:45:00. 
   **Preview** pole ukazuje plán opakování. 
   Další informace najdete v tématu [plánování úloh a pracovní postupy](../connectors/connectors-native-recurrence.md) a [akce pracovního postupu a aktivační události](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Skrýt podrobnosti aktivační události pro nyní, klikněte na tlačítko uvnitř tvaru záhlaví.

   ![Sbalit tvar, který má skrýt podrobnosti](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Uložte svou aplikaci logiky. Na panelu nástrojů návrháře zvolte **Uložit**. 

Aplikace logiky je teď za provozu, ale nic se neděje jiných opakování. Ano přidáte akci, která odpovědí, pokud aktivuje aktivační událost.

## <a name="get-the-travel-time-for-a-route"></a>Získat čas cesta pro trasu

Teď, když máte aktivační událost, přidejte [akce](../logic-apps/logic-apps-overview.md#logic-app-concepts) , který získá cesta čas mezi dvěma místy. Služba Logic Apps poskytuje konektor pro rozhraní API map Bing, takže můžete snadno získat tyto informace. Než začnete tuto úlohu, ujistěte se, máte klíč rozhraní API map Bing, jak je popsáno v tomto kurzu požadavky.

1. V návrháři aplikace logiky, v části aktivační událost, zvolte **+ nový krok** > **přidat akci**.

2. Vyhledejte "mapy" a vyberte tuto akci: **Bing Maps - Get trasy**

3. Pokud nemáte připojení mapy Bing, budete vyzváni k vytvoření připojení. Zadejte podrobnosti o těchto připojení a zvolte **vytvořit**.

   ![Vyberte "Mapy Bing - Get trasy" akce](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Nastavení | Hodnota | Popis |
   | ------- | ----- | ----------- |
   | **Název připojení** | BingMapsConnection | Zadejte název pro připojení. | 
   | **Klíč rozhraní API** | <*your-Bing-Maps-key*> | Zadejte klíč mapy Bing, který jste dříve dostali. Pokud nemáte k dispozici klíč služby Bing Maps, přečtěte si <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">jak získat klíč</a>. | 
   | | | |  

4. Akce s Tento popis přejmenujte:```Get route and travel time with traffic```

5. Zadejte podrobnosti **Get trasy** akce, jak je znázorněno a tu popsané, například:

   ![Zadejte informace pro "Mapy Bing - Get trasy" akce](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Nastavení | Hodnota | Popis |
   | ------- | ----- | ----------- |
   | **Waypoint 1** | <*start-location*> | Vaše trasy počátek | 
   | **Waypoint 2** | <*end – umístění*> | Cílový vaší trasy | 
   | **Vyhněte se** | Žádné | Všechny položky, aby se zabránilo na trase, jako je například dálnice, mýtné atd. | 
   | **Optimize** | timeWithTraffic | Parametr, který se optimalizovat směrování, jako je například vzdálenost, cestují časem aktuálních přenosů a tak dále. Vyberte tento parametr: "timeWithTraffic" | 
   | **Vzdálenost jednotky** | <*your-preference*> | Na jednotku vzdálenosti pro trasu. Tento článek používá tato jednotka: "Míle"  | 
   | **Cesta režimu** | Řídí | Cesta režimu pro trasu. Vyberte tento režim: "Řídí" | 
   | **Datum a čas přenosu** | Žádný | Platí pro pouze v režimu přenosu | 
   | **Typ přenosu typu datum** | Žádný | Platí pro pouze v režimu přenosu | 
   |||| 

   Další informace o těchto parametrů najdete v tématu [vypočítat trasu](https://msdn.microsoft.com/library/ff701717.aspx).

6. Uložte svou aplikaci logiky.

Dále vytvořte proměnné tak, aby můžete převést a uložit aktuální čas cesta jako minut, nikoli sekund. Tímto způsobem můžete vyhnout opakování převod a snadněji používat hodnotu v dalších krocích. 

## <a name="create-variable-to-store-travel-time"></a>Vytvořte proměnnou pro uložení doba

V některých případech můžete chtít provést operace s daty v pracovním postupu a používání výsledky v novější akce. Tyto výsledky uložit, abyste mohli snadno opakovaně používat nebo odkazujte na ně, můžete vytvořit proměnné k uložení výsledků po jejich zpracování. Proměnné můžete vytvořit pouze na nejvyšší úrovni v aplikaci logiky.

Ve výchozím nastavení předchozí **Get trasy** akce vrátí aktuální čas cesta s přenosy dat v sekundách prostřednictvím **cestují provozu doba trvání** pole. Převádění a místo ukládání tuto hodnotu jako minut, můžete snadněji hodnota znovu bez převodu znovu.

1. V části **Get trasy** akce, zvolte **+ nový krok** > **přidat akci**.

2. Vyhledejte "proměnné" a vyberte tuto akci: **proměnné - inicializovat proměnné**

   ![Vyberte "Proměnné - inicializovat proměnná" akce](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Tato akce se tento popis přejmenujte:```Create variable to store travel time```

4. Zadejte podrobnosti pro vaše proměnná podle postupu popsaného tady:

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název** | travelTime | Název pro vaše proměnná | 
   | **Typ** | Integer | Datový typ pro vaše proměnná | 
   | **Hodnota** | Výraz, který převádí aktuální čas cesta z sekund až minut (viz Postup v této tabulce). | Počáteční hodnota pro vaše proměnná | 
   |||| 

   1. Chcete-li vytvořit ve výrazu **hodnotu** pole, klikněte na tlačítko uvnitř pole tak, aby se zobrazí v seznamu dynamického obsahu. 
   V případě potřeby rozšíříte prohlížeč, dokud se zobrazí v seznamu. 
   V seznamu dynamického obsahu, vyberte **výraz**. 

      ![Zadejte informace pro "Proměnné - inicializovat proměnná" akce](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Po kliknutí na tlačítko uvnitř některé textových polí, buď dynamického obsahu seznamu, nebo parametr seznam vložených se zobrazí. Tento seznam obsahuje všechny parametry z předchozí akce, které můžete použít jako vstupy do svého pracovního postupu. 
      Dynamické obsahu seznam obsahuje výraz editoru, kde můžete vybrat funkce pro provádění operací. 
      Tento výraz editor se zobrazí jenom v seznamu dynamického obsahu.

      Váš prohlížeč šířka Určuje, které se zobrazí. 
      Pokud váš prohlížeč je široké, zobrazí se seznamu dynamického obsahu. 
      Pokud váš prohlížeč je omezený, zobrazí se seznam parametrů vložené pod textové pole, která má právě fokus.

   2. V editoru výrazu zadejte tento výraz:```div(,60)```

      ![Zadejte tento výraz: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. Umístěte kurzor uvnitř výrazu mezi levé závorky (**(**) a čárkou (**,**). 
   Zvolte **dynamický obsah**.

      ![Umístěte kurzor, zvolte "Dynamický obsah"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Vyberte v seznamu dynamického obsahu **cesta doba provozu**.

      ![Vyberte pole "Cesta doba provozu"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. Po pole řeší uvnitř výrazu, vyberte **OK**.

      ![Klepněte na tlačítko "OK"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      **Hodnotu** pole se teď zobrazí jak je vidět tady:

      ![Pole "Value" s vyřešit výraz](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Uložte svou aplikaci logiky.

Dál přidejte podmínku, která zkontroluje, zda je větší než omezení konkrétní aktuální čas cesta.

## <a name="compare-travel-time-with-limit"></a>Porovnat čas cesta limit

1. V části předchozí akce, zvolte **+ nový krok** > **přidat podmínku**. 

2. Přejmenujte podmínky se tento popis:```If travel time exceeds limit```

3. Sestavení podmínku, která kontroluje, zda **travelTime** překračuje zadaném limitu jako popsané a zobrazené tady:

   1. V podmínce, klikněte na tlačítko uvnitř **zvolte hodnotu** pole, která se nachází na levé straně (zobrazení široké prohlížeče) nebo v horní (zobrazení úzké prohlížeče).

   2. Ze seznamu dynamického obsahu nebo v seznamu parametrů, vyberte **travelTime** pole v části **proměnné**.

   3. V dialogovém okně porovnání vyberte tento operátor: **je větší než**

   4. V **zvolte hodnotu** pole v pravé (wide zobrazení) nebo dolní (úzké zobrazení), zadejte toto omezení:```15```

   Pokud pracujete v úzké zobrazení, zde je například tom, jak sestavit tuto podmínku:

   ![Vytvořit podmínky v úzké zobrazení](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Uložte svou aplikaci logiky.

Dále přidejte akce se provede při cesta doba překračuje limit.

## <a name="send-email-when-limit-exceeded"></a>Odeslat e-mail, když limit překročen

Nyní přidáte akci, která odešle e-mail, pokud cesta čas překročí limit. Tento e-mail zahrnuje cesta aktuální čas a čas navíc potřeba cestují zadanou trasu. 

1. V podmínce pro **v případě hodnoty true** větev, zvolte **přidat akci**.

2. Vyhledejte "odesílání e-mailu" a vyberte konektor e-mailu a "Odeslat e-mailu akce", kterou chcete použít.

   ![Najděte a vyberte odeslat e-mailu panel.](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Osobní účty Microsoft, vyberte **Outlook.com**. 
   * Ke službě Azure pracovní nebo školní účty, vyberte **Office 365 Outlook**.

3. Pokud ještě nemáte připojení, budete vyzváni k přihlášení ke svému účtu e-mailu.

   Logic Apps vytvoří připojení k e-mailovému účtu.

4. Akce s Tento popis přejmenujte:```Send email with travel time```

5. Do pole **Komu** zadejte e-mailovou adresu příjemce. Pro účely testování pomocí e-mailovou adresu.

6. V **subjektu** pole, zadejte předmět e-mailu a zahrnout **travelTime** proměnné.

   1. Zadejte text ```Current travel time (minutes): ``` s koncovou mezeru. 
   
   2. V seznamu parametrů nebo dynamického obsahu seznamu, vyberte možnost **travelTime** pod **proměnné**. 
   
      Například, pokud je váš prohlížeč v úzké zobrazení:

      ![Zadejte předmět, text a výraz, který vrací čas cesta](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. V **textu** zadejte obsah pro tělo e-mailu. 

   1. Zadejte text ```Add extra travel time (minutes): ``` s koncovou mezeru. 
   
   2. V případě potřeby rozšíříte prohlížeč, dokud se nezobrazí seznamu dynamického obsahu. 
   V seznamu dynamického obsahu, vyberte **výraz**.

      ![Sestavit výraz pro tělo e-mailu](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. V editoru výrazu zadejte tento výraz tak, aby můžete vypočítat počet minut, které překračují limit:```sub(,15)```

      ![Zadejte výraz k výpočtu vratek minut navíc cestují čas](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. Umístěte kurzor uvnitř výrazu mezi levé závorky (**(**) a čárkou (**,**). Zvolte **dynamický obsah**.

      ![Pokračujte ve vytváření výraz k výpočtu vratek minut navíc cestují čas](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. V části **proměnné**, vyberte **travelTime**.

      ![Vyberte pole "travelTime" pro použití ve výrazu](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. Po pole řeší uvnitř výrazu, vyberte **OK**.

      ![Pole "Body" s vyřešit výraz](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      **Textu** pole se teď zobrazí jak je vidět tady:

      ![Pole "Body" s vyřešit výraz](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Uložte svou aplikaci logiky.

V dalším kroku testovací aplikace logiky, která teď vypadá podobně jako tento příklad:

![Aplikace logiky dokončení](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Spusťte aplikaci logiky

Pokud chcete aplikaci logiky spustit ručně, na panelu nástrojů návrháře zvolte **Spustit**. Pokud aktuální cestují čas zůstane v rámci svého limitu, aplikace logiky nemá žádné jiné a čeká další interval před opětovnou kontrolu.
Ale pokud aktuální čas cesta překračuje limit, dostanete e-mail s aktuálním časem cesta a počet minut výše svého limitu. Tady je příklad e-mail s odešle svou aplikaci logiky:

![E-mailu odeslaného s časem cesta](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

Pokud neobdržíte žádné e-mailů, zkontrolujte složku nevyžádanou poštou e-mailu. Filtr nevyžádanou poštou vaše e-mailu může přesměruje tyto druhy e-mailů. Pokud si nejste jisti správným spuštěním aplikace logiky, přečtěte si téma [Řešení potíží s aplikací logiky](../logic-apps/logic-apps-diagnosing-failures.md).

Blahopřejeme, nyní jste vytvořili a spuštění aplikace na základě plánu opakovaného logiky. 

Chcete-li vytvořit další logiku aplikace, které používají **plán - opakování** aktivovat, podívejte se na tyto šablony, které je k dispozici po jste vytvoření aplikace logiky:

* Získáte zaslán denní připomenutí.
* Odstraňte starší objektů BLOB Azure.
* Přidání zprávy do fronty Azure Storage.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte skupinu prostředků, která obsahuje aplikaci logiky a související prostředky. V hlavní nabídce Azure, přejděte na **skupiny prostředků**a vyberte skupinu prostředků pro svou aplikaci logiky. Zvolte **odstranit skupinu prostředků**. Zadejte název skupiny prostředků jako potvrzení a vyberte **odstranit**.

!["Přehled" > "Odstranit skupinu prostředků"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy, navštivte [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Pokud chcete zanechat své nápady na funkce nebo hlasovat, navštivte [web zpětné vazby od uživatelů Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili aplikaci logiky, která kontroluje přenosy podle zadaného plánu (na ráno den v týdnu), a akce trvá (zasílá e-mailu) Pokud cesta čas překročí zadané omezení. Nyní Naučte se vytvářet aplikaci logiky, která odešle seznamu adresátů žádosti o schválení díky integraci služby Azure, služby společnosti Microsoft a jiných aplikací SaaS.

> [!div class="nextstepaction"]
> [Spravovat požadavky na seznamu adresátů](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)