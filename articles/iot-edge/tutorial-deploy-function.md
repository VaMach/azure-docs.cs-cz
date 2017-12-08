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
ms.openlocfilehash: fb295b37819788ed14f54e4123ae0fe1b52d0210
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
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
* [C# pro rozšíření Visual Studio Code (používá technologii OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Můžete nainstalovat rozšíření z panelu rozšíření ve Visual Studio Code.)
* [Azure IoT Edge rozšíření pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Můžete nainstalovat rozšíření z panelu rozšíření ve Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). Pro tento kurz stačí Community Edition (CE) pro vaši platformu. 
* [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="set-up-a-docker-registry"></a>Nastavení registru Docker
V tomto kurzu použijete k vytvoření funkce Azure a vytvoření rozšíření Azure IoT Edge pro VS Code [Docker image](https://docs.docker.com/glossary/?term=image) s ním. Pak push k této Docker bitové kopie [úložiště Docker](https://docs.docker.com/glossary/?term=repository) hostované [Docker registru](https://docs.docker.com/glossary/?term=registry). Nakonec nasadit zabalené jako image Docker [kontejner Docker](https://docs.docker.com/glossary/?term=container) z registru na vaše zařízení IoT okraj.  

Žádné kompatibilní Docker registru můžete použít pro tento kurz. Jsou dvě oblíbených Docker registru služby k dispozici v cloudu **registru kontejner Azure** a **úložiště Docker Hub**:

- [Azure kontejneru registru](https://docs.microsoft.com/en-us/azure/container-registry/) je k dispozici [placené předplatné](https://azure.microsoft.com/en-us/pricing/details/container-registry/). V tomto kurzu **základní** předplatného je dostačující. 

- [Úložiště docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) nabízí jeden volné privátní úložiště, pokud se zaregistrujete k (zdarma) Docker ID. 
    1. Zaregistrujte si Docker ID, postupujte podle pokynů v [zaregistrovat pro Docker ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) na webu Docker. 

    2. Chcete-li vytvořit privátní úložiště Docker, postupujte podle pokynů v [vytvoření nového úložiště na úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) na webu Docker.

V tomto kurzu kde je to vhodné, jsou příkazy uvedené pro registru kontejner Azure i úložiště Docker Hub.

## <a name="create-a-function-project"></a>Vytvoření projektu – funkce
Následující kroky ukazují, jak vytvořit funkce IoT Edge pomocí kódu v jazyce Visual Studio a rozšíření Azure IoT okraj.
1. Kód Open VS.
2. Použití **zobrazení | Integrované Terminálové** příkazu v nabídce otevřete integrované terminálu VS Code.
3. Integrované terminálu, zadejte následující příkaz pro instalaci (nebo aktualizujte) **AzureIoTEdgeFunction** šablony v dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. V integrovaném terminálu zadejte následující příkaz pro vytvoření projektu pro nový modul:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

    >[!NOTE]
    > Tento příkaz vytvoří složce projektu **FilterFunction**, v aktuální pracovní složku. Pokud chcete vytvořit v jiném umístění, změňte adresáře před spuštěním příkazu.

3. Použití **souboru | Otevřete složku** příkaz nabídky, přejděte k **FilterFunction** složky a klikněte na **vyberte složku** otevřete projekt v produktu VS Code.
4. V Průzkumníku VS Code, klikněte na tlačítko **EdgeHubTrigger Csharp** složku, klikněte **run.csx** jej otevřete.
5. Přidejte následující příkaz po `#r "Microsoft.Azure.Devices.Client"` příkaz:

    ```csharp
    #r "Newtonsoft.Json"
    ```

5. Přidejte následující příkazy using po existující `using` příkazy:

    ```csharp
    using Newtonsoft.Json;
    ```

1. Přidejte následující třídy. Tyto třídy definují očekávané schéma pro tělo příchozí zprávy.

    ```csharp
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

1. Nahraďte text **spustit** metoda následujícím kódem. Filtruje zprávy na základě hodnoty teploty v těle zprávy a teploty prahovou hodnotu.

    ```csharp
    const int temperatureThreshold = 25;

    byte[] messageBytes = messageReceived.GetBytes();
    var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

    if (!string.IsNullOrEmpty(messageString))
    {
        // Get the body of the message and deserialize it
        var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
        
        if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
        {
            // We will send the message to the output as the temperature value is greater than the threashold
            var filteredMessage = new Message(messageBytes);
            // We need to copy the properties of the original message into the new Message object
            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
            {
                filteredMessage.Properties.Add(prop.Key, prop.Value);
            }
            // We are adding a new property to the message to indicate it is an alert
            filteredMessage.Properties.Add("MessageType", "Alert");
            // Send the message        
            await output.AddAsync(filteredMessage);
            log.Info("Received and transferred a message with temperature above the threshold");
        }
    }
    ```

11. Uložte soubor.

## <a name="publish-a-docker-image"></a>Publikování bitovou kopii Docker

1. Vytvořte bitovou kopii Docker.
    1. V Průzkumníku VS Code, klikněte na **Docker** složku ji otevřete. Potom vyberte složku pro vaši platformu kontejneru buď **linux x64** nebo **windows nano**. 
    2. Klikněte pravým tlačítkem myši **soubor Docker** souboru a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**. 
    3. V **vyberte složku** pole, přejděte do složky, do projektu, **FilterFunction**a klikněte na tlačítko **vyberte složku jako EXE_DIR**. 
    4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například `<docker registry address>/filterfunction:latest`, kde *docker registru adresu* je vaše ID Docker, pokud používáte úložiště Docker Hub nebo je podobná `<your registry name>.azurecr.io`, pokud používáte Azure kontejneru registru.
 
4. Přihlaste se k Docker. V integrovaném terminálu zadejte následující příkaz: 

    - Úložiště docker Hub (zadejte své přihlašovací údaje po zobrazení výzvy):
        
        ```csh/sh
        docker login
        ```

    - Kontejner Azure registru:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Uživatelské jméno, heslo a přihlášení serveru pro použití v tomto příkazu najdete na [portál Azure] (https://portal.azure.com). Z **všechny prostředky**, klikněte na dlaždici pro váš kontejner Azure registru otevřete jeho vlastnosti a pak klikněte na **přístupové klíče**. Zkopírujte hodnoty v **uživatelské jméno**, **heslo**, a **přihlášení na server** pole. Sould server přihlášení být ve formátu: `<your registry name>.azurecr.io`.

3. Do úložiště Docker push bitovou kopii. Použití **zobrazení | Příkaz palety... | Okraj: Nabízené IoT Edge modulu Docker image** nabídky příkaz a zadejte název bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejný název bitové kopie jste použili v kroku 1.c.

## <a name="add-registry-credentials-to-your-edge-device"></a>Přidejte pověření registru do hraniční zařízení
Přidáte přihlašovací údaje pro vaše registru do hraniční runtime v počítači, na kterém je spuštěn hraniční zařízení. To dává runtime přístup ke kontejneru pro vyžádání obsahu. 

- Pro systém Windows spusťte následující příkaz:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Pro Linux spusťte následující příkaz:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Spuštění řešení

1. V **portál Azure**, přejděte do služby IoT hub.
2. Přejděte na **IoT okraj (preview)** a vyberte zařízení IoT okraj.
1. Vyberte **nastavit moduly**. 
2. Přidat **tempSensor** modulu. Tento krok je jenom potřeba, pokud jste nenasadili dříve **tempSensor** modulu zařízení IoT okraj.
    1. Vyberte **přidání okraj IoT modulu**.
    2. V **název** zadejte `tempSensor`.
    3. V **Image URI** zadejte `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Další nastavení nezměníte a klikněte na tlačítko **Uložit**.
1. Přidat **filterfunction** modulu.
    1. Vyberte **přidat modul Edge IoT** znovu.
    2. V **název** zadejte `filterfunction`.
    3. V **Image** pole, zadejte adresu bitové kopie, třeba `<docker registry address>/filterfunction:latest`.
    74. Klikněte na **Uložit**.
2. Klikněte na **Další**.
3. V **zadejte trasy** kroku, zkopírujte do textového pole JSON níže. Moduly publikovat všechny zprávy do hraniční runtime. Deklarativní pravidel v modulu runtime definovat, kde se tyto zprávy toku. V tomto kurzu musíte dvě trasy. První trasa je určena k přenosu zpráv z teploty senzoru modulu filtru prostřednictvím koncového bodu "input1", což je koncový bod, který jste nakonfigurovali s **FilterMessages** obslužné rutiny. Druhá trasa je určena k přenosu zpráv z modulu filtru do služby IoT Hub. V této trase `upstream` je speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klikněte na **Další**.
5. V **šablona kontrolní** krok, klikněte na tlačítko **odeslání**. 
6. Návrat na stránku Podrobnosti o zařízení IoT okraj a klikněte na tlačítko **aktualizovat**. Byste měli vidět, že se vám nové **filterfunction** modulu společně s **tempSensor** modulu a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

K monitorování zařízení na cloud zprávy odeslané ze zařízení IoT okraj do služby IoT hub:
1. Rozšíření Azure IoT Toolkit nakonfigurujte připojovací řetězec služby IoT hub: 
    1. Použití **zobrazení | Průzkumník** příkazu v nabídce otevřete Průzkumník kódu VS. 
    3. V Průzkumníku, klikněte na tlačítko **zařízení IOT HUB** a pak klikněte na **...** . Klikněte na tlačítko **nastavit připojovací řetězec centra IoT** a zadejte připojovací řetězec pro službu IoT hub, která zařízení IoT Edge připojuje v místním okně. 

        Chcete-li najít připojovací řetězec, klikněte na dlaždici služby IoT hub v portálu Azure a pak klikněte na **zásady sdíleného přístupu**. V **zásady sdíleného přístupu**, klikněte **iothubowner** zásad a zkopírujte IoT Hub připojovací řetězec v **iothubowner** okno.   

1. Ke sledování dat odesílaných do služby IoT hub, použijte **zobrazení | Příkaz palety... | IoT: Spuštění monitorování D2C zpráva** příkazu nabídky. 
2. Chcete-li zastavit monitorování dat, použijte **zobrazení | Příkaz palety... | IoT: Zastavení monitorování D2C zpráva** příkazu nabídky. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili funkce Azure, který obsahuje kód pro filtrování nezpracovaná data generována zařízení IoT okraj. Chcete-li zachovat zkoumat Azure IoT Edge, další informace o použití IoT hraniční zařízení jako brána. 

> [!div class="nextstepaction"]
> [Vytvoření IoT hraniční zařízení brány](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
