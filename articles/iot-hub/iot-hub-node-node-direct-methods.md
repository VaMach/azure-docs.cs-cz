---
title: "Azure IoT Hub přímé metody (uzel) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. K implementaci aplikace simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé použijete SDK služby Azure IoT pro Node.js."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a73c25724a239e56c3ea62a8452bb7c3a2b51be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>Použití přímé metody na zařízení IoT s Node.js
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:

* **CallMethodOnDevice.js**, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.
* **SimulatedDevice.js**, který se připojuje ke službě IoT hub s dříve vytvořenou identitou zařízení a reaguje na metodu volá cloudu.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].
> 
> 

Pro absolvování tohoto kurzu potřebujete:

* Node.js verze 4.0.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje na metodu s názvem cloudem.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. Ve složce **simulateddevice** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz k instalaci balíčku sady SDK pro zařízení **azure-iot-device** a balíčku **azure-iot-device-amqp**:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte nový soubor **SimulatedDevice.js** ve složce **simulateddevice**.
4. Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Přidat **connectionString** proměnné a použít ho k vytvoření **DeviceClient** instance. Nahraďte **{zařízení připojovací řetězec}** s připojením zařízení vygenerovanými v řetězci *vytvoření identity zařízení* části:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Přidejte následující funkci implementovat metodu na zařízení:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Otevřete připojení do služby IoT hub a spusťte inicializovat naslouchací proces metoda:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Soubor **SimulatedDevice.js** uložte a zavřete.

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-method-on-a-device"></a>Volání metody na zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která volá metodu v aplikaci simulovaného zařízení a potom zobrazí odpověď.

1. Vytvořit novou prázdnou složku s názvem **callmethodondevice**. V **callmethodondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v **callmethodondevice** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **CallMethodOnDevice.js** v soubor **callmethodondevice** složky.
4. Přidejte následující `require` příkazy na začátku **CallMethodOnDevice.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Přidejte následující deklaraci proměnné a nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro vaši službu IoT Hub:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. Vytvoření klienta k otevření připojení do služby IoT hub.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Přidejte následující funkci k vyvolání metody zařízení a vytisknout odpověď zařízení do konzoly:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Uložte a zavřete **CallMethodOnDevice.js** souboru.

## <a name="run-the-apps"></a>Spouštění aplikací
Nyní jste připraveni aplikaci spustit.

1. Na příkazovém řádku v **simulateddevice** složky, spusťte následující příkaz, který zahájit naslouchání pro volání metod ze služby IoT Hub:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Na příkazovém řádku v **callmethodondevice** složky, spusťte následující příkaz ke spuštění monitorování služby IoT hub:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Zobrazí se zařízení reagovat na metodu tiskem, zprávu a aplikace, který označuje zobrazení metoda odpověď ze zařízení:
   
    ![][9]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení. Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení. 

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
