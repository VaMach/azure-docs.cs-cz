## Zasílání zpráv do služby Event Hubs

V této části napíšete konzolovou aplikaci pro Windows, která zasílá události do vašeho centra událostí.

1. Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#. Projekt pojmenujte **Odesílatel**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet pro řešení**. 

3. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Zkontrolujte, jestli je v okně **Verze** uvedený název projektu (**Odesílatel**). Klikněte na **Instalovat** a přijměte podmínky použití. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio stáhne a nainstaluje [balíček NuGet knihovny Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) a přidá se na něj odkaz.

4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. K třídě **Program** přidejte následující pole a zástupné hodnoty nahraďte názvem centra událostí, které jste vytvořili v předchozí části, a připojovacím řetězcem na úrovni oboru názvů, který jste si předtím uložili.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Přidejte následující metodu do třídy **Program**:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Tato metoda neustále odesílá události do vašeho centra událostí se zpožděním 200 ms.

7. Nakonec do metody **Main** přidejte následující řádky:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```


<!--HONumber=Sep16_HO3-->


