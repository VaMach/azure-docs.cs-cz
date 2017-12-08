---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (Java) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. Použití zařízení Azure IoT SDK pro jazyk Java k implementaci aplikace simulovaného zařízení a sady SDK pro jazyk Java k implementaci aplikační služby, která upraví konfiguraci zařízení dvojče zařízení pomocí služby Azure IoT."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: dobett
ms.openlocfilehash: 3881e3791c97baf2922722f01f9e6e6ea55ed2e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Použijte požadované vlastnosti pro konfiguraci zařízení

[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci tohoto kurzu máte dvě aplikace konzoly v jazyce Java:

* **simulated-device**, aplikaci ze simulovaného zařízení, která čeká na aktualizace požadované konfigurace a hlásí stav procesu aktualizace simulované konfigurace.
* **Konfigurace sady**, back-end aplikace, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.

> [!NOTE]
> Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**. V takovém případě můžete přeskočit na [vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci Java, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a pak proces aktualizace simulované konfigurace hlásí aktualizace.

1. Vytvořte prázdnou složku s názvem dt-get-started.

1. Ve složce dt-get-started vytvořte projekt Maven s názvem **simulated-device** pomocí následujícího příkazu na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte do složky simulated-devices.

1. Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a přidejte následující závislost na **závislosti** uzlu. Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.30</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat pro nejnovější verzi **klienta zařízení iot** pomocí [Maven hledání] [lnk-maven zařízení search].

1. Přidejte následující **sestavení** uzlu po **závislosti** uzlu. Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Soubor pom.xml uložte a zavřete.

1. Pomocí textového editoru, otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java zdroje.

1. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahraďte **{yourdeviceconnectionstring}** jste si poznamenali v připojovacím řetězcem zařízení *vytvoření identity zařízení* části:

    ```java
    private static String deviceId = "myDeviceId";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    private static TelemetryConfig telemetryConfig;
    ```

1. Chcete-li implementovat obslužná rutina zpětného volání pro zařízení twin stav události (pro účely ladění), přidejte následující vnořenou třídu k **aplikace** třídy:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback 
    {
        public void execute(IotHubStatusCode status, Object context) 
        {
            //System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. Přidejte následující vnořenou třídu k **aplikace** třídy:

    ```java
    private static class TelemetryConfig extends Device 
    {
        private String configId = "0000";
        private String sendFrequency = "45m";

        private Boolean initialRun = true;
        
        private Property telemetryConfig = new Property("telemetryConfig", "{configId=" + configId + ", sendFrequency=" + sendFrequency + "}");

        public void InitTelemetry() throws IOException 
        {
            System.out.println("Report initial telemetry config:");
            System.out.println(this.telemetryConfig);

            this.setReportedProp(this.telemetryConfig);

            client.sendReportedProperties(this.getReportedProp());
        }

        private void UpdateReportedConfiguration() 
        {
            try {
                System.out.println("Initiating config change");
                    
                this.setReportedProp(this.telemetryConfig);
                client.sendReportedProperties(this.getReportedProp());
                    
                System.out.println("Simulating device reset");
                
                Thread.sleep(10000);
                
                System.out.println("Completing config change");
                System.out.println("Config change complete");
            } catch (Exception e) {
                System.out.println("Exception \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
            }
        }

        @Override
        public void PropertyCall(String propertyKey, Object propertyValue, Object context) 
        {
            if (propertyKey.equals("telemetryConfig")) {
                if (!(initialRun)) {
                    System.out.println("Desired property change:");
                    System.out.println(propertyKey + ":" + propertyValue);

                    telemetryConfig.setValue(propertyValue);

                    UpdateReportedConfiguration();
                } else {
                    initialRun = false;
                }
            } 
        }
    }
    ```

    > [!NOTE]
    > Rozšiřuje třídu TelemetryConfig [třídu zařízení] [ lnk-java-device-class] k získání přístupu k zpětná volání požadované vlastnosti.

1. Upravte podpis metody **hlavní** metoda k vyvolání následující výjimky:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. Přidejte následující kód, který **hlavní** metoda pro vytvoření instance **DeviceClient** a **TelemetryConfig**:

    ```java
    client = new DeviceClient(connString, protocol);

    telemetryConfig = new TelemetryConfig();
    ```

1. Přidejte následující kód, který **hlavní** metoda ke spouštění služeb twin zařízení:

    ```java
    try 
    {
        System.out.println("Connecting to hub");
        client.open();
        client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, telemetryConfig, null);

        telemetryConfig.InitTelemetry();

        System.out.println("Wait for desired telemetry...");
        client.subscribeToDesiredProperties(telemetryConfig.getDesiredProp());
    }
    catch (Exception e) 
    {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
        telemetryConfig.clean();
        client.close();
        System.out.println("Shutting down...");
    }
    ```

1. Přidejte následující kód, který **hlavní** metoda vypnout v simulátoru zařízení, pokud je to nezbytné:

    ```java
    System.out.println("Press enter to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    telemetryConfig.clean();
    client.close();
    ```

1. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

1. Sestavení **simulated-device** back-end aplikace a opravte všechny chyby. Na příkazovém řádku přejděte do složky simulated-device a spusťte následující příkaz:

    `mvn clean package -DskipTests`

   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, některé může mít do fronty je a některé může odmítnout s chybový stav. Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.
   > 
   > 

## <a name="create-the-service-app"></a>Vytvořit aplikaci aplikační služby
V této části vytvoříte konzolovou aplikaci Java, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie. Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.

1. Ve složce dt-get-started vytvořte projekt Maven s názvem **konfigurace sady** pomocí následujícího příkazu na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=set-configuration -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte do složky Konfigurace sady.

1. Pomocí textového editoru, otevřete soubor pom.xml ve složce restartování aktivační události a přidejte následující závislost na **závislosti** uzlu. Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.5.22</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat pro nejnovější verzi **klienta služby iot** pomocí [Maven hledání] [lnk-maven-service-search].

1. Přidejte následující **sestavení** uzlu po **závislosti** uzlu. Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Soubor pom.xml uložte a zavřete.

1. Pomocí textového editoru otevřete set-configuration\src\main\java\com\mycompany\app\App.java zdrojový soubor.

1. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahraďte **{youriothubconnectionstring}** IoT hub připojovacím řetězcem jste si poznamenali v *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";
    ```

1. Dotaz a aktualizovat dvojčata zařízení na simulované zařízení, přidejte následující kód, který **hlavní** metoda:

    ```java
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);

    String sendFrequency= "12h";

    try {
        twinClient.getTwin(device);

        // make sure a differing value exists before calling the updateTwin() method, else a null device exception will be thrown
        // this allows the program to be run multiple times for demonstration purposes
        String desiredProperties = device.desiredPropertiesToString();
        if (desiredProperties.contains("sendFrequency=" + sendFrequency))
        {
            sendFrequency = "8h";
        }
            
        Set<Pair> tags = new HashSet<Pair>();
        tags.add(new Pair("telemetryConfig", "{configId=0001, sendFrequency=" + sendFrequency + "}"));

        twinClient.getTwin(device);
        device.setDesiredProperties(tags);

        System.out.println("Config report for: " + deviceId);   
        System.out.println(device);

        twinClient.updateTwin(device);

        String reportedProperties = device.reportedPropertiesToString();
        Boolean waitFlag = true;

        while (waitFlag) {
            if (!reportedProperties.contains("sendFrequency=" + sendFrequency)) {
                Thread.sleep(10000);
            }
            else 
            {
                waitFlag = false;
            }

            twinClient.getTwin(device);
            reportedProperties = device.reportedPropertiesToString();
        }
            
        SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "properties.reported.telemetryConfig='{configId=0001, sendFrequency=" + sendFrequency + "}'", null);

        Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
        while (twinClient.hasNextDeviceTwin(twinQuery)) {
            DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
            System.out.println(d.getDeviceId() + " found with changed telemetryConfig");
        }

        System.out.println("Config report for: " + deviceId);
        twinClient.getTwin(device);
        System.out.println(device);

        } catch (IotHubException e) {
            System.out.println(e.getMessage());
        } catch (IOException e) {
            System.out.println(e.getMessage());
        } catch (InterruptedException e) {
            System.out.println(e.getMessage());
        }
    ```

1. Uložte a zavřete soubor set-desired-configuration\src\main\java\com\mycompany\app\App.java.

1. Sestavení **konfigurace sady** back-end aplikace a opravte všechny chyby. Na příkazovém řádku přejděte do složky, konfigurace sady a spusťte následující příkaz:

    `mvn clean package -DskipTests`
   
    Tento kód načte dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.
    Poté se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub každých 10 sekund a vytiskne konfigurace požadované a oznámená telemetrie. Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.
   
   > [!IMPORTANT]
   > Tato aplikace se dotazuje služby IoT Hub každých 10 sekund pro ilustraci až po aktualizaci zařízení. Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny. Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].
   > 

   > [!IMPORTANT]
   > Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu. To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah.  Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **DeviceTwin** třídy.
   > 
   > 

## <a name="run-the-apps"></a>Spouštění aplikací

Nyní jste připraveni aplikaci spustit.

1. V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby začal přijímat volání twin zařízení ze služby IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Na příkazovém řádku ve složce Konfigurace sady spusťte následující příkaz pro dotazování a aktualizovat dvojčata zařízení na simulovaného zařízení ze služby IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

1. Simulované zařízení reaguje na volání metody přímé restartování:

    ![Aplikaci simulovaného zařízení Java IoT Hub odpoví na volání twin zařízení][img-deviceconfigured]

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení back-end a napsali aplikace na zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav prostřednictvím hlášení Vlastnosti.

Použijte v následujících zdrojích informací další postup:

* odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- images -->
[img-deviceconfigured]: media/iot-hub-java-java-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-java-device-class]: https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._device_twin._device

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
