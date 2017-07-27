# Přehled
## [Azure a IoT](iot-hub-what-is-azure-iot.md)
## [Co je Azure IoT Hub?](iot-hub-what-is-iot-hub.md)
## [Přehled správy zařízení](iot-hub-device-management-overview.md)

# [Začínáme](iot-hub-get-started.md)

## Nastavení zařízení
### [Simulace zařízení na počítači](iot-hub-get-started-simulated.md)
#### [.NET](iot-hub-csharp-csharp-getstarted.md)
#### [Java](iot-hub-java-java-getstarted.md)
#### [Node.js](iot-hub-node-node-getstarted.md)
#### [Python](iot-hub-python-getstarted.md)

### [Použití online simulátoru](iot-hub-raspberry-pi-web-simulator-get-started.md)

### [Použití fyzického zařízení](iot-hub-get-started-physical.md)
#### [Raspberry Pi s Node.js](iot-hub-raspberry-pi-kit-node-get-started.md)
#### [Raspberry Pi s C](iot-hub-raspberry-pi-kit-c-get-started.md)

#### [Intel Edison s Node.js](iot-hub-intel-edison-kit-node-get-started.md)
#### [Intel Edison s C](iot-hub-intel-edison-kit-c-get-started.md)

#### [Adafruit Feather HUZZAH ESP8266 s rozhraním Arduino IDE](iot-hub-arduino-huzzah-esp8266-get-started.md)
#### [Sparkfun ESP8266 Thing Dev s rozhraním Arduino IDE](iot-hub-sparkfun-esp8266-thing-dev-get-started.md)
#### [Adafruit Feather M0 s rozhraním Arduino IDE](iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md)

#### Použití sady IoT Gateway Starter Kit
##### [Nastavení Intel NUC jako brány](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
##### [Připojení brány ke službě IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
##### [Použití brány pro převod dat](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)

## Rozšířené scénáře IoT
### [Správa zasílání zpráv cloudových zařízení pomocí iothub-exploreru](iot-hub-explorer-cloud-device-messaging.md)
### [Uložení zpráv IoT Hub do úložiště dat Azure](iot-hub-store-data-in-azure-table-storage.md)
### [Vizualizace dat ve službě Power BI](iot-hub-live-data-visualization-in-power-bi.md)
### [Vizualizace dat s využitím Web Apps](iot-hub-live-data-visualization-in-web-apps.md)
### [Předpověď počasí s využitím Azure Machine Learningu](iot-hub-weather-forecast-machine-learning.md)
### [Správa zařízení s využitím iothub-exploreru](iot-hub-device-management-iothub-explorer.md)
### [Vzdálené monitorování a oznámení s využitím Logic Apps](iot-hub-monitoring-notifications-with-azure-logic-apps.md)

# Postup
## Plánování
### [Porovnání IoT Hub a Event Hubs](iot-hub-compare-event-hubs.md)
### [Škálování vlastního řešení](iot-hub-scaling.md)
### [Vysoká dostupnost a zotavení po havárii](iot-hub-ha-dr.md)
### [Podpora dalších protokolů](iot-hub-protocol-gateway.md)
## [Vývoj](iot-hub-how-to.md)
### [Příručka pro vývojáře](iot-hub-devguide.md)
#### [Průvodce funkcemi ze zařízení do cloudu](iot-hub-devguide-d2c-guidance.md)
#### [Průvodce funkcemi z cloudu do zařízení](iot-hub-devguide-c2d-guidance.md)
#### [Odesílání a příjem zpráv](iot-hub-devguide-messaging.md)
##### [Odeslání zpráv ze zařízení do cloudu do IoT Hubu](iot-hub-devguide-messages-d2c.md)
##### [Čtení zpráv ze zařízení do cloudu z integrovaného koncového bodu](iot-hub-devguide-messages-read-builtin.md)
##### [Použití vlastních koncových bodů a pravidel směrování pro zprávy ze zařízení do cloudu](iot-hub-devguide-messages-read-custom.md)
##### [Odesílání zpráv z cloudu do zařízení z IoT Hubu](iot-hub-devguide-messages-c2d.md)
##### [Vytvoření a čtení zpráv IoT Hubu](iot-hub-devguide-messages-construct.md)
##### [Volba komunikačního protokolu](iot-hub-devguide-protocols.md)
#### [Odeslání souborů ze zařízení](iot-hub-devguide-file-upload.md)
#### [Správa identit zařízení](iot-hub-devguide-identity-registry.md)
#### [Řízení přístupu k IoT Hubu](iot-hub-devguide-security.md)
#### [Principy dvojčat zařízení](iot-hub-devguide-device-twins.md)
#### [Vyvolání přímých metod v zařízení](iot-hub-devguide-direct-methods.md)
#### [Plánování úloh na několika zařízeních](iot-hub-devguide-jobs.md)
#### [Koncové body IoT Hubu](iot-hub-devguide-endpoints.md)
#### [Dotazovací jazyk](iot-hub-devguide-query-language.md)
#### [Kvóty a omezování](iot-hub-devguide-quotas-throttling.md)
#### [Příklady cen](iot-hub-devguide-pricing.md)
#### [Sady SDK pro zařízení a služby](iot-hub-devguide-sdks.md)
#### [Podpora MQTT](iot-hub-mqtt-support.md)
#### [Glosář](iot-hub-devguide-glossary.md)
### [Použití sady SDK pro zařízení IoT pro C](iot-hub-device-sdk-c-intro.md)
#### [Použití IoTHubClient](iot-hub-device-sdk-c-iothubclient.md)
#### [Použití serializátoru](iot-hub-device-sdk-c-serializer.md)
### Zpracování zpráv ze zařízení do cloudu
#### [.NET](iot-hub-csharp-csharp-process-d2c.md)
#### [Java](iot-hub-java-java-process-d2c.md)
### Odesílání zpráv z cloudu do zařízení
#### [.NET](iot-hub-csharp-csharp-c2d.md)
#### [Java](iot-hub-java-java-c2d.md)
#### [Node.js](iot-hub-node-node-c2d.md)
### [Nahrání souborů ze zařízení](iot-hub-csharp-csharp-file-upload.md)
### Začínáme s dvojčaty zařízení
#### [Back-end Node.js / zařízení Node.js](iot-hub-node-node-twin-getstarted.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-twin-getstarted.md)
#### [Back-end .NET / zařízení .NET](iot-hub-csharp-csharp-twin-getstarted.md)
### Použití přímých metod
#### [Back-end Node.js / zařízení Node.js](iot-hub-node-node-direct-methods.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-direct-methods.md)
#### [Back-end .NET / zařízení .NET](iot-hub-csharp-csharp-direct-methods.md)
#### [Java back-end / zařízení Java](iot-hub-java-java-direct-methods.md)
### Začínáme se správou zařízení
#### [Back-end Node.js / zařízení Node.js](iot-hub-node-node-device-management-get-started.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-device-management-get-started.md)
#### [Java back-end / zařízení Java](iot-hub-java-java-device-management-getstarted.md)
### Jak používat vlastnosti dvojčat
#### [Back-end Node.js / zařízení Node.js](iot-hub-node-node-twin-how-to-configure.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-twin-how-to-configure.md)
#### [Back-end .NET / zařízení .NET](iot-hub-csharp-csharp-twin-how-to-configure.md)
### Použití úloh zařízení k aktualizaci firmwaru zařízení
#### [Back-end Node / zařízení Node](iot-hub-node-node-firmware-update.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-firmware-update.md)
### Úlohy vysílání a plánování
#### [Back-end Node.js / zařízení Node.js](iot-hub-node-node-schedule-jobs.md)
#### [Back-end .NET / zařízení Node.js](iot-hub-csharp-node-schedule-jobs.md)
## Spravovat
### Vytvoření centra IoT 
#### [Použití portálu](iot-hub-create-through-portal.md)
#### [Použití PowerShellu](iot-hub-create-using-powershell.md)
#### [Použití CLI 2.0](iot-hub-create-using-cli.md)
#### [Použití rozhraní příkazového řádku](iot-hub-create-using-cli-nodejs.md)
#### [Použití rozhraní REST API](iot-hub-rm-rest.md)
#### [Použití šablony z PowerShellu](iot-hub-rm-template-powershell.md)
#### [Použití šablony z rozhraní .NET](iot-hub-rm-template.md)
### Konfigurace odesílání souborů
#### [Použití portálu](iot-hub-configure-file-upload.md)
#### [Použití PowerShellu](iot-hub-configure-file-upload-powershell.md)
#### [Použití CLI 2.0](iot-hub-configure-file-upload-cli.md)
### [Hromadná správa zařízení IoT](iot-hub-bulk-identity-mgmt.md)
### [Metriky využití](iot-hub-metrics.md)
### [Monitorování operací](iot-hub-operations-monitoring.md)
### [Konfigurace filtrování IP](iot-hub-ip-filtering.md)
## Zabezpečení
### [Zabezpečení od počátku](iot-hub-security-ground-up.md)
### [Osvědčené postupy zabezpečení](iot-hub-security-best-practices.md)
### [Architektura zabezpečení](iot-hub-security-architecture.md)
### [Zabezpečení nasazení IoT](iot-hub-security-deployment.md)
## Azure IoT Edge
### [Přehled](iot-hub-iot-edge-overview.md)
### Začínáme
#### [Linux](iot-hub-linux-iot-edge-get-started.md)
#### [Windows](iot-hub-windows-iot-edge-get-started.md)
### Simulace zařízení
#### [Linux](iot-hub-linux-iot-edge-simulated-device.md)
#### [Windows](iot-hub-windows-iot-edge-simulated-device.md)
### [Použití skutečného zařízení](iot-hub-iot-edge-physical-device.md)
### Vytvoření modulu
#### [Java](iot-hub-iot-edge-create-module-java.md)
#### [.NET Framework](https://github.com/Azure-Samples/iot-edge-samples#how-to-run-the-net-module-sample-windows-10)
#### [.NET Standard](iot-hub-iot-edge-create-module-dotnet-core.md)
#### [Node.js](iot-hub-iot-edge-create-module-js.md)
### Sestavení
#### [.NET Framework](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample)
#### [Modul .NET Core](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_core_module_sample)
#### [Spravovaná brána .NET Core](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_core_managed_gateway)
#### [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample)
#### [Node.js](https://github.com/Azure/iot-edge/tree/master/samples/nodejs_simple_sample)
#### [Dynamické přidání modulu](https://github.com/Azure/iot-edge/tree/master/samples/dynamically_add_module_sample)
#### [Mimoprocesový modul proxy serveru](https://github.com/Azure/iot-edge/tree/master/samples/proxy_sample)
#### [Hostitel nativního modulu](https://github.com/Azure/iot-edge/tree/master/samples/native_module_host_sample)

# Referenční informace
## [Azure CLI](/cli/azure/iot)
## [.NET (služba)](/dotnet/api/microsoft.azure.devices)
## [.NET (zařízení)](/dotnet/api/microsoft.azure.devices.client)
## [Java (služba)](/java/api/com.microsoft.azure.sdk.iot.service)
## [Java (zařízení)](/java/api/com.microsoft.azure.sdk.iot.device)
## [Sady SDK pro Node.js](http://azure.github.io/azure-iot-sdk-node/)
## [Sady SDK pro zařízení jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html)
## [Azure IoT Edge](http://azure.github.io/iot-edge/)
## [REST (poskytovatel prostředků)](https://docs.microsoft.com/rest/api/iothub/iothubresource)
## [REST (identity zařízení)](https://docs.microsoft.com/rest/api/iothub/deviceapi)
## [REST (dvojčata zařízení)](https://docs.microsoft.com/rest/api/iothub/devicetwinapi)
## [REST (Device Messaging)](https://docs.microsoft.com/rest/api/iothub/httpruntime)
## [REST (úlohy)](https://docs.microsoft.com/rest/api/iothub/jobapi)

# Související
## [Azure IoT Suite](https://azure.microsoft.com/documentation/suites/iot-suite/)
## [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/)
## [Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/)
## [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)

# Zdroje a prostředky
## [Katalog zařízení Azure Certified for IoT](https://catalog.azureiotsuite.com/)
## [Centrum pro vývojáře Azure IoT](https://azure.microsoft.com/develop/iot/)
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/?category=internet-of-things)
## [Nástroj DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)
## [Nástroj iothub-diagnostics](https://github.com/Azure/iothub-diagnostics)
## [Nástroj iothub-explorer](https://github.com/Azure/iothub-explorer)
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/iot-hub/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azureiothub)
## [Ceny](https://azure.microsoft.com/pricing/details/iot-hub/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=iot-hub)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-iot-hub)
## [Technické případové studie](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=iot-hub)
