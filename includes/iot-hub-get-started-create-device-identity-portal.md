## <a name="create-a-device-identity"></a>Vytvoření identity zařízení

V této části můžete použít [portál Azure] [ lnk-azure-portal] k vytvoření identity zařízení v registru identit ve službě IoT hub. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části Registr identit v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. **Explorer zařízení** v portálu pomáhá vygenerujte jedinečné ID zařízení a klíč, který zařízení můžete použít k identifikaci, když se připojí ke službě IoT Hub. V ID zařízení se rozlišují malá a velká písmena.

1. Ujistěte se, že jste přihlášeni k [portál Azure][lnk-azure-portal].

1. Na panelu vlevo klikněte na tlačítko **všechny prostředky** a najít prostředek centra IoT.

    ![Přejděte do služby Iot hub][img-find-iothub]

1. Po otevření prostředku centra IoT klikněte na tlačítko **Explorer zařízení** nástroje a potom klikněte na **přidat** v horní části. Zadejte název pro nové zařízení, jako třeba **myDeviceId**a klikněte na tlačítko **Uložit**.

    ![Vytvoření identity zařízení na portálu][img-create-device]

   Tím se vytvoří novou identitu zařízení pro službu IoT hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. V **Explorer zařízení**na seznam zařízení, klikněte na nově vytvořený zařízení a poznamenejte si **připojovací řetězec, primární klíč**. 

    ![Řetězec připojení zařízení][img-connection-string]

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

