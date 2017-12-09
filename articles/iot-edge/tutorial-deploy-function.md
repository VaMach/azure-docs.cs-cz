---
title: "Nasazení funkce Azure s Azure IoT hranou | Microsoft Docs"
description: "Nasazení funkce Azure jako modul pro hraniční zařízení"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Nasazení funkce Azure jako modul IoT Edge – náhled
Azure Functions můžete použít k nasazení kód, který implementuje obchodní logiku přímo do zařízení IoT okraj. Tento kurz vás provede vytvoření a nasazení funkce Azure, který filtruje data snímačů na simulované zařízení IoT okraj, který jste vytvořili v nasazení Azure IoT Edge na simulované zařízení na [Windows] [ lnk-tutorial1-win]nebo [Linux] [ lnk-tutorial1-lin] kurzy. V tomto kurzu se naučíte:     

> [!div class="checklist"]
> * Vytvoření funkce Azure pomocí sady Visual Studio Code
> * Použít VS Code a Docker na vytvoření bitové kopie Docker a publikujete ho v registru 
> * Nasazení modulu do vašeho zařízení IoT Edge
> * Vygeneruje zobrazení dat


Funkce Azure, který vytvoříte v tomto kurzu filtruje data teploty generována zařízení a pouze pošle proti proudu zpráv do služby Azure IoT Hub, když je nad určenou prahovou hodnotou. 

## <a name="prerequisites"></a>Požadavky

* Azure IoT hraniční zařízení, který jste vytvořili v předchozí kurz na rychlý start.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# pro rozšíření Visual Studio Code (používá technologii OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT Edge rozšíření pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). Pro tento kurz stačí Community Edition (CE) pro vaši platformu. 
* [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
V tomto kurzu použijete modul sestavení a vytvoření rozšíření Azure IoT Edge pro VS Code **kontejneru image** z souborů. Pak push k této bitové kopie **registru** která uchovává a spravuje vaše Image. Nakonec nasazení bitové kopie z registru ke spuštění na vašem zařízení IoT okraj.  

Žádné kompatibilní Docker registru můžete použít pro tento kurz. Jsou dvě oblíbených Docker registru služby k dispozici v cloudu [registru kontejner Azure](https://docs.microsoft.com/azure/container-registry/) a [úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Tento kurz používá registru kontejner Azure. 

1. V [portál Azure](https://portal.azure.com), vyberte **vytvořit prostředek** > **kontejnery** > **registru kontejner Azure** .
2. Pojmenujte registr, vyberte předplatné, vybrat skupinu prostředků a nastavte verze SKU **základní**. 
3. Vyberte **Vytvořit**.
4. Po vytvoření kontejneru registr, přejděte k němu a vyberte **přístupové klíče**. 
5. Přepnutí **uživatel s oprávněními správce** k **povolit**.
6. Zkopírujte hodnoty pro **přihlášení na server**, **uživatelské jméno**, a **heslo**. Tyto hodnoty budete používat později v tomto kurzu. 

## <a name="create-a-function-project"></a>Vytvoření projektu – funkce
Následující kroky ukazují, jak vytvořit funkce IoT Edge pomocí kódu v jazyce Visual Studio a rozšíření Azure IoT okraj.
1. Otevřete Visual Studio Code.
2. Otevřete terminál integrované VS Code, vyberte **zobrazení** > **integrované terminálu**.
3. Instalaci (nebo aktualizaci) **AzureIoTEdgeFunction** šablony v dotnet, spusťte následující příkaz v integrovaném terminálu:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Vytvoření projektu pro nového modulu. Následující příkaz vytvoří složce projektu **FilterFunction**, v aktuální pracovní složku:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Vyberte **soubor** > **otevřete složku**, vyhledejte **FilterFunction** složky a otevřete projekt v produktu VS Code.
4. V Průzkumníku VS Code rozbalte **EdgeHubTrigger Csharp** složku, otevřete **run.csx** souboru.
5. Obsah souboru nahraďte následujícím kódem:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
   ```

11. Uložte soubor.

## <a name="publish-a-docker-image"></a>Publikování bitovou kopii Docker

1. Vytvořte bitovou kopii Docker.
    1. V Průzkumníku VS Code rozbalte **Docker** složky. Poté rozbalte složku pro vaši platformu kontejneru buď **linux x64** nebo **windows nano**. 
    2. Klikněte pravým tlačítkem myši **soubor Docker** souboru a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**. 
    3. Přejděte na **FilterFunction** složky projektu a klikněte na tlačítko **vyberte složku jako EXE_DIR**. 
    4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filterfunction:latest`. Adresa registru kontejneru je stejný jako server přihlášení, který jste zkopírovali z registru. Musí být ve tvaru `<your container registry name>.azurecr.io`.
 
4. Přihlaste se k Docker. V integrovaném terminálu zadejte následující příkaz: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Uživatelské jméno, heslo a přihlášení serveru pro použití v tomto příkazu najdete na [portál Azure] (https://portal.azure.com). Z **všechny prostředky**, klikněte na dlaždici pro váš kontejner Azure registru otevřete jeho vlastnosti a pak klikněte na **přístupové klíče**. Zkopírujte hodnoty v **uživatelské jméno**, **heslo**, a **přihlášení na server** pole. 

3. Do úložiště Docker push bitovou kopii. Vyberte **zobrazení** > **příkaz palety...**  vyhledejte **Edge: Push IoT Edge modulu Docker image**.
4. Automaticky otevírané okno textového pole zadejte stejný název bitové kopie, který jste použili v kroku 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Přidejte pověření registru do hraniční zařízení
Přidáte přihlašovací údaje pro vaše registru do hraniční runtime v počítači, na kterém je spuštěn hraniční zařízení. To dává runtime přístup ke kontejneru pro vyžádání obsahu. 

- Pro systém Windows spusťte následující příkaz:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Pro Linux spusťte následující příkaz:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Spuštění řešení

1. V **portál Azure**, přejděte do služby IoT hub.
2. Přejděte na **IoT okraj (preview)** a vyberte zařízení IoT okraj.
1. Vyberte **nastavit moduly**. 
2. Pokud jste už nasazená **tempSensor** modul s tímto zařízením ji může automaticky doplněno. Pokud ne, použijte následující postup je přidat:
    1. Vyberte **přidání okraj IoT modulu**.
    2. V **název** zadejte `tempSensor`.
    3. V **Image URI** zadejte `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Další nastavení nezměníte a klikněte na tlačítko **Uložit**.
1. Přidat **filterFunction** modulu.
    1. Vyberte **přidat modul Edge IoT** znovu.
    2. V **název** zadejte `filterFunction`.
    3. V **Image** pole, zadejte adresu bitové kopie, třeba `<docker registry address>/filterfunction:latest`.
    74. Klikněte na **Uložit**.
2. Klikněte na **Další**.
3. V **zadejte trasy** kroku, zkopírujte do textového pole JSON níže. První trasa je určena k přenosu zpráv z teploty senzoru modulu filtru prostřednictvím koncového bodu "input1". Druhá trasa je určena k přenosu zpráv z modulu filtru do služby IoT Hub. V této trase `$upstream` je speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klikněte na **Další**.
5. V **šablona kontrolní** krok, klikněte na tlačítko **odeslání**. 
6. Návrat na stránku Podrobnosti o zařízení IoT okraj a klikněte na tlačítko **aktualizovat**. Byste měli vidět, že se vám nové **filterfunction** modulu společně s **tempSensor** modulu a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

K monitorování zařízení na cloud zprávy odeslané ze zařízení IoT okraj do služby IoT hub:
1. Rozšíření Azure IoT Toolkit nakonfigurujte připojovací řetězec služby IoT hub: 
    1. Na portálu Azure přejděte do služby IoT hub a vyberte **zásady sdíleného přístupu**. 
    2. Vyberte **iothubowner** zkopírujte hodnotu **připojovací řetězec primární klíč**.
    1. V Průzkumníku VS Code, klikněte na tlačítko **zařízení IOT HUB** a pak klikněte na **...** . 
    1. Vyberte **nastavit připojovací řetězec centra IoT** a zadejte připojovací řetězec služby Iot Hub v místním okně. 

1. Chcete-li monitorovat dat odesílaných do služby IoT hub, vyberte **zobrazení** > **příkaz palety...**  a vyhledejte **IoT: spuštění monitorování D2C zpráva**. 
2. Chcete-li zastavit monitorování dat, použijte **IoT: zastavení monitorování D2C zpráva** v příkazu palety. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili funkce Azure, který obsahuje kód pro filtrování nezpracovaná data generována zařízení IoT okraj. Chcete-li zachovat zkoumat Azure IoT Edge, další informace o použití IoT hraniční zařízení jako brána. 

> [!div class="nextstepaction"]
> [Vytvoření IoT hraniční zařízení brány](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
