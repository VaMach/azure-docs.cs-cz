## <a name="create-an-iot-hub"></a>Vytvoření centra IoT
Vytvoření služby IoT Hub pro aplikaci simulovaného zařízení, která se ke službě bude připojovat. Následující kroky vám ukážou, jak tuto úlohu dokončit pomocí portálu Azure.

1. Přihlaste se na web [Azure Portal][lnk-portal].
1. Na panelu vlevo klikněte na **Nový** > **Internet věcí** > **IoT Hub**.
   
    ![Panel portálu Azure][1]
1. V okně **Centrum IoT** zvolte konfiguraci pro centrum IoT.
   
    ![Okno centra IoT][2]
   
   1. Do pole **Název** zadejte název centra IoT. Pokud je **název** platný a dostupný, zobrazí se v poli **Název** zelená značka zaškrtnutí.
   1. Vyberte [cenovou a škálovací úroveň][lnk-pricing]. Tento kurz nevyžaduje konkrétní úroveň. V kurzu použijte bezplatnou úroveň F1.
   1. V části **Skupina prostředků** vytvořte skupinu prostředků nebo vyberte existující skupinu. Další informace najdete v článku [Použití skupin prostředků ke správě prostředků Azure][lnk-resource-groups].
   1. V nabídce **Umístění** vyberte umístění pro hostování vašeho centra IoT. V tomto kurzu zvolte nejbližší umístění.
1. Po výběru možností konfigurace centra IoT klikněte na **Vytvořit**.  Vytvoření centra IoT může službě Azure zabrat několik minut. Pokud chcete zkontrolovat stav, můžete průběh sledovat na úvodním panelu nebo na panelu oznámení.
   
    ![Stav nového centra IoT][3]
1. Po úspěšném vytvoření centra IoT kliknutím na dlaždici nového centra IoT na webu Azure Portal otevřete okno nového centra IoT. Poznačte si **Název hostitele** a klikněte na **Zásady sdíleného přístupu**.
   
    ![Okno nového centra IoT][4]
1. V okně **Zásady sdíleného přístupu** klikněte na zásady **iothubowner** a potom si z okna **iothubowner** zkopírujte a poznamenejte připojovací řetězec pro IoT Hub. Další informace najdete v tématu [Řízení přístupu][lnk-access-control] v Příručce pro vývojáře pro IoT Hub.
   
    ![Okno zásad sdíleného přístupu][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
