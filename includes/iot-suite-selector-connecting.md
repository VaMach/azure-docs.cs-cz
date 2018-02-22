> [!div class="op_single_selector"]
> * [C ve Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C v Linuxu](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (obecné)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js v Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C v Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

V tomto kurzu budete implementovat **chladič** zařízení, která odesílá následující telemetrii na vzdálené monitorování [předkonfigurované řešení](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Teplota
* přetížení
* Vlhkost

Pro jednoduchost, vygeneruje kód ukázkové hodnoty telemetrických dat pro **chladič**. Ukázku můžete rozšířit připojením skutečné snímače do svého zařízení a odesláním skutečné telemetrie.

Ukázka zařízení také:

* Odešle metadata do řešení popisují možnosti.
* Reaguje na akcích aktivovaných v **zařízení** stránky v řešení.
* Odesílání reaguje na změny konfigurace z **zařízení** stránky v řešení.

K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Než začnete

Než napíšete kód pro vaše zařízení, nasaďte předkonfigurované řešení vzdáleného monitorování a přidejte nový fyzického zařízení k řešení.

### <a name="deploy-your-remote-monitoring-preconfigured-solution"></a>Nasadit předkonfigurované řešení vzdáleného monitorování

**Chladič** zařízení v tomto kurzu vytvoříte odesílá data do instance [vzdálené monitorování](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) předkonfigurované řešení. Pokud jste ještě nezřídili předkonfigurovaného řešení vzdáleného monitorování v účtu Azure, najdete v části [nasadit předkonfigurované řešení vzdáleného monitorování](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Při procesu nasazení pro dokončení řešení vzdáleného monitorování, klikněte na tlačítko **spusťte** otevřete řídicí panel řešení v prohlížeči.

![Řídicí panel řešení](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Přidání zařízení do řešení vzdáleného monitorování

> [!NOTE]
> Pokud jste již přidali zařízení ve vašem řešení, můžete tento krok přeskočit. Dalším krokem však vyžaduje připojovací řetězec zařízení. Můžete načíst připojovací řetězec zařízení z [portál Azure](https://portal.azure.com) nebo pomocí [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) nástroj příkazového řádku.

Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů. Máte možnost uložit zařízení připojovací řetězec, který obsahuje tyto přihlašovací údaje, když přidáte zařízení řešení. Připojovací řetězec zařízení můžete zahrnout do klientské aplikace později v tomto kurzu.

Chcete-li přidat zařízení do řešení vzdáleného monitorování, proveďte následující kroky na **zařízení** stránky v řešení:

1. Zvolte **+ nové zařízení**a potom zvolte **fyzické** jako **typ zařízení**:

    ![Přidat fyzického zařízení](media/iot-suite-selector-connecting/devicesprovision.png)

1. Zadejte **fyzického chladič** jako ID zařízení. Vyberte **symetrický klíč** a **automaticky generovat klíče** možnosti:

    ![Vyberte možnosti zařízení](media/iot-suite-selector-connecting/devicesoptions.png)

1. Zvolte **použít**. Potom si poznamenejte **ID zařízení**, **primární klíč**, a **primární klíč pro řetězec připojení** hodnoty:

    ![Načíst přihlašovací údaje](media/iot-suite-selector-connecting/credentials.png)

Nyní jste přidali fyzického zařízení k předkonfigurovaného řešení vzdáleného monitorování a jsou uvedené jeho zařízení připojovací řetězec. V následujících částech můžete implementovat aplikace klienta, která používá zařízení připojovací řetězec pro připojení k řešení.

Klientská aplikace implementuje integrované **chladič** model zařízení. Model zařízení předkonfigurované řešení určuje toto zařízení:

* Vlastnosti zařízení sestav řešení. Například **chladič** zařízení hlásí informace o jeho firmware a umístění.
* Typy telemetrických dat, které zařízení odesílá do řešení. Například **chladič** zařízení odesílá přetížení hodnoty, vlhkosti a teploty.
* Metody můžete naplánovat z řešení na zařízení spouštět. Například **chladič** zařízení musí implementovat **restartovat**, **FirmwareUpdate**, **EmergencyValveRelease**, a  **IncreasePressure** metody.