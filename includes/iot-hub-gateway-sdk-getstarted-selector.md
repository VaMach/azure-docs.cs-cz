> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Tento článek poskytuje podrobný návod k [ukázkovému kódu Hello World][lnk-helloworld-sample] ilustrujícího základní součásti architektury [sady Azure IoT Gateway SDK][lnk-gateway-sdk]. Příklad používá sadu Gateway SDK k vytvoření jednoduché brány, která každých pět sekund zaznamená do souboru zprávu „hello world“.

Tento návod ilustruje:

- **Koncepce**: celkový přehled součásti, které tvoří každou bránu, kterou vytvoříte pomocí sady Gateway SDK.  
- **Ukázková architektura Hello World**: popis konceptů, které se vztahují na ukázku Hello World, a jak součásti do sebe zapadají.
- **Postup k vytvoření ukázky**: kroky potřebné k vytvoření ukázkového kódu.
- **Postup spuštění ukázky**: kroky potřebné ke spuštění ukázkového kódu. 
- **Příklad typického výstupu**: příklad výstupu, jaký můžete očekávat při spuštění ukázky.
- **Fragmenty kódu**: sbírka fragmentů kódu předvádějících, jak ukázka Hello, World implementuje klíčové součásti brány.

## Koncepce sady Gateway SDK

Než si projdete ukázkový kód nebo než vytvoříte pomocí sady SDK svou vlastní bránu, měli byste porozumět klíčovým koncepcím, které jsou základem architektury sady Gateway SDK.

### Moduly

Brána se pomocí sady Azure IoT Gateway SDK otvírá vytvořením a propojením *modulů*. Moduly používají pro výměnu dat mezi sebou *zprávy*. Modul obdrží zprávu a provede s ní určitou akci, například ji může transformovat na novou zprávu a pak ji publikovat pro ostatní moduly ke zpracování. Některé moduly pouze vytvářejí nové zprávy a nezpracovávají zprávy příchozí. Řetěz modulů vytvoří kanál zpracování dat, ve kterém každý modul provádí transformaci dat v daném místě kanálu.

![][1]
 
Sada SDK obsahuje následující:

- Předpřipravené moduly, které provádějí běžné funkce brány.
- Rozhraní, které vývojář může použít k vytvoření vlastních modulů.
- Infrastrukturu nutnou pro nasazení a spuštění sady modulů.

Sada SDK poskytuje abstraktní vrstvu, která umožňuje vytvářet brány spouštěné na různých operačních systémech a platformách.

![][2]

### Zprávy

Ačkoli je představa modulů posílajících zprávy pohodlným způsobem, jak popsat koncepci funkce brány, neodráží přesně samotnou její činnost. Moduly spolu navzájem komunikují pomocí sběrnice zpráv – publikují je na sběrnici a sběrnice je pak rozesílá všem připojeným modulům.

K publikování zprávy na sběrnici používají moduly funkci **MessageBus_Publish**. Sběrnice zpráv předává zprávy ostatním modulům zavoláním funkce zpětného volání. Zpráva se skládá ze sady vlastností klíč/hodnota a obsah se předává jako blok paměti.

![][3]

Každý modul musí provádět filtrování zpráv, protože sběrnice zpráv rozesílá zprávy všem připojeným modulům. Modul by měl reagovat jen na zprávy, které jsou pro něho určené. Filtrování zpráv ve výsledku tvoří kanál zpracování zpráv. Modul typicky vyfiltruje svoje zprávy podle jejich vlastností a identifikuje tak ty, které má zpracovat.

## Architektura ukázky Hello World

Ukázka Hello World ilustruje koncepty popsané v předchozí části. Ukázka Hello, World implementuje bránu, jejíž kanál se skládá ze dvou modulů:

-   Modul *hello world* vytvoří každých pět sekund zprávu a předá ji do modulu logger.
-   Modul *logger* zapíše přijatou zprávu do souboru.

![][4]

Jak je popsáno v předchozí části, modul Hello World nepředává každých pět sekund zprávu přímo do modulu logger. Místo toho ji každých pět sekund publikuje na sběrnici zpráv.

Modul logger přijímá zprávy ze sběrnice zpráv a pomocí filtračního mechanismu zkoumá jejich vlastnosti. Když logger zjistí, že by měl zprávu zpracovat, zapíše obsah zprávy do souboru.

Modul logger ze sběrnice zprávy pouze přijímá, nikdy žádné sám nepublikuje.

![][5]

Obrázek nahoře ukazuje architekturu ukázky Hello World a relativní cesty ke zdrojovým souborům, které implementují jednotlivé části, v [úložišti][lnk-gateway-sdk]. Prozkoumejte kód sami, nebo pro orientaci použijte fragmenty kódu uvedené dole.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk


<!--HONumber=Aug16_HO4-->


