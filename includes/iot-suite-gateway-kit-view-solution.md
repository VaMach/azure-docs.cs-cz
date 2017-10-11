## <a name="view-the-solution-dashboard"></a>Zobrazení řídicího panelu řešení

Přes řídicí panel řešení můžete spravovat nasazené řešení. Můžete například zobrazit telemetrická data, přidat zařízení a volat metody.

1. Až bude zřizování dokončeno a dlaždice předkonfigurovaného řešení bude hlásit **Připraveno**, zvolte **Spustit**. Na nové kartě se otevře portál předkonfigurovaného řešení vzdáleného monitorování.

    ![Spuštění předkonfigurovaného řešení][img-launch-solution]

1. Ve výchozím nastavení se na portálu řešení zobrazuje *řídicí panel*. Přejděte do jiných oblastí portálu řešení pomocí nabídky na levé straně stránky.

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

## <a name="add-a-device"></a>Přidání zařízení

Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů. Přihlašovací údaje zařízení můžete zjistit z řídicího panelu řešení. Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.

Pokud chcete přidat zařízení do řešení vzdáleného monitorování, proveďte na řídicím panelu řešení následující kroky:

1. V levém dolním rohu řídicího panelu klikněte na **Přidat zařízení**.

   ![Přidání zařízení][1]

1. Na panelu **Vlastní zařízení** klikněte na **Přidat nové**.

   ![Přidání vlastního zařízení][2]

1. Vyberte možnost **Definovat vlastní ID zařízení**. Zadejte ID zařízení, jako **device01**, klikněte na tlačítko **Zkontrolujte ID** ověření již nepoužili název ve vašem řešení a potom klikněte na **vytvořit** ke zřízení zařízení.

   ![Přidání ID zařízení][3]

1. Poznamenejte si přihlašovací údaje zařízení (**ID zařízení**, **název hostitele centra IoT**, a **klíč zařízení**). Klientské aplikace na Intel NUC musí tyto hodnoty pro připojení k řešení vzdáleného monitorování. Potom klikněte na **Done** (Hotovo).

    ![Zobrazení přihlašovacích údajů zařízení][4]

1. V seznamu zařízení na řídicím panelu řešení vyberte své zařízení. Pak na panelu **Podrobnosti o zařízení** klikněte na **Povolit zařízení**. Stav vašeho zařízení je teď **Spuštěno**. Řešení vzdáleného monitorování teď může z vašeho zařízení přijímat telemetrii a vyvolávat v něm metody.

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png