---
title: "Rozpoznat problémy s zařízení v řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat pravidla a akce automaticky rozpoznat problémy s zařízení na základě prahové hodnoty v řešení vzdáleného monitorování."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/18/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 2f2b221f5739ac370e110d60ed7812ce9ea5e05f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="detect-issues-using-threshold-based-rules"></a>Zjistit problémy při použití pravidla na základě prahové hodnoty

Tento kurz ukazuje možnosti stroj pravidel v řešení vzdáleného monitorování. Zavádět tyto možnosti, tento kurz používá scénáři v aplikaci Contoso IoT.

Contoso má pravidlo, které generuje kritickou výstrahu v případě hlášené tlak **chladič** zařízení překračuje 250 PSI. Jako operátor, který chcete označit **chladič** zařízení, které mohou mít problematické senzorů tak, že vyhledá počáteční přetížení špičky. K identifikaci těchto zařízení, vytvoříte pravidlo pro vygenerovat upozornění, když tlak překračuje 150 PSI.

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Zobrazení pravidla ve vašem řešení
> * Vytvořit nové pravidlo
> * Úprava existujícího pravidla
> * Odstranění pravidla

## <a name="prerequisites"></a>Požadavky

Chcete-li v tomto kurzu, je třeba nasazené instanci řešení vzdáleného monitorování ve vašem předplatném Azure.

Pokud jste nenasadili řešení vzdáleného monitorování ještě by se měla Dokončit [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md) kurzu.

## <a name="view-the-rules-in-your-solution"></a>Zobrazení pravidla ve vašem řešení

**Pravidla a akce** stránky v řešení zobrazí seznam všech aktuální pravidla:

![Stránka pravidla a akce](media/iot-suite-remote-monitoring-automate/rulesactions.png)

Chcete-li zobrazit pouze pravidla, která se týkají **chladič** zařízení, použít filtr:

![Filtrovat seznam pravidel](media/iot-suite-remote-monitoring-automate/rulesactionsfilter.png)

Můžete zobrazit další informace o pravidlo a upravit ho, když ji vyberete v seznamu:

![Zobrazení podrobností pravidla](media/iot-suite-remote-monitoring-automate/rulesactionsdetail.png)

K zakázání, povolení nebo odstranění jednoho nebo více pravidel, vyberte v seznamu více pravidel:

![Vyberte více pravidel](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect.png)

## <a name="create-a-new-rule"></a>Vytvořit nové pravidlo

Chcete-li přidat nové pravidlo, které generuje upozornění při přetížení v **chladič** zařízení překračuje 150 PSI, zvolte **nové pravidlo**:

![Vytvoření pravidla](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule.png)

Vytvoření pravidla, použijte následující hodnoty:

| Nastavení          | Hodnota                                 |
| ---------------- | ------------------------------------- |
| Name (Název)             | Chladič upozornění                       |
| Zdroj           | **Chladič** skupiny zařízení              |
| Aktivační událost pole    | přetížení                              |
| Aktivační událost – operátor | Více než                          |
| Hodnota aktivační události    | 150                                   |
| Úroveň závažnosti   | Upozornění                               |
| Text události varování | Přetížení chladič překročil 150 PSI |

Chcete-li uložit nové pravidlo, zvolte **použít**.

Můžete zobrazit, když pravidlo je spuštěno na **pravidla a akce** stránku nebo na **řídicí panel** stránky.

## <a name="edit-an-existing-rule"></a>Úprava existujícího pravidla

Chcete-li provést změnu do existujícího pravidla, vyberte ho v seznamu pravidel. Potom v **pravidlo podrobností** panelu zvolte **režimu úprav**.

![Upravit pravidlo](media/iot-suite-remote-monitoring-automate/rulesactionsedit.png)

## <a name="disable-a-rule"></a>Zakázat pravidlo

Pro pravidlo dočasně vypnout, ji můžete vypnout v seznamu pravidel. Vyberte pravidlo zakázat a potom vyberte **zakázat**. **Stav** pravidla v seznamu je nyní zakázán změny k označení pravidlo. Můžete znovu povolit pravidlo dříve zakázán pomocí stejného postupu.

![Zakázat pravidlo](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

Můžete povolit nebo zakázat více pravidel ve stejnou dobu, pokud vyberete více pravidel v seznamu.

## <a name="delete-a-rule"></a>Odstranění pravidla

Pokud chcete trvale odstranit pravidlo, v seznamu pravidel příslušného vyberte pravidlo a poté zvolte **odstranit**.

Pokud vyberete více pravidel v seznamu, můžete odstranit víc pravidel ve stejnou dobu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu ukázal, jak na:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Zobrazení pravidla ve vašem řešení
> * Vytvořit nové pravidlo
> * Úprava existujícího pravidla
> * Odstranění pravidla

Teď, když jste se naučili k zjištění problémy při použití pravidla na základě prahové hodnoty, navrhované další kroky jsou další postup:

* [Správa a konfigurace zařízení](./iot-suite-remote-monitoring-manage.md).
* [Řešení potíží a opravám problémů zařízení](./iot-suite-remote-monitoring-maintain.md).
* [Testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->