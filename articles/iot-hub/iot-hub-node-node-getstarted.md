---
title: "Začínáme se službou Azure IoT Hub (Node) | Dokumentace Microsoftu"
description: "Postup odesílání zpráv typu zařízení-cloud ze zařízení do služby Azure IoT Hub pomocí sad SDK Azure IoT pro Node.js. Vytvoříte aplikaci simulovaného zařízení pro odesílání zpráv, aplikaci služby pro registraci zařízení v registru identit a aplikaci služby pro čtení zpráv typu zařízení-cloud ze služby IoT Hub."
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
ms.date: 12/15/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2e4220bedcb0091342fd9386669d523d4da04d1c
ms.openlocfilehash: 5d005e3259333f79b9b9852e325864745ee54b84


---
# <a name="get-started-with-azure-iot-hub-node"></a>Začínáme se službou Azure IoT Hub (Node)
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace Node.js:

* **CreateDeviceIdentity.js** vytváří identitu zařízení a přiřazený bezpečnostní klíč k připojení simulované aplikace zařízení.
* **ReadDeviceToCloudMessages.js** zobrazuje telemetrické zprávy odesílané simulovanou aplikací zařízení.
* **SimulatedDevice.js** propojuje službu IoT Hub s identitou zařízení vytvořenou dříve a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci Node.js, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části **Registr identit** v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

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
6. Přidejte následující kód k vytvoření definice zařízení v registru identit ve službě IoT Hub. Pokud ID zařízení v registru identit neexistuje, vytvoří tento kód nové zařízení. V opačném případě vrátí klíč stávajícího zařízení:
   
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
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
7. Soubor **CreateDeviceIdentity.js** uložte a zavřete.
8. Aplikaci **createdeviceidentity** spustíte tak, že v příkazovém řádku ve složce createdeviceidentity spustíte následující příkaz:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Poznamenejte si **ID zařízení** a **Klíč zařízení**. Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Způsoby zpracování zpráv typu zařízení-cloud v různých škálách najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.
> 
> 

1. Vytvořte prázdnou složku s názvem **readdevicetocloudmessages**. Ve složce **readdevicetocloudmessages** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
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
5. Přidejte následující deklaraci proměnné a nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro vaši službu IoT Hub:
   
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
7. Přidejte následující kód pro vytvoření **EventHubClient**, otevřete připojení k vašemu IoT Hub a vytvořte příjemce pro každý oddíl. Tato aplikace při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. Tento filtr je užitečný v testovacím prostředí, protože uvidíte pouze aktuální sadu zpráv. V produkčním prostředí byste se měli ujistit, že váš kód zpracovává všechny zprávy. Další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial]:
   
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

1. Vytvořte prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz k instalaci balíčku sady SDK pro zařízení **azure-iot-device** a balíčku **azure-iot-device-amqp**:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru vytvořte soubor **SimulatedDevice.js** ve složce **simulateddevice**.
4. Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**. Hodnotu **{youriothostname}** nahraďte názvem služby IoT Hub, kterou jste vytvořili v oddílu *Vytvoření služby IoT Hub*. Hodnotu **{yourdevicekey}** nahraďte klíčem zařízení, který jste vygenerovali v oddílu *Vytvoření identity zařízení*:
   
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
7. Vytvořte zpětné volání a použijte funkci **setInterval**, která bude každou sekundu do služby IoT Hub odesílat zprávu:
   
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
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.
> 
> 

## <a name="run-the-apps"></a>Spouštění aplikací
Nyní jste připraveni aplikaci spustit.

1. V příkazovém řádku ve složce**readdevicetocloudmessages** spusťte následující příkaz, aby se začala monitorovat služba IoT Hub:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Aplikace služby Node.js IoT Hub pro monitorování zpráv typu zařízení-cloud][7]
2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:
   
    ```
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
* [Začínáme se sadou IoT Gateway SDK][lnk-gateway-SDK]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].

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
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Dec16_HO3-->


