> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Tento článek poskytuje podrobný návod k [ukázkovému kódu Hello World][lnk-helloworld-sample] pro ilustraci základních komponent architektury [Azure IoT Edge][lnk-iot-edge]. Ukázka používá Azure IoT Edge k vytvoření jednoduché brány, která každých pět sekund zaznamená do souboru zprávu „hello world“.

Tento návod ilustruje:

* **Hello World ukázková architektura**: Popisuje, jak [architektury koncepty Azure IoT Edge] [ lnk-edge-concepts] týkají Hello, World vzorek a jak součásti zapadají.
* **Postup k vytvoření ukázky**: kroky potřebné k vytvoření ukázkového kódu.
* **Postup spuštění ukázky**: kroky potřebné ke spuštění ukázkového kódu. 
* **Příklad typického výstupu**: příklad výstupu, jaký můžete očekávat při spuštění ukázky.
* **Fragmenty kódu**: kolekce fragmenty kódu zobrazit, jak Hello, World vzorek implementuje klíčové komponenty IoT hraniční brány.


## <a name="hello-world-sample-architecture"></a>Architektura ukázky Hello World
Ukázka Hello World ilustruje koncepty popsané v předchozí části. Hello World vzorek implementuje IoT hraniční bránu, která se skládá ze dvou IoT Edge moduly kanál:

* Modul *hello world* vytvoří každých pět sekund zprávu a předá ji do modulu logger.
* Modul *logger* zapíše přijatou zprávu do souboru.

![Architektura ukázky Hello World vytvořené s použitím služby Azure IoT Edge][4]

Jak je popsáno v předchozí části, modul Hello World nepředává každých pět sekund zprávu přímo do modulu logger. Místo toho ji každých pět sekund publikuje do zprostředkovatele.

Protokolovací modul obdrží od zprostředkovatele zprávu a postupuje podle ní, zatímco zapisuje obsah zprávy do souboru.

Protokolovací modul od zprostředkovatele zprávy pouze přijímá, nikdy žádné sám nepublikuje.

![Způsob, jakým zprostředkovatel provádí směrování zpráv mezi moduly ve službě Azure IoT Edge][5]

Předchozí obrázek znázorňuje architekturu Hello, World vzorek a relativní cesty ke zdrojovým souborům, které implementují různé části vzorku v [úložiště][lnk-iot-edge]. Prozkoumejte kód vlastní, nebo použijte jako vodítko fragmenty kódu v tomto článku.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md