---
title: Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus | Microsoft Docs
description: Kurz .NET pro zprostředkované zasílání zpráv
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm

---
# <a name="service-bus-brokered-messaging-.net-tutorial"></a>Kurz .NET pro zprostředkované zasílání zpráv ve službě Service Bus
Azure Service Bus poskytuje dvě ucelená řešení – jedno přes centralizovanou „předávací“ službu v cloudu, která podporuje spoustu různých přenosových protokolů a webových služeb, jako například SOAP, WS-* nebo REST. Klient nepotřebuje přímé spojení s lokální službou, nemusí ani vědět, kde se služba nachází, a lokální služba nepotřebuje mít ve firewallu otevřené žádné příchozí porty.

Druhé řešení přenosu zpráv umožňuje funkce „zprostředkovaného“ přenosu zpráv. Můžeme o nich uvažovat jako o funkcích asynchronního nebo odděleného přenosu zpráv, které podporují scénáře publikování a odebírání dat, časového oddělení a vyvažování zátěže pomocí infrastruktury přenosu zpráv služby Service Bus. Oddělená komunikace má mnoho výhod – klienti a servery se například můžou spojit podle potřeby a provádět své operace asynchronním způsobem.

V tomto kurzu získáte teoretické i praktické znalosti front, které jsou jednou z hlavních součástí zprostředkovaného zasílání zpráv ve službě Service Bus. Po absolvování řady témat v tomto kurzu budete mít aplikaci, která naplní seznam zpráv, vytvoří frontu a odešle zprávy do této fronty. Tato aplikace nakonec přijme a zobrazí zprávy z fronty, potom vyčistí svoje prostředky a ukončí se. Odpovídající kurz pro vytvoření aplikace, která používá Service Bus Relay, najdete v tématu [Kurz přenosu zpráv přes předávací službu Service Bus](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Úvod a požadavky
Fronty nabízejí doručování zpráv metodou FIFO (First In First Out) pro jednoho nebo několik konkurenčních spotřebitelů. FIFO znamená, že se předpokládá, že příjemci zprávy obvykle přijímají a zpracovávají v pořadí, ve kterém se přidaly do fronty, a každou zprávu přijme a zpracuje jenom jeden spotřebitel zprávy. Klíčovou výhodou použití front je *časové oddělení* součástí aplikace: jinými slovy odesílatelé a spotřebitelé zprávy nemusí zprávy odesílat a spotřebovávat současně, protože zprávy jsou bezpečně uložené ve frontě. Další výhodou je *vyrovnávání zátěže*, které odesílatelům a spotřebitelům umožňuje odesílat a přijímat zprávy různými rychlostmi.

Tady jsou některé administrativní a technické požadavky, které byste před začátkem tohoto kurzu měli splnit. Nejdřív je potřeba vytvořit obor názvů služby a získat klíč sdíleného přístupového podpisu (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby. Kombinace oboru názvů služby a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Vytvoření oboru názvů služby a získání klíče SAS
Nejdřív je potřeba vytvořit obor názvů služby a získat klíč [sdíleného přístupového podpisu](../service-bus/service-bus-sas-overview.md) (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby. Kombinace oboru názvů služby a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Dalším krokem je vytvoření projektu Visual Studia a napsání dvou pomocných funkcí, které načtou seznam zpráv oddělený čárkami do objektu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) se silnou typovou kontrolou.

### <a name="create-a-visual-studio-project"></a>Vytvoření projektu ve Visual Studiu
1. Otevřete Visual Studio jako správce tak, že v nabídce Start kliknete na program pravým tlačítkem a vyberete možnost **Spustit jako správce**.
2. Vytvořte nový projekt konzolové aplikace. Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**. V dialogu **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**), klikněte na šablonu **Konzolová aplikace** a pojmenujte ji jako **QueueSample**. Použijte výchozí **Umístění**. Projekt vytvoříte kliknutím na **OK**.
3. Pomocí správce balíčků NuGet do projektu přidejte knihovny ServiceBus:
   
   1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **QueueSample** a pak klikněte na **Správa balíčků NuGet**.
   2. V dialogu **Správa balíčků NuGet** klikněte na kartu **Procházet** a vyhledejte **Azure Service Bus**, pak klikněte na **Instalovat**.
      <br />
4. V Průzkumníku řešení poklikejte na soubor Program.cs a otevře se v editoru Visual Studio. Změňte název oboru názvů z výchozího názvu `QueueSample` na `Microsoft.ServiceBus.Samples`.
   
    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```
5. Upravte příkazy `using` tak, jak je vidět na následujícím kódu.
   
    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```
6. Vytvořte textový soubor s názvem Data.csv a zkopírujte do něj následující text oddělený čárkami.
   
    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```
   
    Uložte a zavřete soubor Data.csv a poznamenejte si umístění, do kterého jste ho uložili.
7. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu (v tomto příkladu **QueueSample**), klikněte na **Přidat**, a pak klikněte na **Existující položka**.
8. Přejděte k souboru Data.csv, který jste vytvořili v kroku 6. Klikněte na soubor a pak na **Přidat**. Zkontrolujte, že je v seznamu typů souborů označená možnost **Všechny soubory (*.*)**.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Vytvoření metody, která bude parsovat seznam zpráv
1. Ve třídě `Program` před metodou `Main()` deklarujte dvě proměnné: jednu typu **DataTable**, která bude obsahovat seznam zpráv ze souboru Data.csv. Druhá by měla být typu List objekt se silnou typovou kontrolou pro [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Ta obsahuje seznam zprostředkovaných zpráv, který se bude používat v dalších krocích tohoto kurzu.
   
    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
   
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```
2. Mimo `Main()` definujte metodu `ParseCSV()`, která parsuje seznam zpráv v souboru Data.csv a načte zprávy do tabulky [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx), jak je vidět tady. Tato metoda vrátí objekt **DataTable**.
   
    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
   
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
   
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
   
        return tableIssues;
    }
    ```
3. V metodě `Main()` přidejte příkaz, který zavolá metodu `ParseCSVFile()`:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
   
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Vytvoření metody, která načte seznam zpráv
1. Mimo `Main()` definujte metodu `GenerateMessages()`, která vezme objekt **DataTable** vrácený z `ParseCSVFile()` a načte tabulku do seznamu zprostředkovaných zpráv se silnou typovou kontrolou. Metoda pak vrátí objekt **List**, jako v následujícím příkladu. 
   
    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
   
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```
2. Do `Main()` přímo po zavolání `ParseCSVFile()` přidejte příkaz, který zavolá metodu `GenerateMessages()` s hodnotou vrácenou z `ParseCSVFile()` jako argument:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Získání pověření uživatele
1. Nejdřív vytvořte tři globální řetězcové proměnné, do kterých se tyto údaje načtou. Deklarujte tyto proměnné přímo po deklaraci předchozích proměnných, například:
   
    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
   
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 
   
            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```
2. Dál vytvořte funkci, která přijme a uloží obor názvů služby a klíč SAS. Tuto metodu přidejte mimo `Main()`. Příklad: 
   
    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
   
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```
3. Do `Main()` přímo po zavolání `GenerateMessages()` přidejte příkaz, který zavolá metodu `CollectUserInput()`:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
   
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Sestavení řešení
V nabídce **Sestavení** ve Visual Studiu můžete kliknout na **Sestavit řešení** nebo stisknout **Ctrl+Shift+B** a potvrdit přesnost své dosavadní práce.

## <a name="create-management-credentials"></a>Vytvoření pověření ke správě
V tomto kroku definujete operace správy, které použijete pro vytvoření pověření sdíleného přístupového podpisu (SAS), kterým se vaše aplikace bude ověřovat.

1. V zájmu jednoduchosti a přehlednosti se v tomto kurzu všechny operace fronty budou umístěné v samostatné metodě. Ve třídě `Program` po metodě `Main()` vytvořte asynchronní metodu `Queue()`. Příklad:
   
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```
2. Dalším krokem je vytvoření pověření SAS pomocí objektu [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx). Metoda vytvoření převezme název a hodnotu klíče SAS získané v metodě `CollectUserInput()`. Do metody `Queue()` přidejte následující kód:
   
    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```
3. Vytvořte nový objekt správy oboru názvů s identifikátorem URI, který jako argumenty obsahuje název a pověření pro správu získané v předchozím kroku. Tento kód přidejte přímo po kódu přidaném v předchozím kroku. Ujistěte se, že jste `<yourNamespace>` nahradili názvem vašeho oboru názvů služby:
   
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Příklad
V tomto okamžiku by váš kód by měl vypadat podobně jako tento:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Zasílání zpráv do fronty
V tomto kroku vytvoříte frontu, pak do ní odešlete zprávy obsažené v seznamu zprostředkovaných zpráv.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Vytvoření fronty a odeslání zpráv do fronty
1. Nejdřív vytvořte frontu. Pojmenujte ji třeba `myQueue` a deklarujte ji přímo po operacích správy, které jste přidali v metodě `Queue()` v předchozím kroku:
   
    ```
    QueueDescription myQueue;
   
    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }
   
    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```
2. V metodě `Queue()` vytvořte objekt pro vytváření pro přenos zpráv s nově vytvořeným identifikátorem URI služby Service Bus jako argument. Následující kód přidejte přímo po operacích správy, které jste přidali v předchozím kroku. Ujistěte se, že jste `<yourNamespace>` nahradili názvem vašeho oboru názvů služby:
   
    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```
3. Dál vytvořte objekt fronty pomocí třídy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx). Následující kód přidejte přímo po kódu, který jste přidali v posledním kroku:
   
    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```
4. Dál přidejte kód který projde seznam zprostředkovaných zpráv, které jste předtím vytvořili, a každou z nich odešle do fronty. Následující kód přidejte přímo po příkazu `CreateQueueClient()` v předchozím kroku:
   
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Přijetí zpráv z fronty
V tomto kroku získáte seznam zpráv z fronty, kterou jste vytvořili v předchozím kroku.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Vytvoření příjemce a přijetí zpráv z fronty
V metodě `Queue()` iterujte frontou a přijměte zprávy pomocí metody [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) a každou zprávu vypište do konzoly. Následující kód přidejte přímo po kódu, který jste přidali v předchozím kroku:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Pamatujte, že `Thread.Sleep` se používá jen k simulování zpracování zprávy a v opravdové aplikaci pro přenos zpráv nejspíš nebude potřeba.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Ukončení metody Queue a vyčištění prostředků
Přímo po předchozím kódu přidejte následující kód, který vyčistí objekt pro vytváření zpráv a prostředky fronty:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Zavolání metody Queue
Posledním krokem je přidání příkazu, který z `Queue()` zavolá metodu `Main()`. Následující zvýrazněný řádek přidejte na konec Main():

```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();

    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);

    // Add this call
    Queue();
}
```

### <a name="example"></a>Příklad
Tento kód obsahuje kompletní aplikaci **QueueSample**.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }

            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");

            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Sestavení a spuštění aplikace QueueSample
Po dokončení předchozích kroků můžete sestavit a spustit aplikaci **QueueSample**.

### <a name="build-the-queuesample-application"></a>Sestavení aplikace QueueSample
V nabídce **Sestavení** ve Visual Studiu klikněte na **Sestavit řešení** nebo stiskněte **Ctrl+Shift+B**. Pokud narazíte na chyby, zkontrolujte podle příkladu na konci předchozího kroku, že jste kód napsali správně.

## <a name="next-steps"></a>Další kroky
Tento kurz vám ukázal, jak sestavit službu a klientskou aplikaci služby Service Bus pomocí funkcí zprostředkovaného zasílání zpráv služby Service Bus. Podobný kurz, který používá Service Bus [Relay](service-bus-messaging-overview.md#Relayed-messaging), najdete v tématu [Kurz přenosu zpráv přes předávací službu Service Bus](../service-bus-relay/service-bus-relay-tutorial.md).

Pokud se o službě [Service Bus](https://azure.microsoft.com/services/service-bus/) chcete dozvědět víc, pročtěte si následující témata.

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Service Bus](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
* [Architektura služby Service Bus](../service-bus/service-bus-architecture.md)

<!--HONumber=Oct16_HO3-->


