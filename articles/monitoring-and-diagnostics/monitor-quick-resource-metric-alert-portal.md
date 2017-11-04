---
title: "Přijímat oznámení, pokud hodnota metriky splňuje podmínku | Microsoft Docs"
description: "Průvodce rychlým zahájením pomoci uživatelům vytvoření metriky pro aplikace logiky"
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 08d63d47a99bdf9480299a74634bc0e9a09e691e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="receive-a-notification-when-a-metric-value-meets-a-condition"></a>Přijímat oznámení, pokud hodnota metriky splňuje podmínku

Azure monitorování zpřístupní metriky pro mnoho prostředků Azure. Tyto metriky nesou výkonu a stavu tyto prostředky. V mnoha případech metrika hodnoty může ukazovat na něco se problém se prostředek. Můžete vytvořit metriky výstrahy monitorování neobvyklé chování a upozornění, pokud k němu dojde. Tento postup rychlého spuštění prostřednictvím vytvoření aplikace logiky, vytvoření úlohy a vizualizace metriky pro aplikaci logiky. Pak projde vytváření výstrahu a příjem oznámení pro metriky pro prostředek aplikace logiky.

Další informace o metriky a metriky výstrah najdete v tématu [Přehled monitorování Azure metriky](./monitoring-overview-metrics.md) a [přehled výstrah monitorování Azure](./monitoring-overview-alerts.md). 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-logic-app"></a>Vytvoření aplikace logiky

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Vyhledejte a vyberte **aplikace logiky**. Vytvořit novou skupinu prostředků s názvem **myResourceGroup** použít výchozí umístění. Klikněte na tlačítko **Vytvořit**.

3. Zadejte informace o aplikaci logiky a zkontrolujte **připnout na řídicí panel** možnost. Jakmile budete hotovi, klikněte na **Vytvořit**.

    ![Zadejte základní informace o aplikaci logiky na portálu](./media/monitoring-quick-resource-metric-alert-portal/create-logic-app-portal.png)  


4. Aplikace logiky by měl připnuli na řídicí panel. Kliknutím na přejděte do aplikace logiky.

5. V panelu aplikace logiky, vyberte **návrhář aplikace logiky**

     ![Vytvořit aktivační událost opakování v Návrháři logiku aplikace v panelu portálu](./media/monitoring-quick-resource-metric-alert-portal/logic-app-designer.png)  

6. Nastavte můžete hodnoty, jak je vidět v následujícím diagramu.

    ![Konfigurace aktivační událostí aplikace logiky v panelu portálu](./media/monitoring-quick-resource-metric-alert-portal/create-logic-app-triggers.png). 

7. V návrháři, vyberte **opakování** aktivační události.

8. Nastavte interval 20 a frekvenci sekundu zajistit svou aplikaci logiky aktivaci každých 20 sekund.

9. Klikněte **nový krok** tlačítko a vyberte **přidat akci**.

10. Vyberte **HTTP** a vyberte možnost **HTTP HTTP**.

11. Nastavte **metoda** jako POST a **Uri** webovou adresu svého výběru.

12. Klikněte na **Uložit**.

## <a name="view-metrics-for-your-logic-app"></a>Zobrazit metriky pro svou aplikaci logiky

1. Klikněte **monitorování** možnost v levém navigačním podokně.

2. Vyberte **metriky** kartě, vyplňte **předplatné**, **skupiny prostředků**, **typ prostředku** a **prostředků** informace pro svou aplikaci logiky.

3. Ze seznamu metriky, vyberte **spustí spuštění**.

4. Změnit **čas rozsah** grafu pro zobrazení dat za poslední hodinu.

5. Teď byste měli vidět graf vykreslení celkový počet spuštění aplikace logiky bylo zahájeno přes poslední hodinu.

    ![Vykreslení grafu metriky pro prostředek aplikace logiky](./media/monitoring-quick-resource-metric-alert-portal/logic-app-metric-chart.png)

## <a name="create-a-metric-alert-for-your-logic-app"></a>Vytvoření metriky oznámení pro svou aplikaci logiky

1.  V horní pravé části metriky panelu klikněte **přidat metriky upozornění** tlačítko.

2. Název metriky upozornění "myLogicAppAlert" a zadejte stručný popis výstrahy.

3. Nastavte **podmínku** metriky výstrahy jako větší než, nastavte **prahová hodnota** jako "10" a nastavte **období** jako 'za posledních 5 minut.

4. Nakonec v části **email(s) další správce** zadejte e-mailovou adresu. Tato výstraha vám zajistí e-mailu v případě, že aplikace logiky má víc než 10 neúspěšných běží v období 5 minut.

    ![Konfigurace upozornění aplikace logiky v panelu portálu](./media/monitoring-quick-resource-metric-alert-portal/logic-app-metrics-alert-portal.png)

## <a name="receive-metric-alert-notifications-for-your-logic-app"></a>Zobrazí metriky oznámení výstrah pro svou aplikaci logiky
1. Ve chvíli měli byste obdržet e-mail z 'Microsoft Azure výstrah, o tom, výstrahy, aktivoval'.

2. Přejděte zpět do aplikace logiky a změnit aktivační událost opakování intervalu 1 a četnost hodinu.

3. Během několika minut měli byste obdržet e-mail z "Microsoft Azure 'informačních výstrah výstrahu"vyřešen'.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé zahájení v této kolekci stavět na tento rychlý start. Pokud máte v úmyslu pokračovat v pro práci s několik rychlé spuštění nebo se kurzy, neprovádí vyčištění prostředky vytvořené v tento rychlý start. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na portálu Azure Portal.

1. Z nabídky na levé straně na portálu Azure, klikněte na **monitorování**.

2. Vyberte **výstrahy** , najít výstrahy, které jste vytvořili v této příručce rychlý start a klikněte na něj.

3. V panelu metriky výstrah, klikněte na tlačítko **odstranit**.

4. Z nabídky na levé straně na portálu Azure, vyhledejte **aplikace logiky** a pak klikněte na **Logic apps**.

5. Na panelu, klikněte na aplikaci logiky, kterou jste vytvořili v tento rychlý start do textového pole a pak klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili postup vytvoření metriky výstrahy pro vaše prostředky. Další informace o metriky výstrahy kliknutím prostřednictvím našich přehled výstrah.

> [!div class="nextstepaction"]
> [Azure monitorování předplatné akce výstrah](./monitor-quick-audit-notify-action-in-subscription.md )
