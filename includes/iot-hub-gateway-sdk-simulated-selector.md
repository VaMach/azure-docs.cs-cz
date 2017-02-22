> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)
> 
> 

Tento návod k [ukázce Nahrávání do cloudu ze simulovaného zařízení] ukazuje, jak pomocí [sady SDK Azure IoT Gateway][lnk-sdk] odesílat telemetrická data typu zařízení-cloud do služby IoT Hub ze simulovaných zařízení.

Tento návod ilustruje:

1. **Architekturu:** Důležité informace o architektuře ukázky Nahrávání do cloudu ze simulovaného zařízení.
2. **Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.

## <a name="architecture"></a>Architektura
Ukázka Nahrávání do cloudu ze simulovaného zařízení ukazuje, jak pomocí sady SDK vytvořit bránu, která odesílá telemetrická data ze simulovaných zařízení do služby IoT Hub. Simulovaná zařízení se nemohou připojit přímo k službě IoT Hub, protože:

* Tato zařízení nepoužívají komunikační protokol, kterému služba IoT Hub rozumí.
* Tato zařízení nejsou dostatečně inteligentní, aby si pamatovala identitu, kterou jim služba IoT Hub přiřadí.

Brána tyto problémy pro simulovaná zařízení řeší následujícími způsoby:

* Brána rozumí protokolu, který používají simulovaná zařízení, přijímá ze zařízení telemetrická data typu zařízení-cloud a předává tyto zprávy do služby IoT Hub pomocí protokolu, kterému služba rozumí.
* Brána uchovává identity služby IoT Hub pro simulovaná zařízení a funguje jako proxy, když simulovaná zařízení odesílají zprávy do služby IoT Hub.

Následující diagram znázorňuje hlavní součásti ukázky, včetně modulů brány:

![][1]

> [!NOTE]
> Moduly si mezi sebou nepředávají zprávy přímo. Moduly publikují zprávy v interním zprostředkovateli, který doručuje zprávy do dalších modulů pomocí mechanismu předplatného, jak je znázorněno na následujícím diagramu. Další informace najdete v článku [Začínáme se sadou SDK IoT Gateway][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Modul ingestování protokolu
Tento modul je výchozím bodem pro získávání dat ze zařízení a jejich předání přes bránu až do cloudu. V ukázce modul provádí čtyři úlohy:

1. Vytváří simulovaná data o teplotě. Pamatujte, že pokud použijete skutečná zařízení, modul bude číst data z těchto fyzických zařízení.
2. Umisťuje simulovaná data o teplotě do obsahu zprávy.
3. Přidává do zprávy se simulovanými daty o teplotě vlastnost s falešnou adresou MAC.
4. Zpřístupňuje zprávu dalšímu modulu v řetězci.

> [!NOTE]
> Modul **Protocol X ingestion** (Ingestování protokolu X) v diagramu výše se ve zdrojovém kódu nazývá **Simulated device** (Simulované zařízení).
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adresa MAC &lt; - &gt; Modul ID služby IoT Hub
Tento modul prohledává zprávy obsahující vlastnost s adresou MAC aplikace simulovaného zařízení, kterou přidal modul ingestování protokolu. Pokud modul takovou vlastnost najde, přidá do zprávy další vlastnost s klíčem zařízení služby IoT Hub a potom zprávu zpřístupní dalšímu modulu v řetězci. Tímto způsobem se v ukázce přidružují identity zařízení služby IoT Hub k simulovaným zařízením. Vývojář ručně nastavuje mapování mezi adresami MAC a identitami služby IoT Hub jako součást konfigurace modulu. 

> [!NOTE]
> Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub. Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor. Můžete mít například zařízení s jedinečnými sériovými čísly nebo telemetrická data, jejichž součástí je jedinečný název zařízení, a tyto údaje použít k určení identity zařízení služby IoT Hub.
> 
> 

### <a name="iot-hub-communication-module"></a>Komunikační modul služby IoT Hub
Tento modul přebírá zprávy s identitou zařízení služby IoT Hub, kterou jim přiřadil předchozí modul, a pomocí protokolu HTTP odesílá obsah zprávy do služby IoT Hub. HTTP je jedním ze tří protokolů, kterým služba IoT Hub rozumí.

Místo otevírání připojení k službě IoT Hub pro každou aplikaci simulovaného zařízení tento modul otevírá jediné připojení HTTP z brány k službě IoT Hub a v tomto připojení spojuje připojení ze všech simulovaných zařízení. Díky tomu může jediná brána připojit mnohem více zařízení, simulovaných nebo skutečných, než by bylo možné, kdyby pro každé zařízení otevírala jedinečné připojení.

![][2]

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[ukázce Nahrávání do cloudu ze simulovaného zařízení]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

<!--HONumber=Feb17_HO1-->


