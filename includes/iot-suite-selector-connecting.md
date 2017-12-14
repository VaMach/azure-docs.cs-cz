> [!div class="op_single_selector"]
> * [C ve Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C v Linuxu](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (obecné)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js v Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C v Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

V tomto kurzu budete implementovat **chladič** zařízení, která odesílá následující telemetrii na vzdálené monitorování [předkonfigurované řešení](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Teplotní
* přetížení
* Vlhkost

Pro jednoduchost, vygeneruje kód ukázkové hodnoty telemetrických dat pro **chladič**. Ukázku můžete rozšířit připojením skutečné snímače do svého zařízení a odesláním skutečné telemetrie.

Ukázka zařízení také:

* Odešle metadata do řešení popisují možnosti.
* Reaguje na akcích aktivovaných v **zařízení** stránky v řešení.
* Odesílání reaguje na změny konfigurace z **zařízení** stránky v řešení.

K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Než začnete

Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Zřízení předkonfigurovaného řešení vzdáleného monitorování

**Chladič** zařízení v tomto kurzu vytvoříte odesílá data do instance [vzdálené monitorování](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) předkonfigurované řešení. Pokud jste ještě nezřídili předkonfigurovaného řešení vzdáleného monitorování v účtu Azure, najdete v části [nasadit předkonfigurované řešení vzdáleného monitorování](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Po skončení procesu zřizování řešení vzdáleného monitorování klikněte na **Spustit**. Ve vašem prohlížeči se otevře řídicí panel řešení.

![Řídicí panel řešení](media/iot-suite-selector-connecting/dashboard.png)

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Zřízení zařízení v řešení vzdáleného monitorování

> [!NOTE]
> Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit. Při vytváření aplikace klienta potřebujete přihlašovací údaje zařízení.

Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů. Přihlašovací údaje zařízení můžete načíst z řešení **zařízení** stránky. Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.

Chcete-li přidat zařízení do řešení vzdáleného monitorování, proveďte následující kroky na **zařízení** stránky v řešení:

1. Zvolte **zřídit**a potom zvolte **fyzické** jako **typ zařízení**:

    ![Zřídit fyzické zařízení](media/iot-suite-selector-connecting/devicesprovision.png)

1. Zadejte **fyzického chladič** jako ID zařízení. Vyberte **symetrický klíč** a **automaticky generovat klíče** možnosti:

    ![Vyberte možnosti zařízení](media/iot-suite-selector-connecting/devicesoptions.png)

Chcete-li vyhledat přihlašovacích údajů, které zařízení musí používat pro připojení k předkonfigurované řešení, přejděte na portálu Azure v prohlížeči. Přihlaste se k předplatnému.

1. Najděte skupinu prostředků, která obsahuje služeb Azure, které používá vaše řešení vzdáleného monitorování. Skupina prostředků má stejný název jako řešení vzdáleného monitorování, kterou jste zřídili.

1. Přejděte do služby IoT hub v této skupině prostředků. Zvolte **zařízení IoT**:

    ![Průzkumník zařízení](media/iot-suite-selector-connecting/deviceexplorer.png)

1. Vyberte **ID zařízení** jste vytvořili na **zařízení** stránky v řešení vzdáleného monitorování.

1. Poznamenejte si **ID zařízení** a **primární klíč** hodnoty. Tyto hodnoty použít, když přidáte kód k připojení zařízení k řešení.

Můžete mít teď zřídit fyzické zařízení ve vzdálené monitorování předkonfigurované řešení. V následujících částech můžete implementovat aplikace klienta, která používá přihlašovací údaje zařízení pro připojení k řešení.

Klientská aplikace implementuje integrované **chladič** model zařízení. Model zařízení předkonfigurované řešení určuje toto zařízení:

* Vlastnosti zařízení sestav řešení. Například **chladič** zařízení hlásí informace o jeho firmware a umístění.
* Typy telemetrických dat, které zařízení odesílá do řešení. Například **chladič** zařízení odesílá přetížení hodnoty, vlhkosti a teploty.
* Metody můžete naplánovat z řešení na zařízení spouštět. Například **chladič** zařízení musí implementovat **restartovat**, **FirmwareUpdate**, **EmergencyValveRelease**, a  **IncreasePressuree** metody.