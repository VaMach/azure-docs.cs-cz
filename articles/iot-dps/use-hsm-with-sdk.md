---
title: "Postup pro Azure – Použití jiného modulu hardwarového zabezpečení pomocí klientské sady SDK služby Device Provisioning v Azure | Dokumentace Microsoftu"
description: "Postup pro Azure – Použití jiného modulu hardwarového zabezpečení pomocí klientské sady SDK služby Device Provisioning v Azure"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 08/28/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: 
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 184bbdc0a6bef74d0e5ac79afe3858354c6b1695
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk"></a>Použití různých modulů hardwarového zabezpečení pomocí klientské sady SDK služby Device Provisioning
Tyto kroky ukazují, jak použít různé [moduly hardwarového zabezpečení (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) pomocí klientské sady SDK služby Device Provisioning v jazyce C s využitím fyzického zařízení a simulátoru.  Zřizovací služba podporuje dva režimy ověřování: X**.**509 a TPM (Trusted Platform Module).

## <a name="prerequisites"></a>Požadavky

Připravte vývojové prostředí podle části Příprava vývojového prostředí v průvodci [Vytvoření a zřízení simulovaného zařízení] (./quick-create-simulated-device.md).

## <a name="enable-authentication-with-different-hsms"></a>Povolení ověřování pomocí různých HSM

Než bude možné zaregistrovat fyzické zařízení nebo simulátor na webu Azure Portal, musí pro ně být povolený režim ověřování (X**.**509 nebo TPM).  Přejděte do kořenové složky pro azure-iot-sdk-c.  Spusťte příslušný příkaz v závislosti na zvoleném režimu ověřování.

### <a name="use-x509-with-simulator"></a>Použití X**.**509 se simulátorem

Součástí zřizovací služby je emulátor DICE (Device Identity Composition Engine), který generuje certifikát X**.**509 pro ověřování zařízení.  Pokud chcete povolit ověřování pomocí X**.**509, spusťte následující příkaz:

```
cmake -Ddps_auth_type=x509 ..
```

Informace o hardwaru s emulátorem DICE najdete [tady](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Použití X**.**509 s hardwarem

Zřizovací službu je možné použít s X**.**509 na jiném hardwaru.  K navázání připojení je potřeba rozhraní mezi hardwarem a sadou SDK.  Pokud potřebujete informace o rozhraní, obraťte se na výrobce vašeho HSM.

### <a name="use-tpm"></a>Použití TPM

Zřizovací služba se může připojit k hardwarovým čipům TPM pro Windows a Linux pomocí tokenu SAS.  Pokud chcete povolit ověřování pomocí TPM, spusťte následující příkaz:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Použití TPM se simulátorem

Pokud nemáte žádné zařízení s čipy TPM, můžete pro účely vývoje v operačním systému Windows použít simulátor.  Pokud chcete povolit ověřování pomocí TPM a spustit simulátor TPM, spusťte následující příkaz:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Sestavení sady SDK
Před vytvořením registrace zařízení sestavte sadu SDK.

### <a name="linux"></a>Linux
- Sestavení sady SDK v Linuxu:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Pokud chcete sestavit binární soubory pro ladění, přidejte odpovídající možnost CMake do příkazu pro generování projektu, například:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Pro sestavení sady SDK je k dispozici řada [možností konfigurace nástroje CMake](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html). Například můžete zakázat některou z dostupných sad protokolů přidáním argumentu do příkazu CMake pro generování projektu:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Můžete také sestavit a spustit test jednotek:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Pokud chcete sestavit sadu SDK ve Windows, proveďte následující kroky a vygenerujte soubory projektu:
    - Otevřete Developer Command Prompt for VS2015.
    - Z kořenu úložiště spusťte následující příkazy CMake:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Tento příkaz sestaví knihovny x86. Pokud chcete provést sestavení pro x64, upravte argument generátoru cmake: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Pokud vygenerování projektu proběhne úspěšně, ve složce `cmake` by se měl zobrazit soubor řešení sady Visual Studio (.sln). Sestavení sady SDK:
    - Otevřete v sadě Visual Studio soubor **cmake\azure_iot_sdks.sln** a sestavte ho, **NEBO**
    - Spusťte následující příkaz na příkazovém řádku, který jste použili k vygenerování souborů projektu:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Pokud chcete sestavit binární soubory pro ladění, použijte odpovídající argument MSBuild: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Pro sestavení sady SDK je k dispozici řada možností konfigurace nástroje CMake. Například můžete zakázat některou z dostupných sad protokolů přidáním argumentu do příkazu CMake pro generování projektu:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Můžete také sestavit a spustit test jednotek:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Knihovny, které byste měli zahrnout
- Součástí vaší sady SDK by měly být tyto knihovny:
    - Zřizovací služba: dps_http_transport, dps_client, dps_security_client
    - Zabezpečení služby IoT Hub: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-dps"></a>Vytvoření položky registrace zařízení ve službě DPS

### <a name="tpm"></a>TPM
Pokud používáte TPM, postupujte podle pokynů v tématu [Vytvoření a zřízení simulovaného zařízení pomocí služby IoT Hub Device Provisioning](./quick-create-simulated-device.md) a vytvořte položku registrace zařízení ve službě DPS a proveďte simulaci prvního spuštění.

### <a name="x509"></a>X**.**509
1. Pokud chcete zaregistrovat zařízení ve zřizovací službě, je potřeba si u každého zařízení poznamenat Ověřovací klíč a ID registrace, které se zobrazí v nástroji pro zřizování poskytovaném klientskou sadou SDK. Spusťte následující příkaz, který vytiskne kořenový certifikát webu (pro skupiny registrace) a certifikát podpisovatele (pro jednotlivé registrace):
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Přihlaste se k webu Azure Portal, v nabídce vlevo klikněte na tlačítko **Všechny prostředky** a otevřete svou službu DPS.
   - X**.**509 – Jednotlivá registrace: V okně s přehledem zřizovací služby vyberte **Správa registrací**. Vyberte kartu **Jednotlivé registrace** a klikněte na tlačítko **Přidat** v horní části. Jako *Mechanismus* ověření identity vyberte **X**.**509** a nahrajte certifikát podpisovatele, jak to okno vyžaduje. Jakmile budete hotovi, klikněte na tlačítko **Uložit**. 
   - X**.**509 – Skupinová registrace: V okně s přehledem zřizovací služby vyberte **Správa registrací**. Vyberte kartu **Skupinové registrace** a klikněte na tlačítko **Přidat** v horní části. Jako *Mechanismus* ověření identity vyberte **X**.**509**, zadejte název skupiny a název certifikátu a nahrajte kořenový certifikát webu, jak to okno vyžaduje. Jakmile budete hotovi, klikněte na tlačítko **Uložit**. 

## <a name="connecting-to-iot-hub-after-provisioning"></a>Připojení ke službě IoT Hub po zřízení

Po zřízení zařízení pomocí zřizovací služby se toto rozhraní API připojí ke službě IoT Hub s využitím režimu ověřování pomocí HSM: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```
