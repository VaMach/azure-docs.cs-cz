---
title: "Vytvářet, vytvářet a nasazovat aplikace logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Vytváření projektů sady Visual Studio, můžete navrhnout, vytvořit a nasadit Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 614e07ad9fcbe9ad3684ed977f7ffa36e727dddc
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Návrh, vytvoření a nasazení Azure Logic Apps v sadě Visual Studio

I když [portál Azure](https://portal.azure.com/) nabízí skvělý způsob, jak můžete vytvořit a spravovat Azure Logic Apps, můžete použít Visual Studio pro navrhování, sestavování a nasazení aplikace logiky. Visual Studio poskytuje bohaté nástroje, například návrháře logiku aplikace můžete vytvářet aplikace logiky, šablony nasazení a automatizace nakonfigurujte a nasaďte na jakémkoli prostředí. 

Chcete-li začít se službou Azure Logic Apps, zjistěte další [postup vytvoření první aplikace logiky na portálu Azure](quickstart-create-first-logic-app-workflow.md).

## <a name="installation-steps"></a>Postup instalace

Instalace a konfigurace nástrojů Visual Studio pro Azure Logic Apps, postupujte podle těchto kroků.

### <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) nebo Visual Studio 2015
* [Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Přístup k webovému při použití návrháře embedded

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Instalace nástrojů Visual Studio pro Azure Logic Apps

Po instalaci požadavky:

1. Otevřete sadu Visual Studio. Na **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
2. Rozbalte **Online** kategorie, můžete vyhledat online.
3. Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.
4. Chcete-li stáhnout a nainstalovat rozšíření, klikněte na tlačítko **Stáhnout**.
5. Po instalaci, restartujte Visual Studio.

> [!NOTE]
> Můžete také stáhnout [nástroje aplikace logiky Azure pro Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) a [nástroje aplikace logiky Azure pro sadu Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) přímo z Visual Studio Marketplace.

Po dokončení instalace, můžete vytvořit projekt skupiny prostředků Azure pomocí návrháře aplikace logiky.

## <a name="create-your-project"></a>Vytvoření projektu

1. Na **soubor** nabídky, přejděte na **nový**a vyberte **projektu**. Nebo pokud chcete přidat do existujícího řešení projektu, přejděte na **přidat**a vyberte **nový projekt**.

    ![Nabídka Soubor](./media/logic-apps-deploy-from-vs/filemenu.png)

2. V **nový projekt** okně Najít **cloudu**a vyberte **skupiny prostředků Azure**. Název projektu a klikněte na tlačítko **OK**.

    ![Přidat nový projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Vyberte **aplikace logiky** šablony, která vytvoří šablonu prázdné logiku aplikace nasazení budete moci použít. Vyberte šablonu a klikněte na tlačítko **OK**.

    ![Vyberte šablonu aplikace logiky](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Přidali jste nyní projektu aplikace logiky pro vaše řešení. 
    V Průzkumníku řešení by se zobrazit váš soubor nasazení.

    ![Soubor nasazení](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Vytvoření aplikace logiky pomocí návrháře aplikace logiky

Pokud máte projekt skupiny prostředků Azure, který obsahuje aplikace logiky, můžete otevřít návrhář aplikace logiky v sadě Visual Studio k vytvoření pracovního postupu. 

> [!NOTE]
> Návrháře vyžaduje připojení k Internetu dotazu konektory pro dostupné vlastnosti a data. Například pokud použijete konektor Dynamics CRM Online, návrháře dotazuje CRM instanci pro zobrazení k dispozici vlastní a výchozí vlastnosti.

1. Klikněte pravým tlačítkem na vaše `<template>.json` soubor a vyberte **otevřete pomocí návrháře aplikace logiky**. (`Ctrl+L`)

2. Zvolte si předplatné, skupinu prostředků a umístění pro šablonu nasazení.

    > [!NOTE]
    > Návrh aplikace logiky vytvoří připojení k rozhraní API prostředky tento dotaz na vlastnosti při návrhu. Visual Studio používá k vytvoření tato připojení při návrhu vaší vybranou skupinu prostředků. Zobrazení nebo změna všech připojení k rozhraní API, přejděte na portál Azure a vyhledejte **rozhraní API připojení**.

    ![Výběr předplatného.](./media/logic-apps-deploy-from-vs/designer_picker.png)

    Používá definici v návrháři `<template>.json` soubor pro vykreslování.

4. Vytváření a návrhu aplikace logiky. Šablona nasazení aktualizována s vašimi změnami.

    ![Návrhář aplikace logiky v sadě Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Visual Studio. přidá `Microsoft.Web/connections` prostředky do souboru prostředků pro všechna připojení aplikace logiky musí pracovat. Tyto vlastnosti připojení lze nastavit při nasazení a spravovat poté, co nasadíte v **rozhraní API připojení** na portálu Azure.

### <a name="switch-to-json-code-view"></a>Umožňuje přepnout do zobrazení kódu JSON

Chcete-li zobrazit reprezentaci JSON pro svou aplikaci logiky, vyberte **zobrazení kódu** karta v dolní části návrháře.

Chcete-li přepnout zpět na úplné prostředků JSON, klikněte pravým tlačítkem `<template>.json` soubor a vyberte **otevřete**.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Přidání odkazů pro závislé prostředky pro nasazení šablony sady Visual Studio

Pokud chcete svou aplikaci logiky odkazovat na závislé prostředky, můžete použít [funkce šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) v šabloně nasazení aplikace logiky. Například můžete svou aplikaci logiky, chcete-li funkce Azure nebo integrace účtu, který chcete nasadit souběžně s svou aplikaci logiky. Postupujte podle těchto pokynů o tom, jak používat parametry v šabloně nasazení tak, aby návrháře aplikace logiky vykreslí správně. 

Používáte logiku aplikace parametry v těchto druhů triggery a akce:

*   Podřízené pracovní postup
*   Function app
*   APIM volání
*   Adresa URL rozhraní API připojení modulu runtime
*   Cesta připojení rozhraní API

A můžete použít šablonu funkce jako je například parametry, proměnné, resourceId, concat atd. Můžete zde je ukázka, jak můžete nahradit ID prostředku Azure funkce:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

A kde byste použili parametry:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Další příklad můžete parametrizovat operaci odeslání zprávy služby Service Bus:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl je volitelné a lze odebrat ze šablony, pokud je k dispozici.
> 


> [!NOTE] 
> Pro návrháře logiku aplikace pro práci při použití parametrů je nutné zadat výchozí hodnoty, například:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Uložení aplikace logiky

Chcete-li uložit svou aplikaci logiky na kdykoli, přejděte na **soubor** > **Uložit**. (`Ctrl+S`) 

Pokud svou aplikaci logiky má všechny chyby při ukládání aplikace, zobrazí se v sadě Visual Studio **výstupy** okno.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Nasazení aplikace logiky ze sady Visual Studio

Po dokončení konfigurace aplikace, můžete nasadit přímo ze sady Visual Studio v několika krocích. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a přejděte na **nasadit** > **nové nasazení...**

    ![Nové nasazení](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Když se zobrazí výzva, přihlaste se k předplatnému Azure. 

3. Nyní je třeba vybrat podrobnosti o skupině prostředků, ve které chcete nasadit aplikaci logiky. Až budete hotoví, vyberte **nasadit**.

    > [!NOTE]
    > Ujistěte se, že jste vybrali správný soubor šablony a parametry pro skupinu prostředků. Například pokud chcete nasadit do produkčního prostředí, vyberte soubor produkční parametry.

    ![Nasazení do skupiny prostředků](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Stav nasazení se zobrazí v **výstup** okno. 
    Možná budete muset vybrat **Azure zřizování** v **zobrazit výstup z** seznamu.

    ![Výstup stavu nasazení](./media/logic-apps-deploy-from-vs/output.png)

V budoucnu můžete upravit aplikaci logiky ve správě zdrojového kódu a použít k nasazení nové verze sady Visual Studio.

> [!NOTE]
> Pokud změníte definici na portálu Azure přímo, tyto změny se přepíší při nasazení ze sady Visual Studio příště. 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Přidat do existujícího projektu skupiny prostředků aplikace logiky

Pokud máte stávající projekt skupiny prostředků, můžete přidat aplikaci logiky do tohoto projektu v okně osnovy JSON. Můžete také přidat další aplikace logiky spolu s aplikaci, kterou jste vytvořili.

1. Otevřete soubor `<template>.json`.

2. Chcete-li otevřít okno osnovou JSON, přejděte na **zobrazení** > **ostatní okna** > **osnovy JSON**.

3. Chcete-li přidat prostředek k souboru šablony, klikněte na tlačítko **přidat prostředek** v horní části okna osnovy JSON. Nebo v okně osnovou JSON, klikněte pravým tlačítkem na **prostředky**a vyberte **přidat nový prostředek**.

    ![Osnova JSON – okno](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. V **přidat prostředek** dialogové okno, vyhledejte a vyberte **aplikace logiky**. Název aplikace logiky a vyberte **přidat**.

    ![Přidat prostředek](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Další kroky

* [Správa aplikací logiky v Průzkumníku cloudu Visual Studio](logic-apps-manage-from-vs.md)
* [Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).
* [Informace o automatizaci obchodních procesů službou Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Zjistěte, jak integrovat systémy s Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
