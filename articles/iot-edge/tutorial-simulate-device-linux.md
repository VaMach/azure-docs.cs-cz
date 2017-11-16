---
title: "Simulovat Azure IoT Edge v systému Linux | Microsoft Docs"
description: "Instalace modulu runtime Azure IoT Edge v simulovaném zařízení v systému Linux a nasazení první modul"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>Nasazení Azure IoT Edge v simulovaném zařízení v systému Linux – náhled

Azure IoT Edge umožňuje provádět analýzy a zpracování dat v zařízeních, aniž by museli push všechna data do cloudu. Kurzy IoT Edge ukazují, jak nasadit různé typy modulů, ze služby Azure nebo vlastní kód, ale nejdřív je potřeba zařízení, které chcete otestovat. 

Tento kurz vás provede procesem vytvoření simulovaného zařízení IoT Edge, pak nasazení modul, který generuje data snímačů. Získáte informace o těchto tématech:

![Kurz architektura][2]

Simulovaná zařízení, které vytvoříte v tomto kurzu je monitorování, které generuje teploty a vlhkosti a přetížení data. Ostatní kurzy Azure IoT Edge stavět na práci, kterou se zde provádí nasazení moduly, které analyzovat data podnikových statistik. 

## <a name="prerequisites"></a>Požadavky

Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Linux k simulaci zařízení s Internet věcí. Pro úspěšné nasazení IoT hraniční zařízení jsou potřeba následující služby:

- [Instalace Docker pro Linux] [ lnk-docker-ubuntu] a ujistěte se, že je spuštěna. 
- Většina Linuxových distribucích, včetně Ubuntu, už máte Python 2.7 nainstalována. Použijte následující příkaz a ujistěte se, zda je nainstalován pip: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

Tento kurz začněte vytvořením služby IoT Hub.
![Vytvoření centra IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Zaregistrovat IoT hraniční zařízení

Zaregistrujte IoT hraniční zařízení se nově vytvořená IoT Hub.
![Registrovat zařízení][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Instalace a spuštění modulu runtime IoT Edge

Instalace a spuštění modulu runtime Azure IoT Edge ve vašem zařízení. 
![Registrovat zařízení][5]

Modul runtime IoT okraj je nasadit na všechna zařízení IoT okraj. Obsahuje dva moduly. Nejprve agenta IoT Edge usnadňuje nasazení a monitorování modulů na IoT hraniční zařízení. Druhý centra IoT Edge spravuje komunikaci mezi modulů na IoT hraniční zařízení a mezi zařízením a IoT Hub. 

Použijte následující postup k instalaci a spuštění modulu runtime hraniční IoT:

1. Na počítači, kde je potřeba spustit IoT hraniční zařízení Stáhněte skript řízení IoT okraj.

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. Konfigurace modulu runtime připojovacím řetězcem IoT hraniční zařízení z předchozí části.

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Spusťte modul runtime.

   ```
   sudo iotedgectl start
   ```

1. Zkontrolujte, zda IoT Edge agenta je spuštěn jako modul Docker.

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>Nasazení modulu

Spravujte vaše zařízení Azure IoT Edge z cloudu do nasazení modul, který bude posílat telemetrická data do služby IoT Hub.
![Registrovat zařízení][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

V tento rychlý start vytvořit nové zařízení IoT okraj a na něm nainstalován modul runtime IoT okraj. Potom použít portál Azure k modul IoT okraj ke spuštění na zařízení bez nutnosti měnit samotné zařízení. V takovém případě modul, který jste nabídnutých vytvoří prostředí data, která můžete použít pro kurzů k. 

Zobrazení zpráv odesílány z modulu tempSensor:

```cmd/sh
docker logs -f tempSensor
```

Můžete také zobrazit telemetrii zařízení odesílá pomocí [nástroji Průzkumník služby IoT Hub][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili nové zařízení IoT okraj a použít rozhraní cloudu Azure IoT Edge k nasazení kódu do zařízení. Nyní máte simulované zařízení generování nezpracovaná data o jeho prostředí. 

V tomto kurzu je předpokladem pro všechny ostatní kurzy IoT okraj. Můžete pokračovat na všechny ostatní kurzy se dozvíte, jak Azure IoT Edge můžete zapnout tato data do podnikových statistik na hranici.

> [!div class="nextstepaction"]
> [Vývoj a nasazení kódu C# jako modul](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
