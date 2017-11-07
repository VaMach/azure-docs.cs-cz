---
title: "Vytvoření vlastního pravidla v sadě Azure IoT Suite | Microsoft Docs"
description: "Postup vytvoření vlastního pravidla v sadě IoT Suite předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bf2a13035de141766fd935966ce18459dccdaab
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Vytvoření vlastního pravidla v předkonfigurovaného řešení vzdáleného monitorování

## <a name="introduction"></a>Úvod

V předkonfigurovaných řešení, můžete nakonfigurovat [pravidla, která spouští při telemetrie hodnota zařízení dosáhne určité prahové hodnoty][lnk-builtin-rule]. [Použití dynamické telemetrie s vzdálené monitorování předkonfigurované řešení] [ lnk-dynamic-telemetry] popisuje, jak můžete přidat vlastní telemetrii hodnoty, jako například *ExternalTemperature* do řešení. Tento článek ukazuje, jak vytvořit vlastní pravidlo pro dynamické telemetrie typy ve vašem řešení.

Tento kurz používá jednoduchý Node.js simulované zařízení k vygenerování dynamických telemetrie k odeslání do back-end předkonfigurovaných řešení. Potom přidáte vlastní pravidla v **RemoteMonitoring** řešení sady Visual Studio a nasadit tento vlastní back-end do vašeho předplatného Azure.

Pro absolvování tohoto kurzu potřebujete:

* Aktivní předplatné Azure. Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
* [Node.js] [ lnk-node] verze 0.12.x nebo novější k vytvoření simulovaného zařízení.
* K úpravě předkonfigurované řešení Visual Studio 2015 nebo Visual Studio 2017 končit nové pravidel.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

Poznamenejte si název řešení, které jste zvolili pro vaše nasazení. Je třeba tento název řešení později v tomto kurzu.

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

Po ověření, že posílá můžete zastavit konzolovou aplikaci softwaru Node.js **ExternalTemperature** telemetrie s předkonfigurovaným řešením. V okně konzoly zachovat otevřít, protože je znovu spustit tuto konzolovou aplikaci softwaru Node.js po do řešení přidat vlastní pravidlo.

## <a name="rule-storage-locations"></a>Pravidlo umístění úložiště

Informace o pravidlech je uchován v dvou umístěních:

* **DeviceRulesNormalizedTable** tabulce – v této tabulce jsou uloženy normalizovaný odkaz z pravidel definovaných pomocí portálu řešení. Když na portálu řešení zobrazuje pravidla zařízení, vyžádá si tuto tabulku definice pravidla.
* **DeviceRules** objektů blob – tento objekt blob ukládá všechna pravidla pro všechny registrované zařízení a je definován jako odkaz na vstup do úlohy Azure Stream Analytics definována.
 
Při aktualizaci existující pravidlo nebo definovat nové pravidlo na portálu řešení, tabulky a objektů blob jsou aktualizovány tak, aby odrážely změny. Definice pravidla zobrazí na portálu pochází z tabulky úložiště a definice pravidla odkazuje úlohy Stream Analytics pocházejí z objektu blob. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Aktualizace řešení RemoteMonitoring Visual Studio

Následující kroky vám ukážou, jak upravit řešení nástroje RemoteMonitoring Visual Studio zahrnout nové pravidlo, které používá **ExternalTemperature** telemetrická data odesílaná ze simulovaného zařízení:

1. Pokud jste tak již neučinili, klonovat **azure-iot-remote-monitoring** úložiště do vhodného umístění na místním počítači pomocí následujícího příkazu Git:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. V sadě Visual Studio otevřete soubor RemoteMonitoring.sln z vaší místní kopii **azure-iot-remote-monitoring** úložiště.

3. Otevřete soubor Infrastructure\Models\DeviceRuleBlobEntity.cs a přidejte **ExternalTemperature** vlastnost následujícím způsobem:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Ve stejném souboru, přidejte **ExternalTemperatureRuleOutput** vlastnost následujícím způsobem:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Otevřete soubor Infrastructure\Models\DeviceRuleDataFields.cs a přidejte následující **ExternalTemperature** vlastnost po existující **vlhkosti** vlastnost:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. Ve stejném souboru, aktualizovat **_availableDataFields** tak, aby zahrnoval **ExternalTemperature** následujícím způsobem:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Otevřete soubor Infrastructure\Repository\DeviceRulesRepository.cs a upravit **BuildBlobEntityListFromTableRows** metoda následujícím způsobem:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a>Znovu sestavili a nasadili řešení.

Teď můžete nasadit aktualizované řešení k předplatnému Azure.

1. Otevřete příkazový řádek se zvýšenými oprávněními a přejděte do kořenového adresáře místní kopii úložiště azure-iot-remote-monitoring.

2. K nasazení řešení aktualizované, spusťte následující příkaz nahraďte **{název nasazení}** s názvem vašeho nasazení předkonfigurované řešení, které jste si poznamenali dříve:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Aktualizace úlohy služby Stream Analytics

Po dokončení nasazení můžete aktualizovat úlohu služby Stream Analytics používat nové definice pravidla.

1. Na portálu Azure přejděte do skupiny prostředků, která obsahuje vaše prostředky předkonfigurované řešení. Tato skupina prostředků má stejný název, který jste zadali pro řešení během nasazení.

2. Přejděte na název nasazení {} – úloha Stream Analytics pravidla. 

3. Klikněte na tlačítko **Zastavit** zastavit úlohu služby Stream Analytics spuštění. (Je nutné počkat úlohu streamování zastavit před úpravou dotazu).

4. Klikněte na tlačítko **dotazu**. Upravit dotaz tak, aby zahrnují **vyberte** příkaz pro **ExternalTemperature**. Následující příklad ukazuje dokončení dotazu s novým **vyberte** příkaz:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Klikněte na tlačítko **Uložit** změnit aktualizované pravidlo dotazu.

6. Klikněte na tlačítko **spustit** při spuštění úlohy Stream Analytics znovu spustit.

## <a name="add-your-new-rule-in-the-dashboard"></a>Přidat nové pravidlo v řídicím panelu

Nyní můžete přidat **ExternalTemperature** pravidlo pro zařízení na řídicím panelu řešení.

1. Přejděte na portálu řešení.

2. Přejděte na **zařízení** panelu.

3. Vyhledejte jste vytvořili vlastní zařízení, které odesílá **ExternalTemperature** telemetrie a na **podrobnosti o zařízení** panelu, klikněte na tlačítko **přidat pravidlo**.

4. Vyberte **ExternalTemperature** v **datové pole**.

5. Nastavit **prahová hodnota** k 56. Pak klikněte na tlačítko **uložit a zobrazit pravidla**.

6. Vrátit na řídicí panel k zobrazení historie alarmů.

7. V okně konzoly můžete ponechány otevřené, spusťte konzolovou aplikaci softwaru Node.js, aby se začala odesílat **ExternalTemperature** telemetrická data.

8. Všimněte si, že **historie alarmů** tabulka ukazuje nové výstrahy, když se aktivuje nové pravidlo.
 
## <a name="additional-information"></a>Další informace

Změna operátor  **>**  je složitější a překročí podle kroků uvedených v tomto kurzu. Zatímco můžete změnit na jakémkoli operátor chcete úlohy Stream Analytics, odrážející tento operátor na portálu řešení je složitější úlohy. 

## <a name="next-steps"></a>Další kroky
Teď, když už víte, jak vytvořit vlastní pravidla, můžete další informace o předkonfigurovaných řešení:

- [Připojení aplikace logiky k řešení Azure IoT Suite vzdálené monitorování předkonfigurované][lnk-logic-app]
- [Informace metadat zařízení ve vzdálené monitorování předkonfigurované řešení][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-v1-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-v1-logic-apps-tutorial.md