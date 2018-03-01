---
title: "Řešení potíží s zařízení v řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak k řešení potíží a opravám problémů zařízení v řešení vzdáleného monitorování."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: dd01246075a5c0db0ed49133ed51fb56d8fcf8e5
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Řešení potíží a opravám problémů zařízení

V tomto kurzu se dozvíte, jak používat **údržby** stránky v řešení k řešení potíží a opravám problémů zařízení. Zavádět tyto možnosti, tento kurz používá scénáři v aplikaci Contoso IoT.

Contoso je testování novou **prototypu** zařízení v poli. Jako operátor Contoso, si všimnete během testování, které **prototypu** zařízení je aktivován neočekávaně teploty alarmů na řídicím panelu. Teď musíte prozkoumat chování tento vadný **prototypu** zařízení.

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Použití **údržby** stránky k prozkoumání varování
> * Volání metody zařízení k nápravě problému

## <a name="prerequisites"></a>Požadavky

Chcete-li v tomto kurzu, je třeba nasazené instanci řešení vzdáleného monitorování ve vašem předplatném Azure.

Pokud jste nenasadili řešení vzdáleného monitorování ještě by se měla Dokončit [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md) kurzu.

## <a name="use-the-maintenance-dashboard"></a>Řídicí panel údržby

Na **řídicí panel** stránky zjistíte, že existují pocházejících z přidruženo pravidlo výstrahy neočekávané teploty **prototypu** zařízení:

![Výstrahy zobrazuje na řídicím panelu](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Chcete-li prozkoumat další potíže, zvolte **prozkoumat alarmů** možnost vedle varování:

![Prozkoumejte alarmů na řídicím panelu](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

Ukazuje, zobrazení podrobností o varování:

* Pokud bylo spuštěno na upozornění
* Informace o zařízení spojených s varováním stavu
* Telemetrická data ze zařízení spojených s varováním

![Podrobnosti výstrahy](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Aby vzali na vědomí na upozornění, vyberte **poplašný výskytů** a zvolte **potvrzení**. Tato akce umožní dalšími operátory viděli na upozornění a jsou na něm pracovat.

![Potvrdit signály](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Po potvrzení na upozornění, stav výskytu změní na **potvrzeno**.

V seznamu se zobrazí **prototypu** zodpovědná za ohlásí teplotní alarm zařízení:

![Seznam zařízení způsobuje varování](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Opravte problém

Oprava problému s **prototypu** zařízení, je třeba volat **DecreaseTemperature** metoda v zařízení.

Tak, aby fungoval na zařízení, vyberte ho v seznamu zařízení a potom zvolte **plán**. **Prototypu** model zařízení určuje čtyři metody musí podporovat zařízení:

![Zobrazení metod, které podporuje zařízení](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Zvolte **DecreaseTemperature** a nastavte název úlohy na **DecreaseTemperature**. Zvolte **použít**:

![Vytvoření úlohy snížení teplota](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

Chcete-li sledovat stav úlohy na **údržby** vyberte **úlohy**. Použití **úlohy** zobrazení sledovat všechny úlohy a metoda volá v řešení:

![Monitorování úlohy snížení teplota](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Chcete-li zobrazit podrobnosti o konkrétní úlohy nebo volání metody, vyberte v seznamu **úlohy** zobrazení:

![Zobrazení podrobností o úloze](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Další postup

V tomto kurzu jste viděli, jak:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Použití **údržby** stránky k prozkoumání varování
> * Volání metody zařízení k nápravě problému

Nyní jste se naučili jak spravovat zařízení problémy, navrhované dalším krokem je další postup [testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->