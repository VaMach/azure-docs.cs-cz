---
title: "Zřídit Malinová platformy pro vzdálené monitorování v Node.js – Azure | Microsoft Docs"
description: "Popisuje, jak se připojit k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v Node.js malin platformy zařízení."
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: d2cc72f26568a362049f24323ffa598b6012d77f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Připojte zařízení malin pí k předkonfigurovaného řešení vzdáleného monitorování (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

V tomto kurzu se dozvíte, jak se připojit fyzického zařízení k předkonfigurovaného řešení vzdáleného monitorování. V tomto kurzu použijete Node.js, který je vhodný pro prostředí s minimálními prostředků omezení.

### <a name="required-hardware"></a>Požadovaný hardware

Stolní počítač, abyste mohli vzdáleně připojit k na příkazovém řádku malin pí.

[Microsoft IoT Starter Kit malin pí 3](https://azure.microsoft.com/develop/iot/starter-kits/) nebo ekvivalentní součásti. V tomto kurzu používá následující položky ze sady kit:

- Malinová pí 3
- Karta MicroSD (s NOOBS)
- Kabelu USB malé
- Kabel Ethernet

### <a name="required-desktop-software"></a>Požadovaný software plochy

Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.

- Windows nezahrnuje klientem SSH. Doporučujeme používat [PuTTY](http://www.putty.org/).
- Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH. Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Požadovaný software malin platformy

Pokud jste tak ještě neučinili, nainstalujte Node.js verze 4.0.0 nebo později na vaše malin pí. Následující kroky vám ukážou, jak nainstalovat Node.js v6.11.4 na vaše malin platformy:

1. Připojte se k malin pí pomocí `ssh`. Další informace najdete v tématu [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) na [web Pi malin](https://www.raspberrypi.org/).

1. Použijte následující příkaz k aktualizaci vašeho malin platformy:

    ```sh
    sudo apt-get update
    ```

1. Stažení Node.js binární soubory pro vaše platformy malin použijte následující příkaz:

    ```sh
    wget https://nodejs.org/dist/v6.11.4/node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Použijte následující příkaz pro instalaci binárních souborů:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Pomocí následujícího příkazu ověřte, že jste úspěšně nainstalovali Node.js v6.11.4:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Vytvoření řešení Node.js

Proveďte následující kroky pomocí `ssh` připojení k vaší malin platformy:

1. Vytvořte složku s názvem `RemoteMonitoring` do domovské složky na malin pí. Přejděte do této složky na příkazovém řádku:

    ```sh
    cd ~
    mkdir RemoteMonitoring
    cd RemoteMonitoring
    ```

1. Chcete-li stáhnout a nainstalovat balíčky, které potřebujete k dokončení ukázkové aplikace, spusťte následující příkazy:

    ```sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. V `RemoteMonitoring` složky, vytvořte soubor s názvem **remote_monitoring.js**. Otevřete tento soubor v textovém editoru. Na malin platformy, můžete použít `nano` nebo `vi` textové editory.

1. V **remote_monitoring.js** soubor, přidejte následující `require` příkazy:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Přidejte následující deklarace proměnných za příkazy `require`. Nahraďte zástupný symbol hodnoty `{Device Id}` a `{Device Key}` s hodnotami, které jste si poznamenali zařízení zřízené v řešení vzdáleného monitorování. Název hostitele centra IoT z řešení použít k nahrazení `{IoTHub Name}`. Například, pokud je váš název hostitele centra IoT `contoso.azure-devices.net`, nahraďte `{IoTHub Name}` s `contoso`:

    ```nodejs
    var connectionString = 'HostName={IoTHub Name}.azure-devices.net;DeviceId={Device Id};SharedAccessKey={Device Key}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Chcete-li definovat některé základní telemetrická data, přidejte následující proměnné:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Chcete-li definovat některé hodnoty vlastnosti, přidejte následující proměnné:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    ```

1. Přidejte následující proměnnou k určení hlášené vlastností k odeslání do řešení. Tyto vlastnosti obsahovat metadata k popisu metody a používá telemetrická data zařízení:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude
    }
    ```

1. Pokud chcete vytisknout výsledky operace, přidejte následující funkce, pomocné rutiny:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Přidejte následující pomocné funkce sloužící k náhodné telemetrie hodnoty:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Přidejte následující funkci pro zpracování volání metody přímé z řešení. Toto řešení využívá přímé metody tak, aby fungoval na zařízení:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (!!err) {
          console.error('An error ocurred when sending a method response:\n' +
            err.toString());
        } else {
          console.log('Response to method \'' + request.methodName +
            '\' sent successfully.');
        }
      });
    }
    ```

1. Přidejte následující kód do Neodesílat telemetrická data do řešení. Klientská aplikace přidá vlastnosti zprávy k identifikaci schéma zpráva:

    ```node.js
    function sendTelemetry(data, schema) {
      var d = new Date();
      var payload = JSON.stringify(data);
      var message = new Message(payload);
      message.properties.add('$$CreationTimeUtc', d.toISOString());
      message.properties.add('$$MessageSchema', schema);
      message.properties.add('$$ContentType', 'JSON');

      console.log('Sending device message data:\n' + payload);
      client.sendEvent(message, printErrorFor('send event'));
    }
    ```

1. Přidejte následující kód k vytvoření instance klienta:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Přidejte následující kód do:

    - Otevření připojení.
    - Nastavte obslužnou rutinu pro požadované vlastnosti.
    - Odešlete oznámenou vlastnosti.
    - Zaregistrujte obslužné rutiny pro přímé metody.
    - Zahájit odesílání telemetrie.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onDirectMethod);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Uložit změny **remote_monitoring.js** souboru.

1. Spuštění ukázkové aplikace, spusťte následující příkaz na příkazovém řádku na malin platformy:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
