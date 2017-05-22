---
title: "Průvodce řešením propojené továrny v sadě Azure IoT Suite | Dokumentace Microsoftu"
description: "Popis předkonfigurovaného řešení Azure IoT pro propojenou továrnu a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: fd5e1083c65068c7f6b019838586b1bc7e37aa9f
ms.contentlocale: cs-cz
ms.lasthandoff: 05/11/2017


---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Průvodce předkonfigurovaným řešením propojené továrny

[Předkonfigurované řešení][lnk-preconfigured-solutions] sady IoT Suite pro propojenou továrnu je implementace komplexního průmyslového řešení, které:

* Se připojuje k simulovaným průmyslovým zařízením se servery OPC UA na simulovaných výrobních linkách i ke skutečným zařízením serveru OPC UA.
* Ukazuje klíčové ukazatele výkonu a celkovou efektivitu těchto zařízení a výrobních linek.
* Ukazuje možnosti použití cloudové aplikace k interakci se serverovými systémy OPC UA.
* Umožňuje připojení vlastních zařízení serveru OPC UA.
* Umožňuje procházení a úpravy dat na serveru OPC UA.
* Se integruje se službou Azure Time Series Insights (TSI), aby mohlo poskytovat přizpůsobená zobrazení dat ze serverů OPC UA.

Můžete ho použít jako výchozí bod pro vlastní implementaci a [přizpůsobit][lnk-customize] si ho tak, aby splňovalo vaše konkrétní obchodní požadavky.

Tento článek vás provede některými z klíčových prvků řešení propojené továrny, aby vám pomohl pochopit, jak toto řešení funguje. Díky tomu budete moct:

* Odstraňovat potíže v řešení.
* Naplánujte, jak řešení přizpůsobit podle konkrétních požadavků.
* Navrhněte vlastní řešení IoT, které používá služby Azure.

## <a name="logical-architecture"></a>Logická architektura

Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:

![Logická architektura propojené továrny][connected-factory-logical]

## <a name="simulation"></a>Simulace

Simulované stanice a systémy řízení výroby tvoří výrobní linku továrny. Simulovaná zařízení a modul vydavatele OPC jsou založeny na [standardu OPC UA .NET][lnk-OPC-UA-NET-Standard] vydaném nadací OPC Foundation.

Proxy server OPC a vydavatel OPC jsou implementovány jako moduly založené na [Azure IoT Edge][lnk-Azure-IoT-Gateway]. Každá simulovaná výrobní linka má připojenu vyhrazenou bránu.

Všechny simulované komponenty jsou spuštěné v kontejnerech Dockeru hostovaných na virtuálních počítačích Azure s Linuxem. Simulace je ve výchozím nastavení nakonfigurovaná tak, aby bylo spuštěno 8 simulovaných výrobních linek.

## <a name="simulated-production-line"></a>Simulovaná výrobní linka

Výrobní linka vyrábí součásti. Skládá se z různých stanic: montážní stanice, testovací stanice a balicí stanice.

Simulace zpracovává a aktualizuje data vystavená prostřednictvím uzlů OPC UA. Všechny stanice simulované výrobní linky jsou orchestrované systémem řízení výroby (MES) prostřednictvím OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>Simulovaný systém řízení výroby

Systém řízení výroby monitoruje všechny stanice na výrobní lince prostřednictvím OPC UA a zjišťuje tak změny stavu stanic. Voláním metod OPC UA řídí stanice a předává produkt z jedné stanice do další, dokud se proces nedokončí.

## <a name="gateway-opc-publisher-module"></a>Modul vydavatele brány OPC

Modul vydavatele OPC se připojuje ke staničním serverům OPC UA a přihlašuje se k odběru uzlů OPC, které se zřídí. Modul převádí data z uzlu do formátu JSON, šifruje je a odesílá je do služby IoT Hub jako zprávy publikování a odběru OPC UA.

Modul vydavatele OPC vyžaduje pouze výchozí port HTTPS (443) a může fungovat se stávající podnikovou infrastrukturou.

## <a name="gateway-opc-proxy-module"></a>Modul proxy serveru brány OPC

Modul proxy serveru brány OPC UA tuneluje binární příkazy OPC UA a řídicí zprávy a vyžaduje pouze výchozí port HTTPS (443). Může fungovat s existující podnikovou infrastrukturou, včetně webových proxy serverů.

Pomocí metod zařízení ve službě IoT Hub přenáší do balíčků zabalená data protokolu TCP/IP na úrovni aplikace a tak zajišťuje zabezpečení koncových bodů, šifrování dat a integritu pomocí protokolu SSL/TLS.

Samotný binární protokol OPC UA předávaný přes proxy server používá ověřování a šifrování pomocí UA.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Modul vydavatele brány OPC se přihlašuje k odběru uzlů serveru OPC UA, aby mohl zjišťovat změny hodnot dat. Pokud se v některém z uzlů zjistí změna dat, modul odešle zprávy do služby Azure IoT Hub.

Služba IoT Hub poskytuje zdroj událostí pro službu Azure TSI. TSI data ukládá po dobu 30 dní na základě časových razítek připojených ke zprávám. Mezi tato data patří:

* Identifikátor OPC UA ApplicationUri
* Identifikátor OPC UA NodeId
* Hodnota uzlu
* Časové razítko zdroje
* Název OPC UA DisplayName

TSI aktuálně neumožňuje změnu doby, po kterou se data mají uchovat.

Dotazy služby TSI na data uzlů se provádí pomocí SearchSpan (Time.From a Time.To) a agregace podle identifikátoru OPC UA ApplicationUri nebo OPC UA NodeId nebo názvu OPC UA DisplayName.

Při načítání dat pro měřidla celkové efektivity zařízení, klíčových ukazatelů výkonu a grafů časových řad se data agregují podle počtu událostí, celkového součtu, průměrného součtu, minima a maxima.

Časové řady se sestavují jiným způsobem. Celková efektivita zařízení a klíčové ukazatele výkonu se počítají ze základních dat stanic a pronikají do topologie (výrobní linky, továrny, podnik) aplikace.

Kromě toho se časové řady pro celkovou efektivitu zařízení a klíčové ukazatele výkonu počítají v aplikaci kdykoli, kdy je připravený zobrazený časový rozsah. Například zobrazení dne se aktualizuje každou celou hodinu.

Zobrazení časových řad dat uzlu přichází přímo z TSI a používá agregaci časového rozsahu.

## <a name="iot-hub"></a>IoT Hub
Služba [IoT Hub][lnk-IoT Hub] přijímá data odesílaná z modulu vydavatele OPC do cloudu a zpřístupňuje je službě Azure TSI. 

Služba IoT Hub v tomto řešení také:
- Udržuje registr identit, ve kterém jsou uložena ID všech modulů vydavatele OPC a všech modulů proxy serveru OPC.
- Pro obousměrnou komunikaci modulu proxy serveru OPC používá přenosový kanál.

## <a name="azure-storage"></a>Azure Storage
Řešení používá jako diskové úložiště pro virtuální počítač a k ukládání dat nasazení službu Azure Blob Storage.

## <a name="web-app"></a>Webová aplikace
Webová aplikace nasazená jako součást předkonfigurovaného řešení se skládá z integrovaného klienta OPC UA, zpracování upozornění a vizualizace telemetrie.

## <a name="next-steps"></a>Další kroky

Další informace o sadě IoT Suite najdete v následujících článcích:

* [Oprávnění na webu azureiotsuite.com][lnk-permissions]

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/azure-iot-gateway-sdk
[lnk-permissions]: iot-suite-permissions.md
