
# Azure a internet věcí

Vítejte v tématu Microsoft Azure a internet věcí (IoT). Tento článek představuje architekturu řešení IoT popisující běžné vlastnosti řešení IoT, které můžete nasadit pomocí služeb Azure. Řešení IoT vyžadují bezpečnou obousměrnou komunikaci mezi zařízeními, jejichž počet se může vyšplhat k milionům, a back-end řešení. Back-end řešení například může pomocí automatizované prediktivní analýzy odhalit další poznatky z datového proudu událostí ze zařízení do cloudu.

Služba Azure IoT Hub je klíčovým stavebním blokem při implementaci této architektury řešení IoT pomocí služeb Azure. Sada IoT Suite poskytuje kompletní, ucelené implementace této architektury pro konkrétní scénáře IoT. Příklad: 

- Řešení *vzdáleného monitorování* umožňuje monitorovat stav zařízení, jako jsou například prodejní automaty. 
- Řešení *prediktivní údržby* pomáhá předvídat potřeby údržby zařízení, například čerpadel na vzdálených čerpacích stanicích, a zabraňuje neplánovaným výpadkům.

## Architektura řešení IoT

Následující diagram ukazuje typickou architekturu řešení IoT. Diagram neobsahuje žádné konkrétní názvy služeb Azure, ale popisuje klíčové prvky v obecné architektuře řešení IoT. V této architektuře zařízení IoT shromažďují data, která odesílají do cloudové brány. Cloudová brána zpřístupní data pro zpracování dalším back-endovým službám, ze kterých se budou data prostřednictvím řídicího panelu nebo jiného prezentačního zařízení doručovat do dalších obchodních aplikací nebo k lidské obsluze.

![Architektura řešení IoT][img-solution-architecture]

> [AZURE.NOTE] Podrobné informace o architektuře IoT najdete v článku [Referenční architektura IoT v Microsoft Azure][lnk-refarch].

### Připojení zařízení

Zařízení v této architektuře řešení IoT odesílají telemetrická data, například odečty snímačů z čerpací stanice, do koncového bodu cloudu, kde jsou uloženy a zpracovány. Ve scénáři prediktivní údržby může back-end použít datový proud s daty ze snímačů, aby zjistil, kdy konkrétní čerpadlo vyžaduje údržbu. Zařízení můžete také přijímat a reagovat na příkazy z cloudu do zařízení tak, že si přečte zprávy z koncového bodu cloudu. Ve scénáři prediktivní údržby může back-end řešení například odeslat příkazy jiným čerpadlům čerpací stanice, aby těsně před plánovaným začátkem údržby přesměrovala toky, a tím pracovníkovi údržby umožní začít s prací hned, jak dorazí na místo.

Jeden z největších problémů, kterým projekty IoT čelí, je způsob spolehlivého a bezpečného připojení zařízení k back-endu řešení. Zařízení IoT mají se srovnání s různými klienty, například s prohlížeči a mobilními aplikacemi, jiné vlastnosti. Zařízení IoT:

- Jsou často vestavěnými systémy bez lidské obsluhy.
- Mohou být nasazená ve vzdálených umístěních, kam je fyzický přístup nákladný.
- Můžou být dostupná jenom prostřednictvím back-endu řešení. Neexistuje jiný způsob práce se zařízením.
- Můžou mít omezené prostředky pro napájení a zpracování.
- Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
- Můžou potřebovat chráněné, vlastní nebo průmyslové protokoly aplikací.
- Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků musí jakékoli řešení IoT zajistit také škálování, zabezpečení a spolehlivost. Výslednou sadu požadavků na připojení je obtížné implementovat pomocí tradičních technologií, jakými jsou webové kontejnery a zprostředkovatelé zasílání zpráv. Azure IoT Hub a sady SDK pro zařízení IoT usnadňují implementace řešení, které tyto požadavky splňují.

Zařízení může komunikovat přímo s koncovým bodem cloudové brány, nebo pokud zařízení nemůže používat žádné komunikační protokoly, které cloudová brána podporuje, může se připojit prostřednictvím zprostředkující brány. Například [brána protokolu IoT Hub][lnk-protocol-gateway] může provádět překlad protokolu v případě, že zařízení nemohou použít žádný protokol, který služba IoT Hub podporuje.

### Zpracování a analýza dat

V rámci cloudu se většina dat zpracovává v back-endu řešení IoT, jedná se například o filtrování a agregování telemetrických dat a jejich směrování do dalších služeb. Back-end řešení IoT:

- Přijímá škálovaná telemetrická data ze všech zařízení a určuje, jak tato data zpracovávat a ukládat. 
- Může vám umožnit odesílání příkazů z cloudu do určitého zařízení.
- Poskytuje možnosti registrace zařízení, které vám umožní zřizování zařízení a kontrolu toho, která zařízení mají povolení k připojení k vaší infrastruktuře.
- Umožňuje sledování stavu zařízení a sledování jejich aktivit.

Ve scénáři prediktivní údržby ukládá back-end řešení historická telemetrická data. Back-end může pomocí těchto dat rozpoznat vzorce, které naznačují, že je třeba provést údržbu konkrétního čerpadla.

Řešení IoT může obsahovat smyčky automatické zpětné vazby. Analytický modul v back-endu může například z telemetrických dat rozpoznat, že teplota konkrétního zařízení překračuje běžnou provozní úroveň. Řešení následně může do zařízení odeslat příkaz s pokyny k provedení nápravné akce.

### Prezentační a obchodní připojení

Vrstva prezentačního a obchodního připojení umožňuje koncovým uživatelům pracovat s řešením IoT a se zařízeními. Umožňuje uživatelům zobrazit a analyzovat data shromážděná z jejich zařízení. Tato zobrazení můžou mít podobu řídicích panelů nebo sestav BI, které můžou zobrazit historická data i data téměř v reálném čase. Obsluha může například zkontrolovat stav konkrétní čerpací stanice a zobrazit všechny výstrahy vyvolané systémem. Tato vrstva také umožňuje integraci back-endu řešení IoT se stávajícími obchodními aplikacemi a jejich zapojení do podnikových obchodních procesů nebo pracovních postupů. Řešení prediktivní údržby se může integrovat například s plánovacím systémem, který zarezervuje návštěvu technika na čerpací stanici v případě, kdy řešení zjistí, že některé čerpadlo potřebuje údržbu.

![Řídicí panel řešení IoT][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf


<!--HONumber=Oct16_HO3-->


