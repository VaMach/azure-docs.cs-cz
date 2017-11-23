---
title: "Nasazení služby Azure Stream Analytics s hranou Azure IoT | Microsoft Docs"
description: "Nasazení služby Azure Stream Analytics jako modul pro hraniční zařízení"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0d19d1142cf15221f84692f7e613edd6b46b4083
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Nasazení služby Azure Stream Analytics jako modul IoT Edge – náhled

Zařízení IoT může vytvářet velké objemy dat. Tato data v některých případech může být analyzován nebo zpracovaných před dosažením cloudu ke snížení velikosti odeslaná data nebo omezit čekací doba přenosu řešitelné přehledu.

[Azure Stream Analytics] [ azure-stream] (ASA) poskytuje syntaxe bohatě strukturovaných dotazů pro analýzu dat v cloudu i na IoT hraniční zařízení. Další informace o ASA hranu IoT najdete v tématu [ASA dokumentaci](../stream-analytics/stream-analytics-edge.md).

Tento kurz vás provede procesem vytvoření úlohu služby Azure Stream Analytics a jeho nasazení na IoT hraniční zařízení pro zpracování datového proudu místní telemetrie přímo na zařízení a generování výstrah pro jednotku okamžitý zásah na zařízení.  Zahrnutých v tomto kurzu jsou dva moduly. Modul simulované teplotní snímač (tempSensor), který generuje teploty data z 20 do 120 stupňů, zvýší o 1 každých 5 sekund a modul ASA, který odfiltruje z teploty větší než 100 stupňů. Modul ASA také obnoví tempSensor, když průměru 30 sekund dosáhne 100.

Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit úlohu ASA zpracováním dat na okraj
> * Připojit nové úlohy ASA s další moduly IoT Edge
> * Nasadit úlohy ASA IoT hraniční zařízení

## <a name="prerequisites"></a>Požadavky

* Centrum IoT 
* Zařízení, které vytvoříte a nakonfigurujete v rychlý start nebo v nasazení Azure IoT Edge om simulované zařízení v [Windows] [ lnk-tutorial1-win] a [Linux] [ lnk-tutorial1-lin].
* Docker na vašem zařízení IoT Edge
    * [V systému Windows nainstalujte Docker] [ lnk-docker-windows] a ujistěte se, že je spuštěna.
    * [Nainstalujte Docker v systému Linux] [ lnk-docker-linux] a ujistěte se, že je spuštěna.
* Python 2.7.x na vašem zařízení IoT Edge
    * [Instalace jazyka Python 2.7 v systému Windows][lnk-python].
    * Většina Linuxových distribucích, včetně Ubuntu, už máte Python 2.7 nainstalována.  Použijte následující příkaz a ujistěte se, je nainstalována pip: `sudo apt-get install python-pip`.

> [!NOTE]
> Budou vaše zařízení připojovací řetězec a ID zařízení IoT Edge pro tento kurz nezbytné.

Okraj IoT využívá předdefinovaných Azure služby IoT Edge modulů pro rychlé nasazení a Azure Stream Analytics (ASA) je jeden takový modul. Můžete vytvořit úlohu ASA z jeho portálu a pak se na portál služby IoT Hub a nasaďte ho jako modul Edge IoT.  

Další informace o Azure Stream Analytics najdete v tématu **přehled** části [Stream Analytics dokumentaci][azure-stream].

## <a name="create-an-asa-job"></a>Umožňuje vytvořit úlohu služby ASA

V této části můžete vytvořit úlohu služby Azure Stream Analytics využít data ze služby IoT hub, dotaz odeslaný telemetrická data ze zařízení a předávat výsledky do Azure Storage kontejneru (BLOB). Další informace najdete v tématu **přehled** části [Stream Analytics dokumentaci][azure-stream]. 

> [!NOTE]
> Účet služby Azure Storage je nutné k zajištění koncový bod má být použit jako výstup ve vaší úloze ASA. Následující příklad používá typ úložiště objektů BLOB.  Další informace najdete v tématu **objekty BLOB** části [dokumentaci pro Azure Storage][azure-storage].

1. Na portálu Azure přejděte do **vytvořit prostředek -> úložiště**, klikněte na tlačítko **zobrazit všechny**a klikněte na tlačítko **účet úložiště – objekt blob, soubor, tabulka, fronta**.

2. Zadejte název účtu úložiště a vyberte stejné umístění, kde jsou uložené služby IoT Hub. Klikněte na možnost **Vytvořit**. Poznamenejte si název pro pozdější.

    ![Nový účet úložiště][1]

3. Na portálu Azure přejděte na účet úložiště, kterou jste právě vytvořili. Klikněte na tlačítko **procházet objekty BLOB** pod **služby objektů Blob**. 
4. Vytvořte nový kontejner pro modul ASA ukládat data. Nastavení přístupu na úrovni _kontejneru_. Klikněte na **OK**.

    ![Nastavení úložiště][10]

5. Na portálu Azure přejděte do **vytvořit prostředek** > **Internet věcí** a vyberte **úlohy Stream Analytics**.

2. Zadejte název, vyberte **Edge** jako hostitelské prostředí a použít zbývající výchozí hodnoty.  Klikněte na možnost **Vytvořit**.

    >[!NOTE]
    >Úlohy ASA IoT hranu nejsou aktuálně podporované v oblasti USA – západ 2. Vyberte prosím jiné umístění.

    ![Vytvoření ASA][5]

2. Přejděte do úlohu vytvořené v části **úlohy topologie**, vyberte **vstupy**, klikněte na tlačítko **přidat**.

3. Zadejte název `temperature`, zvolte **datový proud** jako typ zdroje a použít výchozí nastavení pro ostatní parametry. Klikněte na možnost **Vytvořit**.

    ![Vstup ASA][2]

    > [!NOTE]
    > Další vstupy mohou zahrnovat specifické koncové body okraje IoT.

4. V části **úlohy topologie**, vyberte **výstupy**, klikněte na tlačítko **přidat**.

5. Zadejte název `alert` a použijte výchozí nastavení. Klikněte na možnost **Vytvořit**.

    ![Výstup ASA][3]

6. V části **úlohy topologie**, vyberte **dotazu**a zadejte následující:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 100
    ```

## <a name="deploy-the-job"></a>Úloha nasazení

Nyní jste připraveni k nasazení úlohy ASA na vašem zařízení IoT okraj.

1. Na portálu Azure ve službě IoT Hub, přejděte na **IoT okraj (preview)** a otevřete váš *{deviceId}*na okno.

1. Vyberte **nastavit moduly**, pak vyberte **Import Azure služby IoT Edge Module**.

1. Vyberte předplatné a úlohy ASA okraj, který jste vytvořili. Potom vyberte účet úložiště. Klikněte na **Uložit**.

    ![Sada modulu][6]

1. Klikněte na tlačítko **přidat modul Edge IoT** pro přidání modulu senzoru teploty. Zadejte _tempSensor_ pro název, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` pro adresa URL obrázku. Další nastavení nezměníte a klikněte na tlačítko **Uložit**.

    ![teplotní modulu][11]

1. Název modulu ASA zkopírujte. Klikněte na tlačítko **Další** nakonfigurovat trasy.

1. Zkopírujte následující příkaz pro **trasy**.  Nahraďte _{moduleName}_ s názvem modulu jste zkopírovali:

    ```json
    {
        "routes": {                                                               
          "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream", 
          "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream", 
          "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")", 
          "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")" 
        }
    }
    ```

1. Klikněte na **Další**.

1. V **šablona kontrolní** krok, klikněte na tlačítko **odeslání**.

1. Vraťte se na stránce s podrobnostmi o zařízení a klikněte na tlačítko **aktualizovat**.  Byste měli vidět, že se vám nové _{moduleName}_ modulu společně s **IoT Edge agenta** modulu a **IoT Edge hub**.

    ![výstup modulu][7]

## <a name="view-data"></a>Zobrazení dat

Nyní můžete přejít na zařízení IoT Edge rezervovat interakce mezi modul ASA a modul tempSensor.

1. Na příkazovém řádku konfigurace modulu runtime připojovacím řetězcem IoT hraniční zařízení:

    ```cmd/sh
    iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords  
    ```

1. Spusťte příkaz ke spuštění modulu runtime:

    ```cmd/sh
    iotedgectl start  
    ```

1. Spusťte příkaz zobrazíte modulů spuštěných:

    ```cmd/sh
    docker ps  
    ```

    ![výstup docker][8]

1. Spusťte příkaz zobrazíte všechny protokoly systému a data metriky. Použijte název modulu z výše:

    ```cmd/sh
    docker logs -f {moduleName}  
    ```

    ![docker protokolu][9]

1. Na portálu Azure v účtu úložiště v části **služby objektů Blob**, klikněte na tlačítko **procházet objekty BLOB**, vyberte vašeho kontejneru a vyberte nově vytvořený soubor JSON.

1. Klikněte na tlačítko **Stáhnout** a zobrazit výsledky.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali kontejner Azure Storage a úlohu streamování Analytics k analýze dat ze zařízení IoT okraj.  Pak načten vlastní modul ASA pro přesun dat ze zařízení prostřednictvím datového proudu, do objektu BLOB pro stahování.  Můžete pokračovat na další kurzy zobrazíte další jak Azure IoT okraj můžete vytvořit řešení pro vaši organizaci.

> [!div class="nextstepaction"] 
> [Nasadit model Azure Machine Learning jako modul][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[2]: ./media/tutorial-deploy-stream-analytics/asa_input.png
[3]: ./media/tutorial-deploy-stream-analytics/asa_output.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/en-us/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/en-us/azure/storage/
[azure-stream]: https://docs.microsoft.com/en-us/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/