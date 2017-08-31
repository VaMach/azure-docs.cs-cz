---
title: "Přehled služby Azure Time Series Insights | Dokumentace Microsoftu"
description: "Seznámení s Azure Time Series Insights, novou službou pro analýzu dat časových řad a řešení IoT"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: abd66208ab7ac30831f3f1eddb2891ed7bcd3995
ms.contentlocale: cs-cz
ms.lasthandoff: 07/21/2017

---

# <a name="what-is-azure-time-series-insights"></a>Co je Azure Time Series Insights

Azure Time Series Insights je spravovaná cloudová služba, která díky komponentám pro úložiště, analýzu a vizualizaci zjednodušuje ingestování, ukládání, prozkoumávání a analýzu miliard událostí současně. Time Series Insights poskytuje globální přehled o datech a umožňuje rychle ověřit vaše řešení IoT a vyhnout se nákladným prostojům zařízení, protože pomáhá odhalovat skryté trendy a anomálie a provádět analýzy hlavních příčin téměř v reálném čase. Time Series Insights ingestuje data časových řad ze zprostředkovatelů událostí (např. služby IoT Hub nebo Event Hubs) a následně je indexuje a vyřazuje na základě konfigurovatelných zásad uchovávání informací. Uživatelé tato data využívají prostřednictvím intuitivního uživatelského rozhraní nebo rozhraní REST API pro dotazy.

![Přehled služby Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primární scénáře

* Monitorování a ověření řešení IoT během několika minut.
* Vizualizace a analýza dat IoT ve velkém měřítku.
* Urychlení analýzy hlavních příčin a detekce anomálií.
* Vytvoření globálního přehledu o více zařízeních, přístrojích a datech.

## <a name="capabilities-and-benefits"></a>Funkce a výhody

* **Snadný začátek:** Služba Azure Time Series Insights nevyžaduje žádnou počáteční přípravu dat a je neuvěřitelně rychlá. Během několika minut se můžete připojit k miliardám událostí ve službě Azure IoT Hub nebo v centru událostí. Po připojení můžete za pár sekund vizualizovat data ze senzorů, pracovat s nimi a rychle tak prověřit řešení IoT. Služba Time Series Insights je snadno použitelná – s daty můžete pracovat, aniž byste museli psát jediný řádek kódu.  Nemusíte se učit žádný nový jazyk, protože Time Series Insights poskytuje plochu pro podrobné volnotextové dotazování pro pokročilé uživatele spolu s prozkoumáváním typu ukázat a kliknout pro všechny.

* **Přehledy téměř v reálném čase:** Time Series Insights může ingestovat stovky milionů událostí senzorů za den s minutovou latencí, takže můžete rychle reagovat na změny. Time Series Insights pomáhá získat podrobný přehled o datech senzorů tím, že pomáhá rychle zjišťovat trendy a anomálie, provádět komplexní analýzy hlavních příčin a vyhnout se nákladným prostojům. Díky povolení křížové korelace mezi historickými daty a daty v reálném čase pomáhá Time Series Insights odhalit skryté trendy v datech.

* **Sestavování vlastních řešení:** Zahrňte data Azure Time Series Insights do svých stávajících aplikací nebo vytvořte nová vlastní řešení s využitím rozhraní Time Series Insights REST API. Můžete vytvářet přizpůsobená zobrazení a sdílet je s ostatními, aby i oni mohli prozkoumat, co jste se objevili.

* **Škálovatelnost:** Služba Time Series Insights je navržena tak, aby podporovala škálování IoT. Ve verzi Preview může ingestovat 1 až 100 milionů událostí za den při výchozím rozsahu uchování 31 dnů. Můžete vizualizovat a analyzovat živé datové proudy téměř v reálném čase spolu s obrovskými objemy historických dat. Míry uchovávání a příchozího přenosu dat se postupem času budou zvyšovat s ohledem na neustále se zvyšující velikost podniku.

## <a name="time-series-insights-glossary"></a>Glosář Time Series Insights

* **Prostředí:** Prostředí je prostředek Azure s kapacitou úložiště a příchozího přenosu dat.  Zákazníci zřizují prostředí s požadovanou kapacitou přes Azure Portal.
* **Zdroj událostí:** Zdroj událostí je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs.  Time Series Insights se připojuje přímo ke zdrojům událostí a ingestuje datový proud bez nutnosti psát kód. V současné době Time Series Insights podporuje Azure Event Hubs a Azure IoT Hubs.
* **Referenční data:** Time Series Insights umožňuje spojit data časových řad s referenčními daty.  Referenčními daty můžou být metadata o zařízeních nebo jiná statická data, která se mění poměrně málo často. Time Series Insights spojí referenční data s datovými proudy a umožní tak vizualizaci a analýzu těchto dat téměř v reálném čase.

