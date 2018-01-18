---
title: "Nasazení modulů na IoT hraniční zařízení pomocí rozšíření IoT 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Nasazení modulů na IoT hraniční zařízení pomocí IoT rozšíření pro Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Nasazení modulů na IoT hraniční zařízení pomocí IoT rozšíření pro Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) je open source pro různé platformy nástroj příkazového řádku pro správu prostředků Azure, jako je například IoT okraj. Azure CLI 2.0 je k dispozici v systému Windows, Linux a systému MacOS.

Azure CLI 2.0 umožňuje spravovat prostředky Azure IoT Hub, instance služby zřizování zařízení a propojené rozbočovače mimo pole. Nové rozšíření IoT vylepšuje 2.0 rozhraní příkazového řádku Azure s funkcemi, jako je Správa zařízení a úplných možností IoT okraj.

V tomto kurzu nejprve dokončit postup nastavení Azure CLI 2.0 a rozšíření IoT. Potom můžete naučit se nasazovat modulů na IoT hraniční zařízení pomocí dostupné příkazy rozhraní příkazového řádku.

## <a name="installation"></a>Instalace 

### <a name="step-1---install-python"></a>Krok 1 – instalace Python

[Python 2.7 x nebo Python 3.x](https://www.python.org/downloads/) je vyžadován.

### <a name="step-2---install-azure-cli-20"></a>Krok 2 – nainstalovat rozhraní příkazového řádku Azure 2.0

Postupujte podle [pokyny k instalaci](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) nastavení Azure CLI 2.0 ve vašem prostředí. Minimálně musí být vaší verzi 2.0 rozhraní příkazového řádku Azure 2.0.24 nebo vyšší. Použití `az –version` k ověření. Tato verze podporuje az rozšíření příkazy a zavádí rozhraní Knack příkaz. Jeden způsob, jak nainstalovat v systému Windows je ke stažení a instalaci [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Krok 3: instalace IoT rozšíření

[V souboru readme rozšíření IoT](https://github.com/Azure/azure-iot-cli-extension) popisuje několik způsobů, jak nainstalovat rozšíření. Nejjednodušší způsob, jak se má spustit `az extension add --name azure-cli-iot-ext`. Po instalaci, můžete použít `az extension list` ověření aktuálně nainstalovaná rozšíření nebo `az extension show --name azure-cli-iot-ext` zobrazíte podrobnosti o rozšíření IoT. Chcete-li odebrat rozšíření, můžete použít `az extension remove --name azure-cli-iot-ext`.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Nasazení modulů na IoT hraniční zařízení
V tomto kurzu se dozvíte, jak vytvořit nasazení služby IoT okraj. V příkladu se dozvíte, jak přihlášení k účtu Azure, vytvořte skupinu prostředků Azure (kontejner, který obsahuje související prostředky pro řešení s Azure), vytvoření služby IoT Hub, vytvořit tři identitu zařízení IoT Edge, nastavte značky a pak vytvořit nasazení služby IoT Edge Cílem těchto zařízení. Kroky instalace popsané dříve než začnete. Pokud nemáte účet Azure, ale můžete [vytvořit bezplatný účet](https://azure.microsoft.com/free/?v=17.39a) ještě dnes. 


### <a name="1-login-to-the-azure-account"></a>1. Přihlášení k účtu Azure
  
    az login

![přihlášení][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Vytvořte skupinu prostředků IoTHubBlogDemo v eastus

    az group create -l eastus -n IoTHubBlogDemo

![Vytvoření skupiny prostředků][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Vytvoření IoT Hub blogDemoHub ve skupině nově vytvořený prostředek

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Vytvoření centra IoT][3]


### <a name="4-create-an-iot-edge-device"></a>4. Vytvoření IoT hraniční zařízení

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![Vytvoření zařízení IoT Edge][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. Použít konfiguraci na IoT hraniční zařízení

Nasazení šablony JSON uložte místně jako souboru txt. Když spustíte příkaz použít konfiguraci budete potřebovat cestu k souboru.

Zde jsou šablonu JSON ukázkové nasazení, která obsahuje jeden modul tempSensor:

```json
{
  "moduleContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": ""
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
              "createOptions": "{}"
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
              "createOptions": "{}"
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 7200
        }
      }
    },
    "tempSensor": {
      "properties.desired": {}
    }
  }
}
```

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Použít konfiguraci][5]

### <a name="6-list-modules"></a>6. Seznam modulů
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Seznam modulů][6]

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili funkce Azure, který obsahuje kód pro filtrování nezpracovaná data generována zařízení IoT okraj. Chcete-li zachovat zkoumat Azure IoT Edge, další informace o použití IoT hraniční zařízení jako brána. 

> [!div class="nextstepaction"]
> [Vytvoření IoT hraniční zařízení brány](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG

