---
title: "Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core) | Microsoft Docs"
description: "Jak začít používat fronty Azure storage v projektu ASP.NET Core v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak začít používat Azure Queue storage v sadě Visual Studio, po vytvoření a odkazuje pomocí sady Visual Studio účet úložiště Azure v projektu ASP.NET Core **přidat připojení služby** dialogové okno. **Přidat připojení služby** operaci nainstaluje příslušné balíčky NuGet pro přístup k úložišti Azure ve vašem projektu a přidá připojovací řetězec pro účet úložiště v konfiguračních souborech projektu.

Azure queue storage je služba pro ukládání velkého počtu zpráv, které jsou přístupné odkudkoli na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až 64 kilobajtů (KB) velikost a jedna fronta můžete obsahovat miliony zpráv, až do limitu celkové kapacity účtu úložiště.

Abyste mohli začít, musíte nejprve vytvořit fronty Azure ve vašem účtu úložiště. Ukážeme vám postup vytvoření fronty v kódu. Jsme budete také ukazují, jak provést základní fronty operací, jako je přidání, úprava, čtení a odebrání zprávy do fronty. Ukázky jsou napsané v jazyce C\# kód a použít knihovnu klienta služby Azure Storage pro .NET. Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).

**Poznámka:** některé z rozhraní API, která provádět volání do úložiště Azure v ASP.NET Core jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Následující kód předpokládá, že asynchronní programování metody jsou používány.

* V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) Další informace o programu manipulace s fronty.
* V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.
* V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.
* V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.

## <a name="access-queues-in-code"></a>Přístup front v kódu
Chcete-li získat přístup k frontám projektů ASP.NET Core, zahrnují následující položky do zdrojového souboru C#, která používá fronty Azure storage.

1. Zajistěte, aby deklarace oboru názvů v horní části souboru C# zahrnují tyto **pomocí** příkazy.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použít následující kód k získání připojovací řetězec úložiště a informace o účtu úložiště z konfigurace služby Azure.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Získání **CloudQueueClient** objekt, který má odkazovat na objekty fronty ve vašem účtu úložiště.  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Získání **CloudQueue** objekt, který má odkazovat na konkrétní fronty.
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Poznámka:** používat všechny výše uvedené kódu před kód v následující ukázky.

### <a name="create-a-queue-in-code"></a>Vytvoření fronty v kódu
Pokud chcete vytvořit fronty Azure v kódu, stačí přidat volání **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a>Přidání zprávy do fronty
Pokud chcete vložit zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání **AddMessageAsync** metoda.

A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.

Tady je příklad, který se vloží zprávu "Hello, World".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Čtení zprávy ve frontě
Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **PeekMessageAsync** metoda.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Přečtěte si a odebrat zprávu ve frontě
Váš kód můžete odebrat (dequeue –) zprávu z fronty ve dvou krocích.

1. Volání **GetMessageAsync** získat další zprávu ve frontě. Zpráva vrácená metodou **GetMessageAsync** stane neviditelnou pro jakýkoli jiný kód čtení zpráv z této fronty. Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.
2. Chcete-li dokončit odebere ji z fronty, volejte **DeleteMessageAsync**.

Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu. Následující kód volání **DeleteMessageAsync** pravým po zpracování zprávy.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Využívání dalších možností pro vyřazení zprávy
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.
Za prvé si můžete načíst dávku zpráv (až 32). Za druhé si můžete nastavit delší nebo kratší časový limit neviditelnosti, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv. V následujícím příkladu kódu se pomocí metody **GetMessages** získá 20 zpráv v jednom volání. Následně se každá zpráva zpracuje pomocí smyčky **foreach**. Také nastaví časový limit neviditelnosti 5 minut pro každou zprávu. Všimněte si, že spuštění 5 minut pro všechny zprávy ve stejnou dobu, takže po 5 minutách mít předán po zavolání **GetMessages**, všechny zprávy, které nebyly odstraněny opět viditelné.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Získání délky fronty
Podle potřeby můžete získat odhadovaný počet zpráv ve frontě. **FetchAttributes** metoda požádá službu front o načtení atributů fronty, včetně počtu zpráv. **ApproximateMethodCount** vlastnost vrací poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Použití vzoru Async-Await s obecné frontě rozhraní API
Tento příklad ukazuje způsob použití vzoru Async-Await s obecné frontě rozhraní API. Ukázka volá asynchronní verzi každé z daných metod. To může zobrazit opravu asynchronní po každé metody. Při použití asynchronní metody pozastaví vzoru Async-Await místní provádění až do dokončení volání. Toto chování umožňuje aktuálnímu vláknu provádět další činnosti, což přispívá k vyhnout se kritickým bodům výkonu a zlepšuje celkovou rychlost reakce aplikace. Další podrobnosti o použití vzoru Async-Await v rozhraní .NET najdete v tématu [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Odstranění fronty
Pokud budete chtít odstranit frontu se všemi zprávami, které v ní jsou, zavolejte metodu **Delete** pro objekt fronty.

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

