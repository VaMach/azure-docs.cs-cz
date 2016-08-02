<properties
    pageTitle="Začínáme se službou Azure IoT Hub pro Javu | Microsoft Azure"
    description="Úvodní kurz pro službu Azure IoT Hub pro Javu. Implementace internetu věcí pomocí služby Azure IoT Hub a jazyka Java spolu se sadami SDK služby Microsoft Azure IoT."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/06/2016"
     ms.author="dobett"/>

# Začínáme se službou Azure IoT Hub pro Javu

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Úvod

Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení pro internet věcí (IoT) a back-endem řešení. Jeden z největších problémů, kterým projekty IoT čelí, je způsob spolehlivého a bezpečného připojení zařízení k back-endu řešení. Tento problém služba IoT Hub řeší následovně:

- Nabízí spolehlivé, flexibilně škálovatelné zasílání zpráv ze zařízení do cloudu a z cloudu do zařízení.
- Umožňuje zabezpečit komunikaci pomocí zabezpečených přihlašovacích údajů a řízení přístupu do zařízení.
- Zahrnuje knihovny zařízení pro nejoblíbenější jazyky a platformy.

V tomto kurzu získáte informace o následujících postupech:

- vytvoření služby IoT Hub pomocí Portálu Azure
- vytvoření identity zařízení ve službě IoT Hub
- vytvoření simulovaného zařízení, které odesílá telemetrická data do back-endu vašeho cloudu

Na konci tohoto kurzu budete mít tři konzolové aplikace Java:

* **create-device-identity** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulovaného zařízení.
* **read-d2c-messages** zobrazuje telemetrická data odesílaná simulovaným zařízením.
* **simulated-device** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu AMQPS.

> [AZURE.NOTE] V článku [Sady SDK služby IoT Hub][lnk-hub-sdks] naleznete informace o různých sadách SDK, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení.

Pro absolvování tohoto kurzu potřebujete:

+ Java SE 8. <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace Javy pro tento návod v systému Windows nebo Linux.

+ Maven 3.  <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace nástroje Maven pro tento návod v systému Windows nebo Linux.

+ Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

V posledním kroku klikněte v okně služby IoT Hub na tlačítko **Nastavení** a poté v okně **Nastavení** na **Zasílání zpráv**. V okně **Zasílání zpráv** si poznamenejte **název kompatibilní s centrem událostí** a **koncový bod kompatibilní s centrem událostí**. Tyto hodnoty budete potřebovat při vytváření aplikace **read-d2c-messages**.

![][6]

Nyní jste vytvořili  službu IoT Hub, máte název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub a název a koncový bod kompatibilní s centrem událostí,  které potřebujete k dokončení kurzu.

## Vytvoření identity zařízení

V této části vytvoříte konzolovou aplikaci Java, která v registru identit ve službě IoT Hub vytvoří novou identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit zařízení. Další informace najdete v části **Registr identit zařízení** tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

1. Vytvořte novou prázdnou složku s názvem iot-java-get-started. Ve složce iot-java-get-started vytvořte pomocí následujícího příkazu na příkazovém řádku nový projekt Maven s názvem **create-device-identity**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do nové složky create-device-identity.

3. Pomocí textového editoru otevřete ve složce create-device-identity soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. To vám umožní použít v aplikaci balíček iothub-service-sdk.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.2</version>
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

7. Do třídy **App** přidejte následující proměnné na úrovni třídy a nahraďte zástupné symboly **{yourhubname}** a **{yourhubkey}** hodnotami, které jste si dříve poznamenali:

    ```
    private static final String connectionString = "HostName={yourhubname}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={yourhubkey}";
    private static final String deviceId = "javadevice";
    
    ```
    
8. Upravte podpis metody **Main** tak, aby zahrnoval výjimky uvedené níže:

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
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Soubor App.java uložte a zavřete.

11. Aplikaci **create-device-identity** pomocí nástroje Maven sestavíte tak, že na příkazovém řádku ve složce create-device-identity spustíte následující příkaz:

    ```
    mvn clean package -DskipTests
    ```

12. Aplikaci **create-device-identity** pomocí nástroje Maven spustíte tak, že na příkazovém řádku ve složce create-device-identity spustíte následující příkaz:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Poznamenejte si **ID zařízení** a **klíč zařízení**. Budete je potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [AZURE.NOTE] V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu do služby. Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].

## Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní s [centrem událostí][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, získáte v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.

> [AZURE.NOTE] Kompatibilní koncový bod centra událostí pro čtení zpráv typu zařízení-cloud vždy používá protokol AMQPS.

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu na příkazovém řádku nový projekt Maven s názvem **read-d2c-messages**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do nové složky read-d2c-messages.

3. Pomocí textového editoru otevřete ve složce read-d2c-messages soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. Tato funkce umožňuje čtení z koncového bodu kompatibilního s centrem událostí pomocí balíčku eventhubs-client ve vaší aplikaci:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.7.1</version> 
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

7. Do třídy **App** přidejte následující proměnné na úrovni třídy. Zástupné symboly **{youriothubkey}**, **{youreventhubcompatiblenamespace}** a **{youreventhubcompatiblename}** nahraďte hodnotami, které jste si poznamenali dříve. Hodnota zástupného symbolu **{youreventhubcompatiblenamespace}** pochází z **koncového bodu kompatibilního s centrem událostí** a má tuto podobu: **xyznamespace** (jinými slovy, v portálu odeberte z hodnoty koncového bodu kompatibilního s centrem událostí předponu **sb://** a příponu **. servicebus.windows.net**):

    ```
    private static String namespaceName = "{youreventhubcompatiblenamespace}";
    private static String eventHubName = "{youreventhubcompatiblename}";
    private static String sasKeyName = "iothubowner";
    private static String sasKey = "{youriothubkey}";
    private static long now = System.currentTimeMillis();
    ```

8. Do třídy **App** přidejte následující metodu **receiveMessages**. Tato metoda vytvoří instanci **EventHubClient** k připojení ke koncovému bodu kompatibilnímu s centrem událostí a poté asynchronně vytvoří instanci **PartitionReceiver** ke čtení z oddílu centra událostí. Až do ukončení aplikace se bude nepřetržitě provádět v cyklu a tisknout podrobnosti o zprávách.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
        client = EventHubClient.createFromConnectionString(connStr.toString()).get();
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
                Iterable<EventData> receivedEvents = receiver.receive().get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
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

    > [AZURE.NOTE] Tato metoda při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. To je užitečné v testovacím prostředí, protože uvidíte aktuální sadu zpráv, ale v produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace naleznete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].

9. Upravte podpis metody **Main** tak, aby zahrnoval výjimku uvedenou níže:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Ve třídě **App** přidejte do metody **Main** následující kód. Kód vytvoří dvě instance **EventHubClient** a **PartitionReceiver** a umožní vám po dokončení zpracování zpráv zavřít aplikaci.

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

    > [AZURE.NOTE] Tento kód předpokládá, že jste vytvořili službu IoT Hub na úrovni F1 (Free). Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.

11. Soubor App.java uložte a zavřete.

12. Aplikaci **read-d2c-messages** pomocí nástroje Maven sestavíte tak, že na příkazovém řádku ve složce read-d2c-messages spustíte následující příkaz:

    ```
    mvn clean package -DskipTests
    ```

## Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu na příkazovém řádku nový projekt Maven s názvem **simulated-device**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do nové složky simulated-devices.

3. Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti. To vám umožní komunikovat se službou IoT Hub a serializovat objekty Java do formátu JSON pomocí balíčku iothub-java-client ve vaší aplikaci:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.2</version>
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
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.Random;
    import javax.naming.SizeLimitExceededException;
    import com.google.gson.Gson;
    ```

7. Do třídy **App** přidejte následující proměnné na úrovni třídy a nahraďte zástupný symbol **{youriothubname}** názvem služby IoT Hub a zástupné symboly **{yourdeviceid}** a **{yourdevicekey}** hodnotami zařízení vygenerovanými v části *Vytvoření identity zařízení*:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQPS;
    private static boolean stopThread = false;
    ```

    Tato ukázková aplikace používá při vytváření instance  objektu **DeviceClient** proměnnou **protokolu**. Ke komunikaci se službou IoT Hub můžete použít protokol HTTPS nebo AMQPS.

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

9. Za účelem zobrazení stavu potvrzení, které služba IoT Hub vrací po zpracování zprávy ze simulovaného zařízení, přidejte do třídy **Aplikace** následující vnořenou třídu **EventCallback**. Tato metoda také po zpracování zprávy upozorní hlavní vlákno v aplikaci.

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status " + status.name());
      
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
          DeviceClient client;
          client = new DeviceClient(connString, protocol);
          client.open();
        
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = "myFirstDevice";
            telemetryDataPoint.windSpeed = currentWindSpeed;
      
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println(msgStr);
        
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
          client.close();
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    }
    ```

    Tato metoda odešle novou zprávu typu zařízení-cloud sekundu poté, co služba IoT Hub potvrdí předchozí zprávu. Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovaným číslem, který simuluje snímač rychlosti větru.

11. Metodu **Main** nahraďte následujícím kódem, který vytvoří vlákno k odesílání zpráv typu zařízení-cloud do služby IoT Hub:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
    
      MessageSender ms0 = new MessageSender();
      Thread t0 = new Thread(ms0);
      t0.start(); 
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      ms0.stopThread = true;
    }
    ```

12. Soubor App.java uložte a zavřete.

13. Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že na příkazovém řádku ve složce simulated-device spustíte následující příkaz:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku  [Řešení přechodných chyb][lnk-transient-faults] programu MSDN.

## Spuštění aplikací

Nyní můžete spustit aplikace.

1. V příkazovém řádku ve složce read-d2c spusťte následující příkaz, aby se začal monitorovat první oddíl služby IoT Hub:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="0"
    ```

    ![][7]

1. V příkazovém řádku ve složce read-d2c spusťte následující příkaz, aby se začal monitorovat druhý oddíl služby IoT Hub:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="1"
    ```

    ![][7]

2. V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![][8]

3. Na dlaždici **Využití** v [portálu Azure][lnk-portal] se zobrazuje počet zpráv odeslaných do služby:

    ![][43]

## Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT Hub v portálu a poté jste vytvořili identitu zařízení v registru identit služby. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby a vytvořili jste aplikaci, která zobrazuje zprávy přijaté službou. V následujících kurzech můžete pokračovat v průzkumu funkcí služby IoT Hub a dalších scénářů platformy IoT:

- V kurzu [Odesílání zpráv typu cloud-zařízení pomocí služby IoT Hub][lnk-c2d-tutorial] zjistíte, jak posílat zprávy do zařízení a zpracovávat zpětnou vazbu o doručení vytvořenou službou IoT Hub.
- V kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial] zjistíte, jak spolehlivě zpracovávat telemetrické a interaktivní zprávy přicházející ze zařízení.
- Kurz [Nahrávání souborů ze zařízení][lnk-upload-tutorial] popisuje schéma, které usnadňuje nahrávání souborů ze zařízení pomocí zpráv typu cloud-zařízení.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


