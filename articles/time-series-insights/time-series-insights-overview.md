---
title: "Přehled služby Azure Time Series Insights | Dokumentace Microsoftu"
description: "Seznámení s Azure Time Series Insights, novou službou pro analýzu dat časových řad a řešení IoT."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/20/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: cc35c00bec18ee1d9ed52b29b68ddacc1136dc70
ms.lasthandoff: 04/25/2017

---

# <a name="overview-of-azure-time-series-insights"></a>Přehled Azure Time Series Insights

Azure Time Series Insights je plně spravovaná cloudová služba, která díky komponentám pro úložiště, analýzu a vizualizaci výrazně zjednodušuje ingestování, ukládání, prozkoumávání a analýzu miliard událostí současně. Time Series Insights poskytuje globální přehled o datech a umožňuje rychle ověřit vaše řešení IoT a vyhnout se nákladným prostojům důležitých zařízení, protože pomáhá odhalovat skryté trendy a anomálie a provádět analýzy hlavních příčin téměř v reálném čase. Time Series Insights ingestuje data časových řad ze zprostředkovatelů událostí (např. služby IoT Hub nebo Event Hubs) a následně je indexuje a vyřazuje na základě konfigurovatelných zásad uchovávání informací. Uživatelé tato data využívají pro své podnikové scénáře přes intuitivní uživatelské rozhraní nebo prostřednictvím rozhraní REST API pro dotazy.

![Přehled služby Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primární scénáře

* Ověřování a monitorování řešení IoT během pár minut
* Intuitivní vizualizace a analýza dat IoT ve velkém měřítku
* Urychlení analýzy hlavních příčin a detekce anomálií
* Vytvoření globálního přehledu o více zařízeních, přístrojích a datech

## <a name="key-capabilities-and-benefits"></a>Klíčové funkce a výhody

* **Snadný začátek:** Služba Azure Time Series Insights nevyžaduje žádnou počáteční přípravu dat a je neuvěřitelně rychlá, takže se můžete připojit k miliardám událostí ve službě Azure IoT Hub nebo v centru událostí během pár minut. Po připojení můžete za pár sekund vizualizovat data ze senzorů, pracovat s nimi a rychle tak prověřit řešení IoT. Služba Time Series Insights je neuvěřitelně intuitivní a snadno použitelná – s daty můžete pracovat, aniž byste museli psát jediný řádek kódu.  Kromě toho se nemusíte učit žádný nový jazyk, protože Time Series Insights poskytuje plochu pro podrobné volnotextové dotazování pro pokročilé uživatele spolu s prozkoumáváním typu ukázat a kliknout pro nováčky.

* **Přehledy téměř v reálném čase během pár sekund:** Vytěžte z vašich dat časových řad vyšší hodnotu pomocí komponent služby Time Series Insights pro úložiště, analýzu a vizualizaci, to vše na jednom místě. Time Series Insights může ingestovat stovky milionů událostí senzorů za den s minutovou latencí, takže můžete rychle reagovat na změny. Time Series Insights pomáhá rychle zjišťovat trendy a anomálie a poskytuje tak podrobnější přehled o datech senzorů, díky čemuž umožňuje provádět komplexní analýzy hlavních příčin a vyhnout se nákladným prostojům. Díky povolení křížové korelace mezi historickými daty a daty v reálném čase umožňuje Time Series Insights uživatelům odhalit ve svých datech skryté trendy.

* **Sestavování vlastních řešení:** Zahrňte data Azure Time Series Insights do svých stávajících aplikací nebo vytvořte nová vlastní řešení s využitím rozhraní Time Series Insights REST API.  Vytvoření a sdílení přizpůsobených zobrazení je navíc hračka, takže ostatní můžou jednoduše prozkoumávat vaše objevy.

* **Škálovatelnost:** Služba Time Series Insights je navržena tak, aby podporovala škálování IoT. Ve verzi Preview může ingestovat 1 až 100 milionů událostí za den při výchozím rozsahu uchování 31 dnů. Time Series Insights umožňuje vizualizaci a analýzu živých datových proudů téměř v reálném čase spolu s obrovskými objemy historických dat. Míry uchovávání a příchozího přenosu dat se postupem času budou zvyšovat s ohledem na neustále se zvyšující velikost podniku.

## <a name="taxonomy-of-time-series-insights"></a>Taxonomie služby Time Series Insights

* **Prostředí:** Prostředí je prostředek Azure s kapacitou úložiště a příchozího přenosu dat.  Zákazníci zřizují prostředí s požadovanou kapacitou přes Azure Portal.
* **Zdroj událostí:** Zdroj událostí je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs.  Time Series Insights se připojuje přímo ke zdrojům událostí a ingestuje datový proud, aniž by uživatelé museli psát jediný řádek kódu. V současné době Time Series Insights podporuje služby Azure Event Hubs a Azure IoT Hub a v budoucnu budou přidány další zdroje událostí.
* **Referenční data:** Time Series Insights umožňuje spojit data časových řad s referenčními daty.  Referenčními daty můžou být metadata o zařízeních nebo jiná statická data, která se mění poměrně málo často. Time Series Insights spojí referenční data s datovými proudy a umožní tak vizualizaci a analýzu těchto dat téměř v reálném čase.

