---
title: "Přijímat události z Azure Event Hubs pomocí Java | Microsoft Docs"
description: "Začínáme přijetí ze služby Event Hubs používá Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 3c1b455e6298367dc50f0943b58f6cf1e7f1c5fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Přijímat události z Azure Event Hubs pomocí Java


## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací. Když Event Hubs shromáždí data, můžete je zpracovat a uložit pomocí libovolného úložného clusteru nebo poskytovatele datové analýzy v reálném čase.

Další informace najdete v tématu [Přehled služby Event Hubs][Event Hubs overview].

Tento kurz ukazuje, jak přijímat události do centra událostí pomocí konzolové aplikace napsané v jazyce Java.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete následující požadavky:

* Vývojové prostředí Java. V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Přijímání zpráv pomocí třídy EventProcessorHost v Javě

**EventProcessorHost** je třída Java, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs. Pomocí třídy EventProcessorHost, můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech. Tento příklad ukazuje způsob použití třídy EventProcessorHost pro jednoho příjemce.

### <a name="create-a-storage-account"></a>vytvořit účet úložiště
Chcete-li použít EventProcessorHost, musíte mít [účet úložiště Azure][Azure Storage account]:

1. Přihlaste se na [portál Azure][Azure portal]a klikněte na tlačítko **+ nový** na levé straně obrazovky.
2. Klikněte na **Storage** a poté klikněte na **Účet úložiště**. V okně **Vytvořit účet úložiště** zadejte název účtu úložiště. Dokončete polí, vyberte požadovanou oblast a pak klikněte na tlačítko **vytvořit**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Klikněte na nově vytvořený účet úložiště a potom klikněte na **Spravovat přístupové klíče**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Zkopírujte primární přístupový klíč do dočasného umístění, k pozdějšímu použití v tomto kurzu.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Vytvoření projektu jazyka Java pomocí hostitele EventProcessor
Klientská knihovna Java pro službu Event Hubs je k dispozici pro použití v projektech Maven z [centrálním úložišti Maven][Maven Package]a můžete odkazovat pomocí následující prohlášení závislostí v souboru projektu Maven:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-eventhubs-eph</artifactId>
  <version>0.14.0</version>
</dependency>
```

Pro různé typy prostředí sestavení, můžete explicitně získat nejnovější vydaná JAR soubory z [centrálním úložišti Maven] [ Maven Package] nebo z [distribuční bod verze na Githubu](https://github.com/Azure/azure-event-hubs/releases).  

1. Pro následující příklad nejprve vytvořte nový projekt Maven pro aplikaci konzoly nebo prostředí v oblíbeném vývojovém prostředí Java. Třídy se nazývá `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Použijte následující kód k vytvoření nové třídy s názvem `EventProcessor`.
   
    ```java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. Vytvořte jeden další třídu s názvem `EventProcessorSample`, pomocí následujícího kódu.
   
    ```java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. Následující pole nahraďte hodnoty používané při vytváření účtu události rozbočovače a úložiště.
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Tento kurz používá jednu instanci třídy EventProcessorHost. Pokud chcete zvýšit propustnost, doporučujeme spustit víc instancí služby EventProcessorHost, pokud možno na samostatných počítačích.  To poskytuje také redundanci. V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí. Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**. Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy EventProcessorHost podle počítačů (nebo rolí), ve kterých jsou nasazené.
> 
> 

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
