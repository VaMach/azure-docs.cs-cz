---
title: "Referenční model dat šablony Azure API Management | Microsoft Docs"
description: "Další informace o reprezentace entitu a typ pro běžné položky použít v modelech dat pro vývojáře šablony portálu v Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 92a52910ef8b9f078a8ea6ac409f89719f889b24
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Referenční model dat šablony Azure API Management
Toto téma popisuje reprezentace entitu a typ pro běžné položky použít v modelech dat pro vývojáře šablony portálu v Azure API Management.  
  
 Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [Rozhraní API](#API)  
-   [Souhrn rozhraní API](#APISummary)  
-   [Aplikace](#Application)  
-   [Přílohy](#Attachment)  
-   [Ukázka kódu](#Sample)  
-   [Komentář](#Comment)  
-   [Filtrování](#Filtering)  
-   [Záhlaví](#Header)  
-   [Požadavek HTTP](#HTTPRequest)  
-   [Odpověď HTTP](#HTTPResponse)  
-   [Problém](#Issue)  
-   [Operace](#Operation)  
-   [Operace nabídky](#Menu)  
-   [Operace položky nabídky](#MenuItem)  
-   [Stránkování](#Paging)  
-   [Parametr](#Parameter)  
-   [Produktu](#Product)  
-   [Poskytovatel](#Provider)  
-   [Reprezentace](#Representation)  
-   [Předplatné](#Subscription)  
-   [Předplatné souhrn](#SubscriptionSummary)  
-   [Informace o účtu uživatele](#UserAccountInfo)  
-   [Přihlášení uživatele](#UseSignIn)  
-   [Registrace uživatele](#UserSignUp)  
  
##  <a name="API"></a>ROZHRANÍ API  
 `API` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje rozhraní API v rámci aktuální instance služby API Management. Hodnota je platná relativní adresa URL ve formátu `apis/{id}` kde `{id}` je identifikátor rozhraní API. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název rozhraní API. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis rozhraní API. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|serviceUrl|Řetězec|Absolutní adresa URL back-end službu implementace toto rozhraní API.|  
|Cesta|Řetězec|Relativní adresa URL jedinečně identifikující toto rozhraní API a všechny jeho cesty prostředku v rámci instance služby API Management. Připojí se k rozhraní API koncový bod základní adresa URL zadaná při vytvoření instance služby a vytvořit veřejnou adresu URL pro toto rozhraní API.|  
|protokoly|pole čísla|Popisuje, na které protokoly může vyvolat operace v tomto rozhraní API. Povolené hodnoty jsou `1 - http` a `2 - https`, nebo obojí.|  
|authenticationSettings|[Nastavení ověřování serveru ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Kolekce nastavení ověřování, které jsou zahrnuté v tomto rozhraní API.|  
|subscriptionKeyParameterNames|Objekt|Volitelná vlastnost, která umožňuje určit vlastní názvy parametrů dotazu nebo hlavičku obsahující klíč předplatného. Když se nachází tato vlastnost musí obsahovat alespoň jeden z následujících dvou vlastností.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>Souhrn rozhraní API  
 `API summary` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje rozhraní API v rámci aktuální instance služby API Management. Hodnota je platná relativní adresa URL ve formátu `apis/{id}` kde `{id}` je identifikátor rozhraní API. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název rozhraní API. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis rozhraní API. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
  
##  <a name="Application"></a>Aplikace  
 `application` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Jedinečný identifikátor aplikace.|  
|Název|Řetězec|Název aplikace.|  
|Popis|Řetězec|Popis aplikace.|  
|URL|IDENTIFIKÁTOR URI|Identifikátor URI pro aplikaci.|  
|Verze|Řetězec|Informace o verzi pro aplikaci.|  
|Požadavky|Řetězec|Popis požadavky pro aplikaci.|  
|Stav|Číslo|Aktuální stav aplikace.<br /><br /> -0 - zaregistrován<br /><br /> -1 - odeslání<br /><br /> -2 - publikována<br /><br /> -3 - odmítnut<br /><br /> -4 - Nepublikováno|  
|RegistrationDate|Data a času|Datum a čas, který byl zaregistrován aplikace.|  
|CategoryId|Číslo|Kategorie aplikace (Finance, zábava atd.)|  
|DeveloperId|Řetězec|Jedinečný identifikátor vývojáře, které odeslána aplikace.|  
|Přílohy|Kolekce [přílohy](#Attachment) entity.|Přílohy pro aplikaci, například snímky obrazovky nebo ikon.|  
|Ikona|[Přílohy](#Attachment)|Ikona pro danou aplikaci.|  
  
##  <a name="Attachment"></a>Přílohy  
 `attachment` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Jedinečné ID|Řetězec|Jedinečný identifikátor pro přílohy.|  
|URL|Řetězec|Adresa URL prostředku.|  
|Typ|Řetězec|Typ přílohy.|  
|Typ obsahu|Řetězec|Typ média přílohu.|  
  
##  <a name="Sample"></a>Ukázka kódu  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Název|Řetězec|Název operace.|  
|fragment kódu|Řetězec|Tato vlastnost je zastaralá a by se neměla používat.|  
|štětce|Řetězec|Které šablony, který se má použít při zobrazení Ukázka kódu zvýrazňování syntaxe kódu. Povolené hodnoty jsou `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, a `csharp`.|  
|šablona|Řetězec|Název této šablony ukázkový kód.|  
|Text|Řetězec|Zástupný symbol pro část ukázka fragmentu kódu.|  
|– Metoda|Řetězec|Metoda HTTP operace.|  
|Schéma|Řetězec|Protokol bude použit pro tento požadavek operation.|  
|Cesta|Řetězec|Cesta k operaci.|  
|query|Řetězec|Příklad řetězce dotazu s definovaných parametrů.|  
|hostitele|Řetězec|Adresa URL brány služby API Management pro rozhraní API, která obsahuje tuto operaci.|  
|Záhlaví|Kolekce [záhlaví](#Header) entity.|Hlavičky pro tuto operaci.|  
|parameters|Kolekce [parametr](#Parameter) entity.|Parametry, které jsou definované pro tuto operaci.|  
  
##  <a name="Comment"></a>Komentář  
 `API` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Číslo|Id komentář.|  
|Commenttext –|Řetězec|Text komentář. Může zahrnovat HTML.|  
|DeveloperCompany|Řetězec|Název společnosti vývojář.|  
|PostedOn|Data a času|Datum a čas, který účtuje komentář.|  
  
##  <a name="Issue"></a>Problém  
 `issue` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Jedinečný identifikátor pro tento problém.|  
|ApiID|Řetězec|Id pro rozhraní API, pro který byla nahlášena tento problém.|  
|Název|Řetězec|Název problému.|  
|Popis|Řetězec|Popis problému.|  
|SubscriptionDeveloperName|Řetězec|Křestní jméno vývojáře, která použije v hlášení problému.|  
|IssueState|Řetězec|Aktuální stav problému. Možné hodnoty jsou navržený, otevřeno, Uzavřeno.|  
|ReportedOn|Data a času|Datum a čas, který ohlásil problém.|  
|Komentáře|Kolekce [komentář](#Comment) entity.|Komentáře k tomuto problému.|  
|Přílohy|Kolekce [přílohy](api-management-template-data-model-reference.md#Attachment) entity.|Přílohy k problému.|  
|Služby|Kolekce [rozhraní API](#API) entity.|Rozhraní API předplatné uživatelem, která selhala problém.|  
  
##  <a name="Filtering"></a>Filtrování  
 `filtering` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Vzor|Řetězec|Aktuální hledaný termín; nebo `null` Pokud neexistuje žádný hledaný termín.|  
|Zástupný symbol|Řetězec|Text, který zobrazit do vyhledávacího pole, pokud neexistuje žádný hledaný termín, který je zadán.|  
  
##  <a name="Header"></a>Záhlaví  
 Tato část popisuje `parameter` reprezentace.  
  
|Vlastnost|Popis|Typ|  
|--------------|-----------------|----------|  
|jméno|Řetězec|Název parametru.|  
|description|Řetězec|Popis parametru.|  
|hodnota|Řetězec|Hodnota hlavičky.|  
|TypeName|Řetězec|Datový typ hodnoty hlavičky.|  
|Možnosti|Řetězec|Možnosti.|  
|Požadované|Logická hodnota|Jestli je požadované hlavičky.|  
|Jen pro čtení|Logická hodnota|Jestli záhlaví je jen pro čtení.|  
  
##  <a name="HTTPRequest"></a>Požadavek HTTP  
 Tato část popisuje `request` reprezentace.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|description|Řetězec|Popis žádost o operaci.|  
|Záhlaví|pole [záhlaví](#Header) entity.|Hlavičky požadavku.|  
|parameters|pole [parametr](#Parameter)|Kolekce parametrů žádost o operaci.|  
|reprezentace|pole [reprezentace](#Representation)|Kolekce reprezentace žádost o operaci.|  
  
##  <a name="HTTPResponse"></a>Odpověď HTTP  
 Tato část popisuje `response` reprezentace.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|statusCode|Kladné celé číslo|Stavový kód odpovědi operaci.|  
|description|Řetězec|Operace popis odpovědi.|  
|reprezentace|pole [reprezentace](#Representation)|Kolekce reprezentace odpověď operace.|  
  
##  <a name="Operation"></a>Operace  
 `operation` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje operace v rámci aktuální instance služby API Management. Hodnota je platná relativní adresa URL ve formátu `apis/{aid}/operations/{id}` kde `{aid}` je identifikátor rozhraní API a `{id}` je identifikátor operaci. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název operace. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis operace. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|Schéma|Řetězec|Popisuje, na které protokoly může vyvolat operace v tomto rozhraní API. Povolené hodnoty jsou `http`, `https`, nebo obojí `http` a `https`.|  
|uriTemplate|Řetězec|Relativní adresa URL Šablona identifikace cílový prostředek pro tuto operaci. Může obsahovat parametry. Příklad:`customers/{cid}/orders/{oid}/?date={date}`|  
|hostitele|Řetězec|Adresu URL brány API Management, který je hostitelem rozhraní API.|  
|HttpMethod|Řetězec|Metoda operaci HTTP.|  
|Požadavek|[Požadavek HTTP](#HTTPRequest)|Entita obsahující podrobnosti požadavku.|  
|odpovědi|pole [odpovědi HTTP](#HTTPResponse)|Pole operace [odpověď HTTP](#HTTPResponse) entity.|  
  
##  <a name="Menu"></a>Operace nabídky  
 `operation menu` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ApiId|Řetězec|Id aktuální rozhraní API.|  
|CurrentOperationId|Řetězec|Id aktuální operace.|  
|Akce|Řetězec|Typ nabídky.|  
|Vlastnost MenuItems|Kolekce [položky nabídky operaci](#MenuItem) entity.|Operace pro aktuální rozhraní API.|  
  
##  <a name="MenuItem"></a>Operace položky nabídky  
 `operation menu item` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Id operace.|  
|Název|Řetězec|Popis operace.|  
|HttpMethod|Řetězec|Metoda Http operace.|  
  
##  <a name="Paging"></a>Stránkování  
 `paging` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Stránka|Číslo|Aktuální číslo stránky.|  
|PageSize|Číslo|Maximální výsledky, který se má zobrazit na jedné stránce.|  
|TotalItemCount|Číslo|Počet položek pro zobrazení.|  
|ShowAll|Logická hodnota|Jestli se má zobrazit všechny výsledky na jedné stránce.|  
|PageCount|Číslo|Počet stránek výsledků.|  
  
##  <a name="Parameter"></a>Parametr  
 Tato část popisuje `parameter` reprezentace.  
  
|Vlastnost|Popis|Typ|  
|--------------|-----------------|----------|  
|jméno|Řetězec|Název parametru.|  
|description|Řetězec|Popis parametru.|  
|hodnota|Řetězec|Hodnota parametru.|  
|Možnosti|Pole řetězců|Hodnot definovaných pro hodnoty parametru dotazu.|  
|Požadované|Logická hodnota|Určuje, zda parametr je povinný, nebo ne.|  
|Typ|Číslo|Jestli je tento parametr parametr cesty (1), nebo parametr řetězce dotazu (2).|  
|TypeName|Řetězec|Typ parametru.|  
  
##  <a name="Product"></a>Produktu  
 `product` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje produktu v aktuální instanci služby API Management. Hodnota je platná relativní adresa URL ve formátu `products/{pid}` kde `{pid}` je identifikátor produktu. Tato vlastnost je jen pro čtení.|  
|Název|Řetězec|Název produktu. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Popis|Řetězec|Popis produktu. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|Výrazy|Řetězec|Produkt podmínky použití. Vývojáři pokusu o přihlášení k odběru produktu se zobrazí a můžou přijmout tyto podmínky před dokončením procesu předplatného.|  
|ProductState|Číslo|Určuje, zda je produkt publikovaný nebo ne. Publikované produkty jsou zjistitelný vývojáři na portál pro vývojáře. Není publikována produkty jsou viditelné pouze správcům.<br /><br /> Povolené hodnoty pro stav produktu jsou:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Logická hodnota|Určuje, jestli uživatel může mít několik odběrů tohoto produktu ve stejnou dobu.|  
|MultipleSubscriptionsCount|Číslo|Počet odběrů tento produkt podle aktuálního uživatele.|  
  
##  <a name="Provider"></a>Zprostředkovatel  
 `provider` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Vlastnosti|slovník řetězců.|Vlastnosti pro tohoto zprostředkovatele ověřování.|  
|authenticationType.|Řetězec|Typ poskytovatele. (Azure Active Directory, Facebook přihlášení, účet Google, Microsoft Account, služby Twitter).|  
|Popisek|Řetězec|Zobrazovaný název zprostředkovatele.|  
  
##  <a name="Representation"></a>Reprezentace  
 Tato část popisuje `representation`.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Typ obsahu|Řetězec|Určuje registrované nebo vlastní typ obsahu pro tento reprezentace, například `application/xml`.|  
|Ukázka|Řetězec|Příkladem reprezentace.|  
  
##  <a name="Subscription"></a>Předplatné  
 `subscription` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje předplatného v rámci aktuální instance služby API Management. Hodnota je platná relativní adresa URL ve formátu `subscriptions/{sid}` kde `{sid}` je identifikátor předplatného. Tato vlastnost je jen pro čtení.|  
|productId|Řetězec|Identifikátor prostředku produktu odebírané produktu. Hodnota je platná relativní adresa URL ve formátu `products/{pid}` kde `{pid}` je identifikátor produktu.|  
|ProductTitle|Řetězec|Název produktu. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Popisvýrobku|Řetězec|Popis produktu. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|ProductDetailsUrl|Řetězec|Relativní adresu URL na podrobnosti o produktu.|  
|state|Řetězec|Stav odběru. Je možné stavy:<br /><br /> - `0 - suspended`– předplatného je zablokovaný a odběrateli nelze volat všechny rozhraní API produktu.<br /><br /> - `1 - active`– je předplatné aktivní.<br /><br /> - `2 - expired`– předplatné dosaženo datum vypršení platnosti a bylo deaktivováno.<br /><br /> - `3 - submitted`– žádosti o odběr byl proveden vývojáře, ale má ještě schválení nebo odmítnutí.<br /><br /> - `4 - rejected`– žádosti o odběr byl odepřen správcem.<br /><br /> - `5 - cancelled`– předplatné zrušil vývojáře nebo správce.|  
|displayName|Řetězec|Zobrazovaný název odběru.|  
|Datum vytvoření|Data a času|Datum odběr byl vytvořen, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Logická hodnota|Jestli předplatné můžete zrušit podle aktuálního uživatele.|  
|IsAwaitingApproval|Logická hodnota|Jestli předplatného čeká na schválení.|  
|Počátečním|Data a času|Počáteční datum pro předplatné, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|Datumvypršení platnosti|Data a času|Datum vypršení platnosti pro předplatné, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Data a času|Datum oznámení pro předplatné, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|primaryKey|Řetězec|Předplatné primární klíč. Maximální délka je 256 znaků.|  
|sekundární klíč|Řetězec|Klíč sekundární předplatného. Maximální délka je 256 znaků.|  
|CanBeRenewed|Logická hodnota|Jestli předplatné můžete obnovovat podle aktuálního uživatele.|  
|HasExpired|Logická hodnota|Jestli vypršela platnost předplatného.|  
|IsRejected|Logická hodnota|Jestli předplatné žádost byla odepřena.|  
|cancelUrl|Řetězec|Relativní adresa Url zrušit předplatné.|  
|RenewUrl|Řetězec|Adresa Url relativní k obnovení odběru.|  
  
##  <a name="SubscriptionSummary"></a>Předplatné souhrn  
 `subscription summary` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje předplatného v rámci aktuální instance služby API Management. Hodnota je platná relativní adresa URL ve formátu `subscriptions/{sid}` kde `{sid}` je identifikátor předplatného. Tato vlastnost je jen pro čtení.|  
|displayName|Řetězec|Zobrazovaný název odběru|  
  
##  <a name="UserAccountInfo"></a>Informace o účtu uživatele  
 `user account info` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|FirstName|Řetězec|Křestní jméno. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Příjmení|Řetězec|Příjmení. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby. Maximální délka je 254 znaků.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|NameIdentifier|Řetězec|Identifikátor účtu stejný jako e-mail uživatele.|  
|ProviderName|Řetězec|Název zprostředkovatele ověřování.|  
|IsBasicAccount|Logická hodnota|Hodnota TRUE, pokud tento účet byl zaregistrován pomocí e-mailu a heslo; false, pokud účet byl zaregistrován pomocí zprostředkovatele.|  
  
##  <a name="UseSignIn"></a>Přihlášení uživatele  
 `user sign in` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby. Maximální délka je 254 znaků.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|ReturnUrl|Řetězec|Adresu URL stránky, kde uživatel klepl přihlásit.|  
|Obdobou|Logická hodnota|Jestli se má uložit informace o aktuálním uživateli.|  
|RegistrationEnabled|Logická hodnota|Zda je povolena registrace.|  
|DelegationEnabled|Logická hodnota|Jestli je povolené delegované přihlášení.|  
|DelegationUrl|Řetězec|Delegované přihlašovací adresa url, pokud je povoleno.|  
|SsoSignUpUrl|Řetězec|Jednotné přihlašování adresu URL pro uživatele, pokud je k dispozici.|  
|AuxServiceUrl|Řetězec|Pokud má aktuální uživatel je správce, toto je odkaz na instanci služby na portálu Azure.|  
|Poskytovatelé|Kolekce [zprostředkovatele](#Provider) entity|Zprostředkovatelé ověřování pro tohoto uživatele.|  
|UserRegistrationTerms|Řetězec|Podmínky, které uživatel musí vyjádřit souhlas s před přihlášením.|  
|UserRegistrationTermsEnabled|Logická hodnota|Jestli jsou povolené podmínky.|  
  
##  <a name="UserSignUp"></a>Registrace uživatele  
 `user sign up` Entita, která má následující vlastnosti.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|PasswordConfirm|Logická hodnota|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|PasswordVerdictLevel|Číslo|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|UserRegistrationTerms|Řetězec|Podmínky, které uživatel musí vyjádřit souhlas s před přihlášením.|  
|UserRegistrationTermsOptions|Číslo|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|ConsentAccepted|Logická hodnota|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby. Maximální délka je 254 znaků.|  
|FirstName|Řetězec|Křestní jméno. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Příjmení|Řetězec|Příjmení. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|UserData|Řetězec|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up) ovládacího prvku.|  
|NameIdentifier|Řetězec|Hodnota používaná metodou [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|ProviderName|Řetězec|Název zprostředkovatele ověřování.|

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
