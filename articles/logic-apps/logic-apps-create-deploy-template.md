---
title: "Vytvoření nasazení šablon pro Azure Logic Apps | Microsoft Docs"
description: "Vytváření šablon Azure Resource Manageru pro logic apps nasazení a release management"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9cfbb294010d48deaf4b4c78c6a6bcd59a387d87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>Vytvoření šablon pro logic apps nasazení a release management

Po vytvoření aplikace logiky můžete chtít vytvořit jako šablonu Azure Resource Manager.
Tímto způsobem můžete snadno nasadit aplikaci logiky žádné prostředí nebo skupinu prostředků, kde je může být nutné.
Další informace o šablonách Resource Manager najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) a [nasazení prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Šablona nasazení aplikace logiky

Aplikace logiky má tři základní součásti:

* **Prostředek aplikace logiky**: obsahuje informace o třeba ceny plán, umístění a definice pracovního postupu.
* **Definice pracovního postupu**: popisuje kroky svou aplikaci logiky pracovního postupu a jak modul Logic Apps by měla spustit pracovní postup.
V aplikaci logiky můžete zobrazit tuto definici **zobrazení kódu** okno.
V prostředků aplikace logiky, můžete najít v této definici `definition` vlastnost.
* **Připojení**: odkazuje na jednotlivé prostředky, které bezpečně uložit metadata o všechna připojení konektoru, jako je například připojovací řetězec a přístupový token.
V prostředků aplikace logiky, aplikaci logiky odkazuje na tyto prostředky v `parameters` oddílu.

Všechny tyto údaje existující aplikace logiky můžete zobrazit pomocí některého nástroje, například [Průzkumníka prostředků Azure](http://resources.azure.com).

Chcete-li šablonu pro použití s nasazením skupin prostředků aplikace logiky, musíte definovat, které prostředky a Parametrizace podle potřeby.
Například Pokud nasazujete pro vývoj, testovací a produkční prostředí, budete pravděpodobně chtít v každém prostředí, použijte jiný připojovací řetězce k databázi SQL.
Nebo můžete chtít nasadit v různých předplatných nebo skupiny prostředků.  

## <a name="create-a-logic-app-deployment-template"></a>Vytvoření šablony nasazení aplikace logiky

Nejjednodušší způsob, jak mají šablony nasazení platný logiku aplikace je použití [Visual Studio Tools for Logic Apps](logic-apps-deploy-from-vs.md).
Nástroje sady Visual Studio generovat platné nasazení šablonu, která lze použít v rámci žádné předplatné nebo umístění.

Několik dalších nástrojů vám může pomoci při vytváření šablony nasazení aplikace logiky.
Můžete vytvořit ručně, který je pomocí prostředky již tady popisovaných vytvořit parametry, podle potřeby.
Další možností je použít [creator šablona aplikace logiky](https://github.com/jeffhollan/LogicAppTemplateCreator) modul prostředí PowerShell. Tento modul open-source nejprve vyhodnotí aplikaci logiky a všechna připojení, používá a poté generuje prostředků šablony s potřebné parametry pro nasazení.
Například pokud máte aplikace logiky, která přijímá zprávy z fronty služby Azure Service Bus a přidá data do Azure SQL database, nástroj zachovává veškerou logiku orchestration a parameterizes připojovací řetězce SQL a sběrnice, takže lze nastavit v nasazení.

> [!NOTE]
> Připojení musí být ve stejné skupině prostředků jako aplikaci logiky.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Instalace modulu PowerShell šablona aplikace logiky
Nejjednodušší způsob, jak nainstalovat modul je prostřednictvím [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), pomocí příkazu `Install-Module -Name LogicAppTemplate`.  

Také můžete nainstalovat modul prostředí PowerShell ručně:

1. Stáhněte si nejnovější verzi [creator šablona aplikace logiky](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Rozbalte složku ve složce modulu PowerShell (obvykle `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Pro modul pro práci s všechny přístupy klienta a předplatné tokenu, doporučujeme použít je s [ARMClient](https://github.com/projectkudu/ARMClient) nástroj příkazového řádku.  To [příspěvku na blogu](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) popisuje ARMClient podrobněji.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generovat šablonu aplikace logiky pomocí prostředí PowerShell
Po instalaci prostředí PowerShell můžete vygenerovat šablonu pomocí následujícího příkazu:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`je ID předplatného Azure. Tento řádek nejprve získá přístupový token prostřednictvím ARMClient, potom prostřednictvím kanálu předá přes do skriptu prostředí PowerShell a potom vytvoří šablonu v souboru JSON.

## <a name="add-parameters-to-a-logic-app-template"></a>Přidání parametrů do šablony aplikace logiky
Po vytvoření šablony aplikace logiky, můžete přidat nebo upravit parametry, které může být nutné. Například pokud vaše definice obsahuje ID prostředku do Azure funkci nebo vnořený pracovní postup, který chcete nasadit v jednom nasazení, můžete do šablony přidat další zdroje informací a Parametrizace ID podle potřeby. Totéž platí i pro všechny odkazy na vlastní rozhraní API nebo Swagger koncové body, které chcete nasadit s každé skupině prostředků.

## <a name="deploy-a-logic-app-template"></a>Nasazení šablony aplikace logiky

Šablony můžete nasadit pomocí všechny nástroje, například prostředí PowerShell, REST API, [Visual Studio Team Services Release Management](#team-services)a nasazení šablony prostřednictvím portálu Azure.
Navíc k uložení hodnoty parametrů, doporučujeme, abyste vytvořili [soubor parametrů](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Zjistěte, jak [nasazení prostředků pomocí šablony Azure Resource Manageru a prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Azure Resource Manageru a webu Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>Autorizaci OAuth připojení

Po nasazení aplikace logiky funguje začátku do konce s platné parametry.
Je však stále nutné autorizovat OAuth připojení k vygenerování tokenu platný přístup.
K autorizaci OAuth připojení, otevřete aplikaci logiky v návrháři aplikace logiky a autorizaci těchto připojení. Nebo pro automatické nasazení, můžete použít skript o souhlas pro každé připojení OAuth.
Na Githubu v části je ukázkový skript [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projektu.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services Release Management

Běžný scénář pro nasazení a správě prostředí, je použít nástroj, jako je správa verzí ve Visual Studio Team Services, pomocí šablony nasazení aplikace logiky. Visual Studio Team Services zahrnuje [nasazení skupiny prostředků Azure](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) úloh můžete přidat do každé sestavení nebo verzi kanálu. Je potřeba mít [instanční objekt](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) pro autorizaci k nasazení a pak může generovat definici verze.

1. Správa verzí, vyberte **prázdný** tak, aby vytvořit prázdnou definici.

    ![Vytvořte prázdnou definici][1]

2. Vyberte všechny prostředky, které jsou potřeba pro to, pravděpodobně včetně šablona aplikace logiky, generovaný ručně nebo jako součást procesu sestavení.
3. Přidat **nasazení skupiny prostředků Azure** úloh.
4. Nakonfigurovat [instanční objekt](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)a odkazovat na šablonu a parametry šablony soubory.
5. Nadále vytváří kroky v procesu verze pro všechny prostředí, automatizovaných testů nebo schvalovatelů podle potřeby.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
