<properties
    pageTitle="Přehled Microsoft Azure IoT Suite | Microsoft Azure"
    description="Přehled předkonfigurovaných řešení pro internet věcí v sadě Azure IoT Suite určených ke sběru, analyzování a uchovávání dat, vizualizacím a integraci s ostatními systémy."
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


# Co je Azure IoT Suite?

Služby v sadě Azure IoT Suite nabízejí celou řadu funkcí. Tyto služby na úrovni řešení pro velké firmy umožňují:

- sběr dat ze zařízení
- analýzu datových proudů za provozu
- ukládání objemných datových sad a dotazy na ně
- vizualizaci historických dat i dat v reálném čase
- integraci se systémy administrativní podpory (back-office)

K zajištění těchto funkcí kombinuje Azure IoT Suite více služeb Azure s vlastními rozšířeními v podobě *předkonfigurovaných řešení*. Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení v oblasti IoT, které by vám měly pomoci snížit časovou náročnost implementace vašich vlastních IoT řešení. Pomocí [sady SDK pro IoT][lnk-sdks] lze tato řešení přizpůsobit a rozšířit tak, aby vyhovovala vaším požadavkům. Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.

Následující video obsahuje úvod k Azure IoT Suite:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## Služby Azure IoT v Azure IoT Suite

Předkonfigurovaná řešení obvykle využívají následující služby:

- Základem Azure IoT Suite je služba [Azure IoT Hub][lnk-iot-hub]. Tato služba zajišťuje obousměrnou výměnu zpráv mezi zařízením a cloudem a funguje jako brána ke cloudu a ostatním klíčovým službám IoT Suite. Umožňuje příjem zpráv z vašich zařízení a odesílání příkazů do zařízení.

- Služba [Azure Stream Analytics][lnk-asa] umožňuje analyzovat data za provozu. Sada IoT Suite využívá tuto službu ke zpracování příchozích telemetrických dat, vytváření agregací a zjišťování událostí. Předkonfigurovaná řešení dále s využitím analýzy datových proudů zpracovávají informační zprávy, které obsahují například metadata nebo odezvy zařízení na příkazy. Řešení využívají službu Stream Analytics ke zpracování zpráv ze zařízení a přenosu těchto zpráv do dalších služeb.

- Služby [Azure Storage][lnk-azure-storage] a [Azure DocumentDB][lnk-document-db] zajišťují funkce pro ukládání dat. Tato předkonfigurovaná řešení využívají úložiště objektů blob k ukládání telemetrie a její zpřístupnění pro analýzu. Službu DocumentDB využívají k ukládání metadat ze zařízení a zpřístupnění funkcí pro správu zařízení v těchto řešeních.

- Funkce pro vizualizaci dat zajišťují služby [Azure Web Apps][lnk-web-apps] a [Microsoft Power BI][lnk-power-bi]. Díky flexibilitě služby Power BI si rychle sestavíte svůj vlastní interaktivní řídicí panel, který bude využívat data ze sady IoT Suite.

Informace o tom, jak vypadá architektura typického řešení IoT, naleznete v tématu [Microsoft Azure a Internet věcí (IoT)][iot-suite-what-is-azure-iot].

## Předkonfigurovaná řešení

Sada Azure IoT Suite zahrnuje předkonfigurovaná řešení, která vám umožňují rychle začít s běžnými scénáři implementace technologie IoT, jako je *Vzdálené sledování* a *Prediktivní údržba*, a objevit všechny jejich možnosti. Tato řešení můžete nasadit ve svém předplatném Azure a následně spustit kompletní scénář IoT od začátku do konce.

## Další kroky

Teď, když máte přehled o tom, co sada IoT Suite dokáže a jaké jsou její hlavní součásti, můžete v článku [Co jsou předkonfigurovaná řešení Azure IoT][lnk-what-are-preconfig] zjistit další informace o předkonfigurovaných řešeních v sadě IoT Suite.

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md



<!--HONumber=Sep16_HO3-->


