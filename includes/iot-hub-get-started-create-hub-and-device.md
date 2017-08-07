## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

1. Na webu [Azure Portal](https://portal.azure.com/) klikněte na **Nový** > **Internet věcí** > **IoT Hub**.

   ![Vytvoření centra IoT na webu Azure Portal](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. V podokně **Centrum IoT** zadejte následující informace o centru IoT:

     **Název:** Zadejte název centra IoT. Pokud je zadaný název platný, zobrazí se zelená značka zaškrtnutí.

     **Úroveň cen a škálování:** Vyberte úroveň **F1 – Free**. Tato možnost je pro tuto ukázku dostačující. Další informace najdete v tématu popisujícím [úrovně cen a škálování](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Skupina prostředků:** Vytvořte skupinu prostředků, která bude hostitelem centra IoT, nebo použijte existující. Další informace najdete v tématu [Použití skupin prostředků ke správě prostředků Azure](../articles/azure-resource-manager/resource-group-portal.md).

     **Umístění:** Vyberte nejbližší umístění k umístění, ve kterém je vytvořeno vaše centrum IoT.

     **Připnout na řídicí panel:** Vyberte tuto možnost pro snadný přístup k centru IoT z řídicího panelu.

   ![Zadání informací pro vytvoření centra IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Klikněte na možnost **Vytvořit**. Vytvoření centra IoT může trvat několik minut. Průběh můžete sledovat v podokně **Oznámení**.

   ![Zobrazení oznámení o průběhu pro centrum IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. Po vytvoření centra IoT na něj klikněte na řídicím panelu. Poznačte si **Název hostitele** a klikněte na **Zásady sdíleného přístupu**.

   ![Získání názvu hostitele pro centrum IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. V podokně **Zásady sdíleného přístupu** klikněte na zásadu **iothubowner** a potom si zkopírujte a poznamenejte **Připojovací řetězec** pro centrum IoT. Další informace najdete v tématu [Řízení přístupu ke službě IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Pro tento kurz nastavení nebudete připojovací řetězec iothubowner potřebovat. Můžete ho však potřebovat pro některé kurzy k různým scénářům IoT po dokončení tohoto nastavení.

   ![Získání připojovacího řetězce centra IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registrace zařízení v centru IoT pro vaše zařízení

1. Na webu [Azure Portal](https://portal.azure.com/) otevřete vaše centrum IoT.

2. Klikněte na **Průzkumník zařízení**.
3. V podokně Průzkumník zařízení klikněte na **Přidat** a přidejte zařízení do centra IoT. Potom udělejte následující:

   **ID zařízení:** Zadejte ID nového zařízení. V ID zařízení se rozlišují malá a velká písmena.

   **Typ ověřování:** Vyberte **Symetrický klíč**.

   **Automaticky generovat klíče:** Zaškrtněte toto políčko.

   **Připojit zařízení k centru IoT:** Klikněte na **Povolit**.

   ![Přidání zařízení v Průzkumníku zařízení centra IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klikněte na **Uložit**.
5. Po vytvoření zařízení ho otevřete v podokně **Průzkumník zařízení**.
6. Poznamenejte si primární klíč připojovacího řetězce.

   ![Získání připojovacího řetězec zařízení](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
