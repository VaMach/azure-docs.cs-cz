<properties
 pageTitle="Přehled služby Azure IoT Hub | Microsoft Azure"
 description="Přehled služby Azure IoT Hub: co je služba IoT Hub, připojení zařízení, schémata komunikace s internetem věcí a schéma komunikace s asistencí služby"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# Co je Azure IoT Hub?

Vítá vás služba Azure IoT Hub. Tento článek obsahuje přehled služby Azure IoT Hub a popisuje důvody implementace řešení internetu věcí (IoT) pomocí této služby.

Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a back-endem řešení. Azure IoT Hub:

- Poskytuje spolehlivé, škálovatelné zasílání zpráv ze zařízení do cloudu a z cloudu do zařízení.
- Umožňuje zabezpečit komunikaci pomocí zabezpečených přihlašovacích údajů a řízení přístupu do zařízení.
- Zajišťuje rozsáhlé monitorování událostí připojení zařízení a správy identity zařízení.
- Zahrnuje knihovny zařízení pro nejoblíbenější jazyky a platformy.

Článek [Srovnání služeb IoT Hub a Event Hubs][lnk-compare] popisuje klíčové rozdíly mezi těmito dvěma službami a zdůrazňuje výhody používání služby IoT Hub ve vašich řešeních pro IoT.

![Služba Azure IoT Hub jako cloudová brána v řešení internetu věcí][img-architecture]

> [AZURE.NOTE] Podrobné informace o architektuře IoT najdete v článku [Referenční architektura IoT v Microsoft Azure][lnk-refarch].

## Výzvy spojené s připojením zařízení IoT

Služba IoT Hub a knihovny zařízení vám pomohou čelit výzvám, které představuje spolehlivé a bezpečné připojení zařízení k back-endu řešení. Zařízení IoT:

- Jsou často vestavěnými systémy bez lidské obsluhy.
- Můžou se nacházet ve vzdálených umístěních, kam je fyzický přístup nákladný.
- Můžou být dostupná jenom prostřednictvím back-endu řešení.
- Můžou mít omezené prostředky pro napájení a zpracování.
- Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
- Můžou potřebovat chráněné, vlastní nebo průmyslové protokoly aplikací.
- Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků musí jakékoli řešení IoT zajistit také škálování, zabezpečení a spolehlivost. Výslednou sadu požadavků na připojení je obtížné implementovat pomocí tradičních technologií, jakými jsou webové kontejnery a zprostředkovatelé zasílání zpráv.

## Proč používat Azure IoT Hub?

Služba Azure IoT Hub řeší problémy s připojením zařízení následujícími způsoby:

-   **Ověřování podle zařízení a zabezpečené připojení**. Každému zařízení můžete zřídit vlastní [klíč zabezpečení][lnk-devguide-security], který umožňuje připojení ke službě IoT Hub. [Registr identit služby IoT Hub][lnk-devguide-identityregistry] ukládá v řešení identity a klíče zařízení. Back-end řešení může jednotlivá zařízení přidat na seznam povolených nebo blokovaných, což umožňuje úplnou kontrolu přístupu zařízení.

-   **Sledování operací připojení zařízení**. O událostech připojení zařízení a operacích správy identity zařízení můžete dostávat podrobné protokoly operací. Tato schopnost sledování vašemu řešení IoT umožní identifikovat problémy s připojením, například zařízení, která se snaží připojit pomocí nesprávných přihlašovacích údajů nebo posílat zprávy příliš často, nebo zamítnout všechny zprávy typu cloud-zařízení.

-   **Rozsáhlá sada knihoven zařízení**. [Sady SDK pro zařízení Azure IoT][lnk-device-sdks] jsou dostupné a podporované pro různé jazyky a platformy – pro řadu distribucí systému Linux, systém Windows a operační systémy v reálném čase. Sady SDK pro zařízení Azure IoT také podporují spravované jazyky,  jako je C#, Java či JavaScript.

-   **Protokoly a rozšiřitelnost IoT**. Pokud vaše řešení nemůže používat knihovny zařízení, služba IoT Hub zpřístupní veřejný protokol, který zařízením  umožní nativně používat protokoly MQTT v3.1.1, HTTP 1.1 nebo AMQP 1.0. Službu IoT Hub můžete také rozšířit tak, aby podporovala vlastní protokoly. Provedete to následovně:

    - Vytvořením brány pole pomocí [sady SDK brány Azure IoT][lnk-gateway-sdk], která převede váš vlastní protokol na jeden ze tří protokolů podporovaných službou IoT Hub. 
    - Přizpůsobením [brány protokolu Azure IoT][protocol-gateway], komponenty s otevřeným zdrojovým kódem, která se spouští v cloudu.

-   **Škálování**. Službu Azure IoT Hub lze škálovat na miliony současně připojených zařízení a miliony událostí za sekundu.

To představuje obecné výhody pro řadu komunikačních schémat. Služba IoT Hub aktuálně umožňuje implementovat následující konkrétní komunikační schémata:

-   **Ingestování zpráv typu zařízení-cloud na základě událostí.** Služba IoT Hub dokáže z vašich zařízení spolehlivě přijímat miliony událostí za sekundu. Poté je může zpracovat na aktivní trase pomocí modulu zpracovatele událostí. Nebo je také může uložit na neaktivní trase pro účely analýzy. Služba IoT Hub uchovává data události až sedm dní, díky čemuž zaručuje spolehlivé zpracování a vyrovná se se zatížením ve špičkách.

-   **Spolehlivé zasílání zpráv typu cloud-zařízení (nebo *příkazů*).** Back-end řešení dokáže pomocí služby IoT Hub do jednotlivých zařízení posílat zprávy se zárukou nejméně jednoho doručení. Každá zpráva má vlastní nastavení Time to Live a back-end může požadovat doklad o doručení i o vypršení platnosti. Tyto doklady zajišťují úplný přehled o životním cyklu zprávy typu cloud-zařízení. Poté můžete implementovat obchodní logiku zahrnující operace, které jsou spuštěné v zařízeních.

-   **Nahrávání souborů a dat uložených v mezipaměti snímače do cloudu.** Vaše zařízení mohou nahrávat soubory do služby Azure Storage pomocí identifikátorů URI SAS, které pro vás spravuje služba IoT Hub. Služba IoT Hub může po doručení souborů do cloudu generovat upozornění a umožnit tak back-endu jejich zpracování.

## Brány

Brána je v řešení IoT obvykle buď [brána protokolu][lnk-gateway], která je nasazena v cloudu, nebo [brána pole][lnk-field-gateway], která je nasazena místně ve vašich zařízeních. Brána protokolu provádí převody protokolů, například MQTT do AMQP. Brána pole může spouštět hraniční analýzu, provádět rozhodnutí v časovém rámci ke zkrácení latence, poskytovat zařízením služby správy, vynucovat bezpečnostní omezení a omezení na ochranu soukromí a také převádět protokoly. Oba typy brány fungují jako prostředníci mezi vaším zařízením a službou IoT Hub.

Brána pole se od jednoduchého zařízení pro směrování provozu (například zařízení pro překládání adres nebo brány firewall) liší, protože při správě přístupu a informačního toku ve vašem řešení obvykle hraje aktivní roli.

Řešení může zahrnovat brány protokolu i pole.

## Jak služba IoT Hub funguje?

Služba Azure IoT Hub implementuje schéma [komunikace s asistencí služby][lnk-service-assisted-pattern], které zprostředkovává komunikaci mezi zařízeními a back-endem řešení. Komunikace s asistencí služby má za cíl navázat důvěryhodné, obousměrné komunikační trasy mezi řídicím systémem, například službou IoT Hub, a zařízeními pro zvláštní účely, která jsou nasazena v nedůvěryhodném fyzickém umístění. Schéma zavádí následující zásady:

- Zabezpečení má přednost před všemi dalšími možnostmi.
- Zařízení nepřijímají nevyžádané informace o síti. Zařízení navazuje všechna připojení a vytváří trasy jako pouze odchozí. Aby zařízení mohlo přijmout příkaz z back-endu, musí pravidelně inicializovat připojení a kontrolovat případné čekající příkazy ke zpracování.
- Zařízení by se měla připojovat pouze k dobře známým službám (nebo k nim vytvářet trasu), se kterými mají partnerský vztah, například ke službě IoT Hub.
- Komunikační trasa mezi zařízením a službou nebo mezi zařízením a bránou je zabezpečená na úrovni aplikačního protokolu.
- Autorizace a ověření na úrovni systému jsou založeny na identifikaci jednotlivých zařízení. Díky tomu jsou přístupové údaje a povolení téměř okamžitě odvolatelné.
- Obousměrná komunikace se v případě zařízení, která se připojují pouze občasně z důvodu problémů s napájením nebo připojením, zjednodušuje tím, že se příkazy a oznámení zařízením pozdržují do doby, než se zařízení připojí a bude je moci přijmout. Služba IoT Hub pro odesílané příkazy udržuje fronty pro konkrétní zařízení.
- Datové části aplikací se zabezpečují samostatně za účelem chráněného přenosu dat prostřednictvím bran do konkrétní služby.

Mobilní průmysl v obrovském měřítku používá schéma komunikace s asistencí služby k implementaci služeb nabízených oznámení, například [služeb nabízených oznámení Windows][lnk-wns], [služby GCM (Google Cloud Messaging)][lnk-google-messaging] nebo [služby Apple Push Notification Service][lnk-apple-push].

## Další kroky

V článku [Přehled správy zařízení ve službě Azure IoT Hub][lnk-device-management] zjistíte, jak Azure IoT Hub umožňuje pomocí správy zařízení IoT založené na standardech provádět vzdálenou správu, konfiguraci a aktualizaci vašich zařízení.

K implementaci klientských aplikací v celé řadě hardwarových platforem a operačních systémů zařízení můžete použít sady SDK zařízení IoT. Sady SDK zařízení IoT zahrnují knihovny, které usnadňují odesílání telemetrických dat do služby IoT Hub a příjem příkazů typu cloud-zařízení. Při používání sad SDK si ke komunikaci se službou IoT Hub můžete vybrat z řady síťových protokolů. Další informace naleznete v tématu [informace sadách SDK pro zařízení][lnk-device-sdks].

Pokud chcete začít s psaním kódu a spouštěním ukázek, přečtěte si kurz [Začínáme se službu IoT Hub][lnk-get-started].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-d2c]: iot-hub-csharp-csharp-process-d2c.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Komunikace s asistencí služby), příspěvek na blogu od Clemense Vasterse"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-guidance.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide.md#identityregistry
[lnk-devguide-security]: iot-hub-devguide.md#security
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=ago16_HO5-->


