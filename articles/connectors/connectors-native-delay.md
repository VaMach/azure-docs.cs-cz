---
title: "Přidání zpoždění v aplikace logiky | Microsoft Docs"
description: "Přehled zpoždění a zpoždění – dokud akcí a jejich použití s aplikací Azure logiku."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Začínáme s zpoždění a zpoždění – dokud akce
Pomocí zpoždění a "zpoždění – dokud" akce, můžete dokončit scénáře pracovního postupu.

Můžete například provést následující věci:

* Počkejte na jeden den v týdnu pro odeslání e-mailu aktualizace stavu.
* Pracovní postup počkat, až budou volání protokolu HTTP má čas na dokončení obnovení a načítání výsledek.

Chcete-li začít používat zpoždění akce v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Pomocí akcí zpoždění
Akce je operace, která se provádí v pracovním postupu, který je definován v aplikaci logiky. [Další informace o akcích](connectors-overview.md).

Tady je v sekvenci příklad použití kroku zpoždění v aplikaci logiky:

1. Po přidání aktivační událost, klikněte na tlačítko **nový krok** a přidejte akci.
2. Vyhledejte **zpoždění** se zprovoznit akce zpoždění. V tomto příkladu jsme vyberte **zpoždění**.
   
    ![Zpoždění akce](./media/connectors-native-delay/using-action-1.png)
3. Dokončete všechny vlastnosti Akce konfigurace zpoždění.
   
    ![Konfigurace zpoždění](./media/connectors-native-delay/using-action-2.png)
4. Klikněte na tlačítko **Uložit** k publikování a aktivovat aplikaci logiky.

## <a name="action-details"></a>Podrobnosti akce
Aktivační událost opakování má následující vlastnosti, které lze konfigurovat.

### <a name="delay-action"></a>Zpoždění akce
Tato akce zpozdí spuštění pro určitý časový interval.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Počet * |Počet |Počet jednotek doba zpoždění před |
| Jednotka * |jednotka |Jednotka času: `Second`, `Minute`, `Hour`, nebo`Day` |

<br>

### <a name="delay-until-action"></a>Zpoždění – dokud akce
Tato akce zpozdí spustit až do zadaného data a času.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Rok * |časové razítko |Roku zpoždění až (GMT) |
| Měsíc * |časové razítko |V měsíci pro zpoždění až (GMT) |
| Den * |časové razítko |Den zpoždění až (GMT) |

<br>

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).

