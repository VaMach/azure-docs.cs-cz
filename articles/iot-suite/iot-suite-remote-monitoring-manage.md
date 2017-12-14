---
title: "Správa zařízení v řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak spravovat zařízení připojená k řešení vzdáleného monitorování."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: fab3fd4163141aadc06b385f5759c19eece7fd14
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="manage-and-configure-your-devices"></a>Správa a konfigurace zařízení

Tento kurz ukazuje zařízení možnosti správy řešení vzdáleného monitorování. Zavádět tyto možnosti, tento kurz používá scénáři v aplikaci Contoso IoT.

Contoso má seřazené nové stroje rozbalte jeden z jejich zařízení zvýšit výstup. Počkejte nové stroje, který bude doručen, ale chcete spustit simulaci ověření chování vašeho řešení. Jakožto obsluha, kterou chcete spravovat a nakonfigurovat zařízení v řešení vzdáleného monitorování.

K zajištění o extensible způsob, jak spravovat a nakonfigurovat zařízení, řešení vzdáleného monitorování používá funkce IoT Hub, jako [úlohy](../iot-hub/iot-hub-devguide-jobs.md) a [přímé metody](../iot-hub/iot-hub-devguide-direct-methods.md). Informace o tom, jak vývojář zařízení implementuje metody na fyzické zařízení, najdete v části [přizpůsobení předkonfigurovaného řešení vzdáleného monitorování](iot-suite-remote-monitoring-customize.md).

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Zřídit simulované zařízení.
> * Otestujte simulované zařízení.
> * Volání metody zařízení z řešení.
> * Změňte konfiguraci zařízení.

## <a name="prerequisites"></a>Požadavky

Chcete-li v tomto kurzu, je třeba nasazené instanci řešení vzdáleného monitorování ve vašem předplatném Azure.

Pokud jste nenasadili řešení vzdáleného monitorování ještě by se měla Dokončit [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md) kurzu.

## <a name="add-a-simulated-device"></a>Přidání simulovaného zařízení

Přejděte na **zařízení** stránky v řešení a potom zvolte **nové zařízení**. V **nové zařízení** panelu, vyberte **Simulovaná**:

![Zřídit simulovaného zařízení](media/iot-suite-remote-monitoring-manage/devicesprovision.png)

Nechte počet zařízení a zajišťují nastavena na **1**. Zvolte **vadný modul** jako **model zařízení**a potom zvolte **použít** vytvořte simulované zařízení:

![Zřídit modul simulované zařízení](media/iot-suite-remote-monitoring-manage/devicesprovisionengine.png)

Další informace o zřizování *fyzické* zařízení, najdete v části [připojení zařízení k předkonfigurovaného řešení vzdáleného monitorování](iot-suite-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Testování simulované zařízení

Chcete-li zobrazit podrobnosti o nové simulované zařízení, vyberte ho v seznamu zařízení na **zařízení** stránky. Informace o vašem zařízení se zobrazí v **zařízení podrobností** panelu:

![Zobrazení nového simulovaného modul zařízení](media/iot-suite-remote-monitoring-manage/devicesviewnew.png)

V **zařízení podrobností**, ověřte nové zařízení odesílá telemetrii. Chcete-li zobrazit datové proudy různých telemetrie ze zařízení, vyberte název telemetrie **vibracím**:

![Vyberte datový proud telemetrie k zobrazení](media/iot-suite-remote-monitoring-manage/devicesvibration.png)

**Zařízení podrobností** panelu se zobrazí další informace o zařízení, například hodnoty značky, podporuje metody a vlastnosti údajů ze zařízení.

Chcete-li zobrazit podrobné diagnostiky, posuňte se dolů a zobrazení **diagnostiky**.

## <a name="act-on-a-device"></a>Fungovat na zařízení

Tak, aby fungoval na jeden nebo více zařízení, vyberte je v seznamu zařízení a potom zvolte **plán**. **Modul** model zařízení určuje čtyři metody musí podporovat zařízení:

![Modul metody](media/iot-suite-remote-monitoring-manage/devicesmethods.png)

Zvolte **restartujte**, nastavte název úlohy na **RestartEngine**a potom zvolte **použít**:

![Plán metodu restartování](media/iot-suite-remote-monitoring-manage/devicesrestartengine.png)

Chcete-li sledovat stav úlohy na **údržby** vyberte **úlohy**:

![Monitorování plány úlohy](media/iot-suite-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Metody v jiných zařízení

Při prozkoumávání typy jiné simulované zařízení, uvidíte, že ostatní typy zařízení podporují různé metody. V nasazení s fyzických zařízení Určuje model zařízení metod, které by měly podporovat zařízení. Vývojář zařízení obvykle zodpovídá za vývoj kód, který umožňuje zařízení fungovat v reakci na volání metody.

Při plánování metodu pro spuštění na několika zařízeních, můžete vybrat více zařízení v seznamu na **zařízení** stránky. **Plán** panelu zobrazí požadované typy metoda společné pro všechny vybrané zařízení.

## <a name="reconfigure-a-device"></a>Překonfigurujte zařízení

Chcete-li změnit konfiguraci zařízení, vyberte ho v seznamu zařízení na **zařízení** stránce a pak vyberte **překonfigurovat**. Reconfigure panel ukazuje pro vybrané zařízení, které můžete změnit hodnoty vlastností:

![Překonfigurujte zařízení](media/iot-suite-remote-monitoring-manage/devicesreconfigure.png)

Chcete-li ke změně, přidat název úlohy, aktualizujte hodnoty vlastností a zvolte **použít**:

![Aktualizujte hodnotu vlastnosti zařízení](media/iot-suite-remote-monitoring-manage/devicesreconfigurephysical.png)

Chcete-li sledovat stav úlohy na **údržby** vyberte **úlohy**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu ukázal, jak na:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Zřídit simulované zařízení.
> * Otestujte simulované zařízení.
> * Volání metody zařízení z řešení.
> * Změňte konfiguraci zařízení.

Teď, když jste se naučili jak ke správě svých zařízení, navrhované další kroky jsou další postup:

* [Řešení potíží a opravám problémů zařízení](iot-suite-remote-monitoring-maintain.md).
* [Testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).
* [Připojte zařízení k předkonfigurovaného řešení vzdáleného monitorování](iot-suite-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->