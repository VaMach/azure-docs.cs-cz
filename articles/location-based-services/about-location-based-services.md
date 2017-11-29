---
title: "Přehled Azure umístění služby založené na | Microsoft Docs"
description: "Úvod do Azure na základě polohy (preview)"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: article
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c8ebce06a72bcaf769a11ec954702463d7489aa0
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Úvod do Azure na základě polohy (preview)
Služby Azure na základě umístění je portfolio geoprostorové služeb, které zahrnují API služby Maps, hledání, směrování, provozu a časová pásma. Celém portfoliu služby vyhovující Azure OneAPI umožňuje použití nástrojů pro vývojáře známé rychle vyvíjet a škálování řešení, které informace o umístění integrovat do řešení Azure. Služby Azure na základě umístění poskytuje vývojářům ze všech odvětví geoprostorové výkonné možnosti naplněná čerstvé mapování dat imperativní poskytovat geografické kontext, který má web a mobilní aplikace. Služby Azure na základě umístění je kompatibilní s sada Azure jedno rozhraní API REST API společně s webové ovládací prvek jazyka JavaScript, aby vývoj super snadno, flexibilní a přenosné napříč více médií. 

Služby Azure na základě umístění se skládá z pěti primární služeb účinnějšími Azure aplikací, které vyžadují geografické kontextu. Jednotlivé služby je vysvětleno v níže uvedené podrobnosti.

**Vykreslení služby** – služba vykreslení Service je určená pro vývojářům vytvářet webové a mobilní aplikace kolem mapování. Služba používá vysoce kvalitní rastrových obrázků grafické, k dispozici v 19 úrovně zvětšení nebo obrázky mapy plně přizpůsobitelná vektoru formátu.

![Umístění Azure na základě Map.png služby](media/about-location-based-services/Introduction_Map.png)

**Směrování služby** – funkce směrování Service je vytvořené s robustní infrastrukturu reálného geometrie výpočty a více pokynů režimu Transport. Služba umožňuje vývojářům vypočítat pokynů napříč několika režimy cesta například Auto náklaďák, jízdních kol nebo proti; a také, počet vstupů například provoz podmínky, váhy omezení nebo nebezpečných podstatným přenosu.

![Umístění Azure na základě Route.png služby](media/about-location-based-services/Introduction_Route.png)

**Služby vyhledávání** – funkce hledání Service je určený pro vývojáře k vyhledání adresy, míst, obchodní výpisech podle názvu nebo kategorie a jiné zeměpisné údaje. Služba vyhledávání můžete také [reverse geocode](https://en.wikipedia.org/wiki/Reverse_geocoding) adresy a křížové ulice podle zeměpisnou šířku a délku. 

![Umístění Azure na základě Search.png služby](media/about-location-based-services/Introduction_Search.png)

**Časové pásmo služby** – služba časové pásmo Service umožňuje informace aktuální, historické a budoucí časové pásmo dotazu pomocí buď páry zeměpisné šířky nebo [ID organizace IANA](http://www.iana.org/). Časové pásmo služby také umožňuje převod Microsoft Windows časové pásmo ID organizace IANA časových pásem, načítání posun časového pásma na čas UTC a získání aktuální čas v příslušných časové pásmo. Typické odpověď JSON pro dotaz na službu časové pásmo vypadá takto:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

**Provoz služby** – služba provoz Service je sada webových služeb, které jsou určené pro vývojářům vytvářet webové a mobilní aplikace, které potřebují provoz. Nabídku je rozdělená do následující:
1. Tok provozu – poskytuje v reálném čase zjištěnou rychlosti a cestují časy pro všechny klíče cest v síti; a, 
2. Provoz incidenty – poskytuje přesné informace o provoz zablokovaný a incidenty kolem silniční sítě.

![Umístění Azure na základě přenosů služeb](media/about-location-based-services/Introduction_Traffic.png)

Služby Azure na základě umístění je vytvořené pro nastavení mobilních zařízení a aplikací platformě můžete napájení, protože programovací model nerozlišuje a podporuje výstup JSON prostřednictvím rozhraní API REST. Kromě toho kg Azure nabízí pohodlný ovládací prvek Map JavaScript s jednoduchý programovací model pro rychlý a snadný vývoj webové a mobilní aplikace. 

Služby Azure na základě umístění používá schéma ověřování na základě klíčů, takže přístup služby je řádu přejdete na [portál Azure](http://portal.azure.com) a vytvoření účtu Azure zjišťování na základě polohy. Váš účet se dodává s dva klíče, které jsou pro vás předem vygeneroval. Spusťte integraci těchto možností umístění přímo do vaší aplikace pomocí některé z vašich klíčů v požadavcích na službu Azure zjišťování na základě polohy.

Zaregistrujte si [služeb na základě umístění Azure, účet ještě dnes!](http://aka.ms/azurelbsportal)

## <a name="next-steps"></a>Další kroky

Teď máte přehled Azure umístění na základě Services (preview). Dalším krokem je pro vyzkoušení ukázkovou aplikaci ji na základě polohy a vytváření scénářem začátku do konce ve webové aplikaci.

> [!div class="nextstepaction"]
> [Spustit ukázku interaktivní mapu vyhledávání pomocí Azure na základě polohy (preview)](quick-demo-map-app.md)
> [vyhledávání nedaleko bodu zájmu pomocí služeb na základě umístění Azure](tutorial-search-location.md)