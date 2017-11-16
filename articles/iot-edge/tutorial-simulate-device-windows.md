---
title: "Simulovat Azure IoT Edge v systému Windows | Microsoft Docs"
description: "Instalace modulu runtime Azure IoT Edge v simulovaném zařízení v systému Windows a nasazení první modul"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 08c501b9132bb21f47f099725d1fad5556befb4c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Nasazení v simulovaném zařízení v systému Windows Azure IoT Edge – náhled

Azure IoT Edge přesune power cloudu do zařízení Internet věcí (IoT). Tento kurz vás provede procesem vytvoření simulované zařízení IoT okraj, který generuje data snímačů. Získáte informace o těchto tématech:

![Kurz architektura][2]

Simulovaná zařízení, které vytvoříte v tomto kurzu je monitorování turbínu větru, který generuje teploty a vlhkosti a přetížení data. Co vás zajímá tato data, protože vaše turbín provést na různých úrovních efektivitu v závislosti na počasí. Ostatní kurzy Azure IoT Edge stavět na práci, kterou se zde provádí nasazení moduly, které analyzovat data podnikových statistik. 

## <a name="prerequisites"></a>Požadavky

Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows k simulaci zařízení s Internet věcí. 

>[!TIP]
>Pokud používáte systém Windows ve virtuálním počítači, povolte [vnořené virtualizace] [ lnk-nested] a přidělit nejméně 2 GB paměti. 

1. Ujistěte se, že používáte podporovanou verzi systému Windows:
   * Windows 10 
   * Windows Server
2. Nainstalujte [Docker pro systém Windows] [ lnk-docker] a ujistěte se, že je spuštěna.
3. Nainstalujte [Python 2.7 v systému Windows] [ lnk-python] a ujistěte se, můžete použít příkaz pip.
4. Spusťte následující příkaz se stáhnout skript řízení IoT okraj.

   ```
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT Edge můžete spustit buď kontejnery Windows nebo Linux kontejnerů. Chcete-li použít Windows kontejnery, budete muset spustit:
>    * Windows 10 patří Creators aktualizovat, nebo
>    * Windows Server. 1709 (sestavení 16299), nebo
>    * Jádro IoT Windows (sestavení 16299) na zařízení na x64
>
> Jádro IoT Windows, postupujte podle pokynů v [instalaci modulu runtime IoT Edge na jádro IoT Windows][lnk-install-iotcore]. Jinak jednoduše [konfigurace Docker používat Windows kontejnery][lnk-docker-containers]a volitelně ověřit vaše požadavky pomocí následujícího příkazu prostředí powershell:
>    ```
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

Tento kurz začněte vytvořením služby IoT Hub.
![Vytvoření centra IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Zaregistrovat IoT hraniční zařízení

Zaregistrujte IoT hraniční zařízení se nově vytvořená IoT Hub.
![Registrovat zařízení][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Konfigurace modulu runtime IoT Edge

Instalace a spuštění modulu runtime Azure IoT Edge ve vašem zařízení. 
![Registrovat zařízení][5]

Modul runtime IoT okraj je nasadit na všechna zařízení IoT okraj. Obsahuje dva moduly. Nejprve agenta IoT Edge usnadňuje nasazení a monitorování modulů na IoT hraniční zařízení. Druhý centra IoT Edge spravuje komunikaci mezi modulů na IoT hraniční zařízení a mezi zařízením a IoT Hub. 


Použijte následující postup k instalaci a spuštění modulu runtime hraniční IoT:

1. Konfigurace modulu runtime připojovacím řetězcem IoT hraniční zařízení z předchozí části.

   ```
   iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Spusťte modul runtime.

   ```
   iotedgectl start
   ```

1. Zkontrolujte, zda IoT Edge agenta je spuštěn jako modul Docker.

   ```
   docker ps
   ```

## <a name="deploy-a-module"></a>Nasazení modulu

Spravujte vaše zařízení Azure IoT Edge z cloudu do nasazení modul, který bude posílat telemetrická data do služby IoT Hub.
![Registrovat zařízení][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

V tento rychlý start vytvořit nové zařízení IoT okraj a na něm nainstalován modul runtime IoT okraj. Potom použít portál Azure k modul IoT okraj ke spuštění na zařízení bez nutnosti měnit samotné zařízení. V takovém případě modul, který jste nabídnutých vytvoří prostředí data, která můžete použít pro kurzů k. 

Zobrazení zpráv odesílány z modulu tempSensor:

```cmd/sh
sudo docker logs -f tempSensor
```

Můžete také zobrazit telemetrii zařízení odesílá pomocí [nástroji Průzkumník služby IoT Hub][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili nové zařízení IoT okraj a použít rozhraní cloudu Azure IoT Edge k nasazení kódu do zařízení. Nyní máte simulované zařízení generování nezpracovaná data o jeho prostředí. 

V tomto kurzu je předpokladem pro všechny ostatní kurzy IoT okraj. Můžete pokračovat na všechny ostatní kurzy se dozvíte, jak Azure IoT Edge můžete zapnout tato data do podnikových statistik na hranici.

> [!div class="nextstepaction"]
> [Vývoj a nasazení kódu C# jako modul](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md