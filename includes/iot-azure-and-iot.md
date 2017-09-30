
# <a name="azure-and-internet-of-things"></a>Azure a internet věcí

Vítejte v tématu Microsoft Azure a internet věcí (IoT). Tento článek představuje běžné vlastnosti řešení IoT, které můžete nasadit pomocí služeb Azure. Řešení IoT vyžadují bezpečnou obousměrnou komunikaci mezi velkým počtem zařízení a back-endem řešení. Back-end řešení může pomocí automatizované prediktivní analýzy odhalit další poznatky z datového proudu událostí ze zařízení do cloudu.

[Azure IoT Hub][lnk-iot-hub] je klíčovým stavebním blokem všech řešení IoT využívajících služby Azure. IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a back-endem řešení. 

[Azure IoT Suite][lnk-iot-suite] poskytuje kompletní a ucelené implementace této architektury pro konkrétní scénáře IoT. Například:

* Řešení *vzdáleného monitorování* umožňuje monitorovat stav zařízení, jako jsou například prodejní automaty.
* Řešení *prediktivní údržby* pomáhá předvídat potřebu údržby zařízení, například čerpadel na vzdálených čerpacích stanicích, a zabraňuje neplánovaným výpadkům.
* Řešení *připojené továrny* vám pomůže připojit a monitorovat průmyslová zařízení.

## <a name="iot-solution-architecture"></a>Architektura řešení IoT

Následující diagram ukazuje typickou architekturu řešení IoT. Diagram neobsahuje žádné konkrétní názvy služeb Azure, ale popisuje klíčové prvky v obecné architektuře řešení IoT. V této architektuře zařízení IoT shromažďují data, která odesílají do cloudové brány. Cloudová brána zpřístupňuje data pro zpracování dalšími back-endovými službami. Back-end řešení poskytuje data obchodním aplikacím nebo lidské obsluze prostřednictvím řídicího panelu nebo sestavy.

![Architektura řešení IoT][img-solution-architecture]

> [!NOTE]
> Podrobné informace o architektuře IoT najdete v článku [Referenční architektura IoT v systému Microsoft Azure][lnk-refarch].

### <a name="device-connectivity"></a>Připojení zařízení

Zařízení v tomto řešení IoT odesílají telemetrická data, například odečty snímačů z čerpací stanice, do koncového bodu cloudu, kde se uloží a zpracují. Ve scénáři prediktivní údržby může back-end řešení použít datový proud s daty ze snímačů k určení, kdy konkrétní čerpadlo vyžaduje údržbu. Zařízení může také přijímat a reagovat na zprávy typu cloud-zařízení tak, že si přečte zprávy z koncového bodu cloudu. Ve scénáři prediktivní údržby může back-end řešení například odesílat zprávy jiným čerpadlům čerpací stanice, aby těsně před plánovaným začátkem údržby přesměrovala toky. Tento postup umožní pracovníkovi údržby začít řešit problém okamžitě.

Jeden z největších problémů, kterým projekty IoT čelí, je způsob spolehlivého a bezpečného připojení zařízení k back-endu řešení. Zařízení IoT mají se srovnání s různými klienty, například s prohlížeči a mobilními aplikacemi, jiné vlastnosti. Zařízení IoT:

* Jsou často vestavěnými systémy bez lidské obsluhy.
* Mohou být nasazená ve vzdálených umístěních, kam je fyzický přístup nákladný.
* Můžou být dostupná jenom prostřednictvím back-endu řešení. Neexistuje jiný způsob práce se zařízením.
* Můžou mít omezené prostředky pro napájení a zpracování.
* Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
* Můžou potřebovat chráněné, vlastní nebo průmyslové protokoly aplikací.
* Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků musí jakékoli řešení IoT zajistit také škálování, zabezpečení a spolehlivost. Výslednou sadu požadavků na připojení je obtížné implementovat pomocí tradičních technologií, jakými jsou webové kontejnery a zprostředkovatelé zasílání zpráv. Azure IoT Hub a sady SDK pro zařízení Azure IoT usnadňují implementace řešení, které tyto požadavky splňují.

Zařízení může komunikovat přímo s koncovým bodem cloudové brány. Pokud zařízení nemůže používat žádné komunikační protokoly, které cloudová brána podporuje, může se připojit prostřednictvím zprostředkující brány. Například [brána protokolu Azure IoT ][lnk-protocol-gateway] může provádět překlad protokolu v případě, že zařízení nemohou použít žádný z protokolů, které služba IoT Hub podporuje.

### <a name="data-processing-and-analytics"></a>Zpracování a analýza dat

V rámci cloudu se většina dat zpracovává v back-endu řešení IoT. Back-end řešení IoT:

* Přijímá škálovaná telemetrická data ze všech zařízení a určuje, jak tato data zpracovávat a ukládat. 
* Může vám umožnit odesílání příkazů z cloudu do konkrétních zařízení.
* Poskytuje možnosti registrace zařízení, které vám umožní zřizování zařízení a kontrolu toho, která zařízení se můžou připojit k vaší infrastruktuře.
* Umožňuje sledování stavu zařízení a sledování jejich aktivit.

Ve scénáři prediktivní údržby ukládá back-end řešení historická telemetrická data. Back-end řešení může pomocí těchto dat rozpoznat vzorce, které naznačují, že je třeba provést údržbu konkrétního čerpadla.

Řešení IoT může obsahovat smyčky automatické zpětné vazby. Analytický modul v back-endu řešení může například z telemetrických dat rozpoznat, že teplota konkrétního zařízení překračuje běžnou provozní úroveň. Řešení následně může do zařízení odeslat příkaz s pokyny k provedení nápravné akce.

### <a name="presentation-and-business-connectivity"></a>Prezentační a obchodní připojení

Vrstva prezentačního a obchodního připojení umožňuje koncovým uživatelům pracovat s řešením IoT a se zařízeními. Umožňuje uživatelům zobrazit a analyzovat data shromážděná z jejich zařízení. Tato zobrazení můžou mít podobu řídicích panelů nebo sestav, které zobrazují historická data i data téměř v reálném čase. Obsluha může například zkontrolovat stav konkrétní čerpací stanice a zobrazit všechny výstrahy vyvolané systémem. Tato vrstva také umožňuje integraci back-endu řešení IoT se stávajícími obchodními aplikacemi a jejich zapojení do podnikových obchodních procesů nebo pracovních postupů. Řešením prediktivní údržby je například možné integrovat se systémem plánování. Kdy řešení zjistí, že některé z čerpadel potřebuje údržbu, plánovací systém zarezervuje návštěvu technika na příslušné čerpací stanici.

![Řídicí panel řešení IoT][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
