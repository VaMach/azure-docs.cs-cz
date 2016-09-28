<properties
 pageTitle="Začínáme s předkonfigurovanými řešeními Azure IoT | Microsoft Azure"
 description="Popis předkonfigurovaných řešení Azure IoT a jejich architektury spolu s odkazy na další prostředky"
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>


# Co jsou předkonfigurovaná řešení Azure IoT Suite?

Předkonfigurovaná řešení Azure IoT Suite jsou implementací běžných vzorů řešení IoT, která můžete nasadit v Azure pomocí svého předplatného. Předkonfigurovaná řešení můžete použít následovně:

- Jako výchozí bod pro své vlastní řešení IoT.
- K získání informací o běžných vzorech v designu a vývoji řešení IoT.

Každé předkonfigurované řešení je kompletní a komplexní implementace, která využívá telemetrická simulovaná zařízení ke generování telemetrie.

Kromě nasazení a spuštění řešení v Azure si můžete stáhnout kompletní zdrojový kód a upravit a rozšířit řešení tak, aby odpovídalo vašim specifickým požadavkům na IoT.

> [AZURE.NOTE] Pokud chcete  nasadit některé z předkonfigurovaných řešení, navštivte [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Další informace, jak nasadit a spustit jedno z řešení, najdete v článku [Začínáme s předkonfigurovanými řešeními IoT][lnk-getstarted-preconfigured].

Následující tabulka ukazuje, jak se řešení mapují na určité funkce IoT.

| Řešení | Přijímání dat | Identita zařízení | Příkazy a ovládání | Pravidla a akce | Prediktivní analýza |
|------------------------|-----|-----|-----|-----|-----|
| [Vzdálené sledování][lnk-getstarted-preconfigured] | Ano | Ano | Ano | Ano | -   |
| [Prediktivní údržba][lnk-predictive-maintenance] | Ano | Ano | Ano | Ano | Ano |

- *Přijímání dat*: Přijímání škálovaných dat do cloudu.
- *Identita zařízení*: Úprava jedinečné identity každého připojeného zařízení.
- *Příkazy a ovládání*: Odesílání zpráv z cloudu do zařízení tak, aby zařízení provedlo určitou akci.
- *Pravidla a akce*: Back-end řešení používá pravidla, podle kterých pracuje na určitých datech typu zařízení-cloud.
- *Prediktivní analýza*: Back-end řešení analyzuje data typu zařízení-cloud a z nich pak předpovídá, kdy by mělo dojít k určitým událostem. Například může analyzovat telemetrická data motoru letadla a určit, kdy bude zapotřebí provést údržbu.

## Přehled předkonfigurovaného řešení vzdáleného monitorování

V tomto článku se budeme zabývat předkonfigurovaným řešením vzdáleného monitorování, protože ilustruje mnoho běžných prvků návrhu, které sdílejí i jiná řešení.

Následující diagram ilustruje klíčové prvky řešení vzdáleného monitorování. Následující odstavce poskytují další informace o těchto prvcích.

![Architektura předkonfigurovaného řešení vzdáleného monitorování][img-remote-monitoring-arch]

## Zařízení

Když nasadíte předkonfigurované řešení vzdáleného monitorování, bude obsahovat čtyři předem zřízená simulovaná zařízení, simulující chladicí zařízení. Tato simulovaná zařízení mají předdefinovaný model teploty a vlhkosti, který vysílá telemetrická data. Tato simulovaná zařízení jsou připojena, aby ilustrovala komplexní tok dat v řešení a poskytovala praktický zdroj telemetrický dat a cíl pro příkazy. To jistě ocení vývojáři back-endů používající řešení jako výchozí bod pro vlastní implementaci.

Když se zařízení poprvé připojí na IoT Hub v rámci předkonfigurovaného řešení vzdáleného monitorování, odešle do centra IoT informační zprávu. V ní bude uveden seznam příkazů, na které zařízení může reagovat. Příkazy pro předkonfigurované řešení vzdáleného monitorování jsou následující: 

- *Ping Device* (Odeslat do zařízení příkaz ping): Zařízení na tento příkaz odpoví potvrzením. Tímto způsobem snadno zjistíte, zda je zařízení stále aktivní a naslouchá.
- *Start Telemetry* (Spustit telemetrii): Dá zařízení pokyn, aby začalo odesílat telemetrii.
- *Stop Telemetry* (Zastavit telemetrii): Dá zařízení pokyn, aby přestalo odesílat telemetrii.
- *Change Set Point Temperature* (Změnit nastavenou teplotu): Ovládá hodnoty simulované teplotní telemetrie, které zařízení odesílá. Tímto způsobem snadno otestujete logiku back-endu.
- *Diagnostic Telemetry* (Diagnostická telemetrie): Ovládá, zda má zařízení odesílat externí teplotu jako telemetrický údaj.
- *Change Device State* (Změnit stav zařízení): Nastaví vlastnosti metadat o stavu zařízení, která zařízení odesílá. Tímto způsobem snadno otestujete logiku back-endu.

Do řešení můžete přidat další simulovaná zařízení, která vysílají stejná telemetrická data a odpovídají na stejné příkazy. 

## IoT Hub

V tomto předkonfigurovaném řešení odpovídá instance IoT Hubu *cloudové bráně* v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

Centrum IoT získává telemetrická data ze zařízení v jednom koncovém bodě. Centrum IoT také udržuje pro každé zařízení specifické koncové body, odkud zařízení získávají příkazy, které do nich byly zaslány.

Centrum IoT zpřístupňuje získaná telemetrická data přes koncový bod čtení telemetrických dat na straně služby.

## Azure Stream Analytics

Předkonfigurované řešení používá tři úlohy [Azure Stream Analytics][lnk-asa] (ASA), jejichž pomocí filtruje datový proud telemetrie ze zařízení:


- *Úloha DeviceInfo (Informace o zařízení)* – Odesílá výstupní data do centra událostí, které do registru zařízení pro řešení směruje zprávy specifické pro registraci. Ty se ve chvíli, kdy se zařízení poprvé připojí nebo odpoví na příkaz **Change Device State**, odesílají do registru zařízení řešení (databáze DocumentDB). 
- *Úloha Telemetry (Telemetrie)* – Odesílá veškerá nezpracovaná telemetrická data do služby Azure Blob Storage, uloží málo používaná data a vypočítá ze získaných telemetrických dat agregované hodnoty, které se zobrazují na řídicím panelu řešení.
- *Úloha Rules (Pravidla)* – Filtruje datový proud telemetrie podle hodnot, které přesahují stanovené mezní hodnoty pravidel. Výstupní data odesílá do centra událostí. Jakmile se spustí pravidlo, na řídicím panelu portálu řešení se tato událost objeví jako nový řádek v tabulce historie alarmů. Také spustí akci založenou na nastaveních definovaných v sekcích portálu Pravidla a akce.

V tomto předkonfigurovaném řešení jsou úlohy ASA součástí **back-endu řešení IoT** v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

## Procesor událostí

V tomto předkonfigurovaném řešení je procesor událostí součástí **back-endu IoT řešení** v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

Úlohy ASA **DeviceInfo** a **Rules** odesílají výstup do center událostí, odkud se přeposílají do dalších služeb back-endu. Toto řešení využívá instanci [EventPocessorHost][lnk-event-processor] spuštěnou v rámci [webové úlohy][lnk-web-job] ke čtení zpráv z těchto center událostí. **EventProcessorHost** používá data úlohy **DeviceInfo**, která slouží k aktualizaci dat zařízení v databázi DocumentDB. Dále používá data úlohy **Rules**, přes která vyvolává aplikaci logiky a aktualizuje zobrazování upozornění na portálu řešení.

## Registr identity zařízení a databáze DocumentDB

Každé centrum IoT obsahuje [registr identity zařízení][lnk-identity-registry], který ukládá klíče zařízení. IoT Hub používá tuto informaci k ověřování zařízení – pokud se má zařízení připojit k centru, musí být registrováno a mít platný klíč.

Toto řešení ukládá další informace o zařízení, například stav, příkazy, které zařízení podporují, a další metadata. Řešení používá databázi DocumentDB, do které ukládá data zařízení, specifická pro toto řešení. Portál řešení pak z této databáze data získává, aby je bylo možné zobrazit a editovat.

Řešení musí také udržovat informace o identitě registru zařízení stále synchronizované s obsahem databáze DocumentDB. Příkaz **EventProcessorHost** používá k řízení synchronizace data z úlohy analýzy datového proudu **DeviceInfo**.

## Portál řešení

![Řídicí panel řešení][img-dashboard]

Portál řešení je uživatelské rozhraní, které je založené na webu a nasazené v cloudu jako součást předkonfigurovaného řešení. Umožňuje:

- Zobrazovat historii telemetrie a alarmů na řídicím panelu.
- Zřizovat nová zařízení.
- Spravovat a sledovat zařízení.
- Odesílat příkazy na konkrétní zařízení.
- Upravovat pravidla a akce.

V tomto předkonfigurovaném řešení je portál řešení součástí **back-endu řešení IoT** a součástí **připojení pro zpracování a obchodní logiku** v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

## Další kroky

Další informace o struktuře řešení IoT najdete v článku [Služby Microsoft Azure IoT: Referenční architektura][lnk-refarch].

Nyní víte, co je to předem nakonfigurované řešení a můžete začít nasazením *vzdáleného sledování* předkonfigurovaného řešení: [Začínáme s předkonfigurovanými řešeními][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide.md#device-identity-registry
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md


<!--HONumber=Sep16_HO3-->


