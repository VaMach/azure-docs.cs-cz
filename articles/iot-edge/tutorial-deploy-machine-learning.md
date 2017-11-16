---
title: "Nasazení Azure Machine Learning s Azure IoT hranou | Microsoft Docs"
description: "Azure Machine Learning nasadit jako modul pro hraniční zařízení"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 9b8475dcc51fb24fadd1faa4a2008b25a4464080
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Nasazení Azure Machine Learning jako modul IoT Edge – náhled

Moduly IoT Edge můžete nasadit kód, který implementuje obchodní logiku přímo do zařízení IoT okraj. Tento kurz vás provede procesem nasazení modul Azure Machine Learning, který bude předpovídat při selhání na data snímačů na simulované zařízení IoT okraj, který jste vytvořili v nasazení Azure IoT Edge na simulované zařízení na základě zařízení [Windows] [ lnk-tutorial1-win] nebo [Linux] [ lnk-tutorial1-lin] kurzy. Získáte informace o těchto tématech: 

> [!div class="checklist"]
> * Modul služby Azure Machine Learning nasadit do zařízení IoT Edge
> * Vygeneruje zobrazení dat

Pokud chcete použít vlastní [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) modelu ve vašem řešení, které budete [nasadit model](https://aka.ms/aml-iot-edge-doc) pro IoT okraj a hostitele v registru kontejneru jako [registru kontejner Azure](../container-registry/index.yml) nebo Docker.

## <a name="prerequisites"></a>Požadavky

* Azure IoT hraniční zařízení, který jste vytvořili v prvním kurzu nebo rychlý start.
* Připojovací řetězec služby IoT Hub pro službu IoT hub, která zařízení IoT Edge připojí k.
* Kontejner Azure ML

## <a name="create-the-azure-ml-container"></a>Vytvoření kontejneru Azure ML
Pokud chcete vytvořit kontejner vaší Azure ML, postupujte podle pokynů v [AI nástrojů pro Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection).

## <a name="run-the-solution"></a>Spuštění řešení

1. Na [portál Azure](https://portal.azure.com), přejděte do služby IoT hub.
1. Přejděte na **IoT okraj (preview)** a vyberte zařízení IoT okraj.
1. Vyberte **nastavit moduly**.
1. Vyberte **přidání okraj IoT modulu**.
1. V **název** zadejte `tempSensor`.
1. V **Image URI** zadejte `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
1. Další nastavení nezměníte a vyberte **Uložit**.
1. Dále pak na **přidat moduly** krok, vyberte **přidání okraj IoT modulu** znovu.
1. V **název** pole, zadejte název kontejneru, které jste udělali v předchozí části. Odkazovat [AI nástrojů pro Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection) nápovědu k hledání názvu.
1. V **Image** zadejte identifikátor URI kontejneru, který jste udělali v předchozí části bitovou kopii. Odkazovat [AI nástrojů pro Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection) nápovědu k hledání bitovou kopii.
1. Klikněte na **Uložit**.
1. Zpět v **přidat moduly** krok, klikněte na tlačítko **Další**.
1. Aktualizace tras pro modul:
1. V **zadejte trasy** kroku, zkopírujte do textového pole JSON níže. Moduly publikovat všechny zprávy do hraniční runtime. Deklarativní pravidel v modulu runtime definovat, kde se tyto zprávy toku. V tomto kurzu musíte dvě trasy. První trasa je určena k přenosu zprávy teplotní snímač machine learning module prostřednictvím koncového bodu "mlInput", což je koncový bod, který použít všechny moduly Azure Machine Learning. Druhá trasa je určena k přenosu zpráv z modulu learning počítač do služby IoT Hub. V této trase '' mlOutput'' je endput používaných pro výstup dat. všechny moduly Azure Machine Learning a "nadřazeného" je speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Klikněte na **Další**. 
1. V kroku '' šablona kontrolní "klikněte na tlačítko"Odeslat". 
1. Návrat na stránku Podrobnosti o zařízení a klikněte na "Aktualizovat."  Měli byste vidět nové '' machinelearningmodule'' systémem spolu s modul '' tempSensor"a"runtime IoT Edge".

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

 V produktu VS Code, použijte **zobrazení | Příkaz palety... | IoT: Spuštění monitorování D2C Messages** příkazu nabídky ke sledování dat odesílaných ve službě IoT Hub. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili modul IoT Edge používá technologii Azure Machine Learning. Můžete pokračovat na všechny ostatní kurzy informace o další způsoby, které mohou pomoci Azure IoT Edge, že zapněte data do podnikových statistik na hranici.

> [!div class="nextstepaction"]
> [Nasazení funkce Azure jako modul](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md