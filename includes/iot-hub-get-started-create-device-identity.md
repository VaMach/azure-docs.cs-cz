## <a name="create-a-device-identity"></a>Vytvoření identity zařízení

V této části můžete použít nástroj Node.js názvem [iothub-explorer] [ iot-hub-explorer] k vytvoření identity zařízení pro účely tohoto kurzu. V ID zařízení se rozlišují malá a velká písmena.

1. Spusťte následující příkazy v prostředí příkazového řádku:

    `npm install -g iothub-explorer@latest`

1. Potom spusťte následující příkaz k přihlášení do vašeho centra. SUBSTITUTE `{iot hub connection string}` jste dříve zkopírovali připojovacím řetězcem IoT Hub:

    `iothub-explorer login "{iot hub connection string}"`

1. Nakonec vytvořte novou identitu zařízení s názvem `myDeviceId` pomocí příkazu:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Poznamenejte si řetězec připojení zařízení od výsledku. Toto zařízení připojovací řetězec se používá aplikace zařízení pro připojení do služby IoT Hub jako zařízení.

![][img-identity]

Odkazovat na [Začínáme se službou IoT Hub] [ lnk-getstarted] k vytváření identit zařízení prostřednictvím kódu programu.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
