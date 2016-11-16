---
title: "Kurz REST pro zprostředkované zasílání zpráv ve službě Service Bus | Dokumentace Microsoftu"
description: "Kurz REST pro zprostředkované zasílání zpráv"
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9b7a8147-a1b1-42fc-b30e-f52e79a902b5
ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 45b72037e2de01b9201edf3e4ebee7e80d996383


---
# <a name="service-bus-brokered-messaging-rest-tutorial"></a>Kurz REST pro zprostředkované zasílání zpráv ve službě Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

V tomto kurzu se dozvíte, jak vytvořit základní frontu a téma/odběr pro Azure Service Bus na základě REST.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů
Nejdřív je potřeba vytvořit obor názvů služby a získat klíč [sdíleného přístupového podpisu](service-bus-sas-overview.md) (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby. Kombinace oboru názvů služby a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-console-client"></a>Vytvoření konzolového klienta
Fronty služby Service Bus vám umožňují ukládat zprávy do fronty FIFO (first in, fisrt out). Témata a odběry implementují vzorec publikovat/odebírat – vytvoříte téma a pak vytvoříte jeden nebo víc odběrů přiřazených k tomu tématu. Když se do tématu odešlou zprávy, okamžitě se odešlou odběratelům tohoto témata.

Kód v tomto kurzu provádí toto:

* Použije váš obor názvů a klíč [Sdíleného přístupového podpisu](service-bus-sas-overview.md) (SAS) k získání přístupu k vašim prostředkům oboru názvů služby Service Bus.
* Vytvoří frontu, odešle do ní zprávu a přečte zprávu z fronty.
* Vytvoří téma a odběr tohoto tématu, odešle do odběru zprávu a přečte ji z odběru.
* Načte ze služby Service Bus všechny informace o frontě, tématu a odběru, včetně pravidel odběru.
* Odstraní prostředky fronty, témata a odběru.

Jelikož je tato služba webová služba ve stylu REST, nepoužívají se žádné speciální typy, protože se při celé výměně používají řetězce. To znamená, že projekt sady Visual Studio nesmí odkazovat na žádné knihovny služby Service Bus.

Když v prvním kroku získáte pověření a obor názvů, můžete pak v sadě Visual Studio vytvořit základní konzolovou aplikaci.

### <a name="create-a-console-application"></a>Vytvoření konzolové aplikace
1. Spusťte Visual Studio jako správce tak, že v nabídce **Start** kliknete na program pravým tlačítkem a vyberete možnost **Spustit jako správce**.
2. Vytvořte nový projekt konzolové aplikace. Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**. V dialogovém okně **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**), klikněte na šablonu **Konzolová aplikace** a pojmenujte ji jako **Microsoft.ServiceBus.Samples**. Použijte výchozí Umístění. Kliknutím na tlačítko **OK** vytvořte projekt.
3. V souboru Program.cs zkontrolujte, že vaše příkazy `using` vypadají takto:
   
    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```
4. V případě potřeby přejmenujte obor názvů pro program z výchozího názvu Visual Studia na `Microsoft.ServiceBus.Samples`.
5. Ve třídě `Program` přidejte tyto globální proměnné:
   
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```
6. V `Main()` vložte tento kód:
   
    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
   
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
   
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
   
        string queueName = "Queue" + Guid.NewGuid().ToString();
   
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
   
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
   
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
   
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
   
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
   
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
   
        // Delete the queue we created
        DeleteResource(queueName);
   
        // Delete the topic we created
        DeleteResource(topicName);
   
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
   
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## <a name="create-management-credentials"></a>Vytvoření pověření ke správě
V dalším kroku se napíše metoda, která zpracuje obor názvů a klíč SAS, které jste zadali v předchozím kroku, a vrátí SAS token. Tento příklad vytvoří SAS token, který bude platit jednu hodinu.

### <a name="create-a-getsastoken-method"></a>Vytvořte metodu GetSASToken()
Následující kód vložte za metodu `Main()` ve třídě `Program`:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## <a name="create-the-queue"></a>Vytvoření fronty
V dalším kroku se napíše metoda, která pomocí příkazu HTTP PUT ve stylu REST vytvoří frontu.

Následující kód vložte přímo po kódu `GetSASToken()`, který jste přidali v předchozím kroku:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="send-a-message-to-the-queue"></a>Zaslání zprávy do fronty
V tomto kroku přidáte metodu, která pomocí příkazu HTTP POST ve stylu REST odešle zprávu do fronty, kterou jste vytvořili v předchozím kroku.

1. Následující kód vložte přímo po kódu `CreateQueue()`, který jste přidali v předchozím kroku:
   
    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
   
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```
2. V hlavičce HTTP `BrokerProperties` jsou umístěné standardní vlastnosti zprostředkované zprávy. Vlastnosti zprostředkovatele musí být serializované ve formátu JSON. Pro zprávu chcete specifikovat hodnotu **TimeToLive** 30 sekund a přidat označení zprávy „M1“, proto přidejte následující kód bezprostředně před volání `webClient.UploadData()` ukázané v předchozím příkladu:
   
    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```
   
    Nezapomeňte, že se přidaly a přidají vlastnosti zprostředkované zprávy. Požadavek na odeslání proto musí specifikovat verzi API, která podporuje všechny vlastnosti zprostředkované zprávy, které jsou součástí požadavku. Pokud specifikovaná verze API nepodporuje některou vlastnost zprostředkované zprávy, bude se taková vlastnost ignorovat.
3. Vlastní vlastnosti zprávy jsou definované jako sada párů klíčových hodnot. Každá vlastní vlastnost je uložená ve své vlastní hlavičce TPPT. Chcete přidat vlastní vlastnosti „Priority“ a „Customer“, proto přidejte následující kód bezprostředně před volání `webClient.UploadData()` ukázané v předchozím příkladu:
   
    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## <a name="receive-and-delete-a-message-from-the-queue"></a>Přijetí a odstranění zprávy z fronty
V dalším kroku se přidá metoda, která pomocí příkazu HTTP DELETE ve stylu REST přijme a odstraní zprávu z fronty.

Následující kód vložte přímo po kódu `SendMessage()`, který jste přidali v předchozím kroku:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## <a name="create-a-topic-and-subscription"></a>Vytvoření témata a odběru
V dalším kroku se napíše metoda, která pomocí příkazu HTTP PUT ve stylu REST vytvoří téma. Potom napíšete metodu, která vytvoří odběr tohoto tématu.

### <a name="create-a-topic"></a>Vytvoření tématu
Následující kód vložte přímo po kódu `ReceiveAndDeleteMessage()`, který jste přidali v předchozím kroku:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### <a name="create-a-subscription"></a>Vytvoření odběru
Následující kód vytvoří odběr tématu, který jste vytvořili v předchozím kroku. Následující kód přidejte přímo po definici `CreateTopic()`:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="retrieve-message-resources"></a>Načtení prostředků zprávy
V tomto kroku přidáte kód, který načte vlastnosti zprávy, a pak odstraní prostředky přenosu zpráv vytvořené v předchozím kroku.

### <a name="retrieve-an-atom-feed-with-the-specified-resources"></a>Načtení informačního kanálu Atom se zadanými prostředky
Následující kód přidejte přímo po metodě `CreateSubscription()`, kterou jste přidali v předchozím kroku:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### <a name="delete-messaging-entities"></a>Odstranění entit přenosu zpráv
Následující kód přidejte přímo po kódu, který jste přidali v předchozím kroku:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### <a name="format-the-atom-feed"></a>Formátování informačního kanálu Atom
Metoda `GetResources()` obsahuje volání metody `FormatXml()`, která přeformátuje načtený informační kanál Atom tak, aby byl lépe čitelný. Následující kód je definice `FormatXml()`, přidejte ho přímo po kódu `DeleteResource()`, který jste přidali v předchozí části:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## <a name="build-and-run-the-application"></a>Sestavení a spuštění aplikace
Teď můžete aplikaci sestavit a spustit. V nabídce **Sestavení** v sadě Visual Studio klikněte na **Sestavit řešení** nebo stiskněte **Ctrl+Shift+B**.

### <a name="run-the-application"></a>Spuštění aplikace
Pokud se neobjevily žádné chyby, spusťte aplikaci stisknutím klávesy F5. Když se zobrazí příslušná výzva, zadejte obor názvů, název klíče SAS a hodnotu klíče SAS, kterou jste získali v prvním kroku.

### <a name="example"></a>Příklad
Následující příklad obsahuje kompletní kód tak, jak by měl vypadat, pokud jste správně provedli všechny kroku v tomto kurzu.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
Další informace najdete v těchto článcích:

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kurz k rozhraní REST pro Service Bus Relay](../service-bus-relay/service-bus-relay-rest-tutorial.md)




<!--HONumber=Nov16_HO2-->


