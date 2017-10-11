---
title: Ceny a fakturace - Azure Logic Apps | Microsoft Docs
description: "Zjistěte, jak funguje pro Azure Logic Apps ceny a fakturace."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: 63784c5e3af360b2f3f8cb330a9df8b27a85d859
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-pricing-model"></a>Cenový model Logic Apps
Azure Logic Apps umožňuje škálovat a spouštět pracovní postupy integrace v cloudu.  Následují informace o fakturaci a cenách plány pro Logic Apps.
## <a name="consumption-pricing"></a>Spotřeba – ceny
Nově vytvořený Logic Apps použití plánu spotřeby. S Logic Apps spotřeba cenový model platíte jenom se používá.  Aplikace logiky nejsou omezeny při použití plánu spotřeby.
Všechny akce provést při spuštění instance aplikace logiky jsou měření podle objemu.
### <a name="what-are-action-executions"></a>Jaké jsou spuštěních akce?
Každý krok v definici aplikace logiky je akce, která zahrnuje aktivačních událostí, tok řízení kroky jako podmínky, oborů, pro každý smyčky provést až smyčky, volání konektory a volání na nativní akce.
Aktivační události jsou zvláštní akce, které slouží k vytváření instancí novou instanci aplikace logiky, když dojde k určité události.  Existuje několik různých chování pro aktivační události, které mohou ovlivnit jak monitorované aplikaci logiky.
* **Aktivační událost dotazování** – této aktivační události průběžně dotazuje koncový bod, dokud neobdrží zprávu, která splňuje kritéria pro vytvoření instance aplikace logiky.  Interval dotazování se dá nakonfigurovat v aktivační události v Návrháři logiku aplikace.  Každý požadavek dotazování i v případě, že ji nebude vytvořit instanci aplikace logiky, počítá jako spuštění akce.
* **Aktivační události Webhooku** – této aktivační události čeká klienta k odeslání požadavku na konkrétní koncový bod.  Každý požadavek odeslaný ke koncovému bodu webhooku se počítá jako spuštění akce. Žádost a aktivační události Webhooku protokolu HTTP jsou obě aktivační události webhooku.
* **Aktivační událost opakování** – této aktivační události vytvoří instanci aplikace logiky na základě intervalu opakování nakonfigurované v aktivační události.  Opakování aktivační událost může být například nakonfigurován pro spouštění každé tři dny nebo i každou minutu.

Spuštění aktivační události se zobrazí v okně prostředků aplikace logiky v části historie aktivační události.

Všechny akce, které byly provedeny, jestli byly provedené úspěšně nebo neúspěšně se měří jako spuštění akce.  Akce, které byly přeskočeny z důvodu nebyla splněna podmínka nebo akce, které nebylo provést, protože aplikace logiky ukončeno před dokončením se počítá jako spuštění akce.

Akce provést v rámci smyčky, se počítají za iteraci smyčky.  Například jedna akce v pro každou smyčku iterace v rámci seznam 10 položek se počítají jako počet položek v seznamu (10) vynásobená počet akcí ve smyčce (1) a jeden pro zahájení smyčky, kterou v tomto příkladu by (10 * 1) + 1 = 11 spuštění akce.
Zakázané Logic Apps nemohou mít nové instance vytvořené a proto při zakázáno, není účtován.  Mějte na paměti, že po zakázání aplikace logiky může trvat nějakou dobu pro instance k uvedení před úplně zakázaná.
### <a name="integration-account-usage"></a>Používání účtů integrace
Součástí na základě spotřeby využití je [integrace účet](logic-apps-enterprise-integration-create-integration-account.md) pro zkoumání, vývoj a budete moci použít pro účely testování [B2B/EDI](logic-apps-enterprise-integration-b2b.md) a [zpracování kódu XML](logic-apps-enterprise-integration-xml.md) funkce Logic Apps bez dalších poplatků. Budete moci vytvořit maximálně jeden účet na oblast a úložiště až 10 smluv a 25 mapy. Schémata, certifikáty a partnery mít žádná omezení a můžete nahrát tolik, jako je třeba.

Kromě zahrnutí účty pro integraci s spotřebou můžete také vytvořit účty pro standardní integraci bez těchto omezení a s naše smlouva SLA standardní logiku aplikace. Další informace najdete v tématu [Azure ceny](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>Plány služby App Service
Aplikace logiky vytvořili odkazující plán služby App Service bude nadále chovat jako předtím. V závislosti na plánu vybrali jsou omezené po předepsaných denní spuštěních se překročí ale fakturují pomocí měření provádění akce.
EA zákazníky, kteří mají plánu služby App Service v jejich odběr, který nemá být explicitně přidružená aplikace logiky, využívat výhody zahrnuté množství.  Například pokud máte v předplatného EA a aplikace logiky ve stejném předplatném standardní plán služby App Service pak vám není účtován 10 000 akce spuštěních za den (viz následující tabulka). 

Plány služby App Service a jejich denní spuštěních povolené akce:
|  | Uvolněte a sdílet nebo základní | Standard | Premium |
| --- | --- | --- | --- |
| Akce spuštěních za den |200 |10 000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Převod plánu služby App Service – ceny na spotřebu
Chcete-li změnit aplikaci logiky, který má plán služby App Service přidružené k využívání modelu, odeberte odkaz na plán aplikační služby v definici aplikace logiky.  Tato změna se provádí pomocí volání rutiny prostředí PowerShell:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Ceny
Podrobnosti o cenách najdete v části [ceny aplikace logiky](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Další kroky
* [Přehled Logic Apps][whatis]
* [Vytvoření první aplikace logiky][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

