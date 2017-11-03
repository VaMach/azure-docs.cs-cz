---
title: "Zpracování zpráv typu zařízení cloud Azure IoT Hub pomocí tras (.Net) | Microsoft Docs"
description: "Postupy zpracování zpráv typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body k odeslání zprávy do dalších služeb back-end."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Zpracování zpráv typu zařízení cloud IoT Hub pomocí tras (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

V tomto kurzu vychází [Začínáme se službou IoT Hub] kurzu. Kurz:

* Ukazuje, jak používat směrování pravidla k odesílání zpráv typu zařízení cloud způsobem snadno, založené na konfiguraci.
* Ukazuje, jak izolovat interaktivní zprávy, které vyžadují okamžitou akci z back-end řešení pro další zpracování. Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM. Naproti tomu datového bodu zprávy, jako je například teplotní telemetrie informačního kanálu do modul analytics.

Na konci tohoto kurzu můžete spustit tři aplikace konzoly .NET:

* **SimulatedDevice**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub] kurzu se odešle zprávy typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.
* **ReadDeviceToCloudMessages** který zobrazí nekritické telemetrické zprávy odesílané aplikace zařízení.
* **ReadCriticalQueue** zrušte fronty kritické zprávy odeslané aplikace zařízení z fronty Service Bus. Tato fronta je připojený ke službě IoT hub.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript. Zjistěte, jak nahradit simulované zařízení v tomto kurzu fyzické zařízení, najdete v článku [Azure střediska pro vývojáře IoT].

Pro absolvování tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.

Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].

## <a name="send-interactive-messages"></a>Odesílat interaktivní zprávy

Upravit aplikaci zařízení, kterou jste vytvořili v [Začínáme se službou IoT Hub] kurzu příležitostně odesílat interaktivní zprávy.

V sadě Visual Studio v **SimulatedDevice** projektu, nahraďte `SendDeviceToCloudMessagesAsync` metoda následujícím kódem:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Tato metoda náhodně přidá vlastnost `"level": "critical"` na zprávy odeslané zařízením, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end řešení. Aplikace zařízení předá tyto informace ve vlastnostech zpráv místo v textu zprávy, že IoT Hub může směrovat zprávy Cíl správné zprávy.

> [!NOTE]
> Vlastnosti zprávy pro směrování zpráv můžete použít pro různé scénáře, včetně studený cesty při zpracování, kromě zde ukazuje příklad hot-path.

> [!NOTE]
> Z důvodu zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN [přechodných chyb].

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a>Směrování zpráv do fronty ve službě IoT hub

V této části:

* Vytvoření fronty Service Bus.
* Připojí ke službě IoT hub.
* Konfigurace služby IoT hub odesílat zprávy do fronty na základě přítomnosti vlastnosti na zprávu.

Další informace o tom, jak zpracování zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][Service Bus queue].

1. Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][Service Bus queue]. Fronty musí být ve stejném předplatném, oblasti jako službu IoT hub. Poznamenejte si název oboru názvů a fronty.

    > [!NOTE]
    > Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena. Pokud některá z těchto možností jsou povolené, koncový bod se zobrazí jako **Unreachable** na portálu Azure.

2. Na portálu Azure otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.
    
    ![Koncové body v centru IoT][30]

3. V **koncové body** okně klikněte na tlačítko **přidat** v horní části přidat fronty do služby IoT hub. Název koncového bodu **CriticalQueue** a použijte rozevírací seznamy a vyberte **fronty Service Bus**, oboru názvů Service Bus, ve které je umístěn fronty a názvu fronty. Až budete hotovi, klikněte na tlačítko **Uložit** dole.
    
    ![Přidání koncového bodu][31]
    
4. Klikněte na tlačítko **trasy** ve službě IoT Hub. Klikněte na tlačítko **přidat** v horní části okna Vytvořit pravidlo směrování pro směrování zpráv do fronty jste právě přidali. Vyberte **DeviceTelemetry** jako zdroj dat. Zadejte `level="critical"` jako podmínka a vyberte fronty, který jste právě přidali jako vlastní koncový bod jako koncový bod směrování pravidlo. Až budete hotovi, klikněte na tlačítko **Uložit** dole.
    
    ![Přidání trasu][32]
    
    Ujistěte se, že záložní cesta je nastavena na **ON**. Tato hodnota je výchozí konfiguraci pro Centrum IoT.
    
    ![Záložní trasy][33]

## <a name="read-from-the-queue-endpoint"></a>Čtení z fronty koncového bodu

V této části čtení zpráv z fronty koncového bodu.

1. V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**. Název projektu **ReadCriticalQueue**.

2. V Průzkumníku řešení klikněte pravým tlačítkem myši **ReadCriticalQueue** projektu a pak klikněte na **spravovat balíčky NuGet**. Zobrazí tuto operaci **Správce balíčků NuGet** okno.

3. Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití. Tato operace stáhne, nainstaluje a přidá odkaz na Azure Service Bus, se všemi jeho závislostmi.

4. Přidejte následující **pomocí** příkazy v horní části **Program.cs** souboru:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Nakonec přidejte následující řádky, které se **hlavní** metoda. Nahraďte připojovacího řetězce s **naslouchání** oprávnění pro fronty:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Spuštění aplikací
Nyní můžete spustit aplikace.

1. V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**. Vyberte **více projektů po spuštění**, pak vyberte **spustit** jako akce pro **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **ReadCriticalQueue** projekty.
2. Stiskněte klávesu **F5** zahájíte jsou tři konzole aplikace. **ReadDeviceToCloudMessages** aplikace má pouze nekritické zpráv odeslaných z **SimulatedDevice** aplikace a **ReadCriticalQueue** aplikace má pouze kritických zprávy.
   
   ![Tři aplikace konzoly][50]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak spolehlivě odesláním zprávy typu zařízení cloud pomocí funkce směrování zpráv služby IoT Hub.

[Odesílání zpráv typu cloud zařízení s centrem IoT] [ lnk-c2d] ukazuje, jak k odesílání zpráv do vašeho zařízení z back end vašeho řešení.

Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite][lnk-suite].

Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].

Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
