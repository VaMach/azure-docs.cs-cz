## <a name="view-device-telemetry"></a>Zobrazení telemetrie zařízení

Můžete zobrazit telemetrická data odesílaná ze zařízení **zařízení** stránky v řešení.

1. Vyberte zařízení, které jsou zřízené v seznamu zařízení na **zařízení** stránky. Panelu se zobrazí informace o vašem zařízení, včetně vykreslení telemetrie zařízení:

    ![Najdete v části Podrobnosti o zařízení](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Zvolte **přetížení** ke změně zobrazení telemetrie:

    ![Zobrazení telemetrie přetížení](media/iot-suite-visualize-connecting/devicespressure.png)

1. Chcete-li zobrazit diagnostické informace o vašem zařízení, přejděte dolů na **diagnostiky**:

    ![Zobrazení zařízení diagnostiky](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Fungovat na zařízení

Chcete-li volat metody na zařízení, použijte **zařízení** stránky v řešení vzdáleného monitorování. Například v řešení vzdáleného monitorování **chladič** zařízení implementovat **restartovat** metoda.

1. Zvolte **zařízení** přejděte na **zařízení** stránky v řešení.

1. Vyberte zařízení, které jsou zřízené v seznamu zařízení na **zařízení** stránky:

    ![Vyberte fyzické zařízení](media/iot-suite-visualize-connecting/devicesselect.png)

1. Chcete-li zobrazit seznam metod, můžete volat na vašem zařízení, zvolte **plán**. Při plánování metodu pro spuštění na několika zařízeních, můžete vybrat více zařízení v seznamu. **Plán** panelu zobrazí požadované typy metoda společné pro všechny vámi vybraná zařízení.

1. Zvolte **restartovat**, nastavte název úlohy na **RebootPhysicalChiller**a zvolte **použít**:

    ![Plánování restartování](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Zobrazí se zpráva v konzole pro spuštění kódu vašeho zařízení, když zařízení zpracovává metodu.

> [!NOTE]
> Chcete-li sledovat stav úlohy v řešení, zvolte **zobrazení**.

## <a name="next-steps"></a>Další kroky

Článek [přizpůsobení předkonfigurovaného řešení vzdáleného monitorování](../articles/iot-suite/iot-suite-remote-monitoring-customize.md) popisuje několik způsobů, jak přizpůsobit předkonfigurovaného řešení.