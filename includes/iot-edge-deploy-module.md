Mezi klíčové funkce Azure IoT okraj je schopnost nasadit modulů na vaše zařízení IoT Edge z cloudu. Modul IoT okraj je balíček spustitelné implementovaný jako kontejner. V této části nasadíte modul, který generuje telemetrická data pro simulované zařízení. 

1. Na portálu Azure přejděte do služby IoT hub.
1. Přejděte na **IoT okraj (preview)** a vyberte zařízení IoT okraj.
1. Vyberte **nastavit moduly**.
1. Vyberte **přidání okraj IoT modulu**.
1. V **název** zadejte `tempSensor`. 
1. V **Image URI** zadejte `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`. 
1. Další nastavení nezměníte a vyberte **Uložit**.

   ![Modul IoT Edge uložit po zadání názvu a identifikátoru URI bitové kopie](./media/iot-edge-deploy-module/name-image.png)

1. Zpět v **přidat moduly** krok, vyberte **Další**.
1. V **určit trasy** krok, vyberte **Další**.
1. V **šablona kontrolní** krok, vyberte **odeslání**.
1. Vraťte se na stránce s podrobnostmi o zařízení a vyberte **aktualizovat**. Měli byste vidět nového modulu tempSensor systémem podél runtime IoT okraj. 

   ![Zobrazení tempSensor v seznamu modulů nasazené][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png