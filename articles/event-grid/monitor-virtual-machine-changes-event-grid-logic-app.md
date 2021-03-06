---
title: "Sledování změn virtuálního počítače - & mřížky událostí Azure Logic Apps | Microsoft Docs"
description: "Kontrolovat změny konfigurace ve virtuálních počítačích (VM) s použitím mřížky událostí Azure a Logic Apps"
keywords: "aplikace logiky, událostí mřížky, virtuální počítač VM"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 11/30/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 3d99dabe778b9b9234db9fe130ba503cd8b57834
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Sledování změn virtuálního počítače se mřížka událostí Azure a Logic Apps

Můžete spustit automatickou [aplikace logiky pracovního postupu](../logic-apps/logic-apps-overview.md) při určité události dojde v Azure prostředků nebo jiných prostředků. Tyto prostředky můžete publikovat tyto události [Azure událostí mřížky](../event-grid/overview.md). Pak mřížky událostí nabízených oznámení události odběratelům, které mají fronty, webhooků, nebo [služby event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) jako koncové body. Jako odběratel můžete před spuštěním automatizované pracovní postupy k provádění úloh – bez psaní jakéhokoli kódu aplikace logiky počkejte tyto události z události mřížky.

Tady jsou například některé události, které vydavatelů můžete odesílat odběratelům prostřednictvím služby Azure událostí mřížky:

* Vytvořit, číst, aktualizovat nebo odstranit prostředek. Například můžete sledovat změny, které může platit poplatky na vaše předplatné Azure a vliv na vašem vyúčtování. 
* Přidat nebo odebrat osoby z předplatného Azure.
* Aplikace provede určité akce.
* Zobrazí se nové zprávy ve frontě.

V tomto kurzu vytvoří aplikace logiky, která sleduje změny k virtuálnímu počítači a odešle e-mailů o těchto změnách. Když vytvoříte aplikaci logiky s předplatným události pro prostředek služby Azure, toku událostí z prostředku prostřednictvím mřížce událostí do aplikace logiky. Tento kurz vás provede procesem vytváření této logiky aplikace:

![Přehled – monitorování virtuální počítač s událostí mřížky a logiku aplikace](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření aplikace logiky, která sleduje události z mřížky události.
> * Přidejte podmínku, která konkrétně kontroluje změny virtuálního počítače.
> * Odeslání e-mailu, když se změní virtuálního počítače.

## <a name="prerequisites"></a>Požadavky

* E-mailový účet z [všechny e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md), jako jsou například aplikace Office 365 Outlook, Outlook.com nebo z Gmailu pro odesílání oznámení. Tento kurz používá Office 365 Outlook.

* A [virtuální počítač](https://azure.microsoft.com/services/virtual-machines). Pokud jste tak již neučinili, vytvořte virtuální počítač prostřednictvím [vytvořit virtuální počítač kurzu](https://docs.microsoft.com/azure/virtual-machines/). Chcete-li virtuální počítač publikování událostí, můžete [nemusíte dělat cokoliv jiného](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Vytvoření aplikace logiky, která sleduje události z mřížky události

Nejprve vytvořte aplikaci logiky a přidejte mřížce aktivační událost, která monitoruje skupinu prostředků pro virtuální počítač. 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). 

2. V levém horním rohu hlavní Azure nabídky vyberte příkaz **vytvořit prostředek** > **Enterprise integrace** > **aplikace logiky**.

   ![Vytvoření aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Vytvoření aplikace logiky s nastavení zadané v následující tabulce:

   ![Zadání podrobností o aplikaci logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *{your-logic-app-name}* | Zadejte jedinečný název aplikace logiky. | 
   | **Předplatné** | *{your-Azure-subscription}* | V tomto kurzu vyberte stejné předplatné Azure, ke všem službám. | 
   | **Skupina prostředků** | *{your-Azure-resource-group}* | V tomto kurzu vyberte stejnou skupinu prostředků Azure pro všechny služby. | 
   | **Umístění** | *{your-Azure-region}* | Vyberte stejnou oblast pro všechny služby v tomto kurzu. | 
   | | | 

4. Až budete připraveni, vyberte **připnout na řídicí panel**a zvolte **vytvořit**.

   Právě jste vytvořili prostředek Azure pro vaši aplikaci logiky. 
   Až Azure nasadí aplikaci logiky, v Návrháři pro Logic Apps se zobrazí šablony pro obvyklé scénáře, abyste mohli začít rychleji.

   > [!NOTE] 
   > Když vyberete **připnout na řídicí panel**, aplikace logiky automaticky otevře v návrháři aplikace logiky. Jinak můžete ručně najít a otevřít aplikaci logiky.

5. Teď zvolte šablonu logiku aplikace. V části **šablony**, zvolte **prázdné aplikace logiky** , můžete vytvořit svou aplikaci logiky od začátku.

   ![Výběr šablony aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Návrhář aplikace logiky nyní ukazuje [ *konektory* ](../connectors/apis-list.md) a [ *aktivační události* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) , můžete použít ke spuštění aplikace logiky a také akce, můžete přidat aktivační událost k provádění úloh po. Aktivační událost je událost, která vytvoří instanci aplikace logiky a spustí pracovní postup aplikace logiky. 
   Aplikace logiky musí aktivační událost jako první položka.

6. Do vyhledávacího pole zadejte "události mřížky" jako filtr. Vyberte této aktivační události: **mřížky událostí Azure - na události prostředků**

   ![Vyberte této aktivační události: "Azure událostí mřížky - na události prostředků"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Po zobrazení výzvy, přihlaste se k mřížce událostí Azure pomocí svých přihlašovacích údajů Azure.

   ![Přihlaste se pomocí přihlašovacích údajů Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Pokud jste přihlášení pomocí osobního účtu Microsoft, jako například @outlook.com nebo @hotmail.com, aktivační události mřížky, se nemusí zobrazit správně. Jako alternativní řešení, zvolte [připojit pomocí objektu služby](../azure-resource-manager/resource-group-create-service-principal-portal.md), nebo jako člen skupiny Azure Active Directory, který je spojen s předplatným Azure, například ověřit *uživatelské jméno*@emailoutlook.onmicrosoft.com.

8. Teď přihlášení k odběru aplikace logiky na události vydavatele. Zadejte podrobnosti pro vaše předplatné událostí uvedených v následující tabulce:

   ![Zadejte podrobnosti pro předplatné událostí](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Předplatné** | *{virtual-machine-Azure-subscription}* | Vyberte předplatné Azure vydavatel události. V tomto kurzu vyberte předplatné Azure pro virtuální počítač. | 
   | **Typ prostředku** | Microsoft.Resources.resourceGroups | Vyberte typ prostředku vydavatel události. V tomto kurzu vyberte zadanou hodnotu, aby aplikace logiky monitoruje jenom skupiny prostředků. | 
   | **Název prostředku** | *{virtual-machine-resource-group-name}* | Vyberte název prostředku vydavatele. V tomto kurzu vyberte název skupiny prostředků pro virtuální počítač. | 
   | Volitelné nastavení, vyberte **zobrazit rozšířené možnosti**. | *{v tématu Popis}* | * **Předpony filtru**: v tomto kurzu ponechte prázdné toto nastavení. Výchozí chování odpovídá všechny hodnoty. Řetězec předpony můžete však zadat jako filtr, například cestu a parametr pro konkrétní zdroje. <p>* **Přípona filtru**: v tomto kurzu ponechte prázdné toto nastavení. Výchozí chování odpovídá všechny hodnoty. Řetězec přípony můžete však zadat jako filtr, například příponu názvu souboru, pokud chcete pouze určité typy souborů.<p>* **Název odběru**: Zadejte jedinečný název pro vaše předplatné událostí. |
   | | | 

   Když jste hotovi, vaše mřížky aktivační událost může vypadat jako tento ukázkový:
   
   ![Podrobnosti o aktivační události příklad události mřížky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Uložte svou aplikaci logiky. Na panelu nástrojů návrháře zvolte **Uložit**. Chcete-li sbalení a skrytí podrobnosti akce v aplikaci logiky, zvolte akce záhlaví.

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Při ukládání aplikace logiky s aktivační signál události mřížky, Azure automaticky vytvoří odběr událostí pro svou aplikaci logiky pro vybraný zdroj. Proto když prostředek publikuje událost k mřížce událostí, mřížky této události automaticky doručí události do aplikace logiky. Tato událost se aktivuje aplikaci logiky potom vytvoří a spustí instance pracovního postupu, který definujete v tyto další kroky.

Aplikace logiky je nyní za provozu a naslouchá na události z události mřížky, ale nic se neděje dokud nepřidáte do pracovního postupu akce. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Přidat podmínku, která kontroluje změny virtuálního počítače

Chcete-li spustit pracovní postup aplikace logiky jenom v případě, že se stane konkrétní události, přidejte podmínku, která kontroluje pro virtuální počítač "" operací zápisu. Při splnění této podmínky svou aplikaci logiky odešle že e-mailu s podrobnostmi o aktualizované virtuálního počítače.

1. V návrháři aplikace logiky, vyberte v části mřížky aktivační události, **nový krok** > **přidat podmínku**.

   ![Přidat podmínku do aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Návrhář aplikace logiky přidá podmínku prázdný do pracovního postupu, včetně cesty akce provést na základě, zda je podmínka vyhodnocena jako true nebo false.

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. V **podmínku** vyberte **upravit v rozšířeném režimu**.
Zadejte tento výraz:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Vaše podmínka teď vypadá jako v tomto příkladu:

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Tento výraz kontroluje události `body` pro `data` objekt kde `operationName` vlastnost je `Microsoft.Compute/virtualMachines/write` operace. 
   Další informace o [schématu události událostí mřížky](../event-grid/event-schema.md).

3. Pokud chcete zadat popis pro podmínku, vyberte **výpustky** (**...** ) tlačítko obrazec podmínky, a potom vyberte **přejmenovat**.

   > [!NOTE] 
   > Novější příklady v tomto kurzu také popisují kroky v pracovním postupu aplikace logiky.

4. Teď zvolte **upravit v režimu základní** tak, aby výraz automaticky vyřeší, jak je znázorněno:

   ![Stav aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Uložte svou aplikaci logiky.

## <a name="send-email-when-your-virtual-machine-changes"></a>Odeslání e-mailu, když se změní virtuálního počítače

Nyní přidejte [ *akce* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) , abyste měli e-mailu, když je zadaná podmínka vyhodnocena jako true.

1. V podmínce pro **v případě hodnoty true** vyberte **přidat akci**.

   ![Akce pro když je splněna podmínka pro přidání](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Do vyhledávacího pole zadejte "e-mailu" jako filtr. Vyhledejte a vyberte konektor odpovídající vašemu poskytovateli e-mailu. Pak pro konektor vyberte akci „odeslat e-mail“. Příklad: 

   * Pro pracovní nebo školní účet Azure vyberte konektor Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte konektor Outlook.com. 
   * Pro účty Gmail vyberte konektor Gmail. 

   My budeme pokračovat s konektorem Office 365 Outlook. 
   Pokud používáte jiného poskytovatele, postup je stejný, ale vaše uživatelské rozhraní může vypadat jinak. 

   ![Vyberte možnost odeslat e-mailu panel.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Pokud ještě nemáte připojení pro poskytovatele e-mailu, přihlaste se ke svému účtu e-mailu po zobrazení dotazu pro ověřování.

4. Zadejte podrobnosti pro e-mailu uvedeného v následující tabulce:

   ![Akce prázdný e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Pokud chcete vyberte pole, které jsou k dispozici v pracovním postupu, klikněte na tlačítko do textového pole, která **dynamický obsah** seznamu otevře, nebo zvolte **přidávat dynamický obsah**. Pro další pole, zvolte **Další informace naleznete v** pro každý oddíl v seznamu. Zavřete **dynamický obsah** vyberte **přidávat dynamický obsah**.

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Komu** | *{recipient-email-address}* |Zadejte e-mailovou adresu příjemce. Pro účely testování můžete použít svou vlastní e-mailovou adresu. | 
   | **Předmět** | Prostředek aktualizovat: **subjektu**| Zadejte obsah předmětu e-mailu. V tomto kurzu, zadejte text, návrhy a vyberte události **subjektu** pole. Zde vaše předmět e-mailu obsahuje název pro aktualizovaný prostředek (virtuálním počítači). | 
   | **Text** | Skupina prostředků: **tématu** <p>Typ události: **typ události**<p>ID události: **ID**<p>Čas: **čas události** | Zadejte obsah e-mailu. V tomto kurzu, zadejte text, návrhy a vyberte události **tématu**, **typ události**, **ID**, a **čas události** pole tak, že e-mailu obsahují název skupiny prostředků, typ události, časové razítko události a ID události pro aktualizaci. <p>Chcete-li přidat prázdné řádky v obsahu, stiskněte Shift + Enter. | 
   | | | 

   > [!NOTE] 
   > Pokud vyberete pole, které představuje pole, návrhář automaticky přidá **pro každou** smyčky kolem akce, která odkazuje na pole. Aplikace logiky tak provede příslušnou akci pro každou položku pole.

   Teď může vaše e-mailu akce vypadat jako tento ukázkový:

   ![Vyberte výstupy chcete zahrnout do e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   A aplikace logiky dokončení může vypadat jako tento příklad:

   ![Hotová aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Uložte svou aplikaci logiky. Chcete-li sbalení a skrytí podrobnosti každá akce v aplikaci logiky, zvolte akce záhlaví.

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Aplikace logiky je nyní za provozu, ale čeká změny k virtuálnímu počítači před jakékoli akce. 
   Pokud teď chcete aplikaci logiky otestovat, pokračujte k další části.

## <a name="test-your-logic-app-workflow"></a>Testování pracovního postupu aplikace logiky

1. Pokud chcete zkontrolovat, že je aplikace logiky získávání určených událostí, aktualizujte virtuální počítač. 

   Například můžete změnit velikost virtuálního počítače na portálu Azure nebo [změnit velikost virtuálního počítače v prostředí Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Po chvíli měli byste obdržet e-mailu. Příklad:

   ![E-mailu o aktualizace virtuálního počítače](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Chcete-li zkontrolovat je spuštěn a aktivuje historie pro svou aplikaci logiky, v nabídce aplikace logiky, zvolte **přehled**. Pokud chcete zobrazit další podrobnosti o spuštění, zvolte řádek příslušného spuštění.

   ![Spuštění aplikace logiky historie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Pokud chcete zobrazit vstupy a výstupy jednotlivých kroků, rozbalte krok, který chcete zkontrolovat. Tyto informace vám můžou pomoct s diagnostikou a laděním problémů ve vaší aplikaci logiky.
 
   ![Podrobnosti o historii spuštění aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Blahopřejeme, jste vytvořili a spusťte aplikaci logiky, která sleduje události prostředků prostřednictvím mřížce událostí a odešle e-mail, když tyto události dojít. Také jste zjistili, jak snadno můžete vytvořit pracovní postupy, které automatizovat procesy a integrovat systémy a cloudových služeb.

Můžete sledovat další změny v konfiguraci s událostí mřížky a aplikacích logiky, například:

* Virtuální počítač získá přístup na základě rolí práva řízení (RBAC).
* Skupina zabezpečení sítě (NSG) v síťovém rozhraní (NIC) jsou provedeny změny.
* Disky pro virtuální počítač po přidání nebo odebrání.
* Veřejná IP adresa je přiřazena k virtuálnímu počítači síťový adaptér.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Tento kurz používá prostředky a provádí akce, které platit poplatky na vaše předplatné Azure. Když jste hotovi s kurz a testování, ujistěte se, že tak zakázat nebo odstranit všechny prostředky, které nechcete platit poplatky.

* Pokud chcete aplikaci logiky zastavit, aniž byste odstranili svou práci, vypněte ji. V nabídce aplikace logiky zvolte **Přehled**. Na panelu nástrojů zvolte **Vypnout**.

  ![Vypnutí aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Pokud se nabídka aplikace logiky nezobrazí, zkuste se vrátit na řídicí panel Azure a otevřít aplikaci logiky znovu.

* Chcete-li trvale odstranit svou aplikaci logiky, v nabídce aplikace logiky, zvolte **přehled**. Na panelu nástrojů zvolte **Odstranit**. Potvrďte, že chcete odstranit aplikaci logiky, a pak zvolte **Odstranit**.

## <a name="next-steps"></a>Další postup

* [Vytvoření a trasy vlastních událostí s událostí mřížky](../event-grid/custom-event-quickstart.md)
