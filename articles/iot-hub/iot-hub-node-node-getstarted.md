<properties
    pageTitle="Začínáme se službou Azure IoT Hub pro Node.js | Microsoft Azure"
    description="Úvodní kurz pro službu Azure IoT Hub pro Node.js. Implementace internetu věcí pomocí služby Azure IoT Hub a softwaru Node.js spolu se sadami SDK služby Microsoft Azure IoT."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/16/2016"
     ms.author="dobett"/>

# Začínáme se službou Azure IoT Hub pro Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace softwaru Node.js:

* **CreateDeviceIdentity.js** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulovaného zařízení.
* **ReadDeviceToCloudMessages.js** zobrazuje telemetrické zprávy odesílané simulovaným zařízením.
* **SimulatedDevice.js** propojuje službu IoT Hub s identitou zařízení vytvořenou dříve a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu AMQPS.

> [AZURE.NOTE] V článku [Sady SDK služby IoT Hub][lnk-hub-sdks] naleznete informace o různých sadách SDK, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení.

Pro absolvování tohoto kurzu potřebujete:

+ Node.js verze 0.12.x nebo novější. <br/> Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace softwaru Node.js pro tento návod v systému Windows nebo Linux.

+ Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial]).

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele IoT Hub a připojovací řetězec, které potřebujete k dokončení zbylé části tohoto kurzu.

## Vytvoření identity zařízení

V této části vytvoříte konzolovou aplikaci softwaru Node.js, která v registru identit ve službě IoT Hub vytvoří novou identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit zařízení. Další informace najdete v části **Registr identit zařízení** tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub.

1. Vytvořte novou prázdnou složku s názvem **createdeviceidentity**. Ve složce **createdeviceidentity** vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```
    npm init
    ```

2. V příkazovém řádku ve složce **createdeviceidentity** spusťte následující příkaz k instalaci balíčku sady SDK pro služby **azure-iothub**:

    ```
    npm install azure-iothub --save
    ```

3. Pomocí textového editoru, vytvořte nový soubor **CreateDeviceIdentity.js** ve složce **createdeviceidentity**.

4. Na začátek souboru **CreateDeviceIdentity.js** přidejte následující příkaz `require`:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Do souboru **CreateDeviceIdentity.js** přidejte následující kód a nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Přidejte následující kód k vytvoření nové definice zařízení v registru identit zařízení ve službě IoT Hub: Pokud ID zařízení v registru neexistuje, vytvoří kód nové zařízení. V opačném případě vrátí klíč stávajícího zařízení:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Soubor **CreateDeviceIdentity.js** uložte a zavřete.

8. Aplikaci **createdeviceidentity** spustíte tak, že na příkazovém řádku ve složce createdeviceidentity zadáte následující příkaz:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Poznamenejte si **ID zařízení** a **klíč zařízení**. Budete je potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [AZURE.NOTE] V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu do služby. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].

## Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, získáte v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body služby IoT Hub kompatibilní se službou Event Hubs.

> [AZURE.NOTE] Kompatibilní koncový bod služby Event Hubs pro čtení zpráv typu zařízení-cloud vždy používá protokol AMQPS.

1. Vytvořte novou prázdnou složku s názvem **readdevicetocloudmessages**. Ve složce **readdevicetocloudmessages** vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```
    npm init
    ```

2. V příkazovém řádku ve složce **readdevicetocloudmessages** spusťte následující příkaz k instalaci balíčku **azure-event-hubs**:

    ```
    npm install azure-event-hubs --save
    ```

3. Pomocí textového editoru, vytvořte nový soubor **ReadDeviceToCloudMessages.js** ve složce **readdevicetocloudmessages**.

4. Na začátek souboru **ReadDeviceToCloudMessages.js** přidejte následující příkazy `require`:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Přidejte následující deklarace proměnné a nahraďte hodnotu zástupného symbolu pomocí připojovacího řetězce pro váš IoT Hub:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Přidejte následující dvě funkce, které vytisknout výstup do konzoly:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Přidejte následující kód pro vytvoření **EventHubClient**, otevřete připojení k vašemu IoT Hub a vytvořte příjemce pro každý oddíl. Tato aplikace při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. To je užitečné v testovacím prostředí, protože uvidíte pouze aktuální sadu zpráv, ale v produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace naleznete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial]:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Soubor **ReadDeviceToCloudMessages.js** uložte a zavřete.

## Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci softwaru Node.js, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```
    npm init
    ```

2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz k instalaci balíčku sady SDK pro zařízení **azure-iot-device** a balíčku **azure-iot-device-amqp**:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Pomocí textového editoru, vytvořte nový soubor **SimulatedDevice.js** ve složce **simulateddevice**.

4. Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Přidejte proměnnou **connectionString** a použijte ji k vytvoření klienta zařízení. Nahraďte **{youriothostname}** názvem IoT Hub, který jste vytvořili v části *Vytvořit IoT Hub* a **{yourdevicekey}** hodnotou klíče zařízení vygenerovanou v části *Vytvořit identitu zařízení*:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Přidejte následující funkci k zobrazení výstupu z aplikace:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Vytvořte zpětné volání a použijte funkci **setInterval**, která bude každou sekundu do služby IoT Hub odesílat novou zprávu.

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Otevřete připojení ke službě IoT Hub a začněte posílat zprávy:

    ```
    client.open(connectCallback);
    ```

9. Soubor **SimulatedDevice.js** uložte a zavřete.

> [AZURE.NOTE] Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku  [Řešení přechodných chyb][lnk-transient-faults] programu MSDN.


## Spuštění aplikací

Nyní můžete spustit aplikace.

1. Na příkazovém řádku ve složce**readdevicetocloudmessages** spusťte následující příkaz, aby se začala monitorovat služba IoT Hub:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![][7]

2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:

    ```
    node SimulatedDevice.js
    ```

    ![][8]

3. Na dlaždici **Využití** v [portálu Azure][lnk-portal] se zobrazuje počet zpráv odeslaných do služby:

    ![][43]

## Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT Hub v portálu a poté jste vytvořili identitu zařízení v registru identit ve službě. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou. 

Pokud chcete pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tyto články:

- [Připojení zařízení][lnk-connect-device]
- [Začínáme se správou zařízení][lnk-device-management]
- [Začínáme se sadou Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit váš internet věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[6]: ./media/iot-hub-node-node-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=sep16_HO1-->


