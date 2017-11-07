## <a name="view-the-telemetry"></a>Zobrazení telemetrie

Pi malin nyní odesílá telemetrii do řešení vzdáleného monitorování. Můžete zobrazit telemetrii na řídicí panel řešení. Na řídicím panelu řešení také mohou zasílat zprávy do vaší malin pí.

- Přejděte na řídicí panel řešení.
- Vyberte zařízení v **zařízení do zobrazení** rozevíracího seznamu.
- Telemetrie z platformy malin zobrazí na řídicím panelu.

![Zobrazení telemetrie z malin platformy][img-telemetry-display]

## <a name="act-on-the-device"></a>Fungovat na zařízení

Na řídicím panelu řešení můžete volat metody na vaše malin pí. Když pí malin připojí k řešení vzdáleného monitorování, odešle informace o metodách, které podporuje.

- Na řídicím panelu řešení, klikněte na tlačítko **zařízení** přejděte **zařízení** stránky. Vyberte vaše Malinová platformy v **seznam zařízení**. Zvolte **metody**:

    ![Seznam zařízení na řídicím panelu][img-list-devices]

- Na **vyvolání metody** vyberte **LightBlink** v **metoda** rozevíracího seznamu.

- Zvolte **InvokeMethod**. Simulátor vytiskne zprávy v konzole na malin pí. Aplikace na platformy malin odešle na potvrzení zpět na řídicí panel řešení:

    ![Zobrazit historii – metoda][img-method-history]

- Můžete přepnout Indikátor zapnout a vypnout pomocí **ChangeLightStatus** metoda s **LightStatusValue** nastavena na **1** pro na nebo **0** pro vypnout.

> [!WARNING]
> Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění. Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.


[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md