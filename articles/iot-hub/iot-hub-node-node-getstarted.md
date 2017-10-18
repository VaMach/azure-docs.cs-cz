---
title: "Začínáme se službou Azure IoT Hub (Node) | Dokumentace Microsoftu"
description: "Zjistěte, jak odesílat zprávy typu zařízení-cloud do služby Azure IoT Hub pomocí sad IoT SDK pro Node.js. Vytvořte simulované zařízení a aplikace služeb pro registraci vašeho zařízení, odesílání zpráv a čtení zpráv ze služby IoT Hub."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3398e38cf7d3d28d9ca4edef5a9bca96aeaf2ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-node"></a>Připojení simulovaného zařízení k IoT Hubu pomocí Node

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace Node.js:

* **CreateDeviceIdentity.js** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulované aplikace zařízení.
* **ReadDeviceToCloudMessages.js** zobrazuje telemetrické zprávy odesílané simulovanou aplikací zařízení.
* **SimulatedDevice.js** propojuje službu IoT Hub s identitou zařízení vytvořenou dříve a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].

Pro absolvování tohoto kurzu potřebujete:

* Node.js verze 4.0.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení

V této části vytvoříte konzolovou aplikaci Node.js, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru zařízení. Další informace najdete v části **Registr identit** v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Spusťte tuto aplikaci, která vygeneruje jedinečné ID a klíč zařízení, pomocí kterých se zařízení identifikuje při odesílání zpráv typu zařízení-cloud.

1. Vytvořte novou prázdnou složku s názvem `createdeviceidentity`. Ve složce `createdeviceidentity` vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```cmd/sh
    npm init
    ```

2. Na příkazovém řádku ve složce `createdeviceidentity` spusťte následující příkaz k instalaci balíčku sady SDK služby `azure-iothub`:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Pomocí textového editoru vytvořte ve složce `createdeviceidentity` soubor **CreateDeviceIdentity.js**.

4. Na začátek souboru **CreateDeviceIdentity.js** přidejte následující příkaz `require`:

    ```nodejs
    'use strict';

    var iothub = require('azure-iothub');
    ```

5. Do souboru **CreateDeviceIdentity.js** přidejte následující kód. Nahraďte zástupnou hodnotu připojovacím řetězcem služby IoT Hub, kterou jste vytvořili v předchozí části:

    ```nodejs
    var connectionString = '{iothub connection string}';

    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Přidejte následující kód k vytvoření definice zařízení v registru identit ve službě IoT Hub. Pokud ID zařízení v registru identit neexistuje, vytvoří tento kód nové zařízení. V opačném případě vrátí klíč stávajícího zařízení:

    ```nodejs
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
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
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. Soubor **CreateDeviceIdentity.js** uložte a zavřete.

8. Aplikaci `createdeviceidentity` spustíte tak, že na příkazovém řádku ve složce `createdeviceidentity` spustíte následující příkaz:

    ```cmd/sh
    node CreateDeviceIdentity.js 
    ```

9. Poznamenejte si **ID zařízení** a **Klíč zařízení**. Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Způsoby zpracování zpráv typu zařízení-cloud v různých škálách najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace použitelné pro koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.

1. Vytvořte prázdnou složku s názvem `readdevicetocloudmessages`. Ve složce `readdevicetocloudmessages` vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```cmd/sh
    npm init
    ```

2. Na příkazovém řádku ve složce `readdevicetocloudmessages` spusťte následující příkaz k instalaci balíčku **azure-event-hubs**:

    ```cmd/sh
    npm install azure-event-hubs --save
    ```

3. Pomocí textového editoru vytvořte ve složce `readdevicetocloudmessages` soubor **ReadDeviceToCloudMessages.js**.

4. Na začátek souboru **ReadDeviceToCloudMessages.js** přidejte následující příkazy `require`:

    ```nodejs
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Přidejte následující deklaraci proměnné a nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro vaši službu IoT Hub:

    ```nodejs
    var connectionString = '{iothub connection string}';
    ```

6. Přidejte následující dvě funkce, které vytisknout výstup do konzoly:

    ```nodejs
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Přidejte následující kód pro vytvoření **EventHubClient**, otevřete připojení k vašemu IoT Hub a vytvořte příjemce pro každý oddíl. Tato aplikace při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. Tento filtr je užitečný v testovacím prostředí, protože uvidíte pouze aktuální sadu zpráv. V produkčním prostředí byste se měli ujistit, že váš kód zpracovává všechny zprávy. Další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial]:

    ```nodejs
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

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci Node.js, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Vytvořte prázdnou složku s názvem `simulateddevice`. Ve složce `simulateddevice` vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:

    ```cmd/sh
    npm init
    ```

2. Na příkazovém řádku ve složce `simulateddevice` spusťte následující příkaz k instalaci balíčku sady SDK pro zařízení **azure-iot-device** a balíčku **azure-iot-device-mqtt**:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Pomocí textového editoru vytvořte ve složce `simulateddevice` soubor **SimulatedDevice.js**.

4. Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:

    ```nodejs
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Přidejte proměnnou `connectionString` a použijte ji k vytvoření instance **klienta**. Nahraďte `{youriothostname}` názvem služby IoT Hub, kterou jste vytvořili v části *Vytvoření služby IoT Hub*. Nahraďte `{yourdevicekey}` hodnotou klíče zařízení, který jste vygenerovali v části *Vytvoření identity zařízení*:

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';

    var client = clientFromConnectionString(connectionString);
    ```

6. Přidejte následující funkci k zobrazení výstupu z aplikace:

    ```nodejs
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Vytvořte zpětné volání a použijte funkci **setInterval**, která bude každou sekundu do služby IoT Hub odesílat zprávu:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Otevřete připojení ke službě IoT Hub a začněte posílat zprávy:

    ```nodejs
    client.open(connectCallback);
    ```

9. Soubor **SimulatedDevice.js** uložte a zavřete.

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.

## <a name="run-the-apps"></a>Spouštění aplikací

Nyní jste připraveni aplikaci spustit.

1. Na příkazovém řádku ve složce `readdevicetocloudmessages` spusťte následující příkaz, aby se začala monitorovat služba IoT Hub:

    ```cmd/sh
    node ReadDeviceToCloudMessages.js 
    ```

    ![Aplikace služby Node.js IoT Hub pro monitorování zpráv typu zařízení-cloud][7]

2. Na příkazovém řádku ve složce `simulateddevice` spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:

    ```cmd/sh
    node SimulatedDevice.js
    ```

    ![Aplikace zařízení služby Node.js IoT Hub pro odesílání zpráv typu zařízení-cloud][8]

3. Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:

    ![Dlaždice Použití webu Azure Portal se zobrazením počtu zpráv odeslaných do služby IoT Hub][43]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub.

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou Azure IoT Edge][lnk-iot-edge]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
