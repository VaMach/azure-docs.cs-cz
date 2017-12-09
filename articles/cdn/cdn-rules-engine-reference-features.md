---
title: "Pravidla ve službě Azure CDN modul funkce | Microsoft Docs"
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
ms.openlocfilehash: 35bae19deb6d4a79367c171aea5d74b6698e023b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="azure-cdn-rules-engine-features"></a>Pravidla ve službě Azure CDN modul funkce
Tento článek obsahuje seznam podrobný popis dostupných funkcí pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Je třetí součást pravidla funkce. Funkce definuje typ akce, který se použije pro typ požadavku identifikovaný sadu podmínek shodu.

## <a name="access-features"></a>Získat přístup k funkcím

Tyto funkce jsou navrženy pro řízení přístupu k obsahu.


Name (Název) | Účel
-----|--------
[Odepřít přístup (403)](#deny-access-403) | Určuje, zda všechny požadavky byly zamítnuty 403 Zakázáno odpovědi.
[Token ověřování](#token-auth) | Určuje, zda je na žádost o použito ověřování na základě tokenu.
[Odmítnutí kód tokenu ověřování](#token-auth-denial-code) | Určuje typ odpovědi, která je vrácena uživateli při požadavku byl odepřen v důsledku ověřování na základě tokenu.
[Token Auth ignorovat případ adresy URL](#token-auth-ignore-url-case) | Určuje, zda jsou adresa URL porovnání provedené na základě tokenu ověřování malá a velká písmena.
[Parametr tokenu ověřování](#token-auth-parameter) | Určuje, zda by měl parametr řetězce dotazu ověřování na základě tokenu přejmenovat.


## <a name="caching-features"></a>Funkce ukládání do mezipaměti

Tyto funkce slouží k přizpůsobení, kdy a jak je obsah uložený v mezipaměti.

Name (Název) | Účel
-----|--------
[Parametry šířky pásma](#bandwidth-parameters) | Určuje, zda parametry omezení šířky pásma (například ec_rate a ec_prebuf) jsou aktivní.
[Omezení šířky pásma](#bandwidth-throttling) | Omezení šířky pásma pro odpověď poskytované servery edge.
[Nepoužívat mezipaměti](#bypass-cache) | Určuje, zda požadavek by měl nepoužívat ukládání do mezipaměti.
[Zpracování hlavička Cache-Control](#cache-control-header-treatment) | Když je aktivní funkce externí Max-Age ovládací prvky server edge generování hlavičky Cache-Control.
[Řetězec dotazu klíče mezipaměti](#cache-key-query-string) | Určuje, zda bude klíč mezipaměti zahrnout nebo vyloučit parametrů řetězce dotazu přidružený k požadavku.
[Přepište klíče mezipaměti](#cache-key-rewrite) | Přepíše klíč mezipaměti přidružený k požadavku.
[Dokončení výplně mezipaměti](#complete-cache-fill) | Určuje, co se stane, když požadavek výsledky v k neúspěšnému přístupu do mezipaměti částečné na hraniční server.
[Komprimovat typy souborů](#compress-file-types) | Definuje formáty souborů, které komprimuje na serveru.
[Výchozí interní Max-Age](#default-internal-max-age) | Určuje výchozí interval maximální stáří hraničního serveru na původní server mezipaměti opětovné ověření.
[Vyprší platnost zacházení záhlaví](#expires-header-treatment) | Když je aktivní funkce externí Max-Age ovládací prvky server edge generování hlavičky Expires.
[Externí Max-Age](#external-max-age) | Určuje maximální stáří interval pro prohlížeč edge server mezipaměti opětovné ověření.
[Vynutit interní Max-Age](#force-internal-max-age) | Určuje maximální stáří interval pro server edge na původní server mezipaměti opětovné ověření.
[Podpora H.264 (HTTP progresivního stahování)](#h264-support-http-progressive-download) | Určuje typy H.264 formáty souborů, které lze použít k vysílání datového proudu obsahu.
[Dodržet No Cache požadavku](#honor-no-cache-request) | Určuje, zda budou předány požadavků na Ne mezipaměť klienta HTTP na zdrojový server.
[Ignorovat počátek No-Cache](#ignore-origin-no-cache) | Určuje, zda CDN ignoruje určité direktivy zpracování zdrojovému serveru.
[Ignorovat Unsatisfiable rozsahů](#ignore-unsatisfiable-ranges) | Určuje, který je vrácen do klientů, pokud žádost vygeneruje 416 požadovaný rozsah nelze uspokojit stavový kód odpovědi.
[Interní Max zastaralé](#internal-max-stale) | Ovládací prvky, jak dlouho po čase normální vypršení platnosti mezipaměti asset může zpracovat z hraniční server, když hraničního serveru se nepodařilo znovu ověřit v mezipaměti asset je zdrojový server.
[Sdílení částečné mezipaměti](#partial-cache-sharing) | Určuje, zda žádost může generovat obsahu částečně v mezipaměti.
[Prevalidate obsah uložený v mezipaměti](#prevalidate-cached-content) | Určuje, zda obsah uložený v mezipaměti vhodné pro včasné opětovné ověření před jeho hodnota TTL nevyprší.
[Aktualizujte souborů z mezipaměti nula bajtů](#refresh-zero-byte-cache-files) | Určuje, jak zpracovává požadavek klienta HTTP pro prostředek 0 bajtů mezipaměti servery edge.
[Nastavit lze uložit do mezipaměti stavové kódy](#set-cacheable-status-codes) | Definuje sadu stavové kódy, které mohou způsobovat obsah uložený v mezipaměti.
[Zastaralé doručování obsahu při chybě](#stale-content-delivery-on-error) | Určuje, zda vypršela platnost, že budou doručeny obsah uložený v mezipaměti, když dojde k chybě během opětovné ověření mezipaměti nebo při načítání požadovaný obsah ze zdrojového serveru zákazníka.
[Zastaralých při Revalidate](#stale-while-revalidate) | Zlepšuje výkon tím, že servery edge zastaralý klient neslouží k žadatel, zatímco probíhá opětovné ověření.

## <a name="comment-feature"></a>Funkce komentáře

Tato funkce slouží ke poskytují další informace v pravidle.

Name (Název) | Účel
-----|--------
[Komentář](#comment) | Umožňuje Poznámka přidávaného v pravidle.
 
## <a name="header-features"></a>Funkce záhlaví

Tyto funkce slouží k přidání, úpravě nebo odstranění hlavičky z požadavku nebo odpovědi.

Name (Název) | Účel
-----|--------
[Hlavička odpovědi stáří](#age-response-header) | Určuje, zda hlavičku odpovědi stáří budou zahrnuty v odpovědi odeslat žadatel.
[Ladění hlavičky odpovědi v mezipaměti](#debug-cache-response-headers) | Určuje, zda odpověď může zahrnovat hlavičku odpovědi X-ES-Debug, který obsahuje informace o zásady ukládání do mezipaměti pro požadovaný prostředek.
[Upravit hlavička požadavku klienta](#modify-client-request-header) | Přepíše, připojí nebo odstraní hlavičky v požadavku.
[Upravit hlavičku odpovědi klienta](#modify-client-response-header) | Přepíše, připojí nebo odstraní hlavičku z odpovědi.
[Nastavit vlastní záhlaví IP klienta](#set-client-ip-custom-header) | Umožňuje IP adresu klienta, který chcete přidat do žádosti jako hlavičku požadavku vlastní.


## <a name="logging-features"></a>Funkce protokolování

Tyto funkce jsou navrženy pro přizpůsobení dat uložených v nezpracované soubory protokolu.

Name (Název) | Účel
-----|--------
[Pole vlastní protokol 1](#custom-log-field-1) | Určuje formát a obsah, který se přiřadí pole vlastního protokolu v nezpracovaných souboru protokolu.
[Řetězec protokolu dotazu](#log-query-string) | Určuje, zda řetězec dotazu se uloží spolu s adresu URL v protokolech přístup.


<!---
## Optimize

These features determine whether a request will undergo the optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied to a request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates the Edge Optimizer configuration associated with a site.

###Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied to a request.

If this feature has been enabled, then the following criteria must also be met before the request will be processed by Edge Optimizer:

- The requested content must use an edge CNAME URL.
- The edge CNAME referenced in the URL must correspond to a site whose configuration has been activated in a rule.

This feature requires the ADN platform and the Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that the request is eligible for Edge Optimizer processing.
Disabled|Restores the default behavior. The default behavior is to deliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

###Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates the Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and the Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests to the corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs to be performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until the Edge Optimizer – Instantiate Configuration feature that references it is removed from the rule.
- The instantiation of a site configuration does not mean that all requests to the corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If the desired site does not appear in the list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin-features"></a>Funkce počátek

Tyto funkce slouží k řízení, jakým způsobem CDN komunikuje se serverem původu.

Name (Název) | Účel
-----|--------
[Udržování požadavky (maximum)](#maximum-keep-alive-requests) | Definuje maximální počet požadavků pro zachování připojení, než je uzavřený.
[Speciálními záhlavími proxy](#proxy-special-headers) | Definuje sadu hlaviček požadavků specifických CDN, které budou předány z hraniční server zdrojový server.


## <a name="specialty-features"></a>Speciální funkce

Tyto funkce poskytují pokročilé funkce pro zkušené uživatele.

Name (Název) | Účel
-----|--------
[Metody HTTP lze uložit do mezipaměti](#cacheable-http-methods) | Určuje sadu další metody HTTP, které mohou být uloženy v mezipaměti v síti.
[Velikost textu lze uložit do mezipaměti žádosti](#cacheable-request-body-size) | Definuje prahovou hodnotu pro určení, zda POST odpověď do mezipaměti.

 
## <a name="url-features"></a>Adresa URL funkce

Tyto funkce umožňují požadavek na přesměrování nebo přepsaná na jinou adresu URL.

Name (Název) | Účel
-----|--------
[Držet se přesměrování](#follow-redirects) | Určuje, zda požadavky můžete přesměrovat k názvu hostitele definované v hlavičce umístění vrácený zdrojový server zákazníka.
[Adresa URL přesměrování](#url-redirect) | Přesměruje požadavky prostřednictvím hlavička umístění.
[Přepisování adres URL](#url-rewrite)  | Přepíše adresu URL požadavku.



## <a name="azure-cdn-rules-engine-features-reference"></a>Referenční dokumentace funkcí modul pravidla ve službě Azure CDN

### <a name="age-response-header"></a>Hlavička odpovědi stáří
**Účel**: Určuje, zda hlavičku odpovědi stáří budou zahrnuty v odpovědi odeslat žadatel.
Hodnota|výsledek
--|--
Povoleno | Hlavička odpovědi stáří je zahrnutý v odpovědi odeslat žadatel.
Zakázáno | Hlavička odpovědi stáří je vyloučen z odpovědi odeslat žadatel.

**Výchozí chování**: zakázáno.

### <a name="bandwidth-parameters"></a>Parametry šířky pásma
**Účel:** Určuje, jestli parametry (například ec_rate a ec_prebuf) omezení šířky pásma bude aktivní.

Parametry omezení šířky pásma určit, jestli rychlost přenosu dat pro požadavek klienta bude omezena na vlastní míru.

Hodnota|výsledek
--|--
Povoleno|Umožňuje u serverů edge případném dalším sdílení dodržovat požadavky omezení šířky pásma.
Zakázáno|Způsobí, že servery edge ignorovat parametry omezení šířky pásma. Požadovaným obsahem se zpracuje normálně (tedy bez omezení šířky pásma).

**Výchozí chování:** povolena.

### <a name="bandwidth-throttling"></a>Omezení šířky pásma
**Účel:** omezení šířky pásma pro odpověď poskytované servery edge.

Při správném nastavení omezení šířky pásma musí být definovány obě z následujících možností.

Možnost|Popis
--|--
KB za sekundu|Tuto možnost nastavte na maximální pásma (Kb za sekundu), který se dá použít k poskytování odpovědi.
Prebuf sekund|Tuto možnost nastavte počet sekund, po které bude odložena servery edge omezení šířky pásma. Účelem tohoto období bez omezení šířky pásma je zabránit přehrávač médií má problémy přerušované nebo vyrovnávací paměti z důvodu omezení šířky pásma.

**Výchozí chování:** zakázané.

### <a name="bypass-cache"></a>Nepoužívat mezipaměti
**Účel:** Určuje, zda požadavek by měl nepoužívat ukládání do mezipaměti.

Hodnota|výsledek
--|--
Povoleno|I v případě, že obsah byl dříve uložené v mezipaměti serverů edge způsobí, že všechny požadavky na Přejít na zdrojový server.
Zakázáno|Způsobí, že servery edge do mezipaměti prostředky podle zásady ukládání do mezipaměti definované v jeho hlavičky odpovědi.

**Výchozí chování:**

- **Velké HTTP:** zakázáno

<!---
- **ADN:** Enabled
--->

### <a name="cacheable-http-methods"></a>Metody HTTP lze uložit do mezipaměti
**Účel:** určuje sadu další metody HTTP, které mohou být uloženy v mezipaměti v síti.

Informace o klíči:

- Tato funkce se předpokládá, že GET odpovědi by měl vždycky být ukládat do mezipaměti. Metodu GET HTTP v důsledku toho by neměl být zahrnuty při nastavování této funkce.
- Tato funkce podporuje jenom metodu POST HTTP. Povolit POST ukládání odpovědí do mezipaměti podle nastavení této funkce: POST 
- Ve výchozím nastavení bude do mezipaměti pouze požadavky, jejichž text je menší než 14 Kb. Použijte funkci velikost textu lze uložit do mezipaměti žádosti nastavit velikost textu maximální žádosti.

**Výchozí chování:** pouze GET odpovědi bude ukládat do mezipaměti.

### <a name="cacheable-request-body-size"></a>Velikost textu lze uložit do mezipaměti žádosti

**Účel:** definuje prahová hodnota pro určení, zda POST odpověď do mezipaměti.

Tato prahová hodnota je určen podle určení velikosti textu maximální požadavku. Požadavky, které obsahují větší textu žádosti nebudou zapisována do mezipaměti.

Informace o klíči:

- Tato funkce se vztahuje pouze při odpovědi POST jsou způsobilé pro ukládání do mezipaměti. Použijte funkci Uložitelný metody HTTP povolení ukládání do mezipaměti požadavek POST.
- Textu požadavku v úvahu pro:
    - hodnoty x--www-form-urlencoded
    - Zajištění jedinečný klíč mezipaměti
- Definování velkého požadavku maximální velikost textu může mít vliv na data výkonu doručení.
    - **Doporučená hodnota:** 14 Kb
    - **Minimální hodnota:** 1 Kb

**Výchozí chování:** 14 Kb

### <a name="cache-control-header-treatment"></a>Zpracování hlavička Cache-Control
**Účel:** řídí generování `Cache-Control` hlavičky, server edge když externí funkce Max-Age je aktivní.

Nejjednodušší způsob, jak dosáhnout tento typ konfigurace se má umístit externí Max-Age a zacházení hlavička Cache-Control funkcí v jednom příkazu.

Hodnota|výsledek
--|--
Přepsání|Zajišťuje, že takto bude probíhat:<br/> -Přepíše hlavička Cache-Control, které jsou generované na zdrojový server. <br/>-Přidá `Cache-Control` záhlaví vyprodukované funkci externí Max-Age do odpovědi.
Předání|Zajišťuje, že `Cache-Control` záhlaví vyprodukované funkci externí Max-Age se nikdy přidá do odpovědi. <br/> Pokud je zdrojový server vytvoří `Cache-Control` záhlaví, se předá pro koncového uživatele. <br/> Pokud je zdrojový server nevytváří `Cache-Control` záhlaví, pak se tato možnost může způsobit, že hlavičku odpovědi k neobsahuje `Cache-Control` záhlaví.
Přidejte Pokud chybí|Pokud `Cache-Control` hlavička nebyla přijata od zdrojového serveru a potom přidá tato možnost `Cache-Control` záhlaví vyprodukované funkci externí Max-Age. Tato možnost je užitečná pro zajištění, že všechny prostředky budou přiřazeny `Cache-Control` záhlaví.
Odebrat| Tato možnost zajistí, že `Cache-Control` hlavičky není součástí hlavičky odpovědi. Pokud `Cache-Control` hlavička již byla přiřazena a potom se odstraní z hlavičky odpovědi.

**Výchozí chování:** přepsat.

### <a name="cache-key-query-string"></a>Řetězec dotazu klíče mezipaměti
**Účel:** Určuje, zda bude klíč mezipaměti zahrnout nebo vyloučit parametrů řetězce dotazu přidružený k požadavku.

Informace o klíči:

- Zadejte jeden nebo více názvy parametrů řetězce dotazu. Název každého parametru by měl být oddělené mezerou.
- Tato funkce určuje, zda parametrů řetězce dotazu bude zahrnout nebo vyloučit z klíče mezipaměti. Další informace naleznete u každé níže uvedené možnosti.

Typ|Popis
--|--
 Zahrnout|  Označuje, že každý zadaný parametr by měl být součástí klíče mezipaměti. Pro každý požadavek, který obsahuje jedinečná hodnota pro parametr řetězce dotazu definované v tato funkce se budou generovat jedinečný klíč mezipaměti. 
 Zahrnout všechny  |Označuje, že je vytvořen jedinečný klíč mezipaměti pro každý požadavek pro prostředek, který obsahuje řetězec dotazu jedinečný. Tento typ konfigurace se nedoporučuje obvykle vzhledem k tomu může dojít k malým procentem přístupů do mezipaměti. Vzhledem k tomu, že bude mít k obsluze další požadavky, tím se zvýší zatížení na původním serveru. Tato konfigurace duplikuje chování ukládání do mezipaměti, které jsou známé jako "jedinečný mezipaměti" na stránce ukládání do mezipaměti řetězce dotazu. 
 Vyloučení | Označuje, že pouze zadané parametry budou vyloučeny z klíče mezipaměti. Všechny ostatní parametrů řetězce dotazu bude součástí klíče mezipaměti. 
 Vyloučit všechny výsledky kategorie  |Označuje, že všechny parametrů řetězce dotazu budou vyloučeny z klíče mezipaměti. Tato konfigurace duplikuje výchozí chování, která se označuje jako "standard-cache" na stránce ukládání do mezipaměti řetězce dotazu ukládání do mezipaměti. 

Výkon stroj pravidel protokolu HTTP umožňuje přizpůsobit způsobem, ve kterém se implementuje ukládání do mezipaměti řetězce dotazu. Například můžete zadat, že dotaz řetězec ukládání do mezipaměti pouze provést na určené umístění nebo typy souborů.

Pokud chcete duplikovat řetězec dotazu ukládání do mezipaměti chování označuje jako "no-cache" na stránce ukládání do mezipaměti řetězce dotazu, je potřeba vytvořit pravidlo, které obsahuje podmínku shodu adresy URL dotazu zástupný znak a funkce mezipaměti jednorázové přihlášení. Podmínka shodu adresy URL dotazu zástupné musí být nastavená na hvězdičku (*).

#### <a name="sample-scenarios"></a>Vzorové scénáře

Následující příklad použití pro tuto funkci obsahuje ukázková žádost a klíče mezipaměti výchozí:

- **Ukázková žádost:** http://wpc.0001.&lt; Domény&gt;/800001/Origin/folder/asset.htm?sessionid=1234 a jazyk = EN & userid = 01
- **Výchozí klíč mezipaměti:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Zahrnout

Ukázková konfigurace:

- **Typ:** patří
- **Parametry:** jazyk

Tento typ konfigurace by generovat následující dotaz řetězec parametr-klíče mezipaměti:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Zahrnout všechny

Ukázková konfigurace:

- **Typ:** zahrnout všechny

Tento typ konfigurace by generovat následující dotaz řetězec parametr-klíče mezipaměti:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Vyloučení

Ukázková konfigurace:

- **Typ:** vyloučit
- **Parametry:** sessionid ID uživatele

Tento typ konfigurace by generovat následující dotaz řetězec parametr-klíče mezipaměti:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Vyloučit všechny výsledky kategorie

Ukázková konfigurace:

- **Typ:** vyloučit všechny výsledky kategorie

Tento typ konfigurace by generovat následující dotaz řetězec parametr-klíče mezipaměti:

    /800001/Origin/folder/asset.htm

### <a name="cache-key-rewrite"></a>Přepište klíče mezipaměti
**Účel:** přepíše klíč mezipaměti přidružený k požadavku.

Klíč mezipaměti je relativní cesta, která určuje prostředek pro účely ukládání do mezipaměti. Jinými slovy servery zkontroluje uložené v mezipaměti verzi prostředek podle cesty podle definice jeho klíče mezipaměti.

Nakonfigurujte tuto funkci tak, že definujete obě z následujících možností:

Možnost|Popis
--|--
Původní cesta| Definování relativní cesta pro typy požadavků, jejichž klíče mezipaměti bude nutné přepsat. Relativní cesta může být definováno výběrem základní původní cestu a poté definování vzor regulárního výrazu.
Nová cesta|Zadejte relativní cestu k nové klíče mezipaměti. Relativní cesta může být definováno výběrem základní původní cestu a poté definování vzor regulárního výrazu. Tuto relativní cestu lze sestavit dynamicky prostřednictvím protokolu HTTP, proměnné
**Výchozí chování:** klíče mezipaměti požadavek je určen podle identifikátoru URI požadavku.

### <a name="comment"></a>Komentář
**Účel:** umožňuje Poznámka přidávaného v pravidle.

Jedno použití pro tuto funkci je poskytnout další informace o obecné účely pravidlo nebo proč konkrétní vyhovují podmínce nebo funkce byla přidána do pravidla.

Informace o klíči:

- Je možné zadat maximálně 150 znaků.
- Použijte pouze alfanumerické znaky.
- Tato funkce nemá vliv na chování pravidla. Smyslem je jenom zajistit oblast, ve kterém můžete zadat informace pro budoucí použití nebo která může pomoci při řešení potíží s pravidlo.

### <a name="complete-cache-fill"></a>Dokončení výplně mezipaměti
**Účel:** Určuje, co se stane, když požadavek výsledkem k neúspěšnému přístupu do mezipaměti částečné na hraniční server.

K neúspěšnému přístupu do mezipaměti částečné popisuje mezipaměti stavu pro určitý prostředek, který nebyl zcela stažen do hraniční server. Pokud prostředek jen částečně v mezipaměti na serveru edge, pak další požadavek pro tento prostředek předá se znovu na zdrojový server.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.
--->
K neúspěšnému přístupu do mezipaměti částečné obvykle dochází, až uživatel zruší stahování nebo pro prostředky, které jsou vyžadovány výhradně pomocí protokolu HTTP rozsah požadavků. Tato funkce je nejvhodnější pro velké prostředky, kde uživatelé nebudou stahovat obvykle je od začátku do konce (například videa). V důsledku toho je tato funkce povolená ve výchozím nastavení na HTTP velké platformě. Na jiných platformách je zakázaná.

Doporučujeme ponechat výchozí konfiguraci pro HTTP velké platformu, protože budou snižovat zatížení na serveru počátek zákazníka a zvýšit rychlost, jakou vašim zákazníkům stažení vašeho obsahu.

Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení, tato funkce nemůže být přidružena následující podmínky shody: Edge Cname, literálu hlavičky požadavku, žádosti o záhlaví zástupný znak, adresa URL dotazu literálu a adresa URL dotazu.

Hodnota|výsledek
--|--
Povoleno|Obnoví výchozí chování. Výchozí chování je vynutit hraničního serveru k zahájení načítání na pozadí prostředku ze zdrojového serveru. Po kterém asset bude v místní mezipaměti na hraniční server.
Zakázáno|Hraniční server bránit v provádění načítání na pozadí pro asset. To znamená, že další požadavek pro tento prostředek z této oblasti způsobí hraniční server můžete požádat ze zdrojového serveru zákazníka.

**Výchozí chování:** povolena.

### <a name="compress-file-types"></a>Komprimovat typy souborů
**Účel:** definuje formáty souborů, které komprimuje na serveru.

Formát souboru lze zadat pomocí jeho typ média Internetu (například Content-Type). Typ média Internetu je nezávislé na platformě metadata, která umožňuje serverům k identifikaci formát souboru konkrétní asset. Seznam běžné typy médií Internetu je uvedený níže.

Typ média Internetu|Popis
--|--
text/plain|Soubory ve formátu prostého textu
text/html| Soubory HTML
text/css|Listů kaskádových stylů (CSS)
Application/x-javascript|JavaScript
aplikace/javascript|JavaScript
Informace o klíči:

- Zadejte víc typů médií Internet omezující každé z nich mezerou. 
- Tato funkce bude komprimovat pouze prostředky, jejíž aktuální velikost je menší než 1 MB. Větší prostředky nebude komprimována servery.
- Určité typy obsahu, například bitové kopie, video a zvukových médiích prostředky (například JPG, MP3, MP4, atd.), jsou již v komprimovaném tvaru. Další kompresi na tyto typy prostředků nebude dojít k výraznému snížení velikosti souboru. Proto se doporučuje, nepovolujte kompresi na tyto typy prostředků.
- Zástupné znaky, jako je například hvězdičky, nejsou podporovány.
- Než přidáte tuto funkci na pravidlo, ujistěte se, nastavit kompresi zakázaná možnost na stránce komprese pro platformu, do které bude použito toto pravidlo.

### <a name="custom-log-field-1"></a>Pole vlastní protokol 1
**Účel:** Určuje formát a obsah, který se přiřadí pole vlastního protokolu v nezpracovaných souboru protokolu.

Hlavním účelem za toto vlastní pole je vám umožní určit, který žádost a hodnoty hlavičky odpovědi se uloží do souborů protokolu.

Ve výchozím nastavení pole vlastní protokol se nazývá "x-ec_custom-1." Ale název tohoto pole lze přizpůsobit z [stránka nastavení protokolu Raw]().

Níže jsou uvedeny formátování, používejte k určení hlavičkách žádostí a odpovědí.

Záhlaví – typ|Formát|Příklady
-|-|-
Hlavička požadavku|%{[RequestHeader]()}[i]() | % {Přijmout Encoding} i <br/> {Odkazující server} i <br/> % {Autorizace} i
Hlavička odezvy|%{[ResponseHeader]()}[o]()| % {Stáří} o <br/> % {Content-Type} o <br/> % {Soubor cookie} o

Informace o klíči:

- Pole vlastní protokol může obsahovat libovolnou kombinaci pole hlavičky a prostý text.
- U tohoto pole platné znaky patří: alfanumerické znaky (0 – 9, a-z a A-Z), pomlčky, dvojtečky, středníky, apostrofy, čárky, tečky, podtržítka, znaky rovná, závorky, závorky a mezery. Symbol procenta a složené závorky jsou povoleny pouze pokud se používá k určení pole hlavičky.
- Pravopis pro každé pole Zadaná hlavička musí odpovídat názvu záhlaví požadovaného požadavků a odpovědí.
- Pokud chcete zadat více záhlaví, pak se doporučuje použít oddělovače označíte, každá hlavička. Můžete například použít zkratkou pro každý záhlaví. Ukázka syntaxi najdete níže.
    - AE: % {přijmout Encoding} i odpověď: % {autorizace} i Berte: % {Content-Type} o 

**Výchozí hodnota:** -

### <a name="debug-cache-response-headers"></a>Ladění hlavičky odpovědi v mezipaměti
**Účel:** Určuje, zda odpověď může zahrnovat hlavičku odpovědi X-ES-Debug, který obsahuje informace o zásady ukládání do mezipaměti pro požadovaný prostředek.

Ladění odpovědi v mezipaměti, záhlaví budou zahrnuty v odpovědi, pokud jsou splněny obě následující:

- Funkce hlavičky ladění odpověď mezipaměti byla povolena na požadované žádosti.
- Výše uvedený požadavek definuje sadu hlavičky odpovědi mezipaměti ladění, které budou zahrnuty v odpovědi.

Ladění odpověď mezipaměti, které mohou být vyžádány hlavičky zahrnutím následující hlavičku a požadované direktivy v požadavku:

X-ES Debug: _Directive1_,_Directive2_,_DirectiveN_

**Příklad:**

X-ES-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Hodnota|výsledek
-|-
Povoleno|Požadavky pro ladění hlavičky odpovědi mezipaměti bude vracet odpovědi obsahující hlavičku X-ES-Debug.
Zakázáno|Hlavička odpovědi X-ES-Debug budou vyloučeny z odpovědi.

**Výchozí chování:** zakázané.

### <a name="default-internal-max-age"></a>Výchozí interní Max-Age
**Účel:** určuje je výchozí interval maximální stáří hraničního serveru na původní server mezipaměti opětovné ověření. Jinými slovy množství času, který bude předat před hraniční server zkontrolujte, zda v mezipaměti asset odpovídá asset uložené na původním serveru.

Informace o klíči:

- Tato akce uskuteční pouze pro odpovědi ze serveru původu nepřiřadili indikace maximální stáří v hlavičce Cache-Control nebo Expires.
- Tato akce neproběhne u prostředků, které se považují za lze uložit do mezipaměti.
- Tato akce nemá vliv na prohlížeči revalidations mezipaměti serveru edge. Tyto typy revalidations jsou určena Cache-Control nebo Expires hlavičky odeslán do prohlížeče, který lze přizpůsobit pomocí funkce Max-Age externí.
- Výsledky této akce nemají pozorovatelné ovlivnění hlavičky odpovědi a vrátí obsah ze serverů edge pro obsah, ale může mít vliv na objem provozu opětovné ověření odeslaných ze serverů edge na původním serveru.
- Nakonfigurujte tuto funkci pomocí:
    - Výběr kód stavu, pro který lze použít výchozí vnitřní, max-age.
    - Určení celočíselnou hodnotu a pak vyberete požadovanou časovou jednotku (například sekund, minut, hodin, atd.). Tato hodnota definuje výchozí interval interní maximální stáří.

- Nastavení na hodnotu "Vypnuto" časovou jednotku intervalu výchozí vnitřní maximální stáří 7 dní pro požadavky, které nebyly přiřazeny indikace maximální stáří v jejich hlavička Cache-Control nebo Expires přiřadí.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení nemůže být přidružena následující podmínky shody této funkce: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí hodnota:** 7 dnů

### <a name="deny-access-403"></a>Odepřít přístup (403)
**Účel**: Určuje, zda všechny požadavky byly zamítnuty 403 Zakázáno odpovědi.

Hodnota | výsledek
------|-------
Povoleno| Způsobí, že všechny požadavky, které splňují kritéria přiřazování zamítnutí 403 Zakázáno odpovědi.
Zakázáno| Obnoví výchozí chování. Výchozí chování je umožnit zdrojový server určit typ odpovědi, který bude vrácen.

**Výchozí chování**: zakázáno

> [!TIP]
   > Možné použití této funkce se má přidružit podmínku shoda hlavičky požadavku, kterou chcete blokovat přístup k protokolu HTTP odkazující servery, které používají vložený odkazy na obsah.

### <a name="expires-header-treatment"></a>Vyprší platnost zacházení záhlaví
**Účel:** ovládací prvky generování hlavičky Expires server edge, když funkci externí Max-Age je aktivní.

Nejjednodušší způsob, jak dosáhnout tento typ konfigurace se má umístit externí Max-Age a vyprší platnost zacházení záhlaví funkcí v jednom příkazu.

Hodnota|výsledek
--|--
Přepsání|Zajišťuje, že takto bude probíhat:<br/>-Přepíše hlavičku Expires generovaný serverem původu.<br/>-Přidá hlavičku Expires vyprodukované funkci externí Max-Age do odpovědi.
Předání|Zajišťuje, že hlavičku Expires vyprodukované funkci externí Max-Age se nikdy přidá do odpovědi. <br/> Pokud je zdrojový server vytvoří hlavičku Expires, se předá pro koncového uživatele. <br/>Pokud je zdrojový server nevytváří hlavičku Expires, tato možnost může způsobit hlavičku odpovědi k neobsahuje hlavičku Expires.
Přidejte Pokud chybí| Pokud hlavička Expires nebyla přijata od zdrojového serveru, tato možnost přidá hlavičku Expires vyprodukované funkci externí Max-Age. Tato možnost je užitečná pro zajištění, že všechny prostředky se přiřadí hlavičku Expires.
Odebrat| Zajišťuje, že není součástí hlavičky odpovědi hlavičku Expires. Pokud již byla přiřazena hlavičku Expires, pak jej se odstraní z hlavičky odpovědi.

**Výchozí chování:** přepsat

### <a name="external-max-age"></a>Externí Max-Age
**Účel:** určuje maximální stáří interval pro prohlížeč edge server mezipaměti opětovné ověření. Jinými slovy množství času, který bude uplynout, než pro novou verzi prostředek z hraniční server můžete zkontrolovat v prohlížeči.

Povolení této funkce vytvoří mezipaměť – ovládací prvek: maximální-stáří a vyprší hlavičky ze serverů edge a jejich odeslání do klienta protokolu HTTP. Ve výchozím nastavení bude tato záhlaví přepsat nebyla vytvořena v původním serveru. Ale způsob zpracování hlavička Cache-Control a funkce vyprší platnost zacházení záhlaví lze toto chování změnit.

Informace o klíči:

- Tato akce nemá vliv na původní server mezipaměti revalidations hraničního serveru. Tyto typy revalidations vyplývají z mezipaměti – ovládací prvek nebo Expires hlavičky přijatých ze zdrojového serveru a lze přizpůsobit pomocí výchozí vnitřní Max-Age a funkcí Force interní Max-Age.
- Nakonfigurujte tuto funkci zadáním celočíselnou hodnotu a vyberte požadovanou časovou jednotku (například sekund, minut, hodin, atd.).
- Nastavení této funkce na zápornou hodnotu způsobí, že servery edge k odeslání mezipaměti – ovládací prvek: Ne-mezipaměti a Expires čas, který je nastavený v minulosti s každou odezva do prohlížeče. I když klientem HTTP nebude odpověď do mezipaměti, toto nastavení nebude mít vliv na schopnost se servery edge odpověď ze zdrojového serveru do mezipaměti.
- Nastavení časovou jednotku na hodnotu "Vypnuto" bude tuto funkci zakázat. Hlavičky Cache – ovládací prvek nebo Expires uložené v mezipaměti k odpovědi na zdrojový server předá do prohlížeče.

**Výchozí chování:** vypnuto

### <a name="follow-redirects"></a>Držet se přesměrování
**Účel:** Určuje, zda požadavky můžete přesměrovat k názvu hostitele definované v hlavičce umístění vrácený zdrojový server zákazníka.

Informace o klíči:

- Požadavky můžete přesměrovat pouze okraj záznamů CNAME, které odpovídají stejnou platformu.

Hodnota|výsledek
-|-
Povoleno|Požadavky můžete přesměrovat.
Zakázáno|Požadavky nebude přesměrovat.

**Výchozí chování:** zakázané.

### <a name="force-internal-max-age"></a>Vynutit interní Max-Age
**Účel:** určuje maximální stáří interval pro server edge na původní server mezipaměti opětovné ověření. Množství času, který bude předat před hraniční server jinými slovy, můžete zkontrolovat, zda v mezipaměti asset odpovídá asset uložené na původním serveru.

Informace o klíči:

- Tato funkce se přepíše maximální stáří intervalu určeném v `Cache-Control` nebo `Expires` hlavičky generované ze zdrojového serveru.
- Tato funkce nemá vliv na prohlížeči revalidations mezipaměti serveru edge. Tyto typy revalidations vyplývají z `Cache-Control` nebo `Expires` hlavičky odesláno prohlížeči.
- Tato funkce nemá pozorovatelné vliv na odpověď doručil hraniční server k žadatel. Ale může mít vliv na objem provozu opětovné ověření odeslaných ze serverů edge na zdrojový server.
- Nakonfigurujte tuto funkci pomocí:
    - Výběr kód stavu, pro které se použijí interní maximální stáří.
    - Určení celočíselná hodnota a výběrem požadovaného časovou jednotku (například sekund, minut, hodin, atd.). Tato hodnota definuje interval maximální stáří žádosti.

- Tato funkce nastavení časovou jednotku na hodnotu "Vypnuto" zakáže. Interní maximální stáří intervalu nesmí být přiděleno požadované prostředky. Pokud původní hlavičku neobsahuje pokyny pro ukládání do mezipaměti, pak asset bude do mezipaměti podle active nastavení ve funkci výchozí vnitřní Max-Age.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení nemůže být přidružena následující podmínky shody této funkce: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** vypnuto

### <a name="h264-support-http-progressive-download"></a>Podpora H.264 (HTTP progresivního stahování)
**Účel:** Určuje typy H.264 formáty souborů, které lze použít k vysílání datového proudu obsahu.

Informace o klíči:

- Definujte sadu povolených H.264 přípony názvů souborů oddělených mezerami v možnost přípony souborů. Přípony souborů možnost přepíše výchozí chování. Podpora MP4 a F4V udržujte zahrnutím tyto přípony názvů souborů, když nastavení této možnosti. 
- Nezapomeňte použít tečku při zadávání každou příponu názvu souboru (například .f4v MP4).

**Výchozí chování:** progresivní stahování HTTP podporuje média MP4 a F4V ve výchozím nastavení.

### <a name="honor-no-cache-request"></a>Dodržet No Cache požadavku
**Účel:** Určuje, zda klientem HTTP je ne mezipaměti budou předány požadavky na zdrojový server.

Požadavek ne mezipaměti nastane, když klient HTTP odešle mezipaměť – ovládací prvek: Ne-mezipaměti nebo Pragma:no-mezipaměti hlavičky v požadavku HTTP.

Hodnota|výsledek
--|--
Povoleno|Umožňuje ne mezipaměť klienta HTTP žádosti předají na zdrojový server, a na zdrojový server vrátí hlavičky odpovědi a text prostřednictvím hraničního serveru zpět do klienta protokolu HTTP.
Zakázáno|Obnoví výchozí chování. Výchozí chování je zabránit požadavků na mezipaměť ne předávaná na zdrojový server.

Pro všechny přenosy produkční důrazně doporučujeme opustit tuto funkci ve svém výchozím zakázáno stavu. Původ servery, jinak nebude Stíněný, z koncovým uživatelům, kteří mohou nechtěně aktivovat mnoho požadavků bez mezipaměti, při aktualizaci webové stránky, nebo z mnoha přehrávače oblíbených médií, které jsou kódované hlavička ne mezipaměti s každou video žádost odeslat. Tuto funkci však může být užitečné pro použití určitých mimo produkční pracovní nebo testování adresářů, aby bylo možné povolit čerstvého obsahu, který mají být vyžádány na vyžádání ze zdrojového serveru.

Stav mezipaměti, která bude hlášena, pro požadavek, který může být přesměrovaní na zdrojový server z důvodu této funkce je TCP_Client_Refresh_Miss. Stavy mezipaměti zprávu, která je k dispozici v základní reporting modulu, poskytuje statistické údaje podle stavu mezipaměti. To umožňuje sledovat počet a procento žádostí, které jsou předávány na zdrojový server z důvodu této funkce.

**Výchozí chování:** zakázané.

### <a name="ignore-origin-no-cache"></a>Ignorovat počátek No-Cache
**Účel:** Určuje, zda CDN bude ignorovat následující direktivy zpracování zdrojovému serveru:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Informace o klíči:

- Nakonfigurujte tuto funkci tak, že definujete mezerami oddělený seznam stavové kódy, pro které se budou ignorovat výše direktivy.
- Sada platný stavové kódy pro tuto funkci: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 a 505.
- Tuto funkci zakážete nastavením na prázdnou hodnotu.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení nemůže být přidružena následující podmínky shody této funkce: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** výchozí chování je respektovat výše direktivy.

### <a name="ignore-unsatisfiable-ranges"></a>Ignorovat Unsatisfiable rozsahů 
**Účel:** určuje odpověď, který bude vrácen na klienty, pokud žádost vygeneruje stavový kód 416 požadovaný rozsah nelze uspokojit.

Ve výchozím nastavení tento kód stavu se vrátí, pokud zadaný rozsah bajtů požadavek nelze uspokojit, server edge a nebyl zadán pole hlavičky požadavku If-Range.

Hodnota|výsledek
-|-
Povoleno|Servery edge zabrání reagovat na požadavek neplatný rozsah bajtů s 416 požadovaný rozsah nelze uspokojit stavový kód. Servery se místo toho doručování požadovaný prostředek a vrátit 200 OK klientovi.
Zakázáno|Obnoví výchozí chování. Výchozí chování je respektovat 416 požadovaný rozsah nelze uspokojit stavový kód.

**Výchozí chování:** zakázané.

### <a name="internal-max-stale"></a>Interní Max zastaralé
**Účel:** ovládací prvky, jak dlouho po čase normální vypršení platnosti mezipaměti asset, může vyhovovat z hraniční server při hraničního serveru se nepodařilo znovu ověřit v mezipaměti asset je zdrojový server.

Za normálních okolností po uplynutí této doby pro maximální stáří prostředek služby, odešle edge server žádost o opětovné ověření na zdrojový server. Původ serveru se pak odpověď s buď 304 nedojde ke změně umožnit hraničního serveru čerstvou zapůjčení na uložené v mezipaměti asset or else s 200 OK poskytnout aktualizovaná verze sady v mezipaměti asset hraničního serveru.

Pokud je server edge nelze navázat připojení je zdrojový server při pokusu o takové opětovné ověření, pak tato interní Max-zastaralé funkce řídí, zda a jak dlouho hraniční server mohou i nadále poskytovat asset nyní zastaralé.

Všimněte si, že tento časový interval spustí, když vyprší platnost assetu, max-age, není, když nastane selhání opětovné ověření. Množství času určeného kombinace maximální stáří plus maximální zastaralé je proto maximální doba, během které prostředek může zpracovat bez úspěšné opětovné ověření. Například pokud prostředek se ukládá do mezipaměti v 9:00 s maximální stáří 30 minut a maximální zastaralé 15 minut, pak se nezdařilo opětovné ověření pokus o 9:44 by mělo za následek koncový uživatel přijetí zastaralé asset uložené v mezipaměti, při selhání opětovné ověření pokus o 9:46 by způsobilo en d uživatel, který obdrží 504 limitu brány.

Žádnou hodnotu pro tuto funkci je nahrazena nakonfigurován `Cache-Control:must-revalidate` nebo `Cache-Control:proxy-revalidate` hlavičky přijatých ze zdrojového serveru. Pokud některý z těchto záhlaví je přijme ze zdrojového serveru, pokud prostředek je původně uložené v mezipaměti, edge server neprovede zastaralé v mezipaměti asset. V takovém případě pokud hraničního serveru se nepodařilo znovu ověřit s počátek po vypršení intervalu maximální stáří assetu, vrátí edge server chybu 504 limitu brány.

Informace o klíči:

- Nakonfigurujte tuto funkci pomocí:
    - Výběr kód stavu, pro které se použijí maximální zastaralé.
    - Určení celočíselnou hodnotu a pak vyberete požadovanou časovou jednotku (například sekund, minut, hodin, atd.). Tato hodnota definuje interní max zastaralé, se použijí.

- Nastavení časovou jednotku na hodnotu "Vypnuto" bude tuto funkci zakázat. V mezipaměti asset nebudou poskytnuty času její normální vypršení platnosti.
- Z důvodu způsobem v mezipaměti, které jsou sledovány nastavení nemůže být přidružena následující podmínky shody této funkce: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** dvě minuty.

### <a name="log-query-string"></a>Řetězec protokolu dotazu
**Účel:** Určuje, zda řetězec dotazu se uloží spolu s adresu URL v protokolech přístup.

Hodnota|výsledek
-|-
Povoleno|Umožňuje ukládání řetězce dotazů při nahrávání adresy URL v přístupu k protokolu. Pokud adresu URL neobsahuje řetězec dotazu, tato možnost nebude mít vliv.
Zakázáno|Obnoví výchozí chování. Výchozí chování je ignorovat řetězce dotazů při zaznamenávání adresy URL v přístupu k protokolu.

**Výchozí chování:** zakázané.

### <a name="maximum-keep-alive-requests"></a>Udržování požadavky (maximum)
**Účel:** definuje maximální počet požadavků pro zachování připojení, než je uzavřený.

Nastavení maximální počet požadavků na nízkou hodnotu, se důrazně nedoporučuje a může způsobit snížení výkonu.

Informace o klíči:

- Tuto hodnotu zadejte jako celé číslo.
- Nezahrnujte čárky nebo tečky v zadané hodnotě.

**Výchozí hodnota:** 10 000 požadavků

### <a name="modify-client-request-header"></a>Upravit hlavička požadavku klienta
**Účel:** každý požadavek obsahuje sadu hlaviček požadavků, které popisují ho. Tato funkce může buď:

- Připojení nebo přepsat hodnotu přiřazenou hlavičku požadavku. Pokud zadaný požadavek hlavička neexistuje, tato funkce ho pak přidá k požadavku.
- Odstraňte hlavičku požadavku z požadavku.

Požadavky, které se předávají do zdrojového serveru se projeví změny provedené při této funkce.

Na hlavička požadavku je možné provádět jednu z následujících akcí:

Možnost|Popis|Příklad
-|-|-
Připojit|Zadaná hodnota přidá na konec existující hodnotu hlavičky žádosti.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/> **Žádosti o hodnotu hlavičky (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky požadavku:** Value1Value2
Přepsání|Hodnota hlavičky požadavku se nastaví na zadanou hodnotu.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/>**Žádosti o hodnotu hlavičky (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky požadavku:** hodnota2 <br/>
Odstranění|Odstraní určenou hlavičku požadavku.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/> **Změňte konfiguraci klienta hlavička požadavku:** odstranit v hlavičce žádosti. <br/>**Výsledek:** hlavičku zadaný požadavek nebude předají na zdrojový server.

Informace o klíči:

- Zkontrolujte, zda je hodnota zadaná v názvu možnosti přesnou shodu pro hlavičku požadované žádosti.
- Případ nebere v úvahu za účelem identifikace hlavičku. Například některé z následujících variace název hlavičky Cache-Control slouží k identifikaci:
    - ovládací prvek mezipaměti
    - CACHE-CONTROL
    - cachE-Control
- Při zadávání názvu záhlaví, použijte pouze alfanumerické znaky, pomlčky nebo podtržítka.
- Odstraňování hlavičku zabraňují předávaná zdrojový server edge servery.
- Následující hlavičky jsou vyhrazeny a nelze změnit pomocí této funkce:
    - předané
    - hostitele
    - prostřednictvím
    - Upozornění
    - x předávaných pro
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.

### <a name="modify-client-response-header"></a>Upravit hlavičku odpovědi klienta
Každá odpověď obsahuje sadu hlaviček odpovědí, které popisují ho. Tato funkce může buď:

- Připojení nebo přepsat hodnotu přiřazenou hlavičky odpovědi. Pokud zadané hlavičky odpovědi neexistuje, tato funkce ho pak přidá do odpovědi.
- Odstraňte hlavičky odpovědi z odpovědi.

Ve výchozím nastavení jsou definovány hodnoty hlavičky odpovědi zdrojový server a servery edge.

Na hlavičku odpovědi je možné provádět jednu z následujících akcí:

Možnost|Popis|Příklad
-|-|-
Připojit|Zadaná hodnota přidá na konec existující hodnotu hlavičky odpovědi.|**Hodnota hlavičky odpovědi (klient):**Value1 <br/> **Hodnota hlavičky odpovědi (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky odpovědi:** Value1Value2
Přepsání|Hodnota hlavičky odpovědi se nastaví na zadanou hodnotu.|**Hodnota hlavičky odpovědi (klient):**Value1 <br/>**Hodnota hlavičky odpovědi (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky odpovědi:** hodnota2 <br/>
Odstranění|Odstraní zadané hlavičky odpovědi.|**Hodnota hlavičky odpovědi (klient):** Value1 <br/> **Změňte konfiguraci klienta hlavička odpovědi:** odstranit dotyčném hlavičku odpovědi. <br/>**Výsledek:** zadané hlavičky odpovědi nebude předají do žadatel.

Informace o klíči:

- Zkontrolujte, zda je hodnota zadaná v názvu možnosti přesnou shodu pro hlavičku požadované odpovědi. 
- Případ nebere v úvahu za účelem identifikace hlavičku. Například některé z následujících variace název hlavičky Cache-Control slouží k identifikaci:
    - ovládací prvek mezipaměti
    - CACHE-CONTROL
    - cachE-Control
- Odstraňování hlavičku zabraňuje v jeho předávaná žadatel.
- Následující hlavičky jsou vyhrazeny a nelze změnit pomocí této funkce:
    - Přijměte kódování
    - stáří
    - připojení
    - kódování obsahu
    - Délka obsahu
    - rozsah obsahu
    - Datum
    - server
    - přípojného
    - kódování přenosu
    - upgrade
    - lišit
    - prostřednictvím
    - Upozornění
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.

### <a name="partial-cache-sharing"></a>Sdílení částečné mezipaměti
**Účel:** Určuje, zda žádost může generovat obsahu částečně v mezipaměti.

Toto částečné mezipaměti může potom použít ke splnění nové požadavky pro tento obsah dokud požadovaný obsah je plně v mezipaměti.

Hodnota|výsledek
-|-
Povoleno|Požadavky můžete vygenerovat obsahu částečně v mezipaměti.
Zakázáno|Požadavky můžete generovat jenom plně v mezipaměti verzi požadovaného obsahu.

**Výchozí chování:** zakázané.

### <a name="prevalidate-cached-content"></a>Prevalidate obsah uložený v mezipaměti
**Účel:** Určuje, zda obsah uložený v mezipaměti vhodné pro včasné opětovné ověření před jeho hodnota TTL nevyprší.

Zadejte množství času před vypršením platnosti požadovaný obsah TTL, během kterého může být poskytnut časná opětovné ověření.

Informace o klíči:

- Výběr "Off", jako časovou jednotku vyžaduje opětovné ověření proběhne po obsah uložený v mezipaměti TTL vypršela platnost. Nesmí být zadaný čas a budou ignorovány.

**Výchozí chování:** vypnout. Opětovné ověření může proběhnout pouze po vypršení platnosti obsahu v mezipaměti TTL.

### <a name="proxy-special-headers"></a>Speciálními záhlavími proxy
**Účel:** definuje sadu hlaviček požadavků specifických CDN, které budou předány z hraniční server zdrojový server.

Informace o klíči:

- Každá hlavička požadavku CDN konkrétní definované v tato funkce se předají na původním serveru.
- Hlavička požadavku CDN konkrétní zabránit předávaná zdrojový server odebráním z tohoto seznamu.

**Výchozí chování:** všechny hlavičky požadavku specifické CDN se předají na zdrojový server.

### <a name="refresh-zero-byte-cache-files"></a>Aktualizujte souborů z mezipaměti nula bajtů
**Účel:** určuje zpracování požadavku klienta HTTP pro prostředek 0 bajtů mezipaměti servery edge.

Platné hodnoty jsou:

Hodnota|výsledek
--|--
Povoleno|Způsobí, že server edge znovu načíst asset ze zdrojového serveru.
Zakázáno|Obnoví výchozí chování. Výchozí chování je poskytovat až platný mezipaměti prostředky na vyžádání.
Tato funkce není vyžadován pro správné ukládání do mezipaměti a doručování obsahu, ale můžou být užitečné jako alternativní řešení. Například dynamického obsahu generátory na počátku serverech může způsobit nechtěně odesílána servery edge odpovědí 0 bajtů. Tyto typy odpovědí jsou obvykle ukládají do mezipaměti servery edge. Pokud znáte odpovědi 0 bajtů se nikdy platné odezvy 

takový obsah pak tato funkce zabránit tyto typy prostředků zpracování vašim klientům.

**Výchozí chování:** zakázané.

### <a name="set-cacheable-status-codes"></a>Nastavit lze uložit do mezipaměti stavové kódy
**Účel:** definuje sadu stavové kódy, které mohou způsobovat obsah uložený v mezipaměti.

Ve výchozím nastavení ukládání do mezipaměti je povolena pouze pro odpovědi 200 OK.

Definujte sadu požadovanou stavové kódy oddělených mezerami.

Informace o klíči:

- Povolte funkci ignorovat No Cache původu. Pokud je tato funkce není povoleno, nemusí mezipaměti odpovědi 200 OK.
- Sada platný stavové kódy pro tuto funkci: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 a 505.
- Tuto funkci nelze použít k zakázání ukládání do mezipaměti pro odpovědi, které generují 200 OK stavový kód.

**Výchozí chování:** ukládání do mezipaměti je povolená jenom pro odpovědi, které generují 200 OK stavový kód.

### <a name="set-client-ip-custom-header"></a>Nastavit vlastní záhlaví IP klienta
**Účel:** přidá hlavičku vlastní, který identifikuje klienta, který podle IP adresy na požadavek.

Možnost název hlavičky definuje název hlavičky vlastní žádosti, kde je uložený IP adresa klienta.

Tato funkce umožňuje zákazníkovi zdrojový server a zjistěte, IP adresa klienta adresy prostřednictvím hlavička vlastní požadavku. Pokud je požadavek z mezipaměti, nebudou na zdrojový server informováni o IP adresu klienta. Proto se doporučuje, aby tuto funkci používat s ADN nebo prostředky, které nebudou zapisována do mezipaměti.

Ujistěte se, že zadaná hlavička název neodpovídá žádnému tyto názvy:

- Názvy záhlaví standardní žádosti. Seznam názvů standardní hlavičku naleznete v [dokumentu RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Názvy vyhrazené záhlaví:
    - předané pro
    - hostitele
    - lišit
    - prostřednictvím
    - Upozornění
    - x předávaných pro
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.

### <a name="stale-content-delivery-on-error"></a>Zastaralé doručování obsahu při chybě
**Účel:** Určuje, zda budou doručeny vypršení platnosti obsahu v mezipaměti, když dojde k chybě během opětovné ověření mezipaměti nebo při načítání požadovaný obsah ze zdrojového serveru zákazníka.

Hodnota|výsledek
-|-
Povoleno|Zastaralé obsah je žadateli zpracovat, když dojde k chybě při připojení ke zdrojovému serveru.
Zakázáno|Chyba na zdrojový server se předají do žadatel.

**Výchozí chování:** zakázáno

### <a name="stale-while-revalidate"></a>Zastaralých při Revalidate
**Účel:** tím, že servery edge při opětovné ověření probíhá doručují zastaralé obsah žadatel zlepšuje výkon.

Informace o klíči:

- Chování této funkce se bude lišit podle vybrané časovou jednotku.
    - **Časová jednotka:** zadejte dobu a vyberte časovou jednotku (například sekund, minut, hodin, atd.) umožňující zastaralé doručování obsahu. Tento typ instalačního programu umožňuje od CDN k prodloužit dobu, po kterou může poskytovat obsahu před vyžadování ověření podle následujícího vzorce:**TTL** + **zastaralé při znovu ověřit čas** 
    - **Vypnutí:** vyberte "vypnuto" vyžadovat opětovné ověření před žádost pro zastaralé obsahu může zpracovat.
        - Nezadávejte časový interval, protože je nepoužitelných a budou ignorovány.

**Výchozí chování:** vypnout. Opětovné ověření musí proběhnout, než je možné dodávat požadovaný obsah.

### <a name="token-auth"></a>Token ověřování
**Účel:** Určuje, zda ověřování na základě tokenu se použijí pro žádost.

Pokud je povoleno ověřování na základě tokenu, bude použito pouze požadavky, které poskytují zašifrovaný token a v souladu s požadavky na určeného tento token.

Šifrovací klíč, který se používá k šifrování a dešifrování hodnoty tokenu je určen podle primární klíč a možnosti zálohování klíče na stránce tokenu ověřování. Mějte na paměti, že šifrovací klíče jsou specifické pro platformu.

Hodnota | výsledek
------|---------
Povoleno | Chrání požadovaný obsah s ověřováním na základě tokenu. Pouze požadavky od klientů, které poskytují platný token a splňovat požadavky na jeho bude dodržet. FTP transakce jsou vyloučeny z ověřování na základě tokenu.
Zakázáno| Obnoví výchozí chování. Výchozí chování je umožnit konfiguraci ověřování na základě tokenu k určení, zda požadavek nebude zabezpečené.

**Výchozí chování:** zakázané.

### <a name="token-auth-denial-code"></a>Odmítnutí kód tokenu ověřování
**Účel:** Určuje typ odpovědi, který bude vrácen uživateli při požadavku byl odepřen v důsledku ověřování na základě tokenu.

K dispozici odpověď kódy jsou uvedeny níže.

Kód odezvy|Název odpovědí|Popis
----------------|-----------|--------
301|Trvale přesunut|Tento kód stavu přesměruje na adresu URL zadanou v hlavičce umístění neoprávnění uživatelé.
302|Najít|Tento kód stavu přesměruje na adresu URL zadanou v hlavičce umístění neoprávnění uživatelé. Tento kód stavu je standardní způsob provedení přesměrování.
307|Dočasné přesměrování|Tento kód stavu přesměruje na adresu URL zadanou v hlavičce umístění neoprávnění uživatelé.
401|Neautorizováno|Kombinování tento kód stavu se hlavička WWW-Authenticate odpovědi umožňuje zobrazit výzvu uživateli pro ověřování.
403|Je zakázané|Toto je standardní 403 Zakázáno stavovou zprávu, která neoprávněný uživatel uvidí při pokusu o přístup k chráněnému obsahu.
404|Soubor nebyl nalezen.|Tento kód stavu označuje, že klient HTTP byl schopen komunikovat se serverem, ale nebyl nalezen požadovaný obsah.

#### <a name="url-redirection"></a>Adresa URL přesměrování

Tato funkce podporuje adresy URL přesměrování na adresu URL uživatelem definované, když je nakonfigurovaný k vrácení 3xx stavový kód. Tato adresa URL uživatelem definované lze zadat tak, že provedete následující kroky:

1. Vyberte kód odpovědi 3xx pro funkci Denial kód tokenu ověřování.
2. Vyberte "Místo" pomocí volby volitelný název hlavičky.
3. Nastavte možnost Volitelná hodnota hlavičky na požadovanou adresu URL.

Pokud adresu URL pro 3xx stavový kód není definován, se na stránce standardní odpověď pro 3xx stavový kód vrátí uživateli.

Adresa URL přesměrování platí jenom pro 3xx kódů odpovědi.

Možnost Volitelná hodnota hlavičky podporuje alfanumerické znaky, znaky uvozovek a mezery.

#### <a name="authentication"></a>Authentication

Tato funkce podporuje možnost zahrnout do odpovědi k neautorizovanému požadavku pro obsah chráněný na základě tokenu ověřování hlavička WWW-Authenticate. Pokud hlavička WWW-Authenticate byla nastavena na "basic" v konfiguraci, bude neoprávněný uživatel výzva pro pověření účtu.

Výše uvedené konfigurace lze dosáhnout následujících kroků:

1. Vyberte "401" jako kód odpovědi pro funkci Denial kód tokenu ověřování.
2. Vyberte možnost volitelný název hlavičky "WWW-Authenticate".
3. Nastavte možnost Volitelná hodnota hlavičky "základní".

Hlavička WWW-Authenticate platí pouze pro kódy 401 odpovědi.

### <a name="token-auth-ignore-url-case"></a>Token Auth ignorovat případ adresy URL
**Účel:** určí, zda jsou adresa URL porovnání provedené na základě tokenu ověřování malá a velká písmena.

Parametry vliv této funkce jsou:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Platné hodnoty jsou:

Hodnota|výsledek
---|----
Povoleno|Způsobí, že server edge ignorovat velká / při porovnávání adres URL pro ověřování na základě tokenu parametry.
Zakázáno|Obnoví výchozí chování. Výchozí chování je pro porovnání adresu URL pro ověřování tokenem být malá a velká písmena.

**Výchozí chování:** zakázané.
 
### <a name="token-auth-parameter"></a>Parametr tokenu ověřování
**Účel:** Určuje, zda by měl parametr řetězce dotazu ověřování na základě tokenu přejmenovat.

Informace o klíči:

- Možnost Hodnota definuje název parametru řetězce dotazu, pomocí kterého je možné zadat token.
- Možnost Hodnota nemůže být nastavena na "ec_token."
- Ujistěte se, že název definovaný v možnosti hodnota obsahuje pouze platné znaky adresy URL.

Hodnota|výsledek
----|----
Povoleno|Možnost Hodnota definuje název parametru řetězce dotazu, přes který by měl být definován tokeny.
Zakázáno|Token je možné zadat jako parametr řetězce dotazu Nedefinovaná v adrese URL žádosti.

**Výchozí chování:** zakázané. Token je možné zadat jako parametr řetězce dotazu Nedefinovaná v adrese URL žádosti.

### <a name="url-redirect"></a>Adresa URL přesměrování
**Účel:** přesměruje požadavky prostřednictvím hlavička umístění.

Konfigurace této funkce vyžaduje nastavení následujících možností:

Možnost|Popis
-|-
Kód|Vyberte kód odpovědi, který bude vrácen do žadatel.
Zdroj & vzor| Tato nastavení definovat vzor požadavek URI, který vystihuje typ požadavků, které může být přesměrována. Bude přesměrovat pouze požadavky, jejichž adresa URL splňuje obě následující kritéria: <br/> <br/> **Zdroj (nebo přístup k obsahu bodu):** vyberte relativní cestu, která identifikuje zdrojový server. To je v části "/XXXX/" a název koncového bodu. <br/> **Zdroj (vzor):** vzor, který identifikuje požadavky relativní cestou musí být definován. Tento vzor regulárního výrazu musí definovat cestu, která spustí přímo po dříve vybrané přístup k obsahu bodu (viz výše). <br/> -Zkontrolujte, že žádost o identifikátor URI (tedy zdroj & vzor) dříve definovaná kritéria není v konfliktu s veškeré podmínky shody definované pro tuto funkci. <br/> -Zadat vzor; Pokud použijete na prázdnou hodnotu jako vzor, se splní všechny řetězce.
Cíl| Zadejte adresu URL, na kterou se přesměruje výše uvedených požadavků. <br/> Vytvořte dynamicky pomocí této adresy URL: <br/> -Vzor regulárního výrazu <br/>-HTTP proměnné <br/> Nahraďte hodnoty zachycení ve vzoru zdrojové do cílové vzor pomocí $ _n_  kde  _n_  identifikuje hodnotu podle pořadí, ve kterém byla zaznamenána. Například $1 představuje první hodnotu zachyceny ve vzorku zdroje, zatímco druhá hodnota představuje $2. <br/> 
Důrazně doporučujeme používat absolutní adresu URL. Použití relativní adresa URL může přesměrovat adresy URL CDN platná cesta UNC.

**Vzorový scénář**

Tento příklad ukazuje, jak přesměrovat okraj URL CNAME, který se přeloží na tuto základní adresu URL CDN: http://marketing.azureedge.net/brochures

Určení požadavků, bude přesměrován na tento základní hraniční CNAME URL: http://cdn.mydomain.com/resources

Tato adresa URL přesměrování může dosáhnout pomocí následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Klíčové body:**

- Požadavek definuje funkci přesměrování URL adresy URL, které bude přesměrován. V důsledku toho nejsou vyžadovány další shodu podmínky. I když podmínky shody byl definován jako "Vždy", bude přesměrován pouze požadavky, které přejděte do složky "brožury" na "marketing" počátek zákazníka. 
- Všechny odpovídající požadavky bude přesměrován na hranici, které CNAME URL definované v cílovém možnosti. 
    - Ukázkový scénář #1: 
        - Ukázková žádost (CDN URL): http://marketing.azureedge.net/brochures/widgets.pdf 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/widgets.pdf  
    - Vzorový scénář #2: 
        - Ukázková žádost (hraniční CNAME URL): http://marketing.mydomain.com/brochures/widgets.pdf 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/widgets.pdf vzorový scénář
    - Vzorový scénář #3: 
        - Ukázková žádost (hraniční CNAME URL): http://brochures.mydomain.com/campaignA/final/productC.ppt 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- Proměnná požadavku schématu (% {schéma}) byl využity v cílovém možnost. Tím se zajistí, že schéma žádosti zůstává beze změny po přesměrování.
- Segmenty adres URL, které zaznamenalo z požadavku se připojují na novou adresu URL prostřednictvím "1 USD."
 
### <a name="url-rewrite"></a>Přepisování adres URL
**Účel:** přepíše adresu URL požadavku.

Informace o klíči:

- Konfigurace této funkce vyžaduje nastavení následujících možností:

Možnost|Popis
-|-
 Zdroj & vzor | Tato nastavení definovat vzor požadavek URI, který vystihuje typ požadavků, které může být přepsána. Bude nutné přepsat pouze požadavky, jejichž adresa URL splňuje obě následující kritéria: <br/>     - **Zdroj (nebo přístup k obsahu bodu):** vyberte relativní cestu, která identifikuje zdrojový server. To je v části "/XXXX/" a název koncového bodu. <br/> - **Zdroj (vzor):** vzor, který identifikuje požadavky relativní cestou musí být definován. Tento vzor regulárního výrazu musí definovat cestu, která spustí přímo po dříve vybrané přístup k obsahu bodu (viz výše). <br/> Ověřte, že žádost o identifikátor URI (tedy zdroj & vzor) dříve definovaná kritéria není v konfliktu s některá z podmínek shodu definované pro tuto funkci. Určete vzorec; Pokud použijete na prázdnou hodnotu jako vzor, se splní všechny řetězce. 
 Cíl  |Zadejte relativní adresu URL, na který se přepsal výše uvedené požadavky: <br/>    1. Výběr bodu přístup k obsahu, který identifikuje zdrojový server. <br/>    2. Definování relativní cestu pomocí: <br/>        -Vzor regulárního výrazu <br/>        -HTTP proměnné <br/> <br/> Nahraďte hodnoty zachycení ve vzoru zdrojové do cílové vzor pomocí $ _n_  kde  _n_  identifikuje hodnotu podle pořadí, ve kterém byla zaznamenána. Například $1 představuje první hodnotu zachyceny ve vzorku zdroje, zatímco druhá hodnota představuje $2. 
 Tato funkce umožňuje servery edge přepsání adresy URL bez tradičních přesměrování. To znamená, že žadatel dostanou stejný kód odpovědi jako v případě, kdyby byla požadována rewritten adresy URL.

**Vzorový scénář 1**

Tento příklad ukazuje, jak přesměrovat okraj URL CNAME, který se přeloží na tuto základní adresu URL CDN: http://marketing.azureedge.net/brochures/

Určení požadavků, bude přesměrován na tento základní hraniční CNAME URL: http://MyOrigin.azureedge.net/resources/

Tato adresa URL přesměrování může dosáhnout pomocí následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Vzorový scénář 2**

Tento příklad ukazuje, jak přesměrovat okraj CNAME URL z velká písmena na malá písmena pomocí regulárních výrazů.

Tato adresa URL přesměrování může dosáhnout pomocí následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Klíčové body:**

- Funkce přepisování adres URL definuje požadavek adresy URL, které bude přepsán. V důsledku toho nejsou vyžadovány další shodu podmínky. I když podmínky shody byl definován jako "Vždy", bude nutné přepsat pouze požadavky, které přejděte do složky "brožury" na "marketing" počátek zákazníka.

- Segmenty adres URL, které zaznamenalo z požadavku se připojují na novou adresu URL prostřednictvím "1 USD."

#### <a name="compatibility"></a>Kompatibilita

Tato funkce zahrnuje odpovídající kritériím, které je nutné splnit, než ji jde použít na žádost. Chcete-li zabránit nastavení konfliktní kritéria shody, tato funkce není kompatibilní s následující podmínky shody:

- JAKO počet
- Původu CDN
- IP adresa klienta
- Původ zákazníka
- Schéma požadavku
- Adresa URL cesta adresáře
- Rozšíření cesty adresy URL
- Název souboru cestu adresy URL
- Literál cestu adresy URL
- Regulární výraz cesty adresy URL
- Cesta URL zástupný znak
- Adresa URL dotazu literál
- Parametr URL dotazu
- Adresa URL dotazu Regex
- Adresa URL dotazu zástupný znak


## <a name="next-steps"></a>Další kroky
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel](cdn-rules-engine.md)
* [Přehled Azure CDN](cdn-overview.md)
