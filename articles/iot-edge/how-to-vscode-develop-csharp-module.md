---
title: "Použití kódu v jazyce Visual Studio k vývoji modulu jazyka C# s hranou Azure IoT | Microsoft Docs"
description: "Vývoji a nasazení modul C# s Azure IoT hranou v produktu VS Code aniž by se přepnula kontextu"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cad28b4e6d4e46058641da19795cd71efdbd0c92
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>Použití kódu v jazyce Visual Studio k vývoji modulu jazyka C# s hranou Azure IoT
Tento článek obsahuje podrobné pokyny pro používání [Visual Studio Code](https://code.visualstudio.com/) jako hlavní vývojový nástroj k vývoji a nasazení moduly IoT okraj. 

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že používáte počítač nebo virtuální počítač se systémem Windows nebo Linux jako vývojovém počítači. Zařízení IoT Edge může být jiné fyzické zařízení nebo zařízení IoT Edge můžete simulovat na vývojovém počítači.

Ujistěte se, že máte dokončené následující kurzy, než začnete v tomto návodu.
- Nasazení v simulovaném zařízení v Azure IoT Edge [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) nebo [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Vývoj a nasazení modul IoT Edge C# do simulovaného zařízení](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Zde je uveden kontrolní seznam, který obsahuje položky že byste měli mít po dokončení předchozí kurzy.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge rozšíření pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# pro rozšíření Visual Studio Code (používá technologii OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [Skript IoT okraj ovládacího prvku](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Šablona AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Aktivní Centrum IoT se alespoň IoT hraniční zařízení.

Doporučujeme také nainstalovat [Docker podporu pro VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) modulu bitové kopie a kontejnery správy.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Nasazení modul Azure IoT Edge v produktu VS Code

### <a name="list-your-iot-hub-devices"></a>Zobrazí seznam zařízení IoT hub
Seznam zařízení IoT hub v produktu VS Code dvěma způsoby. Můžete buď způsob, jak pokračovat.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>Přihlášení účtu Azure v VSCode a zvolte služby IoT hub
1. V příkazu palety (F1 nebo Ctrl + Shift + P), zadejte a vyberte **Azure: Přihlaste**. Pak klikněte na tlačítko  **kopie* & Otevřít** v místní nabídce. Vložte (Ctrl + V) kód v prohlížeči a klikněte na tlačítko Pokračovat. Potom se přihlaste pomocí účtu Azure. Můžete zobrazit informace o účtu v VS Code stavový řádek.
2. V příkazu palety (F1 nebo Ctrl + Shift + P), zadejte a vyberte **IoT: Vyberte IoT Hub**. Nejprve vybrat předplatné, které jste vytvořili službu IoT hub v předchozí kurzu. Zvolte Centrum IoT, který obsahuje IoT hraniční zařízení.

    ![Seznam zařízení](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-iot-hub-connection-string"></a>Nastavit připojovací řetězec centra IoT
V příkazu palety (F1 nebo Ctrl + Shift + P), zadejte a vyberte **IoT: nastavit připojovací řetězec centra IoT**. Ujistěte se, vložte připojovací řetězec v rámci zásad **iothubowner** (najdete ji v zásady sdíleného přístupu služby IoT hub v portálu Azure).
 
Zobrazí seznam zařízení v Průzkumníku zařízení IoT Hub na levé straně panelu.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Spuštění vaší hraniční IoT runtime a nasazení modulu
Instalace a spuštění modulu runtime Azure IoT Edge ve vašem zařízení. A nasadit simulované senzor modul, který bude posílat telemetrická data do služby IoT Hub.
1. V příkazu palety, vyberte **Edge: Instalační program Edge** a zvolte vaší hraniční IoT ID zařízení. Klikněte pravým tlačítkem na hraniční ID zařízení v seznamu zařízení a vyberte **instalace Edge**.

    ![Instalace modulu runtime Edge](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. V příkazu palety, vyberte **Edge: spuštění Edge** spuštění vaší hraniční runtime. Zobrazí odpovídající výstupy v integrovaném terminálu.

    ![Počáteční okraj runtime](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Zkontrolujte stav modulu runtime Edge v Průzkumníku Docker. Zelená znamená, že je spuštěna. Vaše IoT Edge runtime byl úspěšně spuštěn.

    ![Modul runtime Edge běží](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. Teď vaší hraniční runtime běží, což znamená, že váš počítač nyní simuluje hraniční zařízení. Dalším krokem je k simulaci sensorthing, který udržuje odesílání zpráv do hraniční zařízení. V příkazu palety, zadejte a vyberte **Edge: generování Edge konfigurační soubor**. A vyberte složku pro vytvoření tohoto souboru. V souboru generovaného deployment.json, nahradí obsah `<registry>/<image>:<tag>` s `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` a soubor uložte.

    ![Senzor modulu](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Vyberte **Edge: vytvoření nasazení pro hraniční zařízení** a zvolte ID hraniční zařízení k vytvoření nového nasazení. Nebo můžete klikněte pravým tlačítkem na hraniční ID zařízení v seznamu zařízení a vybrat **vytvořit nasazení pro hraniční zařízení**. 

6. Měli byste vidět vaší hraniční IoT spustit v Průzkumníku Docker s simulované senzoru. Klikněte pravým tlačítkem na kontejner v Průzkumníku Docker. Podívejte se na docker protokolů pro každý modul. Navíc můžete zobrazit seznam modulů v seznamu zařízení.

    ![Seznam modulů](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. Klikněte pravým tlačítkem na ID hraniční zařízení a také můžete monitorovat D2C zprávy v produktu VS Code.
8. K zastavení modulu runtime vaší hraniční IoT a modul senzor, typ a vyberte **Edge: Zastavit Edge** v příkazu palety.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Vývoj a nasazení modul C# v produktu VS Code
V kurzu [vyvíjet modulu jazyka C#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), aktualizovat, vytvářet a publikovat bitové kopie modulu v produktu VS Code a potom navštivte portál Azure k nasazení modulu jazyka C#. Tato část představuje způsob použití VS Code k nasazení a monitorování modulu jazyka C#.

### <a name="start-a-local-docker-registry"></a>Spustit místní docker registru
Žádné kompatibilní Docker registru můžete použít pro tento kurz. Jsou dvě oblíbených Docker registru služby k dispozici v cloudu [registru kontejner Azure](https://docs.microsoft.com/azure/container-registry/) a [úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Tato část používá [místním registru Docker](https://docs.docker.com/registry/deploying/), což je snazší pro účely testování během vaší časná vývoje.
V produktu VS Code **integrované Terminálové**(Ctrl + '), spusťte následující příkazy a spusťte místní registru.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Výše příklad ukazuje, konfigurace registru, které jsou vhodné pro testování. Produkční prostředí registru musí být chráněny pomocí protokolu TLS a v ideálním případě by měl použít mechanismus řízení přístupu. Doporučujeme použít [registru kontejner Azure](https://docs.microsoft.com/azure/container-registry/) nebo [úložiště Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) nasazení moduly IoT Edge produkční prostředí.

### <a name="create-an-iot-edge-module-project"></a>Vytvoření projektu modulu IoT Edge
Následující kroky zobrazení můžete jak vytvořit modul IoT Edge založené na rozhraní .NET základní 2.0 pomocí kódu v jazyce Visual Studio a rozšíření Azure IoT okraj. Pokud jste v předchozím kurz dokončili v této části, můžete tuto část bezpečně přeskočit.
1. V sadě Visual Studio Code vyberte **zobrazení** > **integrované terminálu** otevřete integrované terminálu VS Code.
3. Integrované terminálu, zadejte následující příkaz pro instalaci (nebo aktualizujte) **AzureIoTEdgeModule** šablony v dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Vytvoření projektu pro nového modulu. Následující příkaz vytvoří složce projektu **FilterModule**, v aktuální pracovní složku:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Vyberte **soubor** > **otevřete složku**.
4. Vyhledejte **FilterModule** složky a klikněte na tlačítko **vyberte složku** otevřete projekt v produktu VS Code.
5. V Průzkumníku VS Code, klikněte na tlačítko **Program.cs** ho otevřete. V horní části **program.cs**, zahrnout pod obory názvů.
   ```csharp
   using Microsoft.Azure.Devices.Shared;
   using System.Collections.Generic;  
   using Newtonsoft.Json;
   ```

6. Přidat `temperatureThreshold` proměnnou **Program** třídy. Tato proměnná nastaví hodnotu, která nesmí být větší než teplota měřená v pořadí pro data k odeslání do služby IoT Hub. 

   ```csharp
   static int temperatureThreshold { get; set; } = 25;
   ```

7. Přidat `MessageBody`, `Machine`, a `Ambient` třídy k **Program** třídy. Tyto třídy definují očekávané schéma pro tělo příchozí zprávy.

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

8. V **Init** metoda, kód vytvoří a nakonfiguruje **DeviceClient** objektu. Tento objekt umožňuje modul pro připojení k místní Azure IoT Edge modulu runtime odesílat a přijímat zprávy. Připojovací řetězec použitý v **Init** metoda poskytl modulu runtime IoT okraj. Po vytvoření **DeviceClient**, kód zaregistruje zpětné volání pro příjem zpráv z centra IoT Edge prostřednictvím **input1** koncový bod. Nahraďte `SetInputMessageHandlerAsync` metoda s novou a přidejte `SetDesiredPropertyUpdateCallbackAsync` metoda aktualizace požadované vlastnosti. Chcete-li tuto změnu, nahraďte poslední řádek **Init** metoda následujícím kódem:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Přidat `onDesiredPropertiesUpdate` metodu **Program** třídy. Tato metoda přijímá aktualizace na požadované vlastnosti z modulu twin a aktualizací **temperatureThreshold** proměnné tak, aby odpovídaly. Všechny moduly mají vlastní twin modul, který vám umožní nakonfigurovat kód běžících v rámci modul přímo z cloudu.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
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

10. Nahraďte `PipeMessage` metoda s `FilterMessages` metoda. Tato metoda je volána, když modul přijímá zprávy z centra IoT okraj. Filtruje zprávy, které podávají teploty pod prahovou hodnotou teploty nastavených prostřednictvím twin modulu. Přidává také **typ zprávy** vlastnost na zprávu nastavená na hodnotu **výstrahy**. 

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

11. Pro sestavení projektu, klikněte pravým tlačítkem myši **FilterModule.csproj** soubor v Průzkumníku a klikněte na **modul sestavení IoT Edge**. Tento proces kompilovaný modul a exportuje binárního souboru a jeho závislosti do složky, která se používá k vytvoření bitové kopie Docker. 

    ![Sestavení modulu](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Vytvoření bitové kopie Docker a publikujete ho v registru

1. V Průzkumníku VS Code rozbalte **Docker** složky. Poté rozbalte složku pro vaši platformu kontejneru buď **linux x64** nebo **windows nano**.
2. Klikněte pravým tlačítkem myši **soubor Docker** souboru a klikněte na tlačítko **sestavení IoT Edge modulu Docker image**. 

    ![Sestavení docker bitové kopie](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. V **vyberte složku** okně vyhledejte nebo zadejte `./bin/Debug/netcoreapp2.0/publish`. Klikněte na tlačítko **vyberte složku EXE_DIR**.
4. Automaticky otevírané okno textového pole v horní části okna VS Code zadejte název bitové kopie. Například: `<your container registry address>/filtermodule:latest`. Když nasazujete do místního registru, měla by být `localhost:5000/filtermodule:latest`.
5. Do úložiště Docker push bitovou kopii. Použití **Edge: Push IoT Edge modulu Docker image** příkaz a zadejte adresu URL bitové kopie do místní textového pole v horní části okna VS Code. Použijte stejnou adresu URL obrázku jste použili v výše krok. Zkontrolujte protokol konzoly a ujistěte se, že bylo úspěšně posunuto bitovou kopii.

    ![Push docker image](./media/how-to-vscode-develop-csharp-module/push-image.png) ![stisknutí docker image](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>Nasazení moduly IoT Edge

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
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Nahraďte **trasy** část s následující obsah:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > Deklarativní pravidel v modulu runtime definovat, kde se tyto zprávy toku. V tomto kurzu musíte dvě trasy. První trasy určena k přenosu zprávy teplotní snímač modul filtr prostřednictvím koncového bodu "input1", což je koncový bod, který jste nakonfigurovali s FilterMessages obslužnou rutinou. Druhá trasa je určena k přenosu zpráv z modulu filtru do služby IoT Hub. V této trase je nadřazený speciální cílového umístění, které informuje Edge rozbočovače k odesílání zpráv do služby IoT Hub.

3. Uložte tento soubor.
4. V příkazu palety, vyberte **Edge: vytvoření nasazení pro hraniční zařízení**. Pak vyberte ID zařízení IoT Edge k vytvoření nasazení. Klikněte pravým tlačítkem na ID zařízení v seznamu zařízení a vyberte **vytvořit nasazení pro hraniční zařízení**.

    ![Vytvoření nasazení](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Vyberte `deployment.json` vás informovat. V okně výstupu uvidíte odpovídající výstupy pro vaše nasazení.

    ![Nasazení bylo úspěšné.](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. Spuštění vaší hraniční runtime v příkazu palety. **Okraj: Počáteční hranu**
7. Můžete zobrazit vaše IoT Edge runtime spustit v Průzkumníku Docker simulované senzor a modul filtru.

    ![Řešení IoT běží](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. Klikněte pravým tlačítkem na ID hraniční zařízení a také můžete monitorovat D2C zprávy v produktu VS Code.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili modul IoT okraj a nasazené na IoT hraniční zařízení v produktu VS Code. Můžete pokračovat na některý z následujících kurzech Další informace o další scénáře při vývoji Azure IoT Edge v produktu VS Code.

> [!div class="nextstepaction"]
> [Ladění jazyka C# modulu v produktu VS Code](how-to-vscode-debug-csharp-module.md)
