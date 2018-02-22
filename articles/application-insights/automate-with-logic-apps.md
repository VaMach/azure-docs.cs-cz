---
title: "Automatizovat procesy Azure Application Insights pomocí Logic Apps."
description: "Zjistěte, jak rychle procesy můžete automatizovat opakované přidáním konektor Application Insights pro svou aplikaci logiky."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: mbullwin
ms.openlocfilehash: e17d8076a00cab2cf608fe1a690e4a780a69d56f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Automatizovat procesy Application Insights pomocí Logic Apps

Se přistihnete opakovaně spuštění stejné dotazů na data telemetrie zkontrolujte, zda správně pracuje služby? Hledáte automatizovat tyto dotazy pro hledání trendů a anomálií, a následně vytvořit vlastní pracovní postupy je obcházet? Konektor služby Azure Application Insights (preview) pro Logic Apps je ten nejvhodnější nástroj pro tento účel.

Díky této integraci můžete automatizovat procesy množství bez nutnosti napsat jediný řádek kódu. Aplikace logiky můžete vytvořit s konektorem služby Application Insights rychle automatizovat jakýkoli proces Application Insights. 

Můžete přidat i další akce. Díky funkci Logic Apps služby Azure App Service stovky akce k dispozici. Například pomocí aplikace logiky můžete můžete automaticky odesílat e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services. Můžete také použít jednu z dostupných mnoho [šablony](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) chcete urychlit proces vytváření aplikace logiky. 

## <a name="create-a-logic-app-for-application-insights"></a>Vytvoření aplikace logiky pro službu Application Insights

V tomto kurzu zjistěte, jak vytvořit aplikaci logiky, která používá algoritmus autocluster Analytics skupiny atributů v datech pro webovou aplikaci. Tok automaticky odesílá výsledky e-mailem, pouze příklad, jak můžete použít Application Insights analýzy a Logic Apps společně. 

### <a name="step-1-create-a-logic-app"></a>Krok 1: Vytvoření aplikace logiky
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **vytvořit prostředek**, vyberte **Web + mobilní**a potom vyberte **aplikace logiky**.

    ![Nové okno aplikace logiky](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Krok 2: Vytvoření aktivační událost pro svou aplikaci logiky
1. V **návrhář aplikace na základě logiky** okno, v části **začínat běžné aktivační událost**, vyberte **opakování**.

    ![Okno Návrhář aplikace logiky](./media/automate-with-logic-apps/logicapp2.png)

2. V **frekvence** vyberte **den** a pak na **Interval** zadejte **1**.

    ![Návrhář aplikace logiky "Recurrence" okna](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Krok 3: Přidání Application Insights akce
1. Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.

2. V **vybrat akci** vyhledávací pole, typ **Azure Application Insights**.

3. V části **akce**, klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.

    !["Vyberte akci" návrhář aplikace na základě logiky – okno](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Krok 4: Připojení k prostředku Application Insights

K dokončení tohoto kroku, musíte aplikaci ID a klíč rozhraní API pro prostředek. Můžete je znovu načíst z portálu Azure, jak je znázorněno v následujícím diagramu:

![ID aplikace v portálu Azure](./media/automate-with-logic-apps/appid.png) 

Zadejte název připojení, aplikace ID a klíč rozhraní API.

![Okno připojení toku návrhář aplikace na základě logiky](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Krok 5: Zadejte typ dotazu a graf analýzy
V následujícím příkladu dotaz vybere neúspěšných požadavků v rámci poslední den a jejich koreluje s výjimkami, k nimž došlo v rámci operace. Analýza korelaci neúspěšné požadavky zobrazíte na základě identifikátoru operation_Id. Výsledky dotazu pak segmenty pomocí algoritmu autocluster. 

Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte do vašeho toku.

1. V **dotazu** pole, přidejte následující dotaz Analytics: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. V **typ grafu** vyberte **tabulky Html**.

    ![Okno Konfigurace analýzy dotazu](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Krok 6: Konfigurace aplikace logiky k odesílání e-mailu

1. Klikněte na tlačítko **nový krok**a potom vyberte **přidat akci**.

2. Do vyhledávacího pole zadejte **Office 365 Outlook**.

3. Klikněte na tlačítko **Office 365 Outlook – e-mailu**.

    ![Výběr aplikace Outlook Office 365](./media/automate-with-logic-apps/flow2b.png)

4. V **e-mailovou zprávu** okno, postupujte takto:

   a. Zadejte e-mailová adresa příjemce.

   b. Zadejte předmět e-mailu.

   c. Klikněte kamkoli do **textu** pole a pak na dynamické kontextové nabídky, které se otevře napravo, vyberte **textu**.

   d. Klikněte na tlačítko **zobrazit rozšířené možnosti**.

      ![Konfigurace aplikace Outlook Office 365](./media/automate-with-logic-apps/flow5.png)

5. V nabídce dynamického obsahu postupujte takto:

    a. Vyberte **název přílohy**.

    b. Vyberte **obsah přílohy**.
    
    c. V **je HTML** vyberte **Ano**.

      ![Konfigurační obrazovce pro Office 365 e-mailu](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Krok 7: Uložit a testování aplikace logiky
* Klikněte na tlačítko **Uložit** uložte provedené změny.

Počkejte, spusťte aplikaci logiky aktivační událost, nebo aplikaci logiky můžete spustit okamžitě výběrem **spustit**.

![Obrazovky vytvoření aplikace logiky](./media/automate-with-logic-apps/step7.png)

Při spuštění aplikace logiky, příjemce, který jste zadali v seznamu e-mailu obdrží e-mail, který vypadá takto:

![Logiku aplikace e-mailové zprávy](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Další postup

- Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).
- Další informace o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





