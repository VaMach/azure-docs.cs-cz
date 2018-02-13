---
title: "Přehled služby Azure IoT Hub | Dokumentace Microsoftu"
description: "Přehled služby Azure IoT Hub: co je služba IoT Hub, připojení zařízení, schémata komunikace s internetem věcí, brány a schéma komunikace s asistencí služby"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b00c89183ff0d4e7df49d29834508643e68246bb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="overview-of-the-azure-iot-hub-service"></a>Přehled služby Azure IoT Hub

Vítá vás služba Azure IoT Hub. Tento článek obsahuje přehled služby Azure IoT Hub a popisuje důvody implementace řešení internetu věcí (IoT) pomocí této služby. Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a back-endem řešení. Azure IoT Hub:

* Poskytuje několik možností komunikace typu zařízení-cloud a cloud-zařízení. Mezi tyto možnosti patří jednosměrné zasílání zpráv, přenos souborů a metody požadavek-odpověď.
* Poskytuje integrované deklarativní směrování zpráv do dalších služeb Azure.
* Poskytuje dotazovatelné úložiště metadat zařízení a synchronizované stavové informace.
* Umožňuje zabezpečit komunikaci a řízení přístupu pomocí klíčů specifických pro jednotlivá zařízení a certifikátů X.509.
* Zajišťuje rozsáhlé monitorování událostí připojení zařízení a správy identity zařízení.
* Zahrnuje knihovny zařízení pro nejoblíbenější jazyky a platformy.

Článek [Srovnání služeb IoT Hub a Event Hubs][lnk-compare] popisuje klíčové rozdíly mezi těmito dvěma službami a zdůrazňuje výhody používání služby IoT Hub ve vašich řešeních pro IoT.

Další informace o tom, jak Azure a IoT Hub pomáhají se zabezpečením řešení IoT, najdete v tématu [Zabezpečení internetu věcí od počátku][lnk-security-ground-up].

![Služba Azure IoT Hub jako cloudová brána v řešení internetu věcí][img-architecture]

> [!NOTE]
> Podrobné informace o architektuře IoT najdete v článku [Referenční architektura IoT v systému Microsoft Azure][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Výzvy spojené s připojením zařízení IoT

Služba IoT Hub a knihovny zařízení vám pomohou čelit výzvám, které představuje spolehlivé a bezpečné připojení zařízení k back-endu řešení. Zařízení IoT:

* Jsou často vestavěnými systémy bez lidské obsluhy.
* Můžou se nacházet ve vzdálených umístěních, kam je fyzický přístup nákladný.
* Můžou být dostupná jenom prostřednictvím back-endu řešení.
* Můžou mít omezené prostředky pro napájení a zpracování.
* Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
* Můžou potřebovat chráněné, vlastní nebo průmyslové protokoly aplikací.
* Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků musí jakékoli řešení IoT zajistit také škálování, zabezpečení a spolehlivost. Výslednou sadu požadavků na připojení je obtížné implementovat pomocí tradičních technologií, jakými jsou webové kontejnery a zprostředkovatelé zasílání zpráv.

## <a name="why-use-azure-iot-hub"></a>Proč používat Azure IoT Hub?

Azure IoT Hub nabízí bohatou sadu možností komunikace typu [zařízení-cloud][lnk-d2c-guidance] a [cloud-zařízení][lnk-c2d-guidance]. Kromě toho Azure IoT Hub řeší výzvy související se spolehlivým a zabezpečeným připojováním k zařízením, a to následujícími způsoby:

* **Dvojče zařízení**. Pomocí [dvojčat zařízení][lnk-twins] můžete ukládat, synchronizovat a dotazovat se na metadata a stav zařízení. Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení, například metadata, konfigurace a podmínky. IoT Hub udržuje takové dvojče pro každé zařízení, které ke službě IoT Hub připojíte.

* **Ověřování podle zařízení a zabezpečené připojení**. Každému zařízení můžete zřídit vlastní [klíč zabezpečení][lnk-devguide-security], který umožňuje připojení ke službě IoT Hub. [Registr identit služby IoT Hub][lnk-devguide-identityregistry] uchovává identity a klíče zařízení. Back-end řešení může jednotlivá zařízení přidat na seznam povolených nebo blokovaných, což umožňuje úplnou kontrolu přístupu zařízení.

* **Směrování zpráv typu zařízení-cloud do služeb Azure na základě deklarativních pravidel**. IoT Hub vám umožňuje definovat trasy zpráv na základě pravidel směrování, díky čemuž máte kontrolu nad tím, kam služba IoT Hub odesílá zprávy typu zařízení-cloud. Pravidla směrování nevyžadují psaní kódu a mohou nahradit vlastní dispečery zpráv po ingestování.

* **Integrace událostí IoT Hub do podnikových aplikací**. IoT Hub se integruje se službou Azure Event Grid. Pomocí této integrace můžete nakonfigurovat další služby Azure nebo aplikace jiných výrobců tak, aby naslouchaly událostem IoT Hubu. Azure Event Grid umožňuje rychle reagovat na kritické události, a to spolehlivým, škálovatelným a zabezpečeným způsobem.

* **Sledování operací připojení zařízení**. O událostech připojení zařízení a operacích správy identity zařízení můžete dostávat podrobné protokoly operací. Tato schopnost monitorování vašemu řešení IoT umožní identifikovat problémy s připojením. Pomocí těchto protokolů můžete identifikovat zařízení, která uvádějí nesprávné přihlašovací údaje, posílají zprávy příliš často nebo zamítají všechny zprávy typu cloud-zařízení.

* **Rozsáhlá sada knihoven zařízení**. [Sady SDK pro zařízení Azure IoT][lnk-device-sdks] jsou dostupné a podporované pro různé jazyky a platformy – pro řadu distribucí systému Linux, systém Windows a operační systémy v reálném čase. Sady SDK pro zařízení Azure IoT také podporují spravované jazyky,  jako je C#, Java či JavaScript.

* **Protokoly a rozšiřitelnost IoT**. Pokud vaše řešení nemůže používat knihovny zařízení, služba IoT Hub zpřístupní veřejný protokol, který zařízením umožní nativně používat protokoly MQTT v3.1.1, HTTPS 1.1 nebo AMQP 1.0. Službu IoT Hub můžete také rozšířit tak, aby podporovala vlastní protokoly. Provedete to následovně:

  * Vytvořením brány pole pomocí služby [Azure IoT Edge][lnk-iot-edge], která převede váš vlastní protokol na protokol podporovaný službou IoT Hub.
  * Přizpůsobením [protokolové brány Azure IoT][protocol-gateway], opensourcové komponenty, která běží v cloudu.

* **Škálování**. Službu Azure IoT Hub lze škálovat na miliony současně připojených zařízení a miliony událostí za sekundu.

* **Zřizování zařízení**. Služba [IoT Hub Device Provisioning](https://docs.microsoft.com/azure/iot-dps/) je pomocná služba pro IoT Hub. Umožňuje plně automatizované zřizování zařízení za běhu ve správném IoT Hubu, které nevyžaduje zásah člověka. Můžete tak bezpečným a škálovatelným způsobem zřizovat miliony zařízení.

## <a name="gateways"></a>Brány

Brána je v řešení IoT obvykle buď [brána protokolu][lnk-iotedge] nasazená v cloudu, nebo [brána pole][lnk-field-gateway] nasazená místně ve vašich zařízeních.

_Protokolová brána_ provádí převody protokolů, například MQTT do AMQP.

_Hraniční brána_ může:

* Spouštět analýzy na hraničních zařízeních
* Provádět rozhodnutí v časovém rámci, která mohou zkrátit latenci
* Poskytovat zařízením služby správy
* Vynucovat bezpečnostní omezení a omezení na ochranu soukromí
* Převádět protokoly

Oba typy brány fungují jako prostředníci mezi vaším zařízením a službou IoT Hub.

Brána pole se od jednoduchého zařízení pro směrování provozu (například zařízení pro překládání adres nebo brány firewall) liší, protože při správě přístupu a informačního toku ve vašem řešení obvykle hraje aktivní roli.

Řešení může zahrnovat brány protokolu i pole.

## <a name="how-does-iot-hub-work"></a>Jak služba IoT Hub funguje?

Služba Azure IoT Hub implementuje schéma [komunikace s asistencí služby][lnk-service-assisted-pattern], které zprostředkovává komunikaci mezi zařízeními a back-endem řešení. Cílem tohoto schématu je navázat důvěryhodné, obousměrné komunikační trasy mezi řídicím systémem, například službou IoT Hub, a zařízeními pro zvláštní účely v nedůvěryhodném fyzickém umístění. Schéma zavádí následující zásady:

* Zabezpečení má přednost před všemi dalšími možnostmi.

* Zařízení nepřijímají nevyžádané informace o síti. Zařízení navazuje všechna připojení a vytváří trasy jako pouze odchozí. Aby zařízení mohlo přijmout příkaz z back-endu řešení, musí pravidelně inicializovat připojení a kontrolovat případné čekající příkazy ke zpracování.

* Zařízení by se měla připojovat pouze k dobře známým službám (nebo k nim vytvářet trasu), se kterými mají partnerský vztah, například ke službě IoT Hub.

* Komunikační trasa mezi zařízením a službou nebo bránou je zabezpečená na úrovni aplikačního protokolu.

* Autorizace a ověření na úrovni systému jsou založeny na identifikaci jednotlivých zařízení. Díky tomu jsou přístupové údaje a povolení téměř okamžitě odvolatelné.

* V případě zařízení, která se připojují pouze občasně z důvodu problémů s napájením nebo připojením, funguje obousměrná komunikace tak, že příkazy a oznámení pozdrží do doby, než se zařízení připojí a bude je moci přijmout. Služba IoT Hub pro odesílané příkazy udržuje fronty pro konkrétní zařízení.

* Datové části aplikací se zabezpečují samostatně za účelem chráněného přenosu dat prostřednictvím bran do konkrétní služby.

Mobilní průmysl používá schéma komunikace s asistencí služby k implementaci služeb nabízených oznámení, jako jsou například [Služby nabízených oznámení Windows][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] nebo [Apple Push Notification Service][lnk-apple-push].

IoT Hub se podporuje přes cestu veřejného partnerského vztahu ExpressRoute.

## <a name="next-steps"></a>Další kroky

Pokud chcete začít s psaním kódu a spouštěním ukázek, přečtěte si kurz [Začínáme se službou IoT Hub][lnk-get-started].

Informace o tom, jak odesílat zprávy ze zařízení a přijímat je ze služby IoT Hub, a jak konfigurovat trasy zpráv, najdete v tématu [Odesílání a příjem zpráv pomocí služby IoT Hub][lnk-send-messages].

V tématu [Přehled správy zařízení ve službě IoT Hub][lnk-device-management] zjistíte, jak služba IoT Hub umožňuje pomocí správy zařízení založené na standardech provádět vzdálenou správu, konfiguraci a aktualizaci vašich zařízení.

K implementaci klientských aplikací v celé řadě hardwarových platforem a operačních systémů zařízení můžete použít sady SDK pro zařízení Azure IoT. Sady SDK pro zařízení zahrnují knihovny, které usnadňují odesílání telemetrických dat do služby IoT Hub a příjem zpráv typu cloud-zařízení. Při používání sad SDK pro zařízení si ke komunikaci se službou IoT Hub můžete vybrat z řady síťových protokolů. Další informace najdete v tématu [informace sadách SDK pro zařízení][lnk-device-sdks].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Komunikace s asistencí služby), příspěvek na blogu od Clemense Vasterse"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://docs.microsoft.com/azure/iot-edge/
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
