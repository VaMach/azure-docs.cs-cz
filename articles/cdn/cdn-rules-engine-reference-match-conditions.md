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
ms.openlocfilehash: 0abbcf8508cb95d0fb8a9c8e5243426752efe590
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Stroj pravidel Azure CDN splňují podmínky
Toto téma obsahuje podrobný popis dostupných podmínek shodu pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Druhá část pravidla je podmínka shodu. Stav shody identifikuje konkrétní typy žádostí, pro které se provede sadu funkcí.

Například se může použít k filtrování požadavků na obsah v konkrétních místech, generovány z konkrétní IP adresu nebo země, nebo informace hlavičky žádosti.

## <a name="always"></a>Vždy

Stav shody vždy slouží ke použít výchozí sadu funkcí pro všechny požadavky.

## <a name="device"></a>Zařízení

Stav shody zařízení identifikuje požadavky z mobilního zařízení podle jeho vlastnosti.  Mobilní zařízení jsou detekovány prostřednictvím [WURFL](http://wurfl.sourceforge.net/).  Možnosti WURFL a jejich proměnné stroj pravidel CDN jsou uvedeny níže.
<br>
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

### <a name="as-number"></a>JAKO počet 
Tato síť je definována pomocí jeho autonomního systému číslo (ASN). Poskytuje možnost indikující, zda tento stav bude splnit při klienta sítě "Odpovídá" nebo "Nemá neodpovídá" zadaný jako číslo.

**Informace o klíči**
- Zadejte více čísla AS omezující každé z nich mezerou. Například 64514 64515 odpovídá přicházejících od 64514 nebo 64515 požadavky.
- Některé požadavky nemusí vracet platné číslo. Otazník (tj.),?) bude shodovat s požadavky, u kterých platné číslo AS nelze určit.
- Celý jako je třeba zadat číslo pro požadovanou síť. Částečné hodnoty nebude odpovídat.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="country"></a>Země
Zemi lze zadat pomocí jeho kód země. Možnost je k dispozici k označení, zda tento stav bude při splnění země, ze které žádost pochází "Odpovídá" nebo "Nemá neodpovídá" zadané hodnoty.


**Informace o klíči**
- Zadejte více kódy zemí omezující každé z nich mezerou.
- Zástupné znaky nejsou podporovány při zadávání kód země.
- Kódy zemí "EU" a "Asie" není zahrnovat všechny IP adresy v těchto oblastech.
- Některé požadavky nemusí vracet platný kód země. Otazník (tj.),?) bude shodovat s požadavky, u kterých nebylo možné určit platný kód země.
- Kódy zemí rozlišují velká a malá písmena.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

## <a name="origin"></a>Zdroj

Tyto podmínky jsou určeny k identifikaci odpovídají požadavků, které odkazují na úložiště CDN nebo zdrojový server zákazníka.

Name (Název) | Účel
-----|--------
Původu CDN | Identifikuje požadavky na obsah se uloží do úložiště CDN.
Původ zákazníka | Identifikuje požadavky na obsah uložený na původním serveru konkrétního zákazníka.

### <a name="cdn-origin"></a>Původu CDN
Je splněna podmínka této shody, pokud jsou splněny obě následující podmínky:
- Bylo vyžadováno obsah z CDN úložiště.
- Identifikátoru URI požadavku využívá bodem přístup k obsahu (například /000001) definované v tomto stavu shody.
  - Adresa URL CDN: Identifikátoru URI požadavku musí obsahovat bodem vybrané přístup k obsahu.
  - Adresa URL CNAME Edge: Odpovídající konfiguraci hraniční CNAME musí odkazovat bodem vybrané přístup k obsahu.
  
*Poznámky:*
 - Přístup k obsahu bodu identifikuje služba, která by měla sloužit požadovaný obsah.
 - Příkazu a v případě není vhodné používat kombinovat určité podmínky shody. Kombinování podmínku původu CDN shodu s podmínkou shodu zákazníka původu by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu velmi stejné nelze kombinovat dvě podmínky shody původu CDN prostřednictvím příkazu a v případě.
 
### <a name="customer-origin"></a>Původ zákazníka

**Informace o klíči** 
- Tento stav shody bude uspokojit, bez ohledu na to, zda je obsah požadována pomocí název CDN nebo okraj CNAME adresa URL, která odkazuje na počátek vybraného zákazníka.
- Konfigurace počátečního zákazníka odkazuje pravidlo nesmí být odstraněny ze strany zákazníka původu. Před pokusem o odstranění konfigurace počátek zákazníka, ujistěte se, že následující konfigurace neodkazujte ho:
  - Podmínky shody počátek zákazníka
  - Konfigurace aplikace edge CNAME.
- Příkazu a v případě není vhodné používat kombinovat určité podmínky shody. Kombinování podmínku zákazníka počátek shodu s podmínkou shodu původu CDN by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu velmi stejné nelze kombinovat dvě podmínky shody počátek zákazníka prostřednictvím příkazu a v případě.

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

### <a name="client-ip-address"></a>IP adresa klienta
Možnost je k dispozici k označení, zda tento stav bude splnit, když klient je IP adresa "Odpovídá" nebo "Nemá neodpovídá" zadané IP adresy.

**Informace o klíči:**
- Nezapomeňte použít notaci CIDR.
- Zadejte více IP adres nebo bloky IP adres podle omezující každé z nich mezerou.
  - **Příklad IPv4:** 1.2.3.4 10.20.30.40 odpovídá všechny žádosti, které dorazily z 1.2.3.4 nebo 10.20.30.40.
  - **Příklad IPv6:** 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 odpovídá všechny žádosti, které dorazily z 1:2:3:4:5:6:7:8 nebo 10:20:30:40:50:60:70:80.
- Syntaxe pro blok IP adres je základní adresu IP a lomítkem a velikost předponu.
  - **Příklad IPv4:** 5.5.5.64/26 odpovídá všechny žádosti přicházejících ze 5.5.5.64 prostřednictvím 5.5.5.127.
  - **Příklad IPv6:** 1:2:3: / 48 odpovídá všechny žádosti přicházejících ze 1:2:3:0:0:0:0:0 prostřednictvím 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="cookie-parameter"></a>Parametr souboru cookie
**Odpovídá / neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá:** vyžaduje žádost obsahuje zadaný soubor cookie s hodnotou, která odpovídá alespoň jedna z hodnot fronty definovaných v tomto stavu shody.
- **Neodpovídá:** vyžaduje, že žádost splňují jednu z následujících kritérií:
  - Zadaný soubor cookie neobsahuje.
  - Obsahuje zadaný soubor cookie, ale jeho hodnota neodpovídá žádnému z hodnot fronty definovaných v tomto stavu shody.
  
**Informace o klíči:**
- **Název souboru cookie:** 
  - Speciální znaky, včetně hvězdičku, nejsou podporovány při zadávání názvu souboru cookie. To znamená, že jsou vhodné pro porovnání pouze název odpovídá přesný souboru cookie.
  - Je možné zadat pouze název jednoho souboru cookie na jednu instanci této podmínky shody.
  - Porovnání název souboru cookie nerozlišují malá a velká písmena.
- **Hodnota souboru cookie:** 
  - Zadejte více hodnot souboru cookie omezující každé z nich mezerou.
  - Hodnota souboru cookie můžete využít výhod speciální znaky. 
  - Pokud nebyl zadán zástupný znak, pouze v případě přesné shody budou splňovat tato podmínka shodu. 
   - **Příklad:** zadání "Value" bude shodovat s "Value", ale ne "Hodnota1" nebo "Value2."
  - **Ignorovat případ** možnost určuje, zda malá a velká písmena porovnání se budou provádět na hodnotu souboru cookie žádosti.
  - Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
   - Dokončení výplně mezipaměti
   - Výchozí interní Max-Age
   - Vynutit interní Max-Age
   - Ignorovat počátek No-Cache
   - Interní Max zastaralé

### <a name="cookie-parameter-regex"></a>Soubor cookie parametr Regex
Tento stav shody definuje název souboru cookie a hodnotu. Regulární výrazy lze zadat hodnoty požadované souboru cookie. 

**Odpovídá / neodpovídá** možnost určuje podmínky, za kterých bude tento stav shody splnit.
- **Odpovídá:** vyžaduje žádost obsahuje zadaný soubor cookie s hodnotou, která odpovídá zadanému regulárnímu výrazu.
- **Neodpovídá:** vyžaduje, že žádost splňují jednu z následujících kritérií:
  - Zadaný soubor cookie neobsahuje.
  - Obsahuje zadaný soubor cookie, ale jeho hodnota neodpovídá zadanému regulárnímu výrazu.
  
**Informace o klíči:**
- **Název souboru cookie:** 
  - Regulární výrazy a speciální znaky, včetně hvězdičku, nejsou podporovány při zadávání názvu souboru cookie. To znamená, že jsou vhodné pro porovnání pouze název odpovídá přesný souboru cookie.
  - Je možné zadat pouze název jednoho souboru cookie na jednu instanci této podmínky shody.
  - Porovnání název souboru cookie nerozlišují malá a velká písmena.
- **Hodnota souboru cookie:** 
  - Hodnota souboru cookie můžete využít výhod regulární výrazy.
  - **Ignorovat případ** možnost určuje, zda malá a velká písmena porovnání se budou provádět na hodnotu souboru cookie žádosti.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="edge-cname"></a>Hraniční Cname
**Informace o klíči** 
- Seznam dostupných edge záznamů CNAME je omezený na ty, které jsou nakonfigurované na stránce záznamů CNAME Edge odpovídající platformy, na kterém je konfigurován stroj pravidel HTTP.
- Před pokusem o odstranění konfigurace aplikace edge CNAME, ujistěte se, že podmínku shodu Edge Cname neodkazuje ho. Konfigurace Edge CNAME, které byly definovány v pravidle nelze odstranit ze stránky záznamů CNAME okraj. 
- Příkazu a v případě není vhodné používat kombinovat určité podmínky shody. Kombinování podmínku Edge Cname shodu s podmínkou shodu zákazníka původu by například vytvořit vzor pro shodu, která může být nikdy shoduje. Z tohoto důvodu velmi stejné nelze kombinovat dvě podmínky shody Edge Cname prostřednictvím příkazu a v případě.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="referring-domain"></a>Odkazující domény
Název hostitele přidružené odkazující server, pomocí kterého byl požadován obsah Určuje, zda při splnění této podmínky. Možnost je k dispozici k označení, zda tento stav bude splnit při odkazující název hostitele "Odpovídá" nebo "Neodpovídá" zadané hodnoty.
**Informace o klíči:**
- Zadejte názvy hostitelů více omezující každé z nich mezerou.
- Tento stav shody podporuje speciální znaky.
- Pokud zadaná hodnota neobsahuje znak hvězdičky, pak musí být přesná shoda pro název hostitele odkazující server. Například zadání "domena.cz" nepovede ke shodě "www.mydomain.com."
- Ignorovat případ možnost určuje, zda bude provedena porovnání malá a velká písmena.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé
  
 ### <a name="request-header-literal"></a>Literál hlavičky požadavku
**Odpovídá / neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá:** vyžaduje požadavky tak, aby obsahovala zadaný hlavičku a její hodnota musí odpovídat objektu definovanému v tomto stavu shody.
- **Neodpovídá:** vyžaduje, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá definovanému v tomto stavu shody.
  
**Informace o klíči:**
- Porovnání název hlavičky jsou vždy velká a malá písmena. Rozlišování porovnání hodnot hlavičky je určena možnost Ignorovat případ.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé
  
### <a name="request-header-regex"></a>Regex hlavičky požadavku
**Poznámka:** tato funkce vyžaduje stroj pravidel - Advanced pravidla, která je nutné zakoupit samostatně. Obraťte se na správce svého účtu CDN aktivovat. 

**Odpovídá / neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá:** vyžaduje požadavky tak, aby obsahovala zadaný hlavičku a její hodnota musí odpovídat vzorku definované v určený regulární výraz.
- **Neodpovídá:** vyžaduje, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá zadanému regulárnímu výrazu.

**Informace o klíči:**
- Název hlavičky: 
  - Porovnání název hlavičky jsou velká a malá písmena.
  - V názvu záhlaví mezery, měl by být nahrazen "% 20." 
- Hodnota hlavičky: 
  - Hodnota hlavičky může využít výhod regulární výrazy.
  - Rozlišování porovnání hodnot hlavičky je určena možnost Ignorovat případ.
  - Jenom přesné záhlaví hodnota odpovídá do alespoň jedné zadané vzory odpovídají tuto podmínku.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé 

### <a name="request-header-wildcard"></a>Zástupný znak hlavičky požadavku
**Odpovídá / neodpovídá** možnost určuje podmínky, v němž je tato vyhovují podmínce bude splnit.
- **Odpovídá:** vyžaduje požadavky tak, aby obsahovala zadaný hlavičku a její hodnota musí odpovídat aspoň jednu z hodnot fronty definovaných v tomto stavu shody.
- **Neodpovídá:** vyžaduje, že žádost splňují jednu z následujících kritérií:
  - Zadaná hlavička neobsahuje.
  - Zadaná hlavička obsahuje, ale jeho hodnota neodpovídá žádnému ze zadaných hodnot.
  
**Informace o klíči:**
- Název hlavičky: 
  - Porovnání název hlavičky jsou velká a malá písmena.
  - V názvu záhlaví mezery, měl by být nahrazen "% 20." Tuto hodnotu lze také zadat prostory v hodnotu hlavičky.
- Hodnota hlavičky: 
  - Hodnota hlavičky můžete využít výhod speciální znaky.
  - Rozlišování porovnání hodnot hlavičky je určena možnost Ignorovat případ.
  - Jenom přesné záhlaví hodnota odpovídá do alespoň jedné zadané vzory odpovídají tuto podmínku.
  - Zadejte více hodnot omezující každé z nich mezerou.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="request-method"></a>Request – metoda
Jenom prostředky, které jsou požadovány metodou vybrané žádosti bude splňují tuto podmínku. K dispozici požadavek metody jsou následující:
- GET
- HEAD 
- POST 
- MOŽNOSTI 
- PUT 
- ODSTRANIT 
- TRASOVÁNÍ 
- PŘIPOJENÍ 

**Informace o klíči:**
- Ve výchozím nastavení může generovat jenom metody GET žádost o obsah uložený v mezipaměti na naše síť. Všechny ostatní žádosti metody se jednoduše směrovány přes proxy server prostřednictvím naše síť.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

### <a name="request-scheme"></a>Schéma požadavku
K tomuto stavu odpovídají jenom prostředky, které jsou požadovány pomocí vybraný protokol. K dispozici protokoly jsou protokoly HTTP a HTTPS.

**Informace o klíči:**
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení tato podmínka shoda není kompatibilní s následující funkce:
  - Dokončení výplně mezipaměti
  - Výchozí interní Max-Age
  - Vynutit interní Max-Age
  - Ignorovat počátek No-Cache
  - Interní Max zastaralé

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

