---
title: "Nasazení služby Azure Stream Analytics s hranou Azure IoT | Microsoft Docs"
description: "Nasazení služby Azure Stream Analytics jako modul pro hraniční zařízení"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5a143bbf7abb5304ac51782d517c02ec184a05a2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Nasazení služby Azure Stream Analytics jako modul IoT Edge – náhled

Zařízení IoT může vytvářet velké objemy dat. Tato data v některých případech může být analyzován nebo zpracovaných před dosažením cloudu ke snížení velikosti odeslaná data nebo omezit čekací doba přenosu řešitelné přehledu.

Okraj IoT využívá předdefinovaných Azure služby IoT Edge modulů pro rychlé nasazení a [Azure Stream Analytics] [ azure-stream] (ASA) je jeden takový modul. Můžete vytvořit úlohu ASA z jeho portálu a pak se na portál služby IoT Hub a nasaďte ho jako modul Edge IoT.  

Azure Stream Analytics poskytuje syntaxe bohatě strukturovaných dotazů pro analýzu dat v cloudu i na IoT hraniční zařízení. Další informace o ASA hranu IoT najdete v tématu [ASA dokumentaci](../stream-analytics/stream-analytics-edge.md).

Tento kurz vás provede procesem vytvoření úlohu služby Azure Stream Analytics a jeho nasazení na IoT hraniční zařízení pro zpracování datového proudu místní telemetrie přímo na zařízení a generování výstrah pro jednotku okamžitý zásah na zařízení.  Zahrnutých v tomto kurzu jsou dva moduly. Modul simulované teplotní snímač (tempSensor) generuje teploty data z 20 do 120 stupňů, zvýší o 1 každých 5 sekund. Modul služby Stream Analytics resetuje tempSensor dosáhne 70-průměru 30 sekund. V produkčním prostředí tato funkce může sloužit k vypnuté na počítači nebo přijmout preventivní opatření, když teplota dosáhne nebezpečná úrovně. 

Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit úlohu ASA zpracováním dat na okraj
> * Připojit nové úlohy ASA s další moduly IoT Edge
> * Nasadit úlohy ASA IoT hraniční zařízení

## <a name="prerequisites"></a>Požadavky

* Centrum IoT 
* Zařízení, které vytvoříte a nakonfigurujete v rychlý start nebo v nasazení Azure IoT Edge om simulované zařízení v [Windows] [ lnk-tutorial1-win] a [Linux] [ lnk-tutorial1-lin]. Je nutné znát klíč připojení zařízení a ID zařízení. 
* Docker spuštění na vašem zařízení IoT Edge
    * [Nainstalujte Docker v systému Windows][lnk-docker-windows]
    * [Nainstalujte Docker v systému Linux][lnk-docker-linux]
* Python 2.7.x na vašem zařízení IoT Edge
    * [Instalace jazyka Python 2.7 v systému Windows][lnk-python].
    * Většina Linuxových distribucích, včetně Ubuntu, už máte Python 2.7 nainstalována.  Použijte následující příkaz a ujistěte se, je nainstalována pip: `sudo apt-get install python-pip`.


## <a name="create-an-asa-job"></a>Umožňuje vytvořit úlohu služby ASA

V této části můžete vytvořit úlohu služby Azure Stream Analytics využít data ze služby IoT hub, dotaz odeslaný telemetrická data ze zařízení a předávat výsledky do Azure Storage kontejneru (BLOB). Další informace najdete v tématu **přehled** části [Stream Analytics dokumentaci][azure-stream]. 

### <a name="create-a-storage-account"></a>vytvořit účet úložiště

Účet služby Azure Storage je nutné k zajištění koncový bod má být použit jako výstup ve vaší úloze ASA. Následující příklad používá typ úložiště objektů BLOB.  Další informace najdete v tématu **objekty BLOB** části [dokumentaci pro Azure Storage][azure-storage].

1. Na portálu Azure přejděte do **vytvořit prostředek** a zadejte `Storage account` v panelu vyhledávání. Vyberte **účet úložiště – objekt blob, soubor, tabulka, fronta**.

2. Zadejte název účtu úložiště a vyberte stejné umístění, kde se nachází služby IoT hub. Klikněte na možnost **Vytvořit**. Pamatujte název pro pozdější.

    ![Nový účet úložiště][1]

3. Přejděte na účet úložiště, kterou jste právě vytvořili. Klikněte na tlačítko **procházet objekty BLOB**. 
4. Vytvořte nový kontejner pro modul ASA ukládat data. Nastavení přístupu na úrovni **kontejneru**. Klikněte na **OK**.

    ![Nastavení úložiště][10]

### <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

1. Na portálu Azure přejděte do **vytvořit prostředek** > **Internet věcí** a vyberte **úlohy Stream Analytics**.

2. Zadejte název, vyberte **Edge** jako hostitelské prostředí a použít zbývající výchozí hodnoty.  Klikněte na možnost **Vytvořit**.

    >[!NOTE]
    >Úlohy ASA IoT hranu nejsou aktuálně podporované v oblasti západní USA 2. 

3. Přejděte do vytvořené úlohy. Vyberte **vstupy** klikněte **přidat**.

4. Alias vstupu zadejte `temperature`, nastavte typ zdroje **datový proud**a použijte výchozí nastavení pro ostatní parametry. Klikněte na možnost **Vytvořit**.

   ![Vstup ASA](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Vyberte **výstupy** klikněte **přidat**.

6. Alias pro výstup zadejte `alert`a použijte výchozí nastavení pro ostatní parametry. Klikněte na možnost **Vytvořit**.

   ![Výstup ASA](./media/tutorial-deploy-stream-analytics/asa_output.png)


7. Vyberte **dotazu**.
8. Výchozí text nahraďte následující dotaz:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```
9. Klikněte na **Uložit**.

## <a name="deploy-the-job"></a>Úloha nasazení

Nyní jste připraveni k nasazení úlohy ASA na vašem zařízení IoT okraj.

1. Na portálu Azure ve službě IoT hub, přejděte na **IoT okraj (preview)** a otevřete stránku podrobností pro vaše zařízení IoT okraj.
1. Vyberte **nastavit moduly**.
1. Pokud jste předtím nasadili modul tempSensor na tomto zařízení, může autopopulate. Pokud ne, použijte následující postup přidání tohoto modulu:
   1. Klikněte na tlačítko **přidání okraj IoT modulu**
   1. Zadejte `tempSensor` jako název, a `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` pro identifikátor URI bitové kopie. 
   1. Další nastavení nezměníte a klikněte na tlačítko **Uložit**.
1. Chcete-li přidat úlohu ASA Edge, vyberte **Import Azure Stream Analytics IoT Edge Module**.
1. Vyberte předplatné a úlohy ASA okraj, který jste vytvořili. 
1. Vyberte předplatné a účet úložiště, který jste vytvořili. Klikněte na **Uložit**.

    ![Sada modulu][6]

1. Název, který byl automaticky vygenerován pro modul ASA zkopírujte. 

    ![teplotní modulu][11]

1. Klikněte na tlačítko **Další** nakonfigurovat trasy.
1. Zkopírujte následující příkaz pro **trasy**.  Nahraďte _{moduleName}_ s názvem modulu, který jste zkopírovali:

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

1. Vraťte se na stránce s podrobnostmi o zařízení a klikněte na tlačítko **aktualizovat**.  Byste měli vidět nového modulu Stream Analytics společně s **IoT Edge agenta** modulu a **IoT Edge hub**.

    ![výstup modulu][7]

## <a name="view-data"></a>Zobrazení dat

Nyní můžete přejít na zařízení IoT Edge rezervovat interakce mezi modul ASA a modul tempSensor.

Zkontrolujte, zda jsou spuštěny všechny moduly v Docker:

   ```cmd/sh
   docker ps  
   ```

   ![výstup docker][8]

Zobrazit všechna data metrik a protokolování systému. Použijte název modulu Stream Analytics:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Nyní byste měli mít podívejte se na tento počítač teploty postupně roste, dokud nebude dosaženo 70 stupňů po dobu 30 sekund. Pak spustí modul Stream Analytics resetování a teploty počítač vrátí k 21. 

   ![docker protokolu][9]


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali kontejner Azure Storage a úlohu streamování Analytics k analýze dat ze zařízení IoT okraj.  Pak načten vlastní modul ASA pro přesun dat ze zařízení prostřednictvím datového proudu, do objektu BLOB pro stahování.  Můžete pokračovat na další kurzy zobrazíte další jak Azure IoT okraj můžete vytvořit řešení pro vaši organizaci.

> [!div class="nextstepaction"] 
> [Nasadit model Azure Machine Learning jako modul][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
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