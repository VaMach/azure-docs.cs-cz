---
title: "Používat přímé metody Azure IoT Hub (.NET/.NET) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. Zařízení Azure IoT SDK pro platformu .NET, kterou použijete k implementaci aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro .NET k implementaci aplikační služby, která volá metodu přímé služby Azure IoT."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: 9ce1fbebb6417c10618aa182e3c1d9ddf8132fb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnet"></a>Používat přímé metody (.NET/.NET)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu budeme vyvíjet dvě aplikace konzoly .NET:

* **CallMethodOnDevice**, back-end aplikace, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.
* **SimulateDeviceMethods**konzolovou aplikaci, která simuluje zařízení připojení do služby IoT hub s identitou zařízení vytvořenou dříve a reagovat na metodu volá cloudu.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Pokud chcete vytvořit identitu zařízení programově místo, najdete v příslušné části [připojení simulovaného zařízení do služby IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.


## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci .NET, která reaguje na metodu s názvem řešení zpět end.

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Název projektu **SimulateDeviceMethods**.
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. V Průzkumníku řešení klikněte pravým tlačítkem myši **SimulateDeviceMethods** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**. Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití. Tento postup stáhne, nainstaluje a přidá odkaz na [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem zařízení, kterou jste si poznamenali v předchozím oddílu.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Přidejte následující implementovat metodu přímé na zařízení:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written to log.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Nakonec přidejte následující kód, který **hlavní** metoda k otevření připojení do služby IoT hub a inicializovat naslouchací proces metoda:
   
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter to exit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove the "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a pak vyberte **SimulateDeviceMethods** projekt v rozevírací nabídce.        

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení
V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci simulovaného zařízení a potom zobrazí odpověď.

1. V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**. Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější. Název projektu **CallMethodOnDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createserviceapp]
2. V Průzkumníku řešení klikněte pravým tlačítkem myši **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
3. V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání. Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.
   
    ![Okno Správce balíčků NuGet][img-servicenuget]

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

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a pak vyberte **CallMethodOnDevice** projekt v rozevírací nabídce.

## <a name="run-the-applications"></a>Spuštění aplikací
Nyní můžete spustit aplikace.

1. Spuštění aplikace .NET zařízení **SimulateDeviceMethods**. By se měl spustit přijímá metoda volání ze služby IoT Hub: 

    ![Spusťte aplikaci zařízení][img-deviceapprun]
1. Teď, když je zařízení připojené a čekání volání metod, spusťte .NET **CallMethodOnDevice** aplikace k vyvolání metody v aplikaci simulovaného zařízení. Měli byste vidět odpovědi zařízení napsané v konzole.
   
    ![Spusťte aplikaci aplikační služby][img-serviceapprun]
1. Zařízení je pak reaguje na metodu tiskem tuto zprávu:
   
    ![Přímá metoda volána v zařízení][img-directmethodinvoked]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení. Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
