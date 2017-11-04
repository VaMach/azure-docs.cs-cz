> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Tento návod [simulované zařízení cloudu nahrát ukázková] ukazuje, jak používat [Azure IoT Edge] [ lnk-sdk] k odesílání telemetrie zařízení cloud do služby IoT Hub ze simulovaného zařízení .

Tento návod ilustruje:

* **Architektura**: architektury informace o [simulované zařízení cloudu nahrát ukázková].
* **Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.

## <a name="architecture"></a>Architektura

[simulované zařízení cloudu nahrát ukázková] ukazuje, jak vytvořit bránu, která odesílá telemetrii z Simulovaná zařízení do služby IoT hub. Zařízení nemusí být schopni připojit přímo do služby IoT Hub, protože zařízení:

* Nepoužívá komunikační protokol podporovaných službou IoT Hub.
* Není dostatečně inteligentní pamatovat identity přiřazené službou IoT Hub.

IoT vstupní brána může vyřešit tyto problémy následujícími způsoby:

* Brána rozumí protokol zařízení, použije získává telemetrická data zařízení cloud ze zařízení a předá tyto zprávy do služby IoT Hub použití protokolu podporovaných službou IoT hub.

* Brána mapuje identit služby IoT Hub na zařízení a funguje jako proxy server, když zařízení odesílá zprávy do služby IoT Hub.

Následující diagram znázorňuje hlavní komponenty ukázku, včetně modulů hraniční IoT:

![Diagram – zpráva simulované zařízení projde brány do služby IoT Hub][1]

Tato ukázka obsahuje tři moduly, které tvoří bránu:
1. Modul ingestování protokolu
1. Adresa MAC &lt; - &gt; Modul ID služby IoT Hub
1. Komunikační modul služby IoT Hub

Moduly si mezi sebou nepředávají zprávy přímo. Moduly publikování zpráv interní zprostředkovatele, který doručí zpráv na jiné moduly pomocí mechanismu předplatného. Další informace najdete v tématu [Začínáme s Azure IoT Edge][lnk-gw-getstarted].

![Diagram – modulů brány komunikují zprostředkovatele][2]

### <a name="protocol-ingestion-module"></a>Modul ingestování protokolu

Modul přijímání protokol je výchozím bodem pro proces pořízení data ze zařízení, prostřednictvím brány a do cloudu. 

V ukázce tento modul:

1. Vytvoří simulované teplotní data. Pokud používáte fyzické zařízení, modul čte data z těchto fyzických zařízení.
1. Vytvoří zprávu.
1. Simulované teplotní data umístí do obsahu zprávy.
1. Přidá vlastnost s falešných adresu MAC na zprávu.
1. Zpráva zpřístupňuje další modul v řetězu.

Modul přijímání protokol je **simulated_device.c** ve zdrojovém kódu.

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adresa MAC &lt; - &gt; Modul ID služby IoT Hub

MAC &lt; - &gt; IoT Hub ID modulu funguje jako převaděče. Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub. Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor. Například zařízení může mít jedinečné sériová čísla nebo telemetrická data může zahrnovat název jedinečný vložená zařízení.

V ukázce tento modul:

1. Hledá zprávy, které mají vlastnost adresu MAC.
1. Pokud je adresa MAC, přidá jinou vlastnost s klíčem zařízení IoT Hub ke zprávě. 
1. Zpráva zpřístupňuje další modul v řetězu.

Vývojář nastaví mapování mezi adresy MAC i identit služby IoT Hub tak, aby Simulovaná zařízení přidružit identit zařízení IoT Hub. Vývojář přidá mapování ručně jako součást konfigurace modulu.

MAC &lt; - &gt; IoT Hub ID modulu je **identitymap.c** ve zdrojovém kódu. 

### <a name="iot-hub-communication-module"></a>Komunikační modul služby IoT Hub

Modul služby IoT Hub komunikace otevře jednoho připojení HTTPS z brány do služby IoT Hub. Protokol HTTPS je jedním ze tří protokolů podporovaných službou IoT Hub. Tento modul nebude nutné otevřít připojení pro každé zařízení tak, že multiplexní připojení ze všech zařízení prostřednictvím jednoho připojení. Tento přístup umožňuje jednu bránu pro připojení více zařízení. 

V ukázce tento modul:

1. Přijímá zprávy službou IoT Hub zařízení klíčovou vlastnost, kterému byla přiřazena předchozí modulem. 
1. Odešle obsah zprávy do služby IoT Hub pomocí protokolu HTTPS. 

Modul komunikace služby IoT Hub je **iothub.c** ve zdrojovém kódu.

## <a name="before-you-get-started"></a>Než začnete

Než začnete, musíte provést tyto akce:

* [Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure. Název centra je třeba pro tento ukázkový postup. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* Přidejte dva zařízení do služby IoT hub a poznamenejte si jeho ID a klíče zařízení. Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje pro přidání zařízení do služby IoT hub a načtení jejich klíče.


<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[simulované zařízení cloudu nahrát ukázková]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md