---
title: "Rychlý start Azure IoT Edge + Linux | Microsoft Docs"
description: "Vyzkoušení služby Azure IoT Edge spuštěním analýzy na simulované hraniční zařízení"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/16/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: bfa6652eac34f88baf09f55353cf58227a20e4cf
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-linux-device---preview"></a>Rychlý úvod: Nasazení první modul IoT Edge na portálu Azure do zařízení Linux – náhled

Azure IoT Edge přesune power cloudu do zařízení Internet věcí. V tomto tématu se Naučte se používat rozhraní cloudu k nasazení předem kód vzdáleně na IoT hraniční zařízení.

Pokud nemáte aktivní předplatné Azure, vytvořte [bezplatný účet] [ lnk-account] před zahájením.

## <a name="prerequisites"></a>Požadavky

K provedení této úlohy, použijte k simulaci zařízení s Internet věcí počítače nebo virtuálního počítače. Pro úspěšné nasazení IoT hraniční zařízení jsou potřeba následující služby:

- [Nainstalujte Docker v systému Linux] [ lnk-docker-ubuntu] a ujistěte se, že je spuštěna. 
- Většina Linuxových distribucích, včetně Ubuntu, už máte Python 2.7 nainstalována. Použijte následující příkaz a ujistěte se, zda je nainstalován pip: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub-with-azure-cli"></a>Vytvoření služby IoT hub pomocí rozhraní příkazového řádku Azure

Vytvoření služby IoT hub ve vašem předplatném Azure. Bezplatná úroveň služby IoT Hub funguje pro tento rychlý start. Pokud jste dříve používali IoT Hub a už máte bezplatné centrum vytvořit, můžete tuto část přeskočte a přejděte na [zaregistrovat IoT hraniční zařízení][anchor-register]. Každý odběr může mít pouze jeden bezplatné Centrum IoT. 

1. Přihlaste se na web [Azure Portal][lnk-portal]. 
1. Vyberte **cloudové prostředí** tlačítko. 

   ![Tlačítko prostředí cloudu][1]

1. Vytvořte skupinu prostředků. Následující kód vytvoří skupinu prostředků s názvem **IoTEdge** v **západní USA** oblasti:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Vytvoření služby IoT hub v nové skupiny prostředků. Následující kód vytvoří bezplatný **F1** rozbočovače názvem **MyIotHub** ve skupině prostředků **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Zaregistrovat IoT hraniční zařízení

Vytvoření identity zařízení pro simulované zařízení, takže může komunikovat se službou IoT hub. Vzhledem k tomu, že zařízení IoT Edge chovat a lze je spravovat jinak než typické zařízení IoT, je deklarovat to být IoT hraniční zařízení od začátku. 

1. Na portálu Azure přejděte do služby IoT hub.
1. Vyberte **IoT okraj (preview)**.
1. Vyberte **přidání okraj IoT zařízení**.
1. Poskytnout simulovaného zařízení zařízení jedinečné ID.
1. Vyberte **Uložit** abyste přidali svoje zařízení.
1. Vyberte nové zařízení ze seznamu zařízení. 
1. Zkopírujte hodnotu pro **připojovací řetězec – primární klíč** a uložte ho. Tuto hodnotu použijete ke konfiguraci modulu runtime IoT Edge v další části. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Instalace a spuštění modulu runtime IoT Edge

Modul runtime IoT okraj je nasadit na všechna zařízení IoT okraj. Obsahuje dva moduly. Nejprve agenta IoT Edge usnadňuje nasazení a monitorování modulů na IoT hraniční zařízení. Druhý centra IoT Edge spravuje komunikaci mezi modulů na IoT hraniční zařízení a mezi zařízením a IoT Hub. 

Na počítači, kde je potřeba spustit IoT hraniční zařízení Stáhněte skript řízení hraniční IoT:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Konfigurace modulu runtime připojovacím řetězcem IoT hraniční zařízení z předchozí části:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Spusťte modul runtime:
```cmd
sudo iotedgectl start
```

Zkontrolujte Docker, zda je jako modul spuštěn agent IoT Edge:
```cmd
sudo docker ps
```

![V tématu edgeAgent v Docker](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Nasazení modulu

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

V tento rychlý start vytvořit nové zařízení IoT okraj a na něm nainstalován modul runtime IoT okraj. Potom použít portál Azure k modul IoT okraj ke spuštění na zařízení bez nutnosti měnit samotné zařízení. V takovém případě modul, který jste nabídnutých vytvoří prostředí data, která můžete použít pro kurzů k. 

Otevřete příkazový řádek v počítači se systémem simulovaného zařízení znovu. Ověřte, že modul nasazení z cloudu běží na IoT hraniční zařízení:

```cmd
sudo docker ps
```

![Zobrazit tři modulů ve vašem zařízení](./media/tutorial-simulate-device-linux/docker-ps2.png)

Zobrazení zpráv odesílány z modulu tempSensor do cloudu:

```cmd
sudo docker logs -f tempSensor
```

![Zobrazení dat z modulu](./media/tutorial-simulate-device-linux/docker-logs.png)

Můžete také zobrazit telemetrii zařízení odesílá pomocí [nástroji Průzkumník služby IoT Hub][lnk-iothub-explorer]. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete službu IoT Hub, který jste vytvořili, můžete použít [az iot hub odstranění] [ lnk-delete] příkaz k odebrání prostředek a veškerá zařízení s ním spojená:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Další kroky

Jste zjistili, jak nasadit modul IoT okraj do IoT hraniční zařízení. Nyní zkuste nasazení různých typů služeb Azure jako modulů, takže je můžete analyzovat data na hranici. 

* [Nasadit vlastní kód jako modul](tutorial-csharp-module.md)
* [Nasazení funkce Azure jako modul](tutorial-deploy-function.md)
* [Nasazení služby Azure Stream Analytics jako modulu](tutorial-deploy-stream-analytics.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
