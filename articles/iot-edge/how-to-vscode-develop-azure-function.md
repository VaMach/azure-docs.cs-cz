---
title: "Pomocí kódu v jazyce Visual Studio pro vývoj a nasazení Azure Functions okraj Azure IoT | Microsoft Docs"
description: "Vývoj a nasazení C# Azure Functions s Azure IoT hranou v produktu VS Code aniž by se přepnula kontextu"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 219474a4577a76f5ceb9a9efaa3c349d633de047
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-develop-and-deploy-azure-functions-to-azure-iot-edge"></a>Pomocí kódu v jazyce Visual Studio pro vývoj a nasazení Azure Functions okraj Azure IoT

Tento článek obsahuje podrobné pokyny pro používání [Visual Studio Code](https://code.visualstudio.com/) jako hlavní vývojový nástroj k vývoji a nasazení Azure Functions na IoT okraj. 

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows nebo Linux jako vývojovém počítači. Zařízení IoT Edge může být jiné fyzické zařízení nebo zařízení IoT Edge můžete simulovat na vývojovém počítači.

Ujistěte se, že máte dokončené následující kurzy, než začnete v tomto návodu.
- Nasazení v simulovaném zařízení v Azure IoT Edge [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) nebo [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Nasazení služby Azure Functions](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function)

Zde je uveden kontrolní seznam, který obsahuje položky že byste měli mít po dokončení předchozí kurzy.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge rozšíření pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# pro rozšíření Visual Studio Code (používá technologii OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [Skript IoT okraj ovládacího prvku](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Šablona AzureIoTEdgeFunction (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
- Aktivní Centrum IoT se alespoň IoT hraniční zařízení.

Doporučujeme také nainstalovat [Docker podporu pro VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) modulu bitové kopie a kontejnery správy.

> [!NOTE]
> V současné době funkce Azure IoT hranu podporuje pouze C#.

## <a name="deploy-azure-iot-functions-in-vs-code"></a>Nasazení funkce Azure IoT v produktu VS Code
V kurzu [nasazení Azure Functions](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function), aktualizovat, vytvářet a publikovat funkce modulu obrázků v produktu VS Code a potom navštivte portál Azure k nasazení Azure Functions. Tato část obsahuje postupy použijte k nasazení a monitorování Azure Functions VS Code.

### <a name="start-a-local-docker-registry"></a>Spustit místní docker registru
Žádné kompatibilní Docker registru můžete použít pro tento kurz. Jsou dvě oblíbených Docker registru služby k dispozici v cloudu [registru kontejner Azure](https://docs.microsoft.com/azure/container-registry/) a [úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Tato část používá [místním registru Docker](https://docs.docker.com/registry/deploying/), což je snazší pro účely testování během vaší časná vývoje.
V produktu VS Code **integrované Terminálové**(Ctrl + '), spusťte následující příkazy a spusťte místní registru.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Výše příklad ukazuje, konfigurace registru, které jsou vhodné pro testování. Produkční prostředí registru musí být chráněny pomocí protokolu TLS a v ideálním případě by měl použít mechanismus řízení přístupu. Doporučujeme použít [registru kontejner Azure](https://docs.microsoft.com/azure/container-registry/) nebo [úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) nasazení moduly IoT Edge produkční prostředí.

### <a name="create-a-function-project"></a>Vytvoření projektu – funkce
Následující kroky zobrazení můžete jak vytvořit modul IoT Edge založené na rozhraní .NET základní 2.0 pomocí kódu v jazyce Visual Studio a rozšíření Azure IoT okraj. Pokud jste v předchozím kurz dokončili v této části, můžete tuto část bezpečně přeskočit.

1. V sadě Visual Studio Code vyberte **zobrazení** > **integrované terminálu** otevřete integrované terminálu VS Code.
2. Instalaci (nebo aktualizaci) **AzureIoTEdgeFunction** šablony v dotnet, spusťte následující příkaz v integrovaném terminálu:

   ```cmd/sh
   dotnet new -i Microsoft.Azure.IoT.Edge.Function
   ```
3. Vytvoření projektu pro nového modulu. Následující příkaz vytvoří složce projektu **FilterFunction**, v aktuální pracovní složku:

   ```cmd/sh
   dotnet new aziotedgefunction -n FilterFunction
   ```
 
4. Vyberte **soubor > Otevřít složku**, vyhledejte **FilterFunction** složky a otevřete projekt v produktu VS Code.
5. Vyhledejte **FilterFunction** složky a klikněte na tlačítko **vyberte složku** otevřete projekt v produktu VS Code.
6. V Průzkumníku VS Code rozbalte **EdgeHubTrigger Csharp** složku, otevřete **run.csx** souboru.
7. Obsah souboru nahraďte následujícím kódem:

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

8. Uložte soubor.

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Vytvoření bitové kopie Docker a publikujete ho v registru

1. V Průzkumníku VS Code rozbalte **Docker** složky. Poté rozbalte složku pro vaši platformu kontejneru buď **linux x64** nebo **windows nano**.
2. Klikněte pravým tlačítkem myši **soubor Docker** souboru a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**. 

    ![Sestavení docker bitové kopie](./media/how-to-vscode-develop-csharp-function/build-docker-image.png)

3. Přejděte na **FilterFunction** složky projektu a klikněte na tlačítko **vyberte složku jako EXE_DIR**. 
4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filterfunction:latest`. Když nasazujete do místního registru, měla by být `localhost:5000/filterfunction:latest`.
5. Do úložiště Docker push bitovou kopii. Použití **Edge: Push IoT Edge modulu Docker image** příkaz a zadejte adresu URL bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejnou adresu URL obrázku jste použili v výše krok.
    ![Push docker image](./media/how-to-vscode-develop-csharp-function/push-image.png)

### <a name="deploy-your-function-to-iot-edge"></a>Nasazení funkce IoT okraj

1. Otevřete `deployment.json` souboru, nahraďte **moduly** část s následující obsah:
   ```json
   "tempSensor": {
      "version": "1.0",
      "type": "docker",
      "status": "running",
      "restartPolicy": "always",
      "settings": {
         "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
         "createOptions": ""
      }
   },
   "filterfunction": {
      "version": "1.0",
      "type": "docker",
      "status": "running",
      "restartPolicy": "always",
      "settings": {
         "image": "localhost:5000/filterfunction:latest",
         "createOptions": ""
      }
   }
   ```

2. Nahraďte **trasy** část s následující obsah:
   ```json
       "routes":{
           "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
           "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
   ```
   > [!NOTE]
   > Deklarativní pravidel v modulu runtime definovat, kde se tyto zprávy toku. V tomto kurzu musíte dvě trasy. První trasy určena k přenosu zprávy teplotní snímač funkce filtru prostřednictvím koncového bodu "input1", což je koncový bod, který jste nakonfigurovali s FilterMessages obslužnou rutinou. Druhá trasa je určena k přenosu zpráv z funkce filtru do služby IoT Hub. V této trase je nadřazený speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub.

3. Uložte tento soubor.
4. V příkazu palety, vyberte **Edge: vytvoření nasazení pro hraniční zařízení**. Pak vyberte ID zařízení IoT Edge k vytvoření nasazení. Klikněte pravým tlačítkem na ID zařízení v seznamu zařízení a vyberte **vytvořit nasazení pro hraniční zařízení**.

    ![Vytvoření nasazení](./media/how-to-vscode-develop-csharp-function/create-deployment.png)

5. Vyberte `deployment.json` vás informovat. V okně výstupu uvidíte odpovídající výstupy pro vaše nasazení.
6. Spuštění vaší hraniční runtime v příkazu palety. **Okraj: Počáteční hranu**
7. Můžete zobrazit vaše IoT Edge runtime spustit v Průzkumníku Docker pomocí simulované funkce senzor a filtr.

    ![Řešení systémem](./media/how-to-vscode-develop-csharp-function/solution-running.png)

8. Klikněte pravým tlačítkem na ID hraniční zařízení a také můžete monitorovat D2C zprávy v produktu VS Code.

    ![Monitorování zprávy](./media/how-to-vscode-develop-csharp-function/monitor-d2c-messages.png)


## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili funkce Azure IoT hranu a nasazené na IoT hraniční zařízení v produktu VS Code. Můžete pokračovat na některý z následujících kurzech Další informace o další scénáře při vývoji Azure IoT Edge v produktu VS Code.

> [!div class="nextstepaction"]
> [Ladění Azure Functions v produktu VS Code](how-to-vscode-debug-azure-function.md)
