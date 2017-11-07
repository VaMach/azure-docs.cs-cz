---
title: "Připojení zařízení pomocí jazyka C v systému Linux | Microsoft Docs"
description: "Popisuje, jak se připojit zařízení k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C systémem Linux."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: a5768041a13d5ddc355c054dc85ba651b0752aba
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (Linux)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Sestavení a spuštění ukázkových C klienta Linux
Následující kroky ukazují, jak vytvořit klientskou aplikaci, která komunikuje s předkonfigurovaného řešení vzdáleného monitorování. Tato aplikace napsané v jazyce C a vytvořené a spusťte na Ubuntu Linux.

K provedení těchto kroků, je třeba zařízení se systémem Ubuntu verze 15.04 nebo 15.10. Než budete pokračovat, nainstalujte požadované balíčky zařízení Ubuntu pomocí následujícího příkazu:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Nainstalujte klientské knihovny zařízení
Knihovny klienta Azure IoT Hub jsou k dispozici jako balíček můžete nainstalovat pomocí zařízení Ubuntu **výstižný get** příkaz. Proveďte následující kroky k instalaci balíčku, který obsahuje IoT Hub knihovny a hlavičky souborů klienta v počítači Ubuntu:

1. V prostředí přidejte do počítače AzureIoT úložiště:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Nainstalovat balíček azure-iot-sdk-c vývojářů
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a>Nainstalujte analyzátor Parson JSON
Knihovny klienta služby IoT Hub pomocí analyzátoru Parson JSON analyzovat datové části zprávy. Ve složce vhodný ve vašem počítači naklonujte úložiště Parson GitHub pomocí následujícího příkazu:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Příprava projektu
Na počítači Ubuntu, vytvořte složku s názvem **vzdáleného\_monitorování**. V **vzdáleného\_monitorování** složky:

- Vytvořit čtyři soubory **main.c**, **vzdáleného\_monitoring.c**, **vzdáleného\_monitoring.h**, a **CMakeLists.txt**.
- Vytvořte složku s názvem **parson**.

Zkopírujte soubory **parson.c** a **parson.h** z místní kopie Parson úložiště do **vzdáleného\_monitorování nebo parson** složky.

V textovém editoru otevřete **vzdáleného\_monitoring.c** souboru. Přidejte následující příkazy `#include`:
   
```
#include "iothubtransportmqtt.h"
#include "schemalib.h"
#include "iothub_client.h"
#include "serializer_devicetwin.h"
#include "schemaserializer.h"
#include "azure_c_shared_utility/threadapi.h"
#include "azure_c_shared_utility/platform.h"
#include "parson.h"
```

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="call-the-remotemonitoringrun-function"></a>Volání vzdálených\_monitorování\_run – funkce
V textovém editoru otevřete **remote_monitoring.h** souboru. Přidejte následující kód:

```
void remote_monitoring_run(void);
```

V textovém editoru otevřete **main.c** souboru. Přidejte následující kód:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a>Sestavení a spuštění aplikace
Následující kroky popisují způsob použití *CMake* k vytvoření klientské aplikace.

1. V textovém editoru otevřete **CMakeLists.txt** v soubor **remote_monitoring** složky.

1. Přidejte podle následujících pokynů můžete definovat, jak vytvořit klientskou aplikaci:
   
    ```
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```
1. V **remote_monitoring** složky, vytvořte složku pro uložení *Ujistěte se,* soubory, které generuje CMake a znovu spusťte **cmake** a **zkontrolujte** příkazy následujícím způsobem:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Spusťte aplikaci klienta a odesílat telemetrická data do služby IoT Hub:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

