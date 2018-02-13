---
title: "Vytváření pracovních postupů pro zpracování e-mailů a příloh - Azure Logic Apps | Microsoft Docs"
description: "Tento kurz ukazuje, jak vytvořit automatizované pracovní postupy pro zpracování e-mailů a příloh s Azure Logic Apps, Azure Storage a Azure Functions"
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
ms.openlocfilehash: 210731ce2e792452650b7a92cfc542c78a0e8014
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>Zpracování e-mailů a příloh s aplikace logiky

Aplikace logiky Azure umožňuje automatizovat pracovní postupy a integrovat data mezi službami Azure, služby společnosti Microsoft, další aplikace software jako služba (SaaS) a místních systémů. Tento kurz ukazuje, jak můžete vytvořit [aplikace logiky](../logic-apps/logic-apps-overview.md) která zpracovává příchozí e-maily a všechny přílohy. Tato procesy aplikace logiky tohoto obsahu, uloží obsah do úložiště Azure a odešle oznámení kontroly tohoto obsahu. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavit [úložiště Azure](../storage/common/storage-introduction.md) a Storage Explorer pro kontrolu uložit e-mailů a příloh.
> * Vytvoření [Azure funkce](../azure-functions/functions-overview.md) , kterým se odeberou HTML z e-mailů. Tento kurz zahrnuje kód, který můžete použít pro tuto funkci.
> * Vytvoření prázdné aplikace logiky
> * Přidejte aktivační událost, která monitoruje e-mailů pro přílohy.
> * Přidejte podmínku, která kontroluje, jestli e-mailů mají přílohy.
> * Přidáte akci, která volá funkci Azure, když má přílohy e-mailu.
> * Přidáte akci, která se vytvoří úložiště objektů blob pro e-maily a přílohy.
> * Přidáte akci zasílá e-mailová oznámení.

Když jste hotovi, vypadá svou aplikaci logiky tento pracovní postup na vysoké úrovni:

![Aplikace logiky vysoké úrovně dokončení](./media/tutorial-process-email-attachments-workflow/overview.png)

Pokud ještě nemáte předplatné Azure, <a href="https://azure.microsoft.com/free/" target="_blank">zaregistrujte si bezplatný účet Azure</a> před tím, než začnete. 

## <a name="prerequisites"></a>Požadavky

* E-mailový účet od poskytovatele e-mailu podporuje Logic Apps, jako je například Office 365 Outlook, Outlook.com nebo z Gmailu. U jiných poskytovatelů [kontrolní seznam konektory zde](https://docs.microsoft.com/connectors/).

  Tato aplikace logiky používá účet Office 365 Outlook. 
  Pokud používáte jinou e-mailový účet, obecné kroky zůstaly stejné, ale uživatelské rozhraní se může objevit mírně lišit.

* Stáhněte a nainstalujte <a href="http://storageexplorer.com/" target="_blank">volné Microsoft Azure Storage Explorer</a>. Tento nástroj umožňuje zkontrolovat, zda je správně nastavena vašeho kontejneru úložiště.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k <a href="https://portal.azure.com" target="_blank">portál Azure</a> pomocí svých přihlašovacích údajů účtu Azure.

## <a name="set-up-storage-to-save-attachments"></a>Nastavení úložiště pro uložení příloh

Příchozí e-maily a přílohy můžete uložit jako objekty BLOB v [kontejneru úložiště Azure](../storage/common/storage-introduction.md). 

1. Než bude možné vytvořit kontejner úložiště [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) s těmito nastaveními:

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název** | attachmentstorageacct | Název účtu úložiště | 
   | **Model nasazení** | Správce prostředků | [Modelu nasazení](../azure-resource-manager/resource-manager-deployment-model.md) pro správu nasazení prostředků | 
   | **Typ účtu** | Obecné účely | [Typ účtu úložiště](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Výkon** | Standard | Toto nastavení určuje datové typy podporované a média pro ukládání dat. V tématu [typy účtů úložiště](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replikace** | Místně redundantní úložiště (LRS) | Toto nastavení určuje, jak se vaše data zkopíruje, uložené, spravovat a synchronizovat. V tématu [replikace](../storage/common/storage-introduction.md#replication). | 
   | **Požadované zabezpečení přenosu** | Zakázáno | Toto nastavení určuje zabezpečení potřebné pro žádosti od připojení. V tématu [vyžadují zabezpečený přenos](../storage/common/storage-require-secure-transfer.md). | 
   | **Předplatné** | <*your-Azure-subscription-name*> | Název pro vaše předplatné Azure | 
   | **Skupina prostředků** | LA-Tutorial-RG | Název [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) použít k uspořádání a správu souvisejících prostředků. <p>**Poznámka:** skupiny prostředků existuje v rámci v určité oblasti. I když se položky v tomto kurzu nemusí být k dispozici ve všech oblastech, pokuste se použít stejnou oblast, pokud je to možné. | 
   | **Umístění** | Východní USA 2 | Oblast kam se mají ukládat informace o vašem účtu úložiště | 
   | **Konfigurace virtuálních sítí** | Zakázáno | V tomto kurzu zachovat **zakázané** nastavení. | 
   |||| 

   Můžete také použít [prostředí Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) nebo [rozhraní příkazového řádku Azure](../storage/common/storage-quickstart-create-storage-account-cli.md).
  
2. Po Azure nasadí účtu úložiště, získáte přístupový klíč účtu úložiště:

   1. Ve vašem úložišti účet nabídce v části **nastavení**, zvolte **přístupové klíče**. 
   2. Najít **key1** pod **výchozí klíče** a název účtu úložiště.

      ![Zkopírujte a uložte název účtu úložiště a klíč](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Můžete také použít [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) nebo [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list). 

3. Vytvoření kontejneru úložiště e-mailových příloh.
   
   1. V nabídce vašeho účtu úložiště na **přehled** podokně vyberte **objekty BLOB** pod **služby**, zvolte **+ kontejner**.

   2. Zadejte "přílohy" jako název kontejneru. V části **veřejný přístup úroveň**, vyberte **kontejneru (anonymní přístup pro čtení pro kontejnery a objekty BLOB)**a zvolte **OK**.

   Můžete také použít [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), nebo [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create). 
   Když jste hotovi, najdete v účtu úložiště tady na portálu Azure vašeho kontejneru úložiště:

   ![Kontejner úložiště bylo dokončeno](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Storage Explorer v dalším kroku připojte k účtu úložiště.

## <a name="set-up-storage-explorer"></a>Nastavit Storage Explorer

Nyní připojte Storage Explorer k účtu úložiště tak, aby si můžete ověřit, že aplikace logiky správně uloží přílohy jako objekty BLOB v kontejneru vašeho úložiště.

1. Otevřete Průzkumníka úložiště Microsoft Azure. Pokud Storage Explorer zobrazí výzvu k připojení k úložišti Azure, zvolte **použít název účtu úložiště a klíč** > **Další**.
Pokud se zobrazí výzva, zvolte **přidejte účet** na panelu nástrojů explorer.

2. V části **připojit pomocí názvu a klíče**, zadejte název účtu úložiště a přístupový klíč, který jste předtím uložili. Zvolte **Další** > **připojit**.

3. Zkontrolujte, že váš účet úložiště a kontejneru zobrazí správně v Průzkumníku úložiště:

   1. V části **Explorer**, rozbalte položku **(místní a připojené)** > 
    **účty úložiště** > **attachmentstorageaccount** > 
    **Blob kontejnery**.

   2. Potvrďte, že kontejner "přílohy" se teď zobrazí. 
   Příklad:

      ![Storage Explorer - potvrďte kontejneru úložiště](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Dále vytvořte [Azure funkce](../azure-functions/functions-overview.md) , kterým se odeberou HTML z příchozích e-mailu.

## <a name="create-a-function-to-clean-html"></a>Vytvořit funkci pro čištění HTML

Nyní pomocí fragmentu kódu, který poskytuje tyto kroky k vytvoření Azure funkce, která odebere HTML z jednotlivých příchozích e-mailu. Tímto způsobem, obsah e-mailu je čisticí a jejich zpracování. Tuto funkci můžete pak volat z aplikace logiky.

1. Abyste mohli vytvořit funkci, [vytvořit aplikaci function app](../azure-functions/functions-create-function-app-portal.md) s těmito nastaveními:

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název aplikace** | CleanTextFunctionApp | Globálně jedinečný a popisný název pro vaši aplikaci – funkce | 
   | **Předplatné** | <*your-Azure-subscription-name*> | Stejného předplatného Azure, který dřív používal | 
   | **Skupina prostředků** | LA-Tutorial-RG | Stejnou skupinu prostředků Azure, který dřív používal | 
   | **Hostování plán** | Plán Consumption | Toto nastavení určuje, jak přidělit a škálování prostředky, například computing napájení, pro spuštění aplikace funkce. V tématu [hostování plány porovnání](../azure-functions/functions-scale.md). | 
   | **Umístění** | Východní USA 2 | Stejné oblasti, kterou jste dříve používali | 
   | **Úložiště** | cleantextfunctionstorageacct | Vytvořte účet úložiště pro vaši aplikaci funkce. Používejte pouze malá písmena a čísla. <p>**Poznámka:** tento účet úložiště obsahuje funkce aplikací a liší se od vaší dříve vytvořený účet úložiště pro přílohy e-mailu. | 
   | **Application Insights** | Vypnuto | Zapne monitorování aplikace s [Application Insights](../application-insights/app-insights-overview.md), ale pro účely tohoto kurzu ponechat **vypnout** nastavení. | 
   |||| 

   Pokud funkce aplikace, se po nasazení automaticky neotevře, najít aplikace v rámci <a href="https://portal.azure.com" target="_blank">portál Azure</a>. V hlavní nabídce Azure, zvolte **App Services**a vyberte svou aplikaci funkce.

   ![Aplikace vytvořené – funkce](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Pokud **App Services** nebude zobrazovat v nabídce Azure, přejděte na **další služby** místo. Do vyhledávacího pole, najděte a vyberte **funkce aplikace**. Další informace najdete v tématu [vytvoření funkce](../azure-functions/functions-create-first-azure-function.md).

   Můžete také použít [rozhraní příkazového řádku Azure](../azure-functions/functions-create-first-azure-function-azure-cli.md), nebo [prostředí PowerShell a správce prostředků šablony](../azure-resource-manager/resource-group-template-deploy.md).

2. V části **funkce aplikace**, rozbalte položku **CleanTextFunctionApp**a vyberte **funkce**. Na panelu nástrojů funkce zvolte **+ nové funkce**.

   ![Vytvoření nové funkce](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. V části **vyberte šablonu níže nebo přejděte na rychlé spuštění**, vyberte **HttpTrigger - C#** funkce šablony.

   ![Vyberte šablonu – funkce](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. V části **název funkce**, zadejte ```RemoveHTMLFunction```. V části **triggeru protokolu HTTP** > **oprávnění na úrovni**, ponechte výchozí **funkce** hodnotu a vyberte **vytvořit**.

   ![Pojmenujte svoji funkci.](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Když se otevře okno editoru, nahraďte kód šablony s tímto kódem, který odebere HTML a vrátí výsledky pro volající:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Až budete hotoví, zvolte **Uložit**. Chcete-li funkci otestovat, zvolte **testování** pod na šipku (**<**) ikona na pravé straně editoru. 

   ![Otevřete podokno "Test"](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. V **Test** podokně v části **text žádosti**, zadejte tento řádek a vyberte **spustit**.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Testování funkce](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   **Výstup** v okně se zobrazí tento výsledek z funkce:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Po zkontrolování, že funkce funguje, vytvořte svou aplikaci logiky. I když tento kurz ukazuje, jak vytvořit funkci, kterým se odeberou HTML z e-mailů, Logic Apps je také **HTML na Text** konektor.

## <a name="create-your-logic-app"></a>Vytvoření aplikace logiky

1. V hlavní nabídce Azure, zvolte **nový** > **Enterprise integrace** > **aplikace logiky**.

   ![Vytvoření aplikace logiky](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. V části **vytvořit aplikaci logiky**, zadejte tyto informace o aplikaci logiky jako uvedené a popsané. Až budete hotovi, zvolte **Připnout na řídicí panel** > **Vytvořit**.

   ![Zadejte informace o aplikaci logiky](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název** | LA ProcessAttachment | Název pro svou aplikaci logiky | 
   | **Předplatné** | <*your-Azure-subscription-name*> | Stejného předplatného Azure, který dřív používal | 
   | **Skupina prostředků** | LA-Tutorial-RG | Stejnou skupinu prostředků Azure, který dřív používal |
   | **Umístění** | Východní USA 2 | Stejné oblasti, kterou jste dříve používali | 
   | **Log Analytics** | Vypnuto | V tomto kurzu zachovat **vypnout** nastavení. | 
   |||| 

3. Po Azure nasadí vaší aplikace, Návrhář aplikace logiky otevře a zobrazuje stránku s video úvod a šablony pro obecné vzory aplikace logiky. V části **Šablony** zvolte **Prázdná aplikace logiky**.

   ![Výběr šablony prázdné aplikace logiky](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Dál přidejte [aktivační událost](../logic-apps/logic-apps-overview.md#logic-app-concepts) který naslouchá pro příchozí e-maily s přílohami. Všechny aplikace logiky musí začínat aktivační událost, která se stane, aktivuje se při určité události, nebo když nová data splňuje určité podmínky. Další informace najdete v tématu [vytvoření první aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Sledování příchozích e-mailu

1. V designeru zadejte do vyhledávacího pole "když e-mail přijde". Vyberte této aktivační události pro svého poskytovatele e-mailu:  **< *vaše e-mailu zprostředkovatele*> – při doručení nových e-mailů**, například:

   ![Vyberte této aktivační události pro zprostředkovatele e-mailu: "Pokud nový e-mail dorazí"](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Pro pracovní nebo školní účty Azure vyberte Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte Outlook.com. 

2. Pokud budete vyzváni k zadání pověření, přihlaste se k e-mailový účet, aby aplikace logiky můžete připojit k e-mailový účet.

3. Nyní zadejte kritéria, která má aktivační procedura používá k filtrování novou e-mailu.

   1. Zadejte složku, intervalu a četnost kontroly e-mailů.

      ![Zadejte složku, intervalu a četnost kontroly e-mailů](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Nastavení | Hodnota | Popis | 
      | ------- | ----- | ----------- | 
      | **Složka** | Doručená pošta | E-mailové složky ke kontrole | 
      | **Interval** | 1 | Počet intervalů čekat mezi kontrolami | 
      | **Frekvence** | Minuta | Jednotka času pro každý interval mezi kontroluje | 
      |  |  |  | 
  
   2. Zvolte **zobrazit rozšířené možnosti** a určete tato nastavení:

      | Nastavení | Hodnota | Popis | 
      | ------- | ----- | ----------- | 
      | **Has Attachment** | Ano | Získáte pouze e-mailů s přílohami. <p>**Poznámka:** aktivační událost nedojde k odebrání všech e-mailů z vašeho účtu, kontrola pouze nové zprávy a zpracování pouze e-maily, které odpovídají filtru subjektu. | 
      | **Zahrnout přílohy** | Ano | Získat přílohy jako vstup pro pracovní postup, nikoli pouze kontroly příloh. | 
      | **Filtr subjektu** | ```Business Analyst 2 #423501``` | Hledaný text v předmětu e-mailu | 
      |  |  |  | 

4. Skrýt podrobnosti aktivační události pro nyní, klikněte na tlačítko uvnitř záhlaví aktivační události.

   ![Sbalit tvar, který má skrýt podrobnosti](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Uložte svou aplikaci logiky. Na panelu nástrojů návrháře zvolte **Uložit**.

   Aplikace logiky je teď není za chodu ale udělat nic jiných zkontrolujte vaše e-mailů. 
   Dál přidejte podmínku, která určuje kritéria pokračujte pracovního postupu.

## <a name="check-for-attachments"></a>Kontrola příloh

1. V části aktivační událost, zvolte **+ nový krok** > **přidat podmínku**.

   Jakmile se zobrazí obrazec podmínky, ve výchozím seznamu parametry nebo seznamu dynamického obsahu se zobrazí a zobrazí všechny parametry z předchozího kroku, který můžete použít jako vstupy pracovního postupu. 
   Váš prohlížeč šířka Určuje, které se zobrazí.

2. Přejmenujte podmínku s lepší popis.

   1. V záhlaví podmínka, zvolte **výpustky** (**...** ) tlačítko > **přejmenovat**.

      Například, pokud je váš prohlížeč v úzké zobrazení:

      ![Přejmenovat podmínku](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Pokud váš prohlížeč je v široké zobrazení a bloky obsahu seznamu dynamických přístup k se třemi tečkami, zavřete seznamu tak, že zvolíte **přidávat dynamický obsah** uvnitř podmínku. 
      
      ![Zavřít dynamického obsahu seznamu](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Přejmenujte vaše podmínky se tento popis:```If email has attachments and key subject phrase```

3. Popisují podmínku zadáním výrazu. 

   1. Do vnitřní oblasti tvaru podmínky, zvolte **upravit v rozšířeném režimu**.

      ![Upravit podmínky v rozšířeném režimu](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. Do textového pole zadejte tento výraz:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      Tento výraz porovná **HasAttachment** hodnotu vlastnosti z textu aktivační událost, která je e-mailu v tomto kurzu se objekt Boolean ```True```. 
      Pokud jsou obě hodnoty rovny, e-mailu, má alespoň jednu přílohu, předává podmínka, a pokračuje v pracovním postupu.

      Vaše podmínky nyní vypadá v tomto příkladu:

      ![Výraz podmínky](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Zvolte **upravit v režimu základní**. Výraz teď řeší, jak je vidět tady:

      ![Vyřešit výraz](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Ruční vytvoření výrazu, musí pracovat v režimu základní a mít seznamu dynamických otevřít, aby mohl pracovat s Tvůrce. V části **výraz**, můžete vybrat funkce. V části **dynamický obsah**, můžete vybrat parametr pole pro použití v těchto funkcí.
      > Tento kurz později ukazuje, jak ručně výrazy se vytvářejí.

4. Uložte svou aplikaci logiky.

### <a name="test-your-condition"></a>Testování vaší podmínky

Nyní otestovat, zda správně funguje podmínku:

1. Pokud svou aplikaci logiky už neběží, zvolte **spustit** na panelu nástrojů návrháře.

   Tento krok ručně spustí aplikaci logiky aniž byste museli čekat, až předá zadaného intervalu. 
   Však nedojde, dokud zkušební e-mail dorazí v doručené poště. 

2. Pošlete sami sobě e-mailu, který splňuje tato kritéria:

   * Předmět e-mailu je text, který jste zadali v aktivační události **subjektu filtru**:```Business Analyst 2 #423501```

   * E-mailu má jednu přílohu. 
   Teď vytvořte jeden prázdný textový soubor a připojit tento soubor k e-mailu.

   Když e-mail přijde, zkontroluje aplikace logiky přílohy a text zadaný předmětu.
   Pokud podmínka úspěšně projde, aktivační událost aktivuje a způsobí, že modul Logic Apps k vytvoření instance aplikace logiky a spouštění pracovního postupu. 

3. Chcete-li zkontrolovat, jestli aktivační událost vyvolána aplikaci logiky proběhla úspěšně, v nabídce aplikace logiky, zvolte **přehled**.

   ![Zkontrolujte aktivační události a spustí historie](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Pokud svou aplikaci logiky nebyla aktivovat nebo spustit i přes úspěšné aktivační událost, najdete v části [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).

V dalším kroku definují akce trvat, než **v případě hodnoty true** firemní pobočky. Pokud chcete uložit spolu s přílohy e-mailu, odeberte všechny HTML z obsahu e-mailu a potom vytvořit objekty BLOB v kontejneru úložiště pro e-mailu a příloh.

> [!NOTE]
> Aplikace logiky nemusí provádět žádné kroky **-li pravda** větev, když e-mailu nemá přílohy. Jako bonus cvičení po dokončení tohoto kurzu můžete přidat všechny příslušné akce, kterou chcete trvat, než **když má hodnotu false** firemní pobočky.

## <a name="call-the-removehtmlfunction"></a>Volání RemoveHTMLFunction

1. V nabídce aplikace logiky, vyberte **návrhář aplikace na základě logiky**. V **v případě hodnoty true** větev, zvolte **přidat akci**.

2. Vyhledejte "azure functions" a vyberte tuto akci: **Azure Functions – zvolte Azure funkce**

   ![Vyberte akci pro "Azure Functions - vyberte Azure funkce"](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Vyberte svou aplikaci dříve vytvořenou funkce: **CleanTextFunctionApp**

   ![Vyberte svou aplikaci Azure – funkce](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Nyní vyberte funkce: **RemoveHTMLFunction**

   ![Vyberte funkce Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Přejmenujte vaší obrazce funkce pomocí tohoto popisu:```Call RemoveHTMLFunction to clean email body``` 

6. Funkce tvaru zadejte vstup pro funkce ke zpracování. Zadejte text e-mailu jako uvedené a popsané tady:

   ![Zadejte text požadavku pro tuto funkci můžete očekávat](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. V části **text žádosti**, zadejte tento text: 
   
      ```{ "emailBody": ``` 

      Až dokončíte tuto položku v dalších krocích, objeví se chyba o neplatný JSON.
      Pokud jste dříve neotestovali tuto funkci, vstup zadaný pro tuto funkci použít JavaScript Object Notation (JSON). 
      Text žádosti, takže musíte použít stejný formát příliš. 

   2. Ze seznamu parametrů nebo dynamického obsahu seznamu, vyberte **textu** pole v části **při doručení nových e-mailů**.
   Po **textu** pole, přidejte uzavírací složené závorky:```}```

      ![Zadejte text žádosti pro předávání funkce](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      V definici aplikace logiky zobrazí se tato položka v tomto formátu:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Uložte svou aplikaci logiky.

Dál přidejte akci, která vytvoří objekt blob v kontejneru vašeho úložiště pro ukládání obsahu e-mailu.

## <a name="create-blob-for-email-body"></a>Vytvoření objektu blob pro tělo e-mailu

1. V části tvaru funkce Azure, zvolte **přidat akci**. 

2. V části **vybrat akci**, vyhledejte "blob" a vyberte tuto akci: **Azure Blob Storage – objekt blob vytvořit**

   ![Přidat akci pro vytvoření objektu blob pro tělo e-mailu](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Pokud nemáte připojení k účtu úložiště Azure, vytvořte připojení k účtu úložiště s těmito nastaveními jako uvedené a popsané v tomto poli. Až budete hotoví, zvolte **vytvořit**.

   ![Vytvoření připojení k účtu úložiště](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Název připojení** | AttachmentStorageConnection | Popisný název připojení | 
   | **Účet úložiště** | attachmentstorageacct | Název pro účet úložiště, který jste dříve vytvořili pro ukládání příloh | 
   |||| 

4. Přejmenujte **vytvořit blob** akce s popis tohoto:```Create blob for email body```

5. V **vytvořit blob** akce, zadejte tyto informace a vyberte tyto parametry pro vytvoření objektu blob jako uvedené a popisuje:

   ![Zadání informací o objektu blob pro tělo e-mailu](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Cesta ke složce** | /Attachments | Cesta a název kontejneru, který jste dříve vytvořili. Také můžete procházet a vyberte kontejner. | 
   | **Název objektu BLOB** | **Z** pole | Předat název odesílatele e-mailu jako název objektu blob. Z seznam parametrů nebo dynamického obsahu seznamu, vyberte **z** pod **při doručení nových e-mailů**. | 
   | **Obsah objektu BLOB** | **Obsahu** pole | Předejte v obsahu e-mailu bez HTML jako obsah objektu blob. Z seznam parametrů nebo dynamického obsahu seznamu, vyberte **textu** pod **volání RemoveHTMLFunction vymazání obsahu e-mailu**. |
   |||| 

6. Uložte svou aplikaci logiky. 

### <a name="check-attachment-handling"></a>Zkontrolujte zpracování příloh

Nyní otestovat, zda aplikace logiky zpracovává e-mailů způsobem, který jste zadali:

1. Pokud svou aplikaci logiky už neběží, zvolte **spustit** na panelu nástrojů návrháře.

2. Pošlete sami sobě e-mailu, který splňuje tato kritéria:

   * Předmět e-mailu je text, který jste zadali v aktivační události **subjektu filtru**:```Business Analyst 2 #423501```

   * E-mailu má alespoň jednu přílohu. 
   Teď vytvořte jeden prázdný textový soubor a připojit tento soubor k e-mailu.

   * E-mailu má nějaký zkušební obsah v textu, například: 

     ```
     Testing my logic app
     ```

   Pokud svou aplikaci logiky nebyla aktivovat nebo spustit i přes úspěšné aktivační událost, najdete v části [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).

3. Zkontrolujte, že aplikace logiky uloží e-mailu na kontejner správné úložiště. 

   1. V Průzkumníku úložiště rozbalte **(místní a připojené)** > 
    **účty úložiště** > **attachmentstorageacct (externí)** > 
    **Kontejnery objektů blob** > **přílohy**.

   2. Zkontrolujte **přílohy** kontejner pro e-mailu. 

      V tomto okamžiku zobrazí pouze e-mailu v kontejneru protože aplikaci logiky není ještě zpracovat přílohy.

      ![Storage Explorer vyhledejte uložené e-mailu](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Když jste hotovi, odstraňte e-mailu ve Storage Exploreru.

4. Volitelně můžete otestovat **-li pravda** větve, které nemá nic v tomto okamžiku můžete odeslat e-mailu, který nesplňuje kritéria.

Dál přidejte smyčku zpracovat všechny přílohy e-mailu.

## <a name="process-attachments"></a>Proces příloh

Tato aplikace logiky používá **pro každou** smyčky ke zpracování každé přílohy e-mailu.

1. V části **vytvořit objekt blob pro tělo e-mailu** utvářejí, zvolte **... Další**a vyberte tento příkaz: **přidat pro každou**

   ![Přidání smyčky "pro každou"](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Přejmenujte vaší smyčky pomocí tohoto popisu:```For each email attachment```

3. Nyní zadejte data pro smyčky ke zpracování. Klikněte na tlačítko uvnitř **vyberte výstup z předchozích kroků** pole. V seznamu parametrů nebo dynamického obsahu seznamu, vyberte možnost **přílohy**. 

   ![Vyberte "Přílohy"](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   **Přílohy** pole předá pole, které obsahuje všechny přílohy, které jsou součástí e-mailu. 
   **Pro každou** opakování akce na každou položku, který je předán pomocí pole.

4. Uložte svou aplikaci logiky.

Dál přidejte akci, která uloží každé přílohy jako objekt blob ve vaší **přílohy** kontejner úložiště.

## <a name="create-blobs-for-attachments"></a>Vytvoření objektů blob pro přílohy

1. V **pro každou** cykly, zvolte **přidat akci** , můžete zadat úlohu provést u každé nalezen přílohy.

   ![Přidat akci opakovat](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. V části **vybrat akci**, vyhledejte "blob" a pak vyberte tuto akci: **Azure Blob Storage – objekt blob vytvořit**

   ![Přidat akci pro vytvoření objektů blob](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Přejmenujte **vytvořit objekt blob 2** akce s popis tohoto:```Create blob for each email attachment```

4. V **vytvořit objekt blob pro každé přílohy e-mailu** akce, zadejte tyto informace a vyberte parametry pro vytvoření jednotlivých objektů blob jako uvedené a popisuje:

   ![Zadejte informace o objektu blob](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Nastavení | Hodnota | Popis | 
   | ------- | ----- | ----------- | 
   | **Cesta ke složce** | /Attachments | Cesta a název kontejneru jste dříve vytvořili. Můžete také vyhledejte a vyberte kontejner. | 
   | **Název objektu BLOB** | **Název** pole | Z seznam parametrů nebo dynamického obsahu seznamu, vyberte **název** předávat název přílohy pro název objektu blob. | 
   | **Obsah objektu BLOB** | **Obsahu** pole | Z seznam parametrů nebo dynamického obsahu seznamu, vyberte **obsah** předávat přílohy obsahu pro obsah objektu blob. |
   |||| 

5. Uložte svou aplikaci logiky. 

### <a name="check-attachment-handling"></a>Zkontrolujte zpracování příloh

V dalším kroku otestujte, zda aplikace logiky zpracovává přílohy způsobem, který jste zadali:

1. Pokud svou aplikaci logiky už neběží, zvolte **spustit** na panelu nástrojů návrháře.

2. Pošlete sami sobě e-mailu, který splňuje tato kritéria:

   * Předmět e-mailu je text, který jste zadali v aktivační události **subjektu filtru**:```Business Analyst 2 #423501```

   * E-mailu má alespoň dvě přílohy. 
   Teď vytvořte dva prázdné textové soubory a připojit těchto souborů k e-mailu.

   Pokud svou aplikaci logiky nebyla aktivovat nebo spustit i přes úspěšné aktivační událost, najdete v části [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).

3. Zkontrolujte, že aplikace logiky uloží e-mailu a příloh ke kontejneru správné úložiště. 

   1. V Průzkumníku úložiště rozbalte **(místní a připojené)** > 
    **účty úložiště** > **attachmentstorageacct (externí)** > 
    **Kontejnery objektů blob** > **přílohy**.

   2. Zkontrolujte **přílohy** kontejner pro e-mailu a příloh.

      ![Zkontrolujte uložené e-mailu a příloh](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Když jste hotovi, odstraňte e-mailu a příloh ve Storage Exploreru.

V dalším kroku přidáte akci tak, aby aplikace logiky odešle e-mail zkontrolovat přílohy.

## <a name="send-email-notifications"></a>Odesílání e-mailových oznámení

1. V **v případě hodnoty true** větev, v části **pro každé přílohy e-mailu** cykly, zvolte **přidat akci**. 

   ![Akce v části "pro každý" Přidání smyčky](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. V části **vybrat akci**, vyhledejte "odesílání e-mailu" a pak vyberte akci "odeslání e-mailu" pro zprostředkovatele e-mailu, který chcete. Chcete-li filtrovat seznam akcí na konkrétní službu, můžete vybrat konektor nejprve pod **konektory**.

   ![Vyberte akci "odeslání e-mailu" pro váš poskytovatel e-mailu](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Pro pracovní nebo školní účty Azure vyberte Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte Outlook.com. 

3. Pokud budete vyzváni k zadání pověření, přihlaste se k e-mailového účtu tak, aby Logic Apps vytvoří připojení k e-mailový účet.

4. Přejmenujte **e-mailovou zprávu** akce s popis tohoto:```Send email for review```

5. Zadejte informace pro tuto akci a vyberte pole, které chcete zahrnout do e-mailu jako uvedené a popsané. Pokud chcete do textového pole přidat prázdné řádky, stiskněte Shift + Enter.  

   Například, pokud pracujete s seznamu dynamického obsahu:

   ![Odesílání e-mailových oznámení](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Pokud nemůžete najít očekávané pole v seznamu, vyberte **Další informace naleznete v** vedle **při doručení nových e-mailů** v seznamu dynamického obsahu nebo na konci seznamu parametrů.

   | Nastavení | Hodnota | Poznámky | 
   | ------- | ----- | ----- | 
   | **Komu** | <*recipient-email-address*> | Pro účely testování můžete použít svou vlastní e-mailovou adresu. | 
   | **Předmět**  | ```ASAP - Review applicant for position: ```**Subjektu** | Předmět e-mailu, který chcete zahrnout. Z seznam parametrů nebo dynamického obsahu seznamu, vyberte **subjektu** pole v části **při doručení nových e-mailů**. | 
   | **Text** | ```Please review new applicant:``` <p>```Applicant name: ```**z** <p>```Application file location: ```**Cesta** <p>```Application email content: ```**Textu** | Obsah pro tělo e-mailu. Z seznam parametrů nebo dynamického obsahu seznamu vyberte tato pole: <p>-Na **z** pole v části **při doručení nových e-mailů** </br>-Na **cesta** pole v části **vytvořit objekt blob pro tělo e-mailu** </br>-Na **textu** pole v části **volání RemoveHTMLFunction vymazání obsahu e-mailu** | 
   |||| 

   Pokud jste mají vyberte pole, která obsahuje pole, jako jsou například **obsahu**, což je pole, které obsahuje přílohy, návrhář automaticky přidá smyčka "pro každou" kolem akce, která odkazuje na toto pole. 
   Aplikace logiky tak může provést příslušnou akci pro každou položku pole. 
   Chcete-li odebrat smyčky, odeberte pole pro pole, přesuňte odkazující akce mimo smyčky, vyberte na symbol tří teček (**...** ) na název smyčky na panelu a vyberte **odstranit**.
     
6. Uložte svou aplikaci logiky. 

V dalším kroku testovací aplikace logiky, která teď vypadá podobně jako tento příklad:

![Aplikace logiky dokončení](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Spusťte aplikaci logiky

1. Pošlete sami sobě e-mailu, který splňuje tato kritéria:

   * Předmět e-mailu je text, který jste zadali v aktivační události **subjektu filtru**:```Business Analyst 2 #423501```

   * E-mailu má na jeden nebo více přílohy. 
   Prázdný textový soubor lze znovu použít z předchozím testu. 
   Realističtější scénáři připojte soubor obnovit.

   * Tělo e-mailu má tento text, který můžete zkopírovat a vložit:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Spusťte aplikaci logiky. V případě úspěšného svou aplikaci logiky vám pošle e-mailu, který může vypadat v tomto příkladu:

   ![E-mailové oznámení zaslaná aplikace logiky](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Pokud neobdržíte žádné e-mailů, zkontrolujte složku nevyžádanou poštou e-mailu. 
   Filtr nevyžádanou poštou vaše e-mailu může přesměruje tyto druhy e-mailů. 
   Pokud si nejste jisti správným spuštěním aplikace logiky, přečtěte si téma [Řešení potíží s aplikací logiky](../logic-apps/logic-apps-diagnosing-failures.md).

Blahopřejeme, nyní jste vytvořili a spusťte aplikaci logiky, která automatizuje úlohy napříč různými službami Azure a volá některé vlastní kód.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte skupinu prostředků, která obsahuje aplikaci logiky a související prostředky. V hlavní nabídce Azure, přejděte na **skupiny prostředků**a vyberte skupinu prostředků pro svou aplikaci logiky. Zvolte **odstranit skupinu prostředků**. Zadejte název skupiny prostředků jako potvrzení a vyberte **odstranit**.

![Odstraňte skupinu prostředků aplikace logiky](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy, navštivte [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Pokud chcete zanechat své nápady na funkce nebo hlasovat, navštivte [web zpětné vazby od uživatelů Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili aplikaci logiky, která zpracovává a uloží přílohy e-mailu díky integraci služby Azure, například Azure Storage a Azure Functions. Nyní Další informace o jiných konektorů, které můžete použít k vytváření aplikací logiky.

> [!div class="nextstepaction"]
> [Další informace o konektory pro Logic Apps](../connectors/apis-list.md)