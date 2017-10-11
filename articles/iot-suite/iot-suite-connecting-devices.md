---
title: "Připojení zařízení pomocí jazyka C v systému Windows | Microsoft Docs"
description: "Popisuje, jak se připojit zařízení k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C v systému Windows."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Vytvoření ukázkové řešení C v systému Windows
Následující kroky ukazují, jak vytvořit klientskou aplikaci, která komunikuje s předkonfigurovaného řešení vzdáleného monitorování. Tato aplikace napsané v jazyce C a vytvořené a spusťte v systému Windows.

Vytvoření projektu starter v sadě Visual Studio 2015 nebo Visual Studio 2017 a přidání balíčků NuGet IoT Hub zařízení klienta:

1. Ve Visual Studiu Vytvořte konzolovou aplikaci C pomocí Visual C++ **Konzolová aplikace Win32** šablony. Název projektu **RMDevice**.
2. Na **nastavení aplikace** stránku **Win32 – Průvodce aplikací**, ujistěte se, že **Konzolová aplikace** je vybrána a zrušte zaškrtnutí políčka **předkompilované záhlaví** a **kontroluje životního cyklu SDL (Security Development)**.
3. V **Průzkumníku**, odstraňte soubory stdafx.h, targetver.h a stdafx.cpp.
4. V **Průzkumníku**, přejmenujte soubor RMDevice.cpp RMDevice.c.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **RMDevice** projektu a pak klikněte na **spravovat balíčky NuGet balíčky**. Klikněte na tlačítko **Procházet**, vyhledejte a nainstalujte následující balíčky NuGet:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **RMDevice** projektu a pak klikněte na **vlastnosti** otevření projektu **stránky vlastností** Dialogové okno. Podrobnosti najdete v tématu [nastavení vlastností projektu Visual C++][lnk-c-project-properties]. 
7. Klikněte na tlačítko **Linkeru** složku, klikněte **vstup** stránku vlastností.
8. Přidat **crypt32.lib** k **Další závislosti** vlastnost. Klikněte na tlačítko **OK** a potom **OK** znovu k uložení projektu hodnot vlastností.

Přidat Parson JSON knihovnu, která má **RMDevice** projekt a přidejte požadované `#include` příkazy:

1. Ve složce vhodný ve vašem počítači naklonujte úložiště Parson GitHub pomocí následujícího příkazu:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Zkopírujte soubory parson.h a parson.c z místní kopie Parson úložiště k vaší **RMDevice** složce projektu.

1. V sadě Visual Studio, klikněte pravým tlačítkem myši **RMDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.

1. V **přidat existující položku** dialogovém okně, vyberte parson.h a parson.c soubory **RMDevice** složce projektu. Pak klikněte na tlačítko **přidat** přidat tyto dva soubory do projektu.

1. V sadě Visual Studio otevřete soubor RMDevice.c. Nahradit existující `#include` příkazy následujícím kódem:
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Nyní můžete ověřit, že váš projekt má nastavit tak, že vytváření se správnými závislostmi.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Sestavit a spustit ukázku

Přidejte kód, který má být vyvolán **vzdáleného\_monitorování\_spustit** funkce a potom sestavit a spustit aplikaci zařízení.

1. Nahraďte **hlavní** funkce s následující kód k vyvolání **vzdáleného\_monitorování\_spustit** funkce:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klikněte na tlačítko **sestavení** a potom **sestavit řešení** sestavit aplikaci pro zařízení.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **RMDevice** projektu, klikněte na tlačítko **ladění**a potom klikněte na **spustit novou instanci** ke spuštění ukázky. Konzole zobrazí zprávy jako aplikace odesílá telemetrii vzorek pro předkonfigurované řešení, obdrží požadovanou vlastnost hodnotami nastavenými na řídicím panelu řešení a reaguje na metody vyvolané z řídicího panelu řešení.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
