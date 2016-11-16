---
title: "Začínáme se službou Azure IoT Hub pro Node.js | Dokumentace Microsoftu"
description: "Úvodní kurz pro službu Azure IoT Hub pro Node.js. Implementace internetu věcí pomocí služby Azure IoT Hub a softwaru Node.js spolu se sadami SDK služby Microsoft Azure IoT."
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
ms.date: 09/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7ddd9c1ed88e30eaaa200dc6b83d34746da14aa8


---
# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Začínáme se službou Azure IoT Hub pro Node.js
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace Node.js:

* **CreateDeviceIdentity.js** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulovaného zařízení.
* **ReadDeviceToCloudMessages.js** zobrazuje telemetrické zprávy odesílané simulovaným zařízením.
* **SimulatedDevice.js** propojuje službu IoT Hub s identitou zařízení vytvořenou dříve a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu AMQP.

> [!NOTE]
> V článku [Sady SDK služby IoT Hub][lnk-hub-sdks] naleznete informace o různých sadách SDK, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení.
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud nemáte účet, můžete si během několika minut vytvořit [bezplatný][zkušební účet Ink].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele IoT Hub a připojovací řetězec, které potřebujete k dokončení zbylé části tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci Node.js, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit zařízení. Další informace najdete v části **Registr identit zařízení** tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub.

1. Vytvořte novou prázdnou složku s názvem **createdeviceidentity**. Ve složce **createdeviceidentity** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. V příkazovém řádku ve složce **createdeviceidentity** spusťte následující příkaz k instalaci balíčku sady SDK pro služby **azure-iothub**:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru vytvořte ve složce **createdeviceidentity** soubor **CreateDeviceIdentity.js**.
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
6. Přidejte následující kód k vytvoření definice zařízení v registru identit zařízení ve službě IoT Hub. Pokud ID zařízení v registru neexistuje, vytvoří tento kód nové zařízení. V opačném případě vrátí klíč stávajícího zařízení:
   
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
9. Poznamenejte si **ID zařízení** a **klíč zařízení**. Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu do služby. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

## <a name="receive-devicetocloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, získáte v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.
> 
> 

1. Vytvořte novou prázdnou složku s názvem **readdevicetocloudmessages**. Ve složce **readdevicetocloudmessages** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. V příkazovém řádku ve složce **readdevicetocloudmessages** spusťte následující příkaz k instalaci balíčku **azure-event-hubs**:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Pomocí textového editoru vytvořte ve složce **readdevicetocloudmessages** soubor **ReadDeviceToCloudMessages.js**.
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
7. Přidejte následující kód pro vytvoření **EventHubClient**, otevřete připojení k vašemu IoT Hub a vytvořte příjemce pro každý oddíl. Tato aplikace při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. Tento filtr je užitečný v testovacím prostředí, protože uvidíte pouze aktuální sadu zpráv. V produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial]:
   
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

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci Node.js, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
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
5. Přidejte proměnnou **connectionString** a použijte ji k vytvoření klienta zařízení. Hodnotu **{youriothostname}** nahraďte názvem služby IoT Hub, kterou jste vytvořili v oddílu *Vytvoření služby IoT Hub*. Hodnotu **{yourdevicekey}** nahraďte klíčem zařízení, který jste vygenerovali v oddílu *Vytvoření identity zařízení*:
   
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

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku  [Řešení přechodných chyb][lnk-transient-faults] programu MSDN.
> 
> 

## <a name="run-the-applications"></a>Spuštění aplikací
Nyní můžete spustit aplikace.

1. Na příkazovém řádku ve složce**readdevicetocloudmessages** spusťte následující příkaz, aby se začala monitorovat služba IoT Hub:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Klientská aplikace služby Node.js IoT Hub pro monitorování zpráv typu zařízení-cloud][7]
2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Klientská aplikace zařízení Node.js IoT Hub pro odesílání zpráv typu zařízení-cloud][8]
3. Na dlaždici **Využití** v [portálu Azure][lnk-portal] se zobrazuje počet zpráv odeslaných do služby:
   
    ![Dlaždice Použití webu Azure Portal se zobrazením počtu zpráv odeslaných do služby IoT Hub][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě. Pomocí identity zařízení jste aplikaci simulovaného zařízení povolili odesílání zpráv typu zařízení-cloud do služby. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se sadou IoT Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit váš internet věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[zkušební účet Ink]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Nov16_HO2-->


