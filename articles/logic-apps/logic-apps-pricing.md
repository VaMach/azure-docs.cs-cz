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
ms.date: 11/11/2017
ms.author: LADocs; klam
ms.openlocfilehash: f2a92e45b8a759d2d8193ac188efdcfc694a3e6d
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="logic-apps-pricing-model"></a>Cenový model Logic Apps
Můžete vytvořit a spouštění automatizovaných škálovatelnou integraci pracovních postupů v cloudu pomocí Azure Logic Apps. Zde jsou informace o fakturaci a cenách fungování Logic Apps.
## <a name="consumption-pricing-model"></a>Cenový model spotřeby
S nově vytvořený logic apps platíte jenom za můžete použít. Nové aplikace logiky používají spotřeba plánu a ceny model, což znamená, že všechny spuštěních provádí instanci aplikace logiky jsou měření podle objemu.
### <a name="what-are-action-executions"></a>Jaké jsou spuštěních akce?
Každý krok v definici aplikace logiky je akce, která zahrnuje aktivačních událostí, řízení toku kroky, volání vestavěné akce a volání konektory.
### <a name="triggers"></a>Triggery
Aktivační události jsou zvláštní akce, které vytvořit instanci aplikace logiky, když se stane konkrétní události. Aktivační události mít několik různých chování, které ovlivňují, jak je monitorované aplikaci logiky.
* **Aktivační událost dotazování** – této aktivační události neustále kontroluje koncový bod dokud získá zprávu která splňuje kritéria pro vytvoření instance aplikace logiky spustit pracovní postup. Můžete nastavit interval dotazování v aktivační události prostřednictvím návrháře logiku aplikace. Každý požadavek dotazování se počítá jako spuštění, i když je vytvořena žádná instance aplikace logiky.
* **Aktivační události Webhooku** – této aktivační události čeká klienta k odeslání požadavku na konkrétní. Každý požadavek odeslaný ke koncovému bodu webhooku se počítá jako spuštění akce. Například aktivační událost požadavku a Webhooku protokolu HTTP jsou obě aktivační události webhooku.
* **Aktivační událost opakování** – této aktivační události vytvoří instanci aplikace logiky na základě intervalu opakování, které jste nastavili v aktivační události. Můžete například nastavit nahoru opakování aktivační událost, která spouští každé tři dny nebo podle plánu složitější.

Aktivační událost spuštěních můžete najít v podokně přehled aplikace logiky v části historie aktivační události.

### <a name="actions"></a>Akce
Vestavěné akce, například akcí, které volání protokolu HTTP, Azure Functions nebo API Management, jakož i kroky toku řízení, se měří jako nativní akcí a jejich odpovídající typy. Akce, které volají [konektory](https://docs.microsoft.com/connectors) typ "ApiConnection". Konektory jsou klasifikovány jako standard nebo enterprise konektory a jsou v jejich odpovídajících měření podle objemu [ceny][pricing].
Všechny úspěšně a neúspěšně spuštění akce se počítá nebo účtované jako spuštění akce. Akce, které jsou přeskočen, protože unmet podmínky nebo akce, které nelze spustit, protože aplikace logiky ukončeno před dokončením, není však počítají jako spuštění akce. Zakázané logiku aplikace nelze vytvořit instanci nové instance, tak jejich neúčtujeme při jsou zakázány.

> [!NOTE]
> Po zakázání aplikace logiky jeho aktuálně spuštěné instance může trvat nějakou dobu, než úplně zastavit.

Akce, které běží v smyčky, se počítají na každý cyklus ve smyčce. Například jedna akce ve smyčce "pro každou", který zpracovává seznam 10 položek se počítá vynásobením počet položek seznamu (10) podle počtu akcí ve smyčce (1) a jeden pro spuštění smyčky. Ano, v tomto příkladu je výpočet (10 * 1) + 1, což vede k 11 spuštění akce.

### <a name="integration-account-usage"></a>Používání účtů integrace
Na základě spotřeby využití zahrnuje [integrace účet](logic-apps-enterprise-integration-create-integration-account.md) kde můžete prozkoumat, vývoj a testování [B2B/EDI](logic-apps-enterprise-integration-b2b.md) a [zpracování kódu XML](logic-apps-enterprise-integration-xml.md) funkce Logic Apps na Ne Další náklady. Můžete mít jednu z těchto účtů integrace na oblast a úložiště až 10 smluv a 25 mapy. Můžete mít a nahrajte neomezená partnery, schémata a certifikáty.

Služba Logic Apps nabízí také účty pro integraci úrovně basic a standard s podporované SLA aplikace logiky. Můžete použít účty základní integraci při buď chcete použít jenom zpracování zpráv nebo fungují jako partner malé firmy, která má vztah obchodním partnerem s větší obchodní entity. Účty pro integraci standardní podpora složitější vztahy B2B a zvýšit počet entit, které můžete spravovat. Další informace najdete v tématu [Azure ceny](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="pricing"></a>Ceny
Další informace najdete v tématu [ceny aplikace logiky](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Další kroky
* [Přehled Logic Apps][whatis]
* [Vytvoření první aplikace logiky][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

