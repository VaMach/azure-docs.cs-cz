## <a name="create-an-iot-hub"></a>Vytvoření centra IoT
Vytvoření služby IoT Hub pro aplikaci simulovaného zařízení, která se ke službě bude připojovat. Následující kroky vám ukážou, jak tuto úlohu dokončit pomocí portálu Azure.

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Teď, když jste vytvořili centrum IoT, vyhledejte důležité informace, které používáte pro připojení zařízení a aplikací do služby IoT hub. 

1. Po úspěšném vytvoření centra IoT kliknutím na dlaždici nového centra IoT na webu Azure Portal otevřete okno vlastností nového centra IoT. Poznačte si **Název hostitele** a klikněte na **Zásady sdíleného přístupu**.
   
    ![Okno nového centra IoT][4]
1. V části **Zásady sdíleného přístupu** klikněte na zásadu **iothubowner** a potom si z okna **iothubowner** zkopírujte a poznamenejte připojovací řetězec služby IoT Hub. Další informace najdete v tématu [Řízení přístupu][lnk-access-control] v Příručce pro vývojáře pro IoT Hub.
   
    ![Zásady sdíleného přístupu][5]

<!-- Images. -->
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
