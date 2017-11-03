---
title: "Odesílání událostí do centra událostí Azure pomocí Java | Microsoft Docs"
description: "Začínáme odesílá do centra událostí se používá Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b31771001989e20b88bc8d7bca1afceb58ec197c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Odesílání událostí do centra událostí Azure používá Java

## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací. Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.

Další informace najdete v tématu [Přehled služby Event Hubs][Event Hubs overview].

Tento kurz ukazuje, jak odesílat události do centra událostí pomocí konzolové aplikace v jazyce Java. Chcete-li přijímat události pomocí knihovny Java Event Processor Host, přečtěte si téma [v tomto článku](event-hubs-java-get-started-receive-eph.md), nebo klikněte na příslušný jazyk přijímající v levé tabulce obsahu.

K dokončení tohoto kurzu budete potřebovat následující:

* Vývojové prostředí Java. V tomto kurzu budeme předpokládat [Eclipse](https://www.eclipse.org/).
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatná zkušební verze Azure</a>.

## <a name="send-messages-to-event-hubs"></a>Zasílání zpráv do služby Event Hubs
Klientská knihovna Java pro službu Event Hubs je k dispozici pro použití v projektech Maven z [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Tato knihovna pomocí následující prohlášení závislostí v souboru projektu Maven, můžete odkazovat:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Pro různé typy prostředí sestavení, můžete explicitně získat nejnovější vydaná JAR soubory z [Maven centrálním úložišti](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Jednoduchá událost vydavatel, importovat *com.microsoft.azure.eventhubs* balíček pro třídy klienta služby Event Hubs a *com.microsoft.azure.servicebus* balíček pro nástroj třídy, jako běžné výjimky, které jsou sdíleny s klientem zasílání zpráv Azure Service Bus. 

Pro následující příklad nejprve vytvořte nový projekt Maven pro aplikaci konzoly nebo prostředí v oblíbeném vývojovém prostředí Java. Název třídy `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Obor názvů a událostí názvy rozbočovačů nahraďte hodnoty používané při vytváření centra událostí.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Pak vytvořte singulární událostí pomocí transformace řetězec na jeho kódování bajtů ve formátu UTF-8. Poté vytvořte novou instanci služby Event Hubs klienta z připojovacího řetězce a odeslat zprávu.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přijímat události pomocí knihovny EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Přehled služby Event Hubs][Event Hubs overview]
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md