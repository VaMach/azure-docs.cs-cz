---
title: "Splňují podmínky pro stroj pravidel Content Delivery Network | Microsoft Docs"
description: "Referenční dokumentace pro Azure Content Delivery Network pravidla shody stav motoru a funkce."
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
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 944675f4b055c3d0c2559db53e4807a5c00bda64
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="match-conditions-for-the-content-delivery-network-rules-engine"></a>Podmínky shody pro modul Content Delivery Network pravidla
Tento článek obsahuje seznam podrobný popis podmínky k dispozici shodu pro Azure Content Delivery Network [stroj pravidel](cdn-rules-engine.md).

Druhá část pravidla je podmínka shodu. Stav shody identifikuje konkrétní typy žádostí, pro které se provede sadu funkcí.

Například můžete použít podmínku shodu pro:
- Filtr požadavků na obsah v konkrétních místech.
- Filtrovat žádosti vygenerované z konkrétní IP adresu nebo zemi.
- Filtrovat žádosti podle informace hlavičky.

## <a name="always-match-condition"></a>Vždy odpovídat podmínky

Stav shody vždy výchozí sadu funkcí, platí pro všechny požadavky.

Název | Účel
-----|--------
[Vždy](#always) | Výchozí sadu funkcí, se vztahuje na všechny požadavky.

## <a name="device-match-condition"></a>Stav shody zařízení

Stav shody zařízení identifikuje požadavky z mobilního zařízení podle jeho vlastnosti.  

Název | Účel
-----|--------
[Zařízení](#device) | Identifikuje požadavky z mobilního zařízení podle jeho vlastnosti.

## <a name="location-match-conditions"></a>Podmínky shody umístění

Podmínky shody umístění identifikaci požadavků podle umístění žadatele.

Název | Účel
-----|--------
[JAKO počet](#as-number) | Určuje požadavky, které pocházejí z konkrétní sítě.
[Země](#country) | Určuje požadavky, které pocházejí z zadaný zemích.

## <a name="origin-match-conditions"></a>Podmínky shody počátek

Podmínky shody počátek určete požadavky, které odkazují na Content Delivery Network úložiště nebo na původním serveru zákazníka.

Název | Účel
-----|--------
[Původu CDN](#cdn-origin) | Identifikuje požadavky na obsah uložený v síti pro doručování obsahu úložiště.
[Původ zákazníka](#customer-origin) | Identifikuje požadavky na obsah uložený na původním serveru konkrétního zákazníka.

## <a name="request-match-conditions"></a>Podmínky žádosti o shody

Porovnání podmínky žádosti o identifikaci požadavků na základě jejich vlastností.

Název | Účel
-----|--------
[IP adresa klienta](#client-ip-address) | Určuje požadavky, které pocházejí z konkrétní IP adresu.
[Parametr souboru cookie](#cookie-parameter) | Kontroluje soubory cookie související s každou žádostí pro zadanou hodnotu.
[Soubor cookie parametr Regex](#cookie-parameter-regex) | Kontroluje soubory cookie související s každou žádostí pro určený regulární výraz.
[Hraniční Cname](#edge-cname) | Určuje požadavky, které odkazují na konkrétní edge CNAME.
[Odkazující domény](#referring-domain) | Určuje požadavky, které byly uvedené z názvů zadaného hostitele.
[Literál hlavičky požadavku](#request-header-literal) | Určuje požadavky, které obsahují zadaná hlavička nastavit na zadanou hodnotu.
[Regex hlavičky požadavku](#request-header-regex) | Určuje požadavky, které obsahují zadaná hlavička nastavena na hodnotu, která odpovídá zadanému regulárnímu výrazu.
[Zástupný znak hlavičky požadavku](#request-header-wildcard) | Určuje požadavky, které obsahují zadaná hlavička nastavena na hodnotu, která odpovídá zadanému vzoru.
[Request – metoda](#request-method) | Identifikuje požadavky jejich metodou HTTP.
[Schéma požadavku](#request-scheme) | Identifikuje požadavků podle jejich protokolu HTTP.

## <a name="url-match-conditions"></a>Podmínky pro shodu adresy URL

Podmínky shodu adresy URL identifikaci požadavků podle jejich adresy URL.

Název | Účel
-----|--------
Adresa URL cesta adresáře | Identifikuje požadavků podle jejich relativní cestu.
Rozšíření cesty adresy URL | Identifikuje požadavků podle přípony názvu souboru.
Název souboru cestu adresy URL | Identifikuje požadavky podle názvu souboru.
Literál cestu adresy URL | Porovná relativní cestu požadavku se zadanou hodnotou.
Regulární výraz cesty adresy URL | Porovná požadavek na relativní cestu k zadanému regulárnímu výrazu.
Cesta URL zástupný znak | Porovná požadavek na relativní cestu k zadanému vzoru.
Adresa URL dotazu literál | Porovná řetězec dotazu požadavku se zadanou hodnotou.
Parametr URL dotazu | Určuje požadavky, které obsahují parametru řetězce dotazu zadaná nastavena na hodnotu, která odpovídá zadanému vzoru.
Adresa URL dotazu Regex | Určuje požadavky, které obsahují parametru řetězce dotazu zadaná nastavena na hodnotu, která odpovídá zadanému regulárnímu výrazu.
Adresa URL dotazu zástupný znak | Porovná zadanou hodnotu na řetězec dotazu žádosti.


## <a name="reference-for-rules-engine-match-conditions"></a>Referenční dokumentace pro podmínky shody stroj pravidel

---
### <a name="always"></a>Vždy

Stav shody vždy výchozí sadu funkcí, platí pro všechny požadavky.

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="as-number"></a>JAKO počet 
Číslo AS sítě je definována pomocí čísla autonomního systému (ASN). Poskytuje možnost indikující, zda tento stav bude splnit při klienta sítě "Odpovídá" nebo "Nemá neodpovídá" zadané číslo ASN.

Informace o klíči:
- Zadejte čísla ASN více omezující každé z nich mezerou. Například 64514 64515 odpovídá požadavků, doručení z 64514 nebo 64515.
- Některé žádosti nemusí vracet platné číslo ASN. Otazník (?) bude shodovat s požadavky, u kterých nebylo možné určit platné číslo ASN.
- Je nutné zadat celé číslo ASN pro požadovanou síť. Částečné hodnoty nebude odpovídat.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="cdn-origin"></a>Původu CDN
Je splněna podmínka původu CDN shody, pokud jsou splněny obě následující podmínky:
- Bylo vyžadováno obsah z úložiště Content Delivery Network.
- Požadavek URI používá bodem přístup k obsahu (například /000001), který je definován v tomto stavu shody.
  - Adresu URL pro sítě obsahu doručení: Identifikátoru URI požadavku musí obsahovat bodem vybrané přístup k obsahu.
  - Adresa URL CNAME Edge: Odpovídající konfiguraci hraniční CNAME musí odkazovat bodem vybrané přístup k obsahu.
  
Informace o klíči:
 - Přístup k obsahu bodu identifikuje služba, která by měla sloužit požadovaný obsah.
 - Nepoužívejte příkazu a v případě kombinovat určité podmínky shody. Kombinování podmínku původu CDN shodu s podmínkou shodu zákazníka původu by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu nelze kombinovat dvě podmínky shody původu CDN prostřednictvím příkazu a v případě.

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="client-ip-address"></a>IP adresa klienta
K označení, zda bude splněna podmínka IP adresa klienta, když je klient IP adresa "Odpovídá" nebo "Nemá neodpovídá" zadané IP adresy se poskytuje možnost.

Informace o klíči:
- Nezapomeňte použít notaci CIDR.
- Zadejte více IP adres nebo bloky IP adres podle omezující každé z nich mezerou.
  - **Příklad IPv4**: 1.2.3.4 10.20.30.40 odpovídá všechny požadavky, které přicházejí z 1.2.3.4 nebo 10.20.30.40.
  - **Příklad IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 odpovídá všechny požadavky, které přicházejí z 1:2:3:4:5:6:7:8 nebo 10:20:30:40:50:60:70:80.
- Syntaxe pro blok IP adres je základní adresu IP a lomítkem a velikost předponu.
  - **Příklad IPv4**: 5.5.5.64/26 odpovídá všechny požadavky, které přicházejí z 5.5.5.64 prostřednictvím 5.5.5.127.
  - **Příklad IPv6**: 1:2:3: / 48 odpovídá všechny požadavky, které přicházejí z 1:2:3:0:0:0:0:0 prostřednictvím 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="cookie-parameter"></a>Parametr souboru cookie
**Odpovídá**/**neodpovídá** možnost určuje podmínky, za kterých parametr souboru Cookie vyhovují podmínce bude splnit.
- **Odpovídá**: vyžaduje žádost obsahuje zadaný soubor cookie s hodnotu, která odpovídá alespoň jeden z hodnoty, které jsou definované v tomto stavu shody.
- **Neodpovídá**: vyžaduje se, že žádost splňují jednu z následujících kritérií:
  - Zadaný soubor cookie neobsahuje.
  - Obsahuje zadaný soubor cookie, ale jeho hodnota neodpovídá žádnému z hodnot, které jsou definované v tomto stavu shody.
  
Informace o klíči:
- Název souboru cookie: 
  - Speciální znaky, včetně hvězdičku, nejsou podporované, když zadáváte název souboru cookie. To znamená, že jsou vhodné pro porovnání pouze název odpovídá přesný souboru cookie.
  - Jeden instanci tento stav shody lze zadat pouze název jednoho souboru cookie.
  - Porovnání název souboru cookie nerozlišují malá a velká písmena.
- Hodnota souboru cookie: 
  - Zadejte více hodnot souboru cookie omezující každé z nich mezerou.
  - Hodnota souboru cookie můžete využít výhod speciální znaky. 
  - Pokud nebyl zadán zástupný znak, pouze v případě přesné shody bude splňovat na shodu. Například zadání "Value" bude shodovat s "Value", ale ne "Hodnota1" nebo "Value2."
  - **Ignorovat případ** možnost určuje, zda malá a velká písmena porovnání se budou provádět na hodnotu souboru cookie žádosti.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="cookie-parameter-regex"></a>Soubor cookie parametr Regex
Stav shody Regex parametr souboru Cookie definuje název souboru cookie a hodnotu. Regulární výrazy můžete zadat hodnoty požadované souboru cookie. 

**Odpovídá**/**neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá**: vyžaduje žádost obsahuje zadaný soubor cookie s hodnotou, která odpovídá zadanému regulárnímu výrazu.
- **Neodpovídá**: vyžaduje se, že žádost splňují jednu z následujících kritérií:
  - Zadaný soubor cookie neobsahuje.
  - Obsahuje zadaný soubor cookie, ale jeho hodnota neodpovídá zadanému regulárnímu výrazu.
  
Informace o klíči:
- Název souboru cookie: 
  - Regulární výrazy a speciální znaky, včetně hvězdičku, nejsou podporované, když zadáváte název souboru cookie. To znamená, že jsou vhodné pro porovnání pouze název odpovídá přesný souboru cookie.
  - Jeden instanci tento stav shody lze zadat pouze název jednoho souboru cookie.
  - Porovnání název souboru cookie nerozlišují malá a velká písmena.
- Hodnota souboru cookie: 
  - Hodnota souboru cookie můžete využít výhod regulární výrazy.
  - **Ignorovat případ** možnost určuje, zda malá a velká písmena porovnání se budou provádět na hodnotu souboru cookie žádosti.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

--- 
### <a name="country"></a>Země
Můžete zadat země přes jeho kód země. Možnost je k dispozici k označení, zda tento stav bude při splnění země, ze které žádost pochází "Odpovídá" nebo "Nemá neodpovídá" zadanými hodnotami.

Informace o klíči:
- Zadejte více kódy zemí omezující každé z nich mezerou.
- Pokud zadáváte kód země, nejsou podporovány zástupné znaky.
- Kódy zemí "EU" a "Asie" není zahrnovat všechny IP adresy v těchto oblastech.
- Některé žádosti nemusí vracet platný kód země. Otazník (?) bude shodovat s požadavky, u kterých nebylo možné určit platný kód země.
- Kódy zemí rozlišují velká a malá písmena.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="customer-origin"></a>Původ zákazníka

Informace o klíči: 
- Bez ohledu na to, zda je požadovaný obsah prostřednictvím adresu URL síťového doručování obsahu nebo okraj CNAME adresa URL, která odkazuje na počátek vybraného zákazníka. bude splnit podmínky shody počátek zákazníka.
- Konfigurace počátečního zákazníka, která odkazuje pravidlo nelze odstranit ze strany zákazníka původu. Před pokusem o odstranění konfigurace počátek zákazníka, ujistěte se, že následující konfigurace neodkazujte ho:
  - Shoda podmínku počátek zákazníka
  - Konfigurace aplikace edge CNAME
- Nepoužívejte příkazu a v případě kombinovat určité podmínky shody. Kombinování podmínku zákazníka počátek shodu s podmínkou shodu původu CDN by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu nelze kombinovat dvě podmínky shody počátek zákazníka prostřednictvím příkazu a v případě.

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="device"></a>Zařízení

Stav shody zařízení identifikuje požadavky z mobilního zařízení podle jeho vlastnosti. Mobilní zařízení jsou detekovány prostřednictvím [WURFL](http://wurfl.sourceforge.net/). Následující tabulka uvádí možnosti WURFL a jejich proměnné pro modul Content Delivery Network pravidla.
<br>
> [!NOTE] 
> V jsou podporovány následující proměnné **změnit hlavičky žádosti klienta** a **upravit hlavičku odpovědi klienta** funkce.

Schopnost | Proměnná | Popis | Ukázkové hodnoty
-----------|----------|-------------|----------------
Název značky | % {wurfl_cap_brand_name} | Řetězec, který označuje brand název zařízení. | Samsung
Operačního systému zařízení | % {wurfl_cap_device_os} | Řetězec, který označuje operační systém nainstalovaný na zařízení. | iOS
Verze operačního systému zařízení | % {wurfl_cap_device_os_version} | Řetězec, který označuje číslo verze operačního systému na zařízení nainstalovaná. | 1.0.1
Duální orientace | % {wurfl_cap_dual_orientation} | Logická hodnota, která určuje, zda je zařízení podporuje dva orientace. | true (pravda)
Upřednostňovaný souboru DTD protokolu HTML | % {wurfl_cap_html_preferred_dtd} | Řetězec, který označuje definice typu upřednostňované dokumentu (DTD) mobilních zařízení pro obsah HTML. | Žádné<br/>xhtml_basic<br/>HTML5
Vložené bitové kopie | % {wurfl_cap_image_inlining} | Logická hodnota, která určuje, zda je zařízení podporuje Base64 kódovaný bitové kopie. | false (nepravda)
Se systémem Android | % {wurfl_vcap_is_android} | Logická hodnota, která označuje, zda zařízení používá operační systém Android. | true (pravda)
IOS | % {wurfl_vcap_is_ios} | Logická hodnota, která označuje, zda zařízení používá iOS. | false (nepravda)
Je inteligentní TV | % {wurfl_cap_is_smarttv} | Logická hodnota, která určuje, zda zařízení je inteligentní televize. | false (nepravda)
Je Smartphone | % {wurfl_vcap_is_smartphone} | Logická hodnota, která určuje, zda je zařízení smartphone. | true (pravda)
Je Tablet | % {wurfl_cap_is_tablet} | Logická hodnota, která určuje, zda je zařízení tablet. Toto je popis nezávislé operačního systému. | true (pravda)
Je bezdrátových zařízení | % {wurfl_cap_is_wireless_device} | Logická hodnota, která určuje, jestli zařízení je považováno za bezdrátových zařízení. | true (pravda)
Název marketing | % {wurfl_cap_marketing_name} | Řetězec, který označuje marketing název zařízení. | BlackBerry 8100 Pearl
Prohlížeč pro mobilní zařízení | % {wurfl_cap_mobile_browser} | Řetězec, který označuje prohlížeče, který slouží k vyžádání obsahu ze zařízení. | Chrome
Verze mobilní prohlížeče | % {wurfl_cap_mobile_browser_version} | Řetězec, který určuje verzi prohlížeče, který slouží k vyžádání obsahu ze zařízení. | 31
Název modelu | % {wurfl_cap_model_name} | Řetězec určující název modelu zařízení. | S3
Progresivní stahování | % {wurfl_cap_progressive_download} | Logická hodnota, která určuje, zda zařízení podporuje přehrávání zvuku a videa, zatímco stále probíhá stahování. | true (pravda)
Datum vydání | % {wurfl_cap_release_date} | Řetězec, který označuje za rok a měsíc přidání zařízení WURFL databáze.<br/><br/>Formát:`yyyy_mm` | 2013_december
Výška řešení | % {wurfl_cap_resolution_height} | Celé číslo, které určuje výšku zařízení v pixelech. | 768
Šířka řešení | % {wurfl_cap_resolution_width} | Celé číslo, které označuje zařízení šířku v pixelech. | 1024

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="edge-cname"></a>Hraniční Cname
Informace o klíči: 
- Seznam dostupných edge záznamů CNAME je omezený na ty, které jsou nakonfigurované na stránce Edge záznamů CNAME, které odpovídá platformě, na kterém je konfigurován stroj pravidel HTTP.
- Před dalším pokusem o odstranění konfigurace aplikace edge CNAME, ujistěte se, že podmínku Edge Cname shoda se neodkazuje. Konfigurace Edge CNAME, které byly definovány v pravidle nelze odstranit ze stránky záznamů CNAME okraj. 
- Nepoužívejte příkazu a v případě kombinovat určité podmínky shody. Kombinování podmínku Edge Cname shodu s podmínkou shodu zákazníka původu by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu nelze kombinovat dvě podmínky shody Edge Cname prostřednictvím příkazu a v případě.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="referring-domain"></a>Odkazující domény
Název hostitele přidružené odkazující server, pomocí kterého byl požadován obsah, určuje, zda je splněna podmínka odkazující domény. Poskytuje možnost indikující, zda tento stav bude splnit při odkazující název hostitele "Odpovídá" nebo "Nemá neodpovídá" zadanými hodnotami.

Informace o klíči:
- Zadejte více názvů hostitelů omezující každé z nich mezerou.
- Tento stav shody podporuje speciální znaky.
- Pokud zadaná hodnota neobsahuje znak hvězdičky, musí být přesná shoda pro název hostitele odkazující server. Například zadání "domena.cz" nepovede ke shodě "www.mydomain.com."
- **Ignorovat případ** možnost určuje, zda bude provedena porovnání malá a velká písmena.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---  
### <a name="request-header-literal"></a>Literál hlavičky požadavku
**Odpovídá**/**neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá**: vyžaduje obsahovat Zadaná hlavička požadavku. Její hodnota musí odpovídat ten, který je definován v tomto stavu shody.
- **Neodpovídá**: vyžaduje se, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá ten, který je definován v tomto stavu shody.
  
Informace o klíči:
- Porovnání název hlavičky jsou vždy velká a malá písmena. **Ignorovat případ** možnost určuje rozlišování porovnání hodnota hlavičky.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---  
### <a name="request-header-regex"></a>Regex hlavičky požadavku
**Odpovídá**/**neodpovídá** možnost určuje podmínky, za kterých Regex hlavičky požadavku vyhovují podmínce bude splnit.
- **Odpovídá**: vyžaduje obsahovat Zadaná hlavička požadavku. Její hodnota musí odpovídat vzorku, který je definován v určený regulární výraz.
- **Neodpovídá**: vyžaduje se, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá zadanému regulárnímu výrazu.

Informace o klíči:
- Název hlavičky: 
  - Porovnání název hlavičky jsou velká a malá písmena.
  - V názvu záhlaví mezery, měl by být nahrazen "% 20." 
- Hodnota hlavičky: 
  - Hodnota hlavičky můžete využít výhod regulární výrazy.
  - **Ignorovat případ** možnost určuje rozlišování porovnání hodnota hlavičky.
  - Jenom přesné záhlaví hodnota odpovídá do alespoň jedné zadané vzory odpovídají tuto podmínku.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé 

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="request-header-wildcard"></a>Zástupný znak hlavičky požadavku
**Odpovídá**/**neodpovídá** možnost určuje podmínky, za kterých zástupný znak hlavičky požadavku vyhovují podmínce bude splnit.
- **Odpovídá**: vyžaduje obsahovat Zadaná hlavička požadavku. Její hodnota musí odpovídat aspoň jednu z hodnot, které jsou definované v tomto stavu shody.
- **Neodpovídá**: vyžaduje se, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá žádnému ze zadaných hodnot.
  
Informace o klíči:
- Název hlavičky: 
  - Porovnání název hlavičky jsou velká a malá písmena.
  - V názvu záhlaví mezery, měl by být nahrazen "% 20." Tuto hodnotu můžete taky zadat prostory v hodnotu hlavičky.
- Hodnota hlavičky: 
  - Hodnota hlavičky můžete využít výhod speciální znaky.
  - **Ignorovat případ** možnost určuje rozlišování porovnání hodnota hlavičky.
  - Jenom přesné záhlaví hodnota odpovídá do alespoň jedné zadané vzory odpovídají tuto podmínku.
  - Zadejte více hodnot omezující každé z nich mezerou.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="request-method"></a>Request – metoda
Jenom prostředky, které jsou požadovány prostřednictvím metody vybrané žádosti budou splňovat podmínky metoda žádosti. K dispozici požadavek metody jsou následující:
- GET
- HEAD 
- POST 
- MOŽNOSTI 
- PUT 
- DELETE 
- TRASOVÁNÍ 
- PŘIPOJENÍ 

Informace o klíči:
- Ve výchozím nastavení může generovat jenom metody GET žádost o obsah uložený v mezipaměti v síti. Všechny ostatní metody žádosti jsou směrovány přes proxy server v síti.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

---
### <a name="request-scheme"></a>Schéma požadavku
Jenom prostředky, které jsou požadovány prostřednictvím vybraný protokol budou splňovat podmínky schéma požadavku. K dispozici protokoly jsou protokoly HTTP a HTTPS.

Informace o klíči:
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

[Zpět na začátek](#azure-cdn-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Další postup
* [Přehled sítě pro doručování obsahu Azure](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel](cdn-rules-engine.md)

