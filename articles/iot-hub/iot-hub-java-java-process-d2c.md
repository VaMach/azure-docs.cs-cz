---
title: "Zpracování zpráv typu zařízení cloud Azure IoT Hub (Java) | Microsoft Docs"
description: "Postupy zpracování zpráv typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body k odeslání zprávy do dalších služeb back-end."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 0fb3e9012ae88112515ebb552e49fa463a087f54
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a>Zpracování zpráv typu zařízení cloud IoT Hub (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečené obousměrnou komunikaci mezi miliony zařízení a řešením back-end. Ostatní kurzy ([Začínáme se službou IoT Hub] a [odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d]) ukazují, jak využívat základní funkce zařízení cloud a z cloudu do zařízení zasílání zpráv služby IoT Hub.

V tomto kurzu vychází uvedeném v kódu [Začínáme se službou IoT Hub] kurzu a ukazuje, jak pomocí směrování zpráv ke zpracování zpráv typu zařízení cloud škálovatelné způsobem. Tento kurz ukazuje, jak ke zpracování zpráv, které vyžadují okamžitou akci z back-end řešení. Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM. Naopak datového bodu zprávy jednoduše informačního kanálu do modul analytics. Například teplotní telemetrie ze zařízení, které je nutné uložit pro pozdější analýzu je zpráva datového bodu.

Na konci tohoto kurzu můžete spustit tři aplikace konzoly v jazyce Java:

* **simulated-device**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub] kurzu posílání zpráv typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund. Tato aplikace používá protokol AMQP ke komunikaci se službou IoT Hub.
* **Read-d2c-messages** zobrazuje telemetrické zprávy odesílané aplikace zařízení.
* **čtení kritické fronty** zrušte fronty kritické zprávy z fronty Service Bus připojit ke službě IoT hub.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript. Pokyny, jak nahradit zařízení v tomto kurzu fyzické zařízení a jak pro připojení zařízení do služby IoT Hub, najdete v článku [Azure střediska pro vývojáře IoT].

Pro absolvování tohoto kurzu potřebujete:

* Úplnou verzi práci [Začínáme se službou IoT Hub] kurzu.
* Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Odesílat interaktivní zprávy z aplikace na zařízení
V této části upravíte zařízení aplikaci, kterou jste vytvořili v [Začínáme se službou IoT Hub] kurzu příležitostně odesílat zprávy, které vyžadují okamžitou zpracování.

1. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java. Tento soubor obsahuje kód pro **simulated-device** aplikace, které jste vytvořili v [Začínáme se službou IoT Hub] kurzu.

2. Nahraďte **MessageSender** třídy následujícím kódem:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
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
   
    Tato metoda náhodně přidá vlastnost `"level": "critical"` na zprávy odeslané zařízením, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end aplikace. Aplikace předá tyto informace ve vlastnostech zpráv místo v textu zprávy, že IoT Hub může směrovat zprávy Cíl správné zprávy.
   
   > [!NOTE]
   > Vlastnosti zprávy pro směrování zpráv můžete použít pro různé scénáře, včetně studený cesty při zpracování, kromě zde ukazuje příklad aktivní trase.

2. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

    > [!NOTE]
    > Z důvodu zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN [přechodných chyb].

3. Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a>Přidat do fronty pro IoT hub a směrování zpráv do ní

V této části Vytvoření fronty Service Bus, připojte ho do služby IoT hub a konfigurace služby IoT hub odesílat zprávy do fronty na základě přítomnosti vlastnosti na zprávu. Další informace o tom, jak zpracování zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][lnk-sb-queues-java].

1. Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][lnk-sb-queues-java]. Poznamenejte si název oboru názvů a fronty.

2. Na portálu Azure otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.

    ![Koncové body v centru IoT][30]

3. V **koncové body** okně klikněte na tlačítko **přidat** v horní části přidat fronty do služby IoT hub. Název koncového bodu **CriticalQueue** a použijte rozevírací seznamy a vyberte **fronty Service Bus**, oboru názvů Service Bus, ve které je umístěn fronty a názvu fronty. Až budete hotovi, klikněte na tlačítko **Uložit** dole.

    ![Přidání koncového bodu][31]

4. Klikněte na tlačítko **trasy** ve službě IoT Hub. Klikněte na tlačítko **přidat** v horní části okna Vytvořit pravidlo směrování pro směrování zpráv do fronty jste právě přidali. Vyberte **DeviceTelemetry** jako zdroj dat. Zadejte `level="critical"` jako podmínka a vyberte fronty, který jste právě přidali jako vlastní koncový bod jako koncový bod směrování pravidlo. Až budete hotovi, klikněte na tlačítko **Uložit** dole.

    ![Přidání trasu][32]

    Ujistěte se, že záložní cesta je nastavena na **ON**. Toto nastavení je výchozí konfigurace služby IoT hub.

    ![Záložní trasy][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Volitelné) Čtení z fronty koncového bodu

Volitelně můžete čtení zpráv z fronty koncového bodu podle pokynů v [začít pracovat s fronty][lnk-sb-queues-java]. Název aplikace **čtení kritické fronty**.

## <a name="run-the-applications"></a>Spuštění aplikací

Nyní jste připraveni ke spuštění tři aplikací.

1. Ke spuštění **read-d2c-messages** aplikace, v příkazovém řádku nebo prostředí přejděte do složky read-d2c a spusťte následující příkaz:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Spustit read-d2c-messages][readd2c]

2. Ke spuštění **čtení kritické fronty** aplikace, v příkazovém řádku nebo prostředí přejděte do složky pro čtení kritické fronty a spusťte následující příkaz:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit pro čtení kritické zprávy][readqueue]

3. Ke spuštění **simulated-device** aplikaci, v příkazovém řádku nebo prostředí přejděte do složky simulated-device a spusťte následující příkaz:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit simulated-device][simulateddevice]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak spolehlivě odesláním zprávy typu zařízení cloud pomocí funkce směrování zpráv služby IoT Hub.

[Odesílání zpráv typu cloud zařízení s centrem IoT] [ lnk-c2d] ukazuje, jak k odesílání zpráv do vašeho zařízení z back end vašeho řešení.

Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite][lnk-suite].

Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].

Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Začínáme se službou IoT Hub]: iot-hub-java-java-getstarted.md
[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot
[přechodných chyb]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Přechodná chyba zpracování]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
