## <a name="configure-the-nodejs-simulated-device"></a>Konfigurace simulované zařízení Node.js
1. Na řídicím panelu vzdáleného monitorování, klikněte na tlačítko **+ přidat zařízení** a pak přidejte *vlastní zařízení*. Poznamenejte IoT Hub, název hostitele, id zařízení a klíč zařízení. Musíte je později v tomto kurzu při přípravě remote_monitoring.js zařízení klientské aplikace.
2. Ujistěte se, že Node.js verze 0.12.x nebo novější je nainstalován na vývojovém počítači. Spustit `node --version` na příkazovém řádku nebo v prostředí pro kontrolu verze. Informace o používání Správce balíčků instalace Node.js na platformě Linux najdete v tématu [instalací Node.js prostřednictvím Správce balíčků][node-linux].
3. Pokud jste nainstalovali Node.js, klonování nejnovější verzi [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště na vývojovém počítači. Vždy nutné použít **hlavní** větve pro nejnovější verzi knihovny a ukázky.
4. Z vaší místní kopii [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště, kopírovat následující dva soubory ze složky uzlu, zařízení nebo ukázky k prázdné složce na vývojovém počítači:
   
   * Packages.JSON
   * remote_monitoring.js
5. Otevřete soubor remote_monitoring.js a vyhledejte definici následující proměnné:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Nahraďte **[připojovací řetězec služby IoT Hub zařízení]** připojovacím řetězcem zařízení. Použijte hodnoty pro název hostitele služby IoT Hub, id zařízení a klíč zařízení, který jste si poznamenali v kroku 1. Zařízení připojovací řetězec má následující formát:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Pokud je váš název hostitele služby IoT Hub **contoso** a je id zařízení **mydevice**, připojovací řetězec vypadá jako následující fragment kódu:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Uložte soubor. Spusťte následující příkazy v prostředí nebo příkazového řádku ve složce, která obsahuje tyto soubory k instalaci nezbytných balíčků a pak spusťte ukázkovou aplikaci:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Sledovat dynamické telemetrie v akci
Řídicí panel zobrazuje teploty a vlhkosti telemetrie z existující Simulovaná zařízení:

![Výchozí řídicí panel][image1]

Pokud vyberete Node.js simulované zařízení, které jste spustili v předchozí části, uvidíte teploty a vlhkosti, externí teplotní telemetrie:

![Přidat externí teplotu na řídicí panel][image2]

Řešení vzdáleného monitorování automaticky zjišťuje typ další externí teplotní telemetrie a přidá do grafu na řídicím panelu.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png