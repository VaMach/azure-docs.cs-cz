---
title: "Azure Location Based Services – přehled | Dokumentace Microsoftu"
description: "Úvod do služeb Azure Location Based Services (Preview)"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: overview
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9b4b54c3a4cf0ed4350f570259f6997e4398682b
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Úvod do služeb Azure Location Based Services (Preview)
Azure Location Based Services je portfolio geoprostorových služeb, mezi které patří rozhraní API pro mapy, vyhledávání, trasy, provoz a časová pásma. Portfolio služeb odpovídajících standardu Azure OneAPI umožňuje používat známé nástroje pro vývojáře k rychlému vývoji a škálování komponent, které integrují informace o poloze do vašich řešení v Azure. Azure Location Based Services poskytuje vývojářům ze všech odvětví výkonné geoprostorové funkce společně s čerstvými mapovými daty, které jsou nezbytné k zajištění geografického kontextu pro webové a mobilní aplikace. Azure Location Based Services je sada rozhraní odpovídajících standardu Azure One API doplněná o webové řízení v JavaScriptu pro zajištění snadného, flexibilního a přenositelného vývoje napříč různými médii. 

Služby Azure Location Based Services se skládají z pěti primární služeb pro podporu aplikací v Azure, které vyžadují geografický kontext. Každá ze služeb je podrobně popsána níže.

**Render Service** – Vykreslovací služba je určena vývojářům, kteří potřebují vytvářet webové a mobilní mapové aplikace. Služba nabízí buď vysoce kvalitní rastrové podklady v 19 úrovních přiblížení, nebo plně přizpůsobitelné vektorové obrazy.

![Azure Location Based Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Route Service** – Služba generování tras využívá robustní výpočty na základě reálné geometrie a infrastruktury a trasy pro různé způsoby dopravy. Služba umožňuje vývojářům vypočítat trasy v několika různých režimech, např. auto, nákladní vůz, kolo nebo chůze, a také zohlednit různá další zadání, jako je aktuální provoz, omezení hmotnosti nebo doprava nebezpečných látek.

![Azure Location Based Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Search Service** – Vyhledávací služba umožňuje vývojářům vyhledávat adresy, místa, výpisy firem a další podle názvu nebo kategorie a jiných zeměpisných údajů. Vyhledávací služba může také [reverzně geokódovat](https://en.wikipedia.org/wiki/Reverse_geocoding) adresy a křižovatky na základě zeměpisných souřadnic. 

![Azure Location Based Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Time Zone Service** – Služba časových pásem umožňuje získávat aktuální, historické i budoucí informace související s časovými pásmy buď podle zeměpisné šířky a délky, nebo [ID organizace IANA](http://www.iana.org/). Služba časových pásem také umožňuje převod ID časových pásem Microsoft Windows na ID organizace IANA, zjištění rozdílu časového pásma od UTC a získání aktuálního času v zadaném časovém pásmu. Typická odpověď JSON na dotaz na službu časového pásma vypadá takto:

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

**Traffic Service** – Sada webových služeb určená pro vývojáře, kteří ve svých webových nebo mobilních aplikacích potřebují informace o provozu. Nabídka je rozdělená do následujících oblastí:
1. Aktuální provoz – poskytuje v reálném čase zjištěné rychlosti a doby průjezdu pro všechny klíčové silnice sítě, a 
2. Dopravní nehody – poskytuje přesné informace o zablokované dopravě a o nehodách na silniční síti.

![Azure Location Based Services Traffic](media/about-location-based-services/Introduction_Traffic.png)

Služby Azure Location Based Services jsou navržené pro mobilní aplikace a je možné na nich založit i víceplatformní aplikace, protože jejich programovací model je agnostický a podporuje výstup JSON prostřednictvím rozhraní REST API. Kromě toho Azure LBS nabízejí pohodlný javascriptový ovládací prvek mapy s jednoduchým programovacím modelem pro rychlý a snadný vývoj webových i mobilních aplikací. 

Služby Azure Location Based Services používá schéma ověřování na základě klíčů, takže přístup ke služby je otázkou přechodu na web [Azure Portal](http://portal.azure.com) a vytvoření účtu Azure Location Based Services. V účtu budou pro vás předem vygenerované dva klíče. Při zahájení integrace těchto polohových služeb do vaší aplikace můžete v požadavcích na službu Azure Location Based Services použít kterýkoli z obou klíčů.

Vytvořte si účet [Azure Location Based Services](http://aka.ms/azurelbsportal) ještě dnes!

## <a name="next-steps"></a>Další kroky

Nyní máte přehled o službě Azure Location Based Services (Preview). Dalším krokem je vyzkoušení ukázkové aplikace demonstrující služby Location Based Services a scénář vytvoření kompletní webové aplikace.

> [!div class="nextstepaction"]
> [Spusťte ukázku interaktivního hledání v mapě s použitím Azure Location Based Services (Preview)](quick-demo-map-app.md)
> [Vyhledat okolní body zájmu pomocí Azure Location Based Services](tutorial-search-location.md)