## <a name="create-an-iot-hub"></a>Vytvoření centra IoT
Vytvoření služby IoT Hub pro aplikaci simulovaného zařízení, která se ke službě bude připojovat. Následující kroky vám ukážou, jak tuto úlohu dokončit pomocí portálu Azure.

1. Přihlaste se na web [Azure Portal][lnk-portal].

1. Vyberte **Nový** > **Internet věcí** > **IoT Hub**.
   
    ![Panel portálu Azure][1]

1. V podokně **Centrum IoT** zadejte následující informace o centru IoT:

   * **Název:** Vytvořte název centra IoT. Pokud je zadaný název platný, zobrazí se zelená značka zaškrtnutí.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Úroveň cen a škálování:** V tomto kurzu vyberte úroveň **F1 – Free**. Další informace najdete v tématu popisujícím [úrovně cen a škálování][lnk-pricing].

   * **Skupina prostředků:** Vytvořte skupinu prostředků, která bude hostitelem centra IoT, nebo použijte existující. Další informace najdete v tématu [Použití skupin prostředků ke správě prostředků Azure][lnk-resource-groups].

   * **Umístění:** Vyberte vám nejbližší umístění.

   * **Připnout na řídicí panel:** Zaškrtněte tuto možnost pro snadný přístup k centru IoT z řídicího panelu.

    ![Okno Centrum IoT][2]

1. Klikněte na možnost **Vytvořit**. Vytvoření centra IoT může trvat několik minut. Průběh můžete sledovat v podokně **Oznámení**.

1. Po úspěšném vytvoření centra IoT kliknutím na dlaždici nového centra IoT na webu Azure Portal otevřete okno vlastností nového centra IoT. Teď, když jste vytvořili centrum IoT, vyhledejte důležité informace, které používáte pro připojení zařízení a aplikací do služby IoT hub. Poznačte si **Název hostitele** a klikněte na **Zásady sdíleného přístupu**.
   
    ![Okno nového centra IoT][4]

1. V části **Zásady sdíleného přístupu** klikněte na zásadu **iothubowner** a potom si z okna **iothubowner** zkopírujte a poznamenejte připojovací řetězec služby IoT Hub. Další informace najdete v tématu [Řízení přístupu][lnk-access-control] v Příručce pro vývojáře pro IoT Hub.
   
    ![Zásady sdíleného přístupu][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
