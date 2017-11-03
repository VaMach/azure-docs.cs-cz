---
title: "Používat přímé metody Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. Implementace aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro .NET k implementaci aplikační služby, která volá metodu přímé služby Azure IoT pomocí zařízení Azure IoT SDK pro Node.js."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: nberdy
ms.openlocfilehash: 76f1d32b4afeacae1488b4cf28be6c8cf7f4ea37
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnode"></a>Používat přímé metody (.NET nebo uzel)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu budeme vyvíjet .NET a konzolovou aplikaci softwaru Node.js:

* **CallMethodOnDevice.sln**, back-end aplikace .NET, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.
* **SimulatedDevice.js**, aplikace Node.js, která simuluje zařízení připojení do služby IoT hub s dříve vytvořenou identitou zařízení a reaguje na metodu volá cloudu.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 4.0.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje na metodu s názvem řešení zpět end.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v **simulateddevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte soubor v **simulateddevice** složku a pojmenujte ji **SimulatedDevice.js**.
4. Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Přidat **connectionString** proměnné a použít ho k vytvoření **DeviceClient** instance. Nahraďte **{zařízení připojovací řetězec}** s připojením zařízení vygenerovanými v řetězci *vytvoření identity zařízení* části:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Přidejte následující funkci k přímé metodu implementovat na zařízení:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Otevření připojení do služby IoT hub a inicializovat naslouchací proces metoda:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Soubor **SimulatedDevice.js** uložte a zavřete.

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení
V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci simulovaného zařízení a potom zobrazí odpověď.

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Název projektu **CallMethodOnDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.
   
    ![Okno Správce balíčků NuGet][11]

4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu do třídy **Program**:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Tato metoda volá metodu přímé s názvem `writeLine` na `myDeviceId` zařízení. Pak zapíše odpověď od zařízení v konzole. Všimněte si, jak je možné zadat hodnotu časového limitu pro zařízení reagovat.
7. Nakonec do metody **Main** přidejte následující řádky:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a>Spuštění aplikací
Nyní můžete spustit aplikace.

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a pak vyberte **CallMethodOnDevice** projekt v rozevírací nabídce.

2. Na příkazovém řádku v **simulateddevice** složky, spusťte následující příkaz, který zahájit naslouchání pro volání metod ze služby IoT Hub:
   
    ```
    node SimulatedDevice.js
    ```
   Počkejte simulované zařízení otevřete:![][7]
3. Teď, když je zařízení připojené a čekání volání metod, spusťte .NET **CallMethodOnDevice** aplikace k vyvolání metody v aplikaci simulovaného zařízení. Měli byste vidět odpovědi zařízení napsané v konzole.
   
    ![][8]
4. Zařízení je pak reaguje na metodu tiskem tuto zprávu:
   
    ![][9]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení. Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
