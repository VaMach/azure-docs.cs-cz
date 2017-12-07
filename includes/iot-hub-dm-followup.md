## <a name="customize-and-extend-the-device-management-actions"></a>Přizpůsobit a rozšířit akce správy zařízení

Řešení IoT můžete rozšiřovat definovanou sadu vzory správy zařízení a povolit vlastní vzorky pomocí dvojče zařízení a primitiv metoda cloud zařízení. Další akce správy zařízení příklady obnovení továrního nastavení, aktualizaci firmwaru, aktualizace softwaru, řízení spotřeby, správu sítí a připojení a šifrování dat.

## <a name="device-maintenance-windows"></a>Časová období údržby zařízení

Obvykle můžete nakonfigurovat zařízení k provádění akcí v čase, který minimalizuje přerušení a výpadky. Časová období údržby zařízení jsou běžně používané vzor zadat čas, kdy zařízení musí aktualizovat její konfiguraci. Back-end řešení můžete definovat a aktivace zásady na zařízení, která umožňuje údržby použít požadované vlastnosti dvojče zařízení. Pokud zařízení obdrží zásady okno údržby, slouží vlastnost hlášené dvojče zařízení nahlásit stav zásad. Back-end aplikačním pak můžete použít dotazy twin zařízení ověřit identitu pro dodržování předpisů u zařízení a každou zásadu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu použít přímá metoda ke spuštění vzdálené restartování na zařízení. Hlášen vlastnostech používají k hlášení čas posledního restartování ze zařízení a dotaz dvojče zařízení chcete zjistit, čas posledního restartování zařízení z cloudu.

Chcete-li pokračovat, Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes aktualizaci firmwaru letecké, najdete v části:

[Kurz: Jak to provést aktualizaci firmwaru][lnk-fwupdate]

Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

Chcete-li pokračovat, Začínáme se službou IoT Hub, najdete v části [Začínáme se službou IoT Edge][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md