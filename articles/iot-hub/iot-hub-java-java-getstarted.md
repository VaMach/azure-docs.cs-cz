---
title: "Začínáme se službou Azure IoT Hub (Java) | Dokumentace Microsoftu"
description: "Postup odesílání zpráv typu zařízení-cloud ze zařízení do služby Azure IoT Hub pomocí sad SDK Azure IoT pro Javu. Vytvoříte aplikaci simulovaného zařízení pro odesílání zpráv, aplikaci služby pro registraci zařízení v registru identit a aplikaci služby pro čtení zpráv typu zařízení-cloud ze služby IoT Hub."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c2b0c6b125ededd30e9db8e7f42796bdf6b413d4
ms.openlocfilehash: 559ecab373adf6441635f2ed0d572ab02159f50c
ms.lasthandoff: 03/01/2017


---
# <a name="get-started-with-azure-iot-hub-java"></a>Začínáme se službou Azure IoT Hub (Java)
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace Java:

* **create-device-identity** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulované aplikace zařízení.
* **read-d2c-messages** zobrazuje telemetrická data odesílaná simulovanou aplikací zařízení.
* **simulated-device** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Java SE 8. <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace Javy pro tento návod v systému Windows nebo Linux.
* Maven 3.  <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace nástroje [Maven][lnk-maven] pro tento návod v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Na závěr si poznamenejte hodnotu **Primární klíč**. Potom klikněte na **Koncové body** a na integrovaný koncový bod **Události**. V okně **Vlastnosti** si poznamenejte **název kompatibilní s centrem událostí** a adresu **koncového bodu kompatibilního s centrem událostí**. Tyto tři hodnoty budete potřebovat k vytvoření aplikace **read-d2c-messages**.

![Okno Zasílání zpráv služby IoT Hub na webu Azure Portal][6]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub, primární klíč služby IoT Hub a název a koncový bod kompatibilní s centrem událostí, které potřebujete k dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci Java, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části **Registr identit** v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

1. Vytvořte prázdnou složku s názvem iot-java-get-started. Ve složce iot-java-get-started vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **create-device-identity**. Všimněte si, že se jedná o jeden dlouhý příkaz:
   
    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```
2. Na příkazovém řádku přejděte do složky create-device-identity.
3. Pomocí textového editoru otevřete ve složce create-device-identity soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. Tato závislost vám umožní použít v aplikaci balíček iothub-service-sdk:
   
    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.11</version>
    </dependency>
    ```
4. Soubor pom.xml uložte a zavřete.
5. Pomocí textového editoru otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java.
6. Do souboru přidejte následující příkazy pro **import**:
   
    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```
7. Do třídy **App** přidejte následující proměnné na úrovni třídy a nahraďte zástupné symboly **{yourhubconnectionstring}** hodnotou, kterou jste si předtím poznamenali:
   
    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
   
    ```
8. Upravte podpis metody **Main** tak, aby zahrnoval následující výjimky:
   
    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
9. Přidejte následující kód jako obsah metody **Main**. Tento kód vytvoří v registru identit služby IoT Hub zařízení s názvem *javadevice* (pokud zatím neexistuje). Potom zobrazí ID a klíč zařízení, které budete později potřebovat:
   
    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
   
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device ID: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```
10. Soubor App.java uložte a zavřete.
11. Aplikaci **create-device-identity** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:
    
    ```
    mvn clean package -DskipTests
    ```
12. Aplikaci **create-device-identity** pomocí nástroje Maven spustíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:
    
    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```
13. Poznamenejte si **ID zařízení** a **Klíč zařízení**. Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hub][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Způsoby zpracování zpráv typu zařízení-cloud v různých škálách najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.
> 
> 

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **read-d2c-messages**. Všimněte si, že se jedná o jeden dlouhý příkaz:
   
    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```
2. Na příkazovém řádku přejděte do složky read-d2c-messages.
3. Pomocí textového editoru otevřete ve složce read-d2c-messages soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. Tato závislost umožňuje čtení z koncového bodu kompatibilního s centrem událostí pomocí balíčku eventhubs-client ve vaší aplikaci:
   
    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.10.0</version> 
    </dependency>
    ```
4. Soubor pom.xml uložte a zavřete.
5. Pomocí textového editoru otevřete soubor  read-d2c-messages\src\main\java\com\mycompany\app\App.java.
6. Do souboru přidejte následující příkazy pro **import**:
   
    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
   
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```
7. Do třídy **App** přidejte následující proměnné na úrovni třídy. Zástupné symboly **{youriothubkey}**, **{youreventhubcompatibleendpoint}** a **{youreventhubcompatiblename}** nahraďte hodnotami, které jste si předtím poznamenali:
   
    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```
8. Do třídy **App** přidejte následující metodu **receiveMessages**. Tato metoda vytvoří instanci **EventHubClient** k připojení ke koncovému bodu kompatibilnímu s centrem událostí a poté asynchronně vytvoří instanci **PartitionReceiver** ke čtení z oddílu centra událostí. Až do ukončení aplikace se bude nepřetržitě provádět v cyklu a vypisovat podrobnosti o zprávách.
   
    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```
   
   > [!NOTE]
   > Tato metoda při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. Tato metoda je užitečná v testovacím prostředí, protože uvidíte aktuální sadu zpráv. V produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].
   > 
   > 
9. Upravte podpis metody **Main** tak, aby zahrnoval následující výjimku:
   
    ```
    public static void main( String[] args ) throws IOException
    ```
10. Ve třídě **App** přidejte do metody **Main** následující kód. Tento kód vytvoří dvě instance **EventHubClient** a **PartitionReceiver** a umožní vám po dokončení zpracování zpráv zavřít aplikaci:
    
    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```
    
    > [!NOTE]
    > Tento kód předpokládá, že jste vytvořili službu IoT Hub na úrovni F1 (Free). Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.
    > 
    > 
11. Soubor App.java uložte a zavřete.
12. Aplikaci **read-d2c-messages** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce read-d2c-messages spustíte následující příkaz:
    
    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **simulated-device**. Všimněte si, že se jedná o jeden dlouhý příkaz:
   
    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```
2. Na příkazovém řádku přejděte do složky simulated-devices.
3. Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti. Tato závislost vám umožní komunikovat se službou IoT Hub a serializovat objekty Java do formátu JSON pomocí balíčku iothub-java-client ve vaší aplikaci:
   
    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.16</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```
4. Soubor pom.xml uložte a zavřete.
5. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.
6. Do souboru přidejte následující příkazy pro **import**:
   
    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```
7. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahraďte hodnotu **{youriothubname}** názvem vaší služby IoT Hub a hodnotu **{yourdevicekey}** klíčem zařízení, který jste vygenerovali v části *Vytvoření identity zařízení*:
   
    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**. Ke komunikaci se službou IoT Hub můžete použít protokol MQTT, AMQP nebo HTTP.
8. Telemetrická data, která vaše zařízení odesílá do služby IoT Hub, určete přidáním následující vnořené třídy **TelemetryDataPoint** do třídy **App**.
   
    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. Za účelem zobrazení stavu potvrzení, které služba IoT Hub vrací po zpracování zprávy ze simulované aplikace zařízení, přidejte do třídy **Aplikace** následující vnořenou třídu **EventCallback**. Tato metoda také po zpracování zprávy upozorní hlavní vlákno v aplikaci:
   
    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```
10. Do třídy **App** přidejte následující vnořenou třídu **MessageSender**. Metoda **run** v této třídě vygeneruje ukázková telemetrická data, která se odešlou do služby IoT Hub, a před odesláním další zprávy počká na potvrzení:
    
    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
    
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
    
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
    
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```
    
    Tato metoda odešle novou zprávu typu zařízení-cloud sekundu poté, co služba IoT Hub potvrdí předchozí zprávu. Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovaným číslem, který simuluje snímač rychlosti větru.
11. Metodu **Main** nahraďte následujícím kódem, který vytvoří vlákno k odesílání zpráv typu zařízení-cloud do služby IoT Hub:
    
    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```
12. Soubor App.java uložte a zavřete.
13. Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:
    
    ```
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.
> 
> 

## <a name="run-the-apps"></a>Spouštění aplikací
Nyní jste připraveni aplikaci spustit.

1. V příkazovém řádku ve složce read-d2c spusťte následující příkaz, aby se začal monitorovat první oddíl služby IoT Hub:
   
    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```
   
    ![Aplikace služby Java IoT Hub pro monitorování zpráv typu zařízení-cloud][7]
2. V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:
   
    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```
   
    ![Aplikace zařízení služby Java IoT Hub pro odesílání zpráv typu zařízení-cloud][8]
3. Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:
   
    ![Dlaždice Použití webu Azure Portal se zobrazením počtu zpráv odeslaných do služby IoT Hub][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se sadou IoT Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven]: https://maven.apache.org/what-is-maven.html
