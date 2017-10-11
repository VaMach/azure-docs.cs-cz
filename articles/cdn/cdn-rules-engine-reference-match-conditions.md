---
title: "Azure CDN pravidla podmínky shody modul | Microsoft Docs"
description: "Referenční dokumentace pro Azure CDN pravidla shody stav motoru a funkce."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: f4886b1d78dfa87cf25737fb46c12b5963034f27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Stroj pravidel Azure CDN splňují podmínky
Toto téma obsahuje podrobný popis dostupných podmínek shodu pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Druhá část pravidla je podmínka shodu. Stav shody identifikuje konkrétní typy žádostí, pro které se provede sadu funkcí.

Například se může použít k filtrování požadavků na obsah v konkrétních místech, generovány z konkrétní IP adresu nebo země, nebo informace hlavičky žádosti.

## <a name="always"></a>Vždy

Stav shody vždy slouží ke použít výchozí sadu funkcí pro všechny požadavky.

## <a name="device"></a>Zařízení

Stav shody zařízení identifikuje požadavky z mobilního zařízení podle jeho vlastnosti.  Mobilní zařízení jsou detekovány prostřednictvím [WURFL](http://wurfl.sourceforge.net/).  Možnosti WURFL a jejich proměnné stroj pravidel CDN jsou uvedeny níže.

> [!NOTE] 
> V jsou podporovány následující proměnné **změnit hlavičky žádosti klienta** a **upravit hlavičku odpovědi klienta** funkce.

Schopnost | Proměnná | Popis | Ukázkové hodnoty
-----------|----------|-------------|----------------
Název značky | % {wurfl_cap_brand_name} | Řetězec, který označuje brand název zařízení. | Samsung
Operačního systému zařízení | % {wurfl_cap_device_os} | Řetězec, který označuje operační systém nainstalovaný na zařízení. | IOS
Verze operačního systému zařízení | % {wurfl_cap_device_os_version} | Řetězec, který označuje číslo verze operačního systému na zařízení nainstalovaná. | 1.0.1
Duální orientace | % {wurfl_cap_dual_orientation} | Logická hodnota, která určuje, zda je zařízení podporuje dva orientace. | Hodnota TRUE
Upřednostňovaný souboru DTD protokolu HTML | % {wurfl_cap_html_preferred_dtd} | Řetězec, který označuje definice typu upřednostňované dokumentu (DTD) mobilních zařízení pro obsah HTML. | None<br/>xhtml_basic<br/>HTML5
Vložené bitové kopie | % {wurfl_cap_image_inlining} | Logická hodnota, která určuje, zda je zařízení podporuje Base64 kódovaný bitové kopie. | False
Se systémem Android | % {wurfl_vcap_is_android} | Logická hodnota, která označuje, zda zařízení používá operační systém Android. | Hodnota TRUE
IOS | % {wurfl_vcap_is_ios} | Logická hodnota, která označuje, zda zařízení používá iOS. | False
Je inteligentní TV | % {wurfl_cap_is_smarttv} | Logická hodnota, která určuje, zda zařízení je inteligentní televize. | False
Je Smartphone | % {wurfl_vcap_is_smartphone} | Logická hodnota, která určuje, zda je zařízení smartphone. | Hodnota TRUE
Je Tablet | % {wurfl_cap_is_tablet} | Logická hodnota, která určuje, zda je zařízení tablet. Toto je popis nezávislé operačního systému. | Hodnota TRUE
Je bezdrátových zařízení | % {wurfl_cap_is_wireless_device} | Logická hodnota, která určuje, jestli zařízení je považováno za bezdrátových zařízení. | Hodnota TRUE
Název marketing | % {wurfl_cap_marketing_name} | Řetězec, který označuje marketing název zařízení. | BlackBerry 8100 Pearl
Prohlížeč pro mobilní zařízení | % {wurfl_cap_mobile_browser} | Řetězec, který označuje prohlížeče slouží k vyžádání obsahu ze zařízení. | Chrome
Verze mobilní prohlížeče | % {wurfl_cap_mobile_browser_version} | Řetězec, který určuje verzi prohlížeče slouží k vyžádání obsahu ze zařízení. | 31
Název modelu | % {wurfl_cap_model_name} | Řetězec určující název modelu zařízení. | S3
Progresivní stahování | % {wurfl_cap_progressive_download} | Logická hodnota, která určuje, zda zařízení podporuje přehrávání zvuku a videa, zatímco stále probíhá stahování. | Hodnota TRUE
Datum vydání | % {wurfl_cap_release_date} | Řetězec, který označuje za rok a měsíc přidání zařízení WURFL databáze.<br/><br/>Formát:`yyyy_mm` | 2013_december
Výška řešení | % {wurfl_cap_resolution_height} | Celé číslo, které určuje výšku zařízení v pixelech. | 768
Šířka řešení | % {wurfl_cap_resolution_width} | Celé číslo, které označuje zařízení šířku v pixelech. | 1024


## <a name="location"></a>Umístění

Tyto shodu podmínky slouží k identifikaci požadavků podle umístění žadatele.

Name (Název) | Účel
-----|--------
JAKO počet | Určuje požadavky, které pocházejí z konkrétní sítě.
Země | Určuje požadavky, které pocházejí z zadaný zemích.


## <a name="origin"></a>Zdroj

Tyto podmínky jsou určeny k identifikaci odpovídají požadavků, které odkazují na úložiště CDN nebo zdrojový server zákazníka.

Name (Název) | Účel
-----|--------
Původu CDN | Identifikuje požadavky na obsah se uloží do úložiště CDN.
Původ zákazníka | Identifikuje požadavky na obsah uložený na původním serveru konkrétního zákazníka.


## <a name="request"></a>Žádost

Tyto shodu podmínky slouží k identifikaci požadavků na základě jejich vlastností.

Name (Název) | Účel
-----|--------
IP adresa klienta | Určuje požadavky, které pocházejí z konkrétní IP adresu.
Parametr souboru cookie | Kontroluje soubory cookie související s každou žádostí pro zadanou hodnotu.
Soubor cookie parametr Regex | Kontroluje soubory cookie související s každou žádostí pro určený regulární výraz.
Hraniční Cname | Určuje požadavky, které odkazují na konkrétní edge CNAME.
Odkazující domény | Určuje požadavky, které byly uvedené ze zadaného hostname(s).
Literál hlavičky požadavku | Určuje požadavky, které obsahují zadaná hlavička nastavena na zadané hodnoty.
Regex hlavičky požadavku | Určuje požadavky, které obsahují zadaná hlavička nastavena na hodnotu, která odpovídá zadanému regulárnímu výrazu.
Zástupný znak hlavičky požadavku | Určuje požadavky, které obsahují zadaná hlavička nastavena na hodnotu, která odpovídá zadanému vzoru.
Request – metoda | Identifikuje požadavky jejich metodou HTTP.
Schéma požadavku | Identifikuje požadavků podle jejich protokolu HTTP.

## <a name="url"></a>ADRESA URL

Tyto shodu podmínky slouží k identifikaci požadavků podle jejich adresy URL.

Name (Název) | Účel
-----|--------
Adresa URL cesta adresáře | Identifikuje požadavků podle jejich relativní cestu.
Rozšíření cesty adresy URL | Identifikuje požadavků podle jejich příponu názvu souboru.
Název souboru cestu adresy URL | Identifikuje požadavků podle jejich název souboru.
Literál cestu adresy URL | Porovná relativní cestu požadavku se zadanou hodnotou.
Regulární výraz cesty adresy URL | Porovná požadavek na relativní cestu k zadanému regulárnímu výrazu.
Cesta URL zástupný znak | Porovná požadavek na relativní cestu k zadanému vzoru.
Adresa URL dotazu literál | Porovná řetězec dotazu požadavku se zadanou hodnotou.
Parametr URL dotazu | Určuje požadavky, které obsahují parametru řetězce dotazu zadaná nastavena na hodnotu, která odpovídá zadanému vzoru.
Adresa URL dotazu Regex | Určuje požadavky, které obsahují parametru řetězce dotazu zadaná nastavena na hodnotu, která odpovídá zadanému regulárnímu výrazu.
Adresa URL dotazu zástupný znak | Porovná zadané hodnoty proti řetězec dotazu žádosti.


## <a name="next-steps"></a>Další kroky
* [Přehled Azure CDN](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel](cdn-rules-engine.md)

