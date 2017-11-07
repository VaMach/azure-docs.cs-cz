---
title: "Azure IoT Suite a aplikacích logiky | Microsoft Docs"
description: "Kurz o tom, jak spojit Logic Apps, abyste Azure IoT Suite pro obchodní proces."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: f4457b44c97fadc58406430fc0f31b3e0bac6682
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Kurz: Připojení k Azure IoT Suite vzdálené monitorování předkonfigurované řešení aplikace logiky
[Microsoft Azure IoT Suite] [ lnk-internetofthings] předkonfigurovanému řešení vzdáleného monitorování je skvělým způsobem, jak rychle začít používat sadu začátku do konce funkce, která exemplifies řešení IoT. Tento kurz vás provede procesem přidání aplikace logiky do vaší Microsoft Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování. Tyto kroky ukazují, jak může trvat i další řešení IoT připojením k obchodní proces.

*Pokud hledáte návod o tom, jak zřídit předkonfigurované řešení vzdáleného monitorování, přečtěte si téma [kurz: Začínáme s přednastavenými řešeními IoT][lnk-getstarted].*

Než začnete tento kurz, proveďte následující kroky:

* Zřídit předkonfigurovaného řešení vzdáleného monitorování ve vašem předplatném Azure.
* Vytvořte účet sendgrid vám umožňuje povolit odesílání e-mailu, která aktivuje obchodní proces. Můžete si zaregistrovat Bezplatný zkušební účet v [sendgrid vám umožňuje](https://sendgrid.com/) kliknutím **zkuste zdarma**. Po registraci pro bezplatný zkušební účet je potřeba vytvořit [klíč rozhraní API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) v Sendgridu, která uděluje oprávnění k odesílání pošty. Později v tomto kurzu potřebujete tento klíč rozhraní API.

K dokončení tohoto kurzu, musíte Visual Studio 2015 nebo Visual Studio 2017 chtěli změnit nastavení akcí v back-end předkonfigurovaných řešení.

Za předpokladu, že jste už zřízené vzdáleného monitorování předkonfigurované řešení, přejděte do skupiny prostředků pro toto řešení v [portál Azure][lnk-azureportal]. Skupina prostředků má stejný název jako název řešení jste si zvolili při zřízení řešení vzdáleného monitorování. Ve skupině prostředků uvidíte všechny zřízené prostředky Azure pro vaše řešení s výjimkou aplikace Azure Active Directory, které můžete najít na portálu Azure Classic. Následující snímek obrazovky ukazuje příklad **skupiny prostředků** okno pro vzdálené monitorování předkonfigurované řešení:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Pokud chcete začít, nastavte pro použití s předkonfigurované řešení aplikace logiky.

## <a name="set-up-the-logic-app"></a>Nastavit aplikaci logiky
1. Klikněte na tlačítko **přidat** v horní části vaší okně skupiny prostředků na portálu Azure.
2. Vyhledejte **aplikace logiky**, vyberte ho a pak klikněte na tlačítko **vytvořit**.
3. Vyplňte **název** a používat stejné **předplatné** a **skupiny prostředků** jste použili při zřizování řešení vzdáleného monitorování. Klikněte na možnost **Vytvořit**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Po dokončení nasazení uvidíte, že aplikace logiky je uveden jako prostředků ve vaší skupině prostředků.
5. Klikněte na aplikaci logiky a přejděte do okna aplikace logiky, vyberte **prázdné aplikace logiky** šablony otevřete **logiku aplikace Návrhář**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Vyberte **požadavku**. Tuto akci Určuje, že příchozí požadavek HTTP s konkrétní JSON formátu aktivační událost jednání datové části.
7. Do schématu JSON textu žádosti vložte následující kód:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Po uložení aplikaci logiky, ale nejdřív je nutné přidat akci, můžete zkopírovat adresu URL pro metodu post protokolu HTTP.
   > 
   > 
8. Klikněte na tlačítko **+ nový krok** pod ruční aktivační událost. Pak klikněte na tlačítko **přidat akci**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Vyhledejte **sendgrid vám umožňuje - odesílání e-mailu** a klikněte na něj.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Zadejte název připojení, jako například **SendGridConnection**, zadejte **klíč rozhraní API sendgrid vám umožňuje** jste vytvořili při nastavení účtu Sendgridu a klikněte na tlačítko **vytvořit**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Přidání e-mailové adresy, které vlastníte na obojí **z** a **k** pole. Přidat **vzdáleného sledování upozornění [DeviceId]** k **subjektu** pole. V **obsahu e-mailu** pole, přidejte **zařízení [DeviceId] ohlásil [measurementName] s hodnotou [measuredValue].** Můžete přidat **[DeviceId]**, **[measurementName]**, a **[measuredValue]** kliknutím v **můžete vložit data z předchozích kroků** části.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Klikněte na tlačítko **Uložit** v horní nabídce.
13. Klikněte na tlačítko **požadavku** aktivační události a zkopírujte **Http Post na tuto adresu URL** hodnotu. Později v tomto kurzu musíte tuto adresu URL.

> [!NOTE]
> Logic Apps umožňují spustit [mnoho různých typů akce] [ lnk-logic-apps-actions] včetně akce v Office 365. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>Nastavit webovou úlohu EventProcessor
V této části se připojit k aplikaci logiky můžete vytvořit předkonfigurované řešení. Pro dokončení této úlohy, přidejte adresu URL pro aktivaci aplikace logiky k akci, která aktivuje se v případě hodnotu senzor zařízení překračuje prahovou hodnotu.

1. Pomocí vašeho klienta git clone nejnovější verzi [azure-iot-remote-monitoring úložiště github][lnk-rmgithub]. Například:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. V sadě Visual Studio, otevřete **RemoteMonitoring.sln** z místní kopie úložiště.
3. Otevřete **ActionRepository.cs** v soubor **infrastruktury\\úložiště** složky.
4. Aktualizace **actionIds** slovníku pomocí **Http Post na tuto adresu URL** jste si poznamenali u aplikace logiky následujícím způsobem:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Uložte změny v řešení a ukončení Visual Studio.

## <a name="deploy-from-the-command-line"></a>Nasazení z příkazového řádku
V této části nasadíte vaší aktualizovanou verzi řešení vzdáleného monitorování nahradit verzi aktuálně spuštěných v Azure.

1. Následující [dev nastavení] [ lnk-devsetup] pokyny pro nastavení prostředí pro nasazení.
2. Chcete-li nasadit místně, postupujte [místní nasazení] [ lnk-localdeploy] pokyny.
3. Chcete-li nasadit do cloudu a aktualizovat vaše stávající nasazení cloudu, postupujte [cloudové nasazení] [ lnk-clouddeploy] pokyny. Název původního nasazení použijte jako název nasazení. Například pokud byla volána původního nasazení **demologicapp**, použijte následující příkaz:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Při spuštění skriptu sestavení, nezapomeňte použít stejný účet Azure, předplatné, oblast a instanci Active Directory, které jste použili při zřizování řešení.

## <a name="see-your-logic-app-in-action"></a>V tématu aplikace logiky v akci
Předkonfigurované řešení vzdáleného monitorování má dvě pravidla ve výchozím nastavení při zřizování řešení. Obě pravidla jsou na **SampleDevice001** zařízení:

* Teplotní > 38.00
* Vlhkosti > 48.00

Aktivační události pravidlo teploty **vyvolat alarmů** akce a vlhkost pravidla aktivační události **SendMessage** akce. Za předpokladu, že používá stejnou adresu URL pro obě akce **ActionRepository** třídy, vaše aplikace logiky aktivačních událostí pro buď pravidlo. Obě pravidla sendgrid vám umožňuje používat k odesílání e-mailu na **k** adresu podrobné informace o této výstrahy.

> [!NOTE]
> Aplikace logiky i nadále aktivovat pokaždé, když je splněna prahovou hodnotu. Aby se zabránilo zbytečným e-mailů, můžete buď vypněte pravidla na portálu řešení nebo zakázat aplikaci logiky v [portál Azure][lnk-azureportal].
> 
> 

Kromě doručování e-mailů, můžete také zjistit při spuštění aplikace logiky na portálu:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Další kroky
Teď, když jste použili aplikaci logiky pro připojení k obchodní proces předkonfigurované řešení, můžete se další informace o možnostech přizpůsobení předkonfigurovaných řešení:

* [Použití dynamické telemetrie s předkonfigurovaného řešení vzdáleného monitorování][lnk-dynamic]
* [Informace metadat zařízení v předkonfigurovaného řešení vzdáleného monitorování][lnk-devinfo]

[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
