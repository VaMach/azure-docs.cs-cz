---
title: "Používat přímé metody Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. Zařízení Azure IoT SDK pro jazyk Java, kterou použijete k implementaci aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro jazyk Java k implementaci aplikační služby, která volá metodu přímé služby Azure IoT."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 4fb759ecd7767c126bc22165494652039ba1caa4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-java"></a>Používat přímé metody (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:

* **vyvolání metody přímo**, back-end aplikaci Java, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.
* **simulated-device**, aplikaci Java, která simuluje zařízení do služby IoT hub s identitou zařízení můžete vytvořit připojení. Tato aplikace odpoví na přímo volat z back-end.

> [!NOTE]
> Informace o sadách SDK, které můžete použít k vytváření aplikací pro spuštění na zařízení a back end vašeho řešení najdete v tématu [SDK služby Azure IoT][lnk-hub-sdks].

Pro absolvování tohoto kurzu potřebujete:

* Java SE 8. <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace Javy pro tento návod v systému Windows nebo Linux.
* Maven 3.  <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace nástroje [Maven][lnk-maven] pro tento návod v systému Windows nebo Linux.
* [Verze Node.js 0.10.0 nebo novější](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci Java, která reaguje na metodu s názvem řešení zpět end.

1. Vytvořte prázdnou složku s názvem iot-java-direct-method.

1. Ve složce iot-java-direct-method vytvořit projekt Maven s názvem **simulated-device** pomocí následujícího příkazu na příkazovém řádku. Příkaz je jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte do složky simulated-devices.

1. Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti. Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta zařízení iot ve vaší aplikaci:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete vyhledat nejnovější verzi **iot-device-client** pomocí [vyhledávání Maven][lnk-maven-device-search].

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

1. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

1. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` klíčem zařízení hodnotou, kterou jste vygenerovaných *vytvoření identity zařízení* části:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**. 

1. Chcete-li vrátit stavový kód do služby IoT hub, přidejte následující vnořenou třídu k **aplikace** – třída:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Pro zpracování metoda přímé volání z back-end řešení, přidejte následující vnořenou třídu k **aplikace** třídy:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. K vytvoření **DeviceClient** a naslouchat volání přímé metod, přidat **hlavní** metodu **aplikace** – třída:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java

1. Sestavení **simulated-device** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte do složky simulated-device a spusťte následující příkaz:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení

V této části vytvoříte konzolovou aplikaci Java, která volá metodu přímé a potom zobrazí odpověď. Tato Konzolová aplikace připojí do služby IoT Hub k vyvolání metody direct.

1. Ve složce iot-java-direct-method vytvořit projekt Maven s názvem **vyvolání metody přímo** pomocí následujícího příkazu na příkazovém řádku. Příkaz je jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte do složky invoke-direct-method.

1. Pomocí textového editoru, otevřete soubor pom.xml ve složce metodu invoke přímo a přidejte následující závislost na **závislosti** uzlu. Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].

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

1. Pomocí textového editoru otevřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java.

1. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. K vyvolání metody na simulované zařízení, přidejte následující kód, který **hlavní** metoda:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Uložte a zavřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java

1. Sestavení **vyvolání metody přímo** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte do složky, metodu invoke přímo a spusťte následující příkaz:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Spouštění aplikací

Nyní jste připraveni ke spuštění aplikace konzoly.

1. V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby začal přijímat volání metody ze služby IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikaci simulovaného zařízení Java IoT Hub naslouchat volání přímá metoda][8]

1. Na příkazovém řádku ve složce metodu invoke přímo spusťte následující příkaz pro volání metody na simulovaného zařízení ze služby IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub volat přímá metoda][7]

1. Simulované zařízení reaguje na volání přímé metody:

    ![Aplikaci simulovaného zařízení Java IoT Hub odpoví na volání přímá metoda][9]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení. Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení.

Chcete-li prozkoumat dalších scénářů platformy IoT, přečtěte si téma [plánování úloh na několika zařízeních][lnk-devguide-jobs].

Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
