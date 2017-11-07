## <a name="view-the-telemetry"></a>Zobrazení telemetrie

Pi malin nyní odesílá telemetrii do řešení vzdáleného monitorování. Můžete zobrazit telemetrii na řídicí panel řešení. Na řídicím panelu řešení také mohou zasílat zprávy do vaší malin pí.

- Přejděte na řídicí panel řešení.
- Vyberte zařízení v **zařízení do zobrazení** rozevíracího seznamu.
- Telemetrie z platformy malin zobrazí na řídicím panelu.

![Zobrazení telemetrie z malin platformy][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Spustit aktualizaci firmwaru

Proces aktualizace firmwaru stáhne a nainstaluje aktualizovaná verze aplikace klienta zařízení na malin pí. Další informace o procesu aktualizace firmwaru, naleznete v popisu vzoru aktualizace firmwaru v [přehled správy zařízení s centrem IoT][lnk-update-pattern].

Proces aktualizace firmwaru iniciovat volání metody na zařízení. Tato metoda je asynchronní a vrátí ihned zahájí proces aktualizace. Zařízení používá hlášené vlastnosti oznámit řešení o průběhu aktualizace.

Vyvolání metody na vaše malin platformy na řídicím panelu řešení. Když pí malin poprvé připojí k řešení vzdáleného monitorování, odešle informace o metodách, které podporuje. 

[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
