---
title: Azure IoT Edge C# modulu | Microsoft Docs
description: "Vytvořte modul IoT Edge s kód C# a nasadíte ho do hraniční zařízení"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c778c412bf6d65c5b6ee92d603aac7acfa6139eb
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Vývoj a nasazení modul IoT Edge C# na simulovaného zařízení – náhled

Moduly IoT Edge můžete nasadit kód, který implementuje obchodní logiku přímo do zařízení IoT okraj. V tomto kurzu vás provede vytvořením a nasazením modul IoT okraj, který filtruje data snímačů na simulované zařízení IoT okraj, který jste vytvořili v nasazení Azure IoT Edge na simulované zařízení na [Windows] [ lnk-tutorial1-win]nebo [Linux] [ lnk-tutorial1-lin] kurzy. V tomto kurzu se naučíte:    

> [!div class="checklist"]
> * Visual Studio Code použít k vytvoření modul IoT Edge založené na .NET core 2.0
> * Pomocí Visual Studio Code a Docker vytvoření bitové kopie docker a publikujete ho v registru 
> * Nasazení modulu do vašeho zařízení IoT Edge
> * Vygeneruje zobrazení dat


Modul IoT okraj, který vytvoříte v tomto kurzu filtruje data teploty generována zařízení a pouze při odešle zprávy proti proudu je nad určenou prahovou hodnotou. 

## <a name="prerequisites"></a>Požadavky

* Azure IoT hraniční zařízení, který jste vytvořili v předchozí kurz na rychlý start.
* Připojovací řetězec služby IoT Hub pro službu IoT hub, která zařízení IoT Edge připojí k.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge rozšíření pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Můžete nainstalovat rozšíření z panelu rozšíření ve Visual Studio Code.)
* [C# pro rozšíření Visual Studio Code (používá technologii OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Můžete nainstalovat rozšíření z panelu rozšíření ve Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). Pro tento kurz stačí Community Edition (CE) pro vaši platformu. Ujistěte se, že instalujete v počítači, který jste spustili VS Code.
* [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Zvolte nebo si zaregistrovat registru Docker
V tomto kurzu použijete modul sestavení a vytvoření rozšíření Azure IoT Edge pro VS Code [Docker image](https://docs.docker.com/glossary/?term=image). Pak push k této Docker bitové kopie [úložiště Docker](https://docs.docker.com/glossary/?term=repository) hostované na [Docker registru](https://docs.docker.com/glossary/?term=registry). Nakonec nasadit zabalené jako image Docker [kontejner Docker](https://docs.docker.com/glossary/?term=container) z registru na vaše zařízení IoT okraj.  

Žádné kompatibilní Docker registru můžete použít pro tento kurz. Jsou dvě oblíbených Docker registru služby k dispozici v cloudu **registru kontejner Azure** a **úložiště Docker Hub**:

- [Azure kontejneru registru](https://docs.microsoft.com/en-us/azure/container-registry/) je k dispozici [placené předplatné](https://azure.microsoft.com/en-us/pricing/details/container-registry/). V tomto kurzu **základní** předplatného je dostačující. 

- [Úložiště docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) nabízí jeden volné privátní úložiště, pokud se zaregistrujete k (zdarma) Docker ID. 
    1. Zaregistrujte si Docker ID, postupujte podle pokynů v [zaregistrovat pro Docker ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) na webu Docker. 

    2. Chcete-li vytvořit privátní úložiště Docker, postupujte podle pokynů v [vytvoření nového úložiště na úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) na webu Docker.

V tomto kurzu kde je to vhodné, příkazů bude poskytnuta pro registru kontejner Azure i úložiště Docker Hub.

## <a name="create-an-iot-edge-module-project"></a>Vytvoření projektu modulu IoT Edge
Následující kroky zobrazení můžete jak vytvořit modul IoT Edge založené na rozhraní .NET základní 2.0 pomocí kódu v jazyce Visual Studio a rozšíření Azure IoT okraj.
1. Kód Open VS.
2. Použití **zobrazení | Integrované Terminálové** příkazu v nabídce otevřete integrované terminálu VS Code.
3. Integrované terminálu, zadejte následující příkaz pro instalaci (nebo aktualizujte) **AzureIoTEdgeModule** šablony v dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. V integrovaném terminálu zadejte následující příkaz pro vytvoření projektu pro nový modul:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > Tento příkaz vytvoří složce projektu **FilterModule**, v aktuální pracovní složku. Pokud chcete vytvořit v jiném umístění, změňte adresáře před spuštěním příkazu.
 
3. Použití **souboru | Otevřete složku** příkaz nabídky, přejděte k **FilterModule** složky a klikněte na **vyberte složku** otevřete projekt v produktu VS Code.
4. V Průzkumníku VS Code, klikněte na tlačítko **Program.cs** ho otevřete.
5. Přidejte následující pole, které chcete **Program** třídy.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. Přidejte následující třídy **Program** třídy. Tyto třídy definují očekávané schéma pro tělo příchozí zprávy.

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

1. V **Init** metoda, kód vytvoří a nakonfiguruje **DeviceClient** objektu. Tento objekt umožňuje modul pro připojení k místní Azure IoT Edge modulu runtime odesílat a přijímat zprávy. Parametr řetězce připojení, který se používá v **Init** metoda poskytl modulu runtime IoT Edge v proměnné prostředí **EdgeHubConnectionString**. Po vytvoření **DeviceClient**, kód zaregistruje zpětné volání pro příjem zpráv z centra IoT Edge prostřednictvím **input1** koncový bod. Nahraďte metodu používanou jako zpětné volání pro zpracování zprávy s novou a připojte zpětné volání pro aktualizace požadované vlastnosti na twin modulu. Chcete-li tuto změnu, nahraďte poslední řádek **Init** metoda následujícím kódem:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. Přidejte následující metodu do **programu** třída aktualizovat **temperatureThreshold** pole podle požadované vlastnosti odeslaných službou back-end prostřednictvím twin modulu. Všechny moduly mají vlastní twin modulu. Modul twin umožňuje nakonfigurovat kód spuštěný v modulu služby back-end.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"].exists())
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

1. Nahraďte **PipeMessage** metoda s následující metodu. Tato metoda je volána, když modul je zprávy odeslané z rozbočovače okraj. Filtruje zprávy na základě hodnoty teploty v těle zprávy a prahová hodnota teploty nastavených prostřednictvím twin modulu.

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Sestavte projekt. Použití **zobrazení | Průzkumník** příkazu v nabídce otevřete Průzkumník kódu VS. V Průzkumníku, klikněte pravým tlačítkem myši **FilterModule.csproj** souboru a klikněte na tlačítko **IoT Edge sestavení modulu** zkompilovat modul a exportovat binárního souboru a jeho závislosti do složky, jestli je vytvořená bitovou kopii Docker z v dalším kroku.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Vytvoření bitové kopie Docker a publikujete ho v registru

1. Vytvořte bitovou kopii Docker.
    1. V Průzkumníku VS Code, klikněte na **Docker** složku ji otevřete. Potom vyberte složku pro vaši platformu kontejneru buď **linux x64** nebo **windows nano**. 
    2. Klikněte pravým tlačítkem myši **soubor Docker** souboru a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**. 
    3. V **vyberte složku** buď Procházet a vyberte nebo zadejte `./bin/Debug/netcoreapp2.0/publish`. Klikněte na tlačítko **vyberte složku EXE_DIR**.
    4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Příklad:`<docker registry address>/filtermodule:latest`, kde *docker registru adresu* je vaše ID Docker, pokud používáte úložiště Docker Hub nebo podobná `<your registry name>.azurecr.io`, pokud používáte Azure kontejneru registru.
 
4. Přihlaste se k Docker. V integrovaném terminálu zadejte následující příkaz: 

    - Úložiště docker Hub (zadejte své přihlašovací údaje po zobrazení výzvy):
        
        ```csh/sh
        docker login
        ```

    - Pro kontejner Azure registru:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Uživatelské jméno, heslo a přihlášení serveru pro použití v tomto příkazu najdete na [portál Azure] (https://portal.azure.com). Z **všechny prostředky**, klikněte na dlaždici pro váš kontejner Azure registru otevřete jeho vlastnosti a pak klikněte na **přístupové klíče**. Zkopírujte hodnoty v **uživatelské jméno**, **heslo**, a **přihlášení na server** pole. Sould server přihlášení být ve formátu: `<your registry name>.azurecr.io`.

3. Do úložiště Docker push bitovou kopii. Použití **zobrazení | Příkaz palety... | Okraj: Nabízené IoT Edge modulu Docker image** nabídky příkaz a zadejte název bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejný název bitové kopie jste použili v kroku 1.c.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>Přidejte pověření registru do hraniční runtime v hraniční zařízení
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

1. V [portál Azure](https://portal.azure.com), přejděte do služby IoT hub.
2. Přejděte na **IoT okraj (preview)** a vyberte zařízení IoT okraj.
3. Vyberte **nastavit moduly**. 
2. Přidat **tempSensor** modulu. Tento krok je jenom potřeba, pokud jste nenasadili dříve **tempSensor** modulu zařízení IoT okraj.
    1. Vyberte **přidání okraj IoT modulu**.
    2. V **název** zadejte `tempSensor`.
    3. V **Image URI** zadejte `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Další nastavení nezměníte a klikněte na tlačítko **Uložit**.
9. Přidat **filtermodule**
    1. Vyberte **přidat modul Edge IoT** znovu.
    2. V **název** zadejte `filtermodule`.
    3. V **Image** pole, zadejte adresu bitové kopie, třeba `<docker registry address>/filtermodule:latest`.
    4. Zkontrolujte **úpravy modulu twin** pole.
    5. Nahraďte kód JSON do textového pole pro dvojici modul s následujícím kódu JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klikněte na **Uložit**.
12. Klikněte na **Další**.
13. V **zadejte trasy** kroku, zkopírujte do textového pole JSON níže. Moduly publikovat všechny zprávy do hraniční runtime. Deklarativní pravidel v modulu runtime definovat, kde se tyto zprávy toku. V tomto kurzu musíte dvě trasy. První trasa je určena k přenosu zpráv z teploty senzoru modulu filtru prostřednictvím koncového bodu "input1", což je koncový bod, který jste nakonfigurovali s **FilterMessages** obslužné rutiny. Druhá trasa je určena k přenosu zpráv z modulu filtru do služby IoT Hub. V této trase `upstream` je speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klikněte na **Další**.
5. V **šablona kontrolní** krok, klikněte na tlačítko **odeslání**. 
6. Návrat na stránku Podrobnosti o zařízení IoT okraj a klikněte na tlačítko **aktualizovat**. Byste měli vidět, že se vám nové **filtermodule** společně s **tempSensor** modulu a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Vygeneruje zobrazení dat

K monitorování zařízení na cloud zprávy odeslané ze zařízení IoT okraj do služby IoT hub:
1. Rozšíření Azure IoT Toolkit nakonfigurujte připojovací řetězec služby IoT hub: 
    1. Použití **zobrazení | Průzkumník** příkazu v nabídce otevřete Průzkumník kódu VS. 
    3. V Průzkumníku, klikněte na tlačítko **zařízení IOT HUB** a pak klikněte na **...** . Klikněte na tlačítko **nastavit připojovací řetězec centra IoT** a zadejte připojovací řetězec pro službu IoT hub, která zařízení IoT Edge připojuje v místním okně. 

        Chcete-li najít připojovací řetězec, klikněte na dlaždici služby IoT hub v portálu Azure a pak klikněte na **zásady sdíleného přístupu**. V **zásady sdíleného přístupu**, klikněte **iothubowner** zásad a zkopírujte IoT Hub připojovací řetězec v **iothubowner** okno.   

1. Ke sledování dat odesílaných do služby IoT hub, použijte **zobrazení | Příkaz palety... | IoT: Spuštění monitorování D2C zpráva** příkazu nabídky. 
2. Chcete-li zastavit monitorování dat, použijte **zobrazení | Příkaz palety... | IoT: Zastavení monitorování D2C zpráva** příkazu nabídky. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili hraniční IoT modul, který obsahuje kód pro filtrování nezpracovaná data generována zařízení IoT okraj. Můžete pokračovat na některý z následujících kurzech informace o další způsoby, které mohou pomoci Azure IoT Edge, že zapněte data do podnikových statistik na hranici.

> [!div class="nextstepaction"]
> [Nasazení funkce Azure jako modul](tutorial-deploy-function.md)
> [nasazení Azure Stream Analytics jako modul](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
