---
title: "Správa aplikací logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Správa aplikace logiky a dalších prostředků Azure pomocí Průzkumníka cloudové služby Visual Studio"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 34ce65ec272c9fe5be8f95371179de092c5df2f3
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Správa aplikace logiky s cloudu Průzkumníka Visual Studio

I když [portál Azure](https://portal.azure.com/) nabízí skvělý způsob, jak můžete navrhnout a spravovat Azure Logic Apps, můžete použít Průzkumníka cloudové služby Visual Studio pro správu mnoho prostředků Azure, včetně aplikace logiky. Průzkumník cloudu Visual Studio umožňuje procházet, spravovat, upravit, a stažení publikované aplikace logiky. Úlohy správy zahrnují povolit, zakázat a spustit zobrazení historie. 

Než budete moct přístup a Správa aplikace logiky v sadě Visual Studio, nainstalujte a nakonfigurujte tyto nástroje Visual Studio pro Azure Logic Apps. 

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2015 nebo Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)
* [Visual Studio Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Přístup k webovému při použití návrháře embedded

## <a name="install-visual-studio-tools-for-logic-apps"></a>Instalace nástrojů Visual Studio pro Logic Apps

Po instalaci požadavky, stáhněte a nainstalujte nástroje aplikace logiky Azure pro sadu Visual Studio.

1. Otevřete sadu Visual Studio. Na **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
2. Rozbalte **Online** kategorie, můžete hledat online v Galerii Visual Studia.
3. Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.
4. Chcete-li stáhnout a nainstalovat rozšíření, klikněte na tlačítko **Stáhnout**.
5. Po instalaci, restartujte Visual Studio.

> [!NOTE]
> Pokud chcete stáhnout nástroje aplikace logiky Azure pro sadu Visual Studio přímo, přejděte na [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Procházením vyhledejte aplikace logiky v Průzkumníku cloudu

1.  Chcete otevřít v Průzkumníku cloudu **zobrazení** nabídce zvolte **Průzkumník cloudu**.
2.  Procházením vyhledejte aplikaci logiky, skupinu prostředků nebo typ prostředku. 

    * Pokud podle typů prostředků, vyberte předplatné Azure, rozbalte **Logic Apps** a vyberte svou aplikaci logiky. 
    * Pokud ve skupině prostředků, rozbalte skupinu prostředků, která má svou aplikaci logiky a vyberte svou aplikaci logiky.

    Pokud chcete zobrazit příkazy pro svou aplikaci logiky, klikněte pravým tlačítkem na svou aplikaci logiky buď, nebo v dolní části Průzkumník cloudu, vyberte z **akce** nabídky.

    ![Procházením vyhledejte aplikaci logiky](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Upravit aplikaci logiky pomocí návrháře aplikace logiky

V Průzkumníku cloudu můžete otevřít aplikace logiky aktuálně nasazená v Návrháři stejné, které budete používat v portálu Azure. 

* Chcete-li upravit aplikaci logiky v Průzkumníku cloudu, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **otevřete pomocí editoru aplikace logiky**. 

* Chcete-li publikovat aktualizace do cloudu, zvolte **publikovat**. 

* Chcete-li spustit novou spustit, zvolte **spustit aktivační událost**.

![Návrhář pro Logic Apps](./media/logic-apps-manage-from-vs/designer.png)

Z návrháře, můžete také **Stáhnout** aplikace logiky. Tato akce automaticky parameterizes definici aplikace logiky a uloží definici jako šablonu nasazení Azure Resource Manager. Tato šablona nasazení můžete přidat do projektu skupiny prostředků Azure.

## <a name="browse-your-logic-app-run-history"></a>Procházet aplikace logiky historie spouštění

Chcete-li zobrazit historii spouštění aplikace logiky, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **historie spouštění otevřete**. Chcete-li změnit pořadí historii spuštění na základě některé vlastnosti zobrazené, vyberte na záhlaví sloupce.

![Historie běhů](media/logic-apps-manage-from-vs/runs.png)

Pokud chcete zobrazit historii spouštění instance, můžete zkontrolovat spuštění výsledky, včetně vstupy a výstupy z každého kroku, dvakrát klikněte na jednu z instancí spuštění.

![Výsledky historie spouštění, vstupy a výstupy z kroků](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Další postup

* [Vytvoření první aplikace logiky](quickstart-create-first-logic-app-workflow.md)
* [Návrh, vytvoření a nasazení aplikace logiky v sadě Visual Studio](logic-apps-deploy-from-vs.md)
* [Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).
* [Video: Automatizovat firemní procesy službou Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: Integrate systémy s Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
