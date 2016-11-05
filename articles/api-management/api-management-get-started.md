---
title: Správa vašeho prvního rozhraní API ve službě Azure API Management | Microsoft Docs
description: Naučte se vytvářet rozhraní API, přidávat operace a začněte používat API Management.
services: api-management
documentationcenter: ''
author: steved0x
manager: erikre
editor: ''

ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/24/2016
ms.author: sdanie

---
# Správa vašeho prvního rozhraní API ve službě Azure API Management
## <a name="overview"> </a>Přehled
Tento průvodce vám pomůže rychle začít s používáním služby Azure API Management a provést první volání rozhraní API.

## <a name="concepts"> </a>Co je Azure API Management?
S pomocí služby Azure API Management můžete vzít jakýkoli back-end a na jeho základě spustit plnohodnotný program s rozhraním API.

Mezi obvyklé scénáře patří:

* **Zabezpečení mobilní infrastruktury** pomocí přístupu s klíči rozhraní API prostřednictvím brány, ochrana před útoky DOS pomocí omezování nebo používání rozšířených zásad zabezpečení, například ověřování tokenů JWT.
* **Povolování ekosystémů partnera ISV** prostřednictvím nabídky rychlého připojení partnera skrze portál pro vývojáře a vytvořením průčelí rozhraní API za účelem oddělení od interních implementací, které ještě nejsou zralé k tomu, aby je partner využíval.
* **Spuštění interního programu s rozhraním API** prostřednictvím nabídky centralizovaného umístění, kde může organizace komunikovat o dostupnosti a nejnovějších změnách rozhraní API, přístup prostřednictvím brány na základě zabezpečeného kanálu mezi bránou rozhraní API a back-endem.

Systém se skládá z následujících součástí:

* **Brána rozhraní API** je koncovým bodem, který plní následující úkoly:
  
  * Přijímá volání rozhraní API a směruje je na váš back-end.
  * Ověřuje klíče rozhraní API, tokeny JWT, certifikáty a další přihlašovací údaje.
  * Vynucuje kvóty využití a omezení četnosti.
  * Transformuje rozhraní API za chodu beze změny kódu.
  * Ukládá odezvy back-endu do určené mezipaměti.
  * Protokoluje metadata volání pro účely analýzy.
* **Portál vydavatele** je rozhraní pro správu, ve kterém můžete svůj program s rozhraním API nastavit. Použijte ho k následujícím akcím:
  
  * definování nebo import schématu rozhraní API
  * balení rozhraní API do produktů
  * nastavení zásad, například kvót nebo transformací rozhraní API
  * získání přehledů z analýz
  * správa uživatelů
* **Portál pro vývojáře** funguje jako hlavní webová služba pro vývojáře, kde můžete provádět následující akce:
  
  * Číst dokumentaci k rozhraní API.
  * Vyzkoušet rozhraní API prostřednictvím interaktivní konzoly.
  * Vytvořit účet a přihlásit se k odběru za účelem získání klíčů rozhraní API.
  * Přistupovat k analýzám jejich využití.

## <a name="create-service-instance"> </a>Vytvoření instance služby API Management
> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][Bezplatná zkušební verze Azure].
> 
> 

Prvním krokem při práci se službou API Management je vytvoření instance služby. Přihlaste se k webu [Portál Azure Classic][Portál Azure Classic] a klikněte na **Nový**, **App Services**, **API Management** a **Vytvořit**.

![Nová instance služby API Management][api-management-create-instance-menu]

V případě **adresy URL** zadejte název jedinečné subdomény, kterou chcete použít jako adresu URL služby.

Vyberte požadované **Předplatné** a **Oblast** pro instanci služby. Po provedení výběru klikněte na tlačítko **Další**.

![Nová služba API Management][api-management-create-instance-step1]

Zadejte text **Contoso Ltd.** jako **Název organizace** a potom zadejte svou e-mailovou adresu do pole **E-mail správce**.

> [!NOTE]
> Tato e-mailová adresa se bude používat pro zasílání oznámení ze systému API Management. Další informace najdete v článku [Konfigurace oznámení a e-mailových šablon ve službě Azure API Management][Konfigurace oznámení a e-mailových šablon ve službě Azure API Management].
> 
> 

![Nová služba API Management][api-management-create-instance-step2]

Instance služby API Management jsou dostupné ve třech úrovních: Developer (vývojář), Standard a Premium. Ve výchozím nastavení se nové instance služby API Management vytvářejí na úrovni Developer. Pokud chcete vybrat úroveň Standard nebo Premium, zaškrtněte políčko **Upřesnit nastavení** a na následující obrazovce vyberte požadovanou úroveň.

> [!NOTE]
> Úroveň Developer (vývojář) slouží k vývoji, testování a pilotnímu nasazení programů s rozhraním API v situacích, kde příliš nezáleží na dostupnosti. Na úrovních Standard a Premium můžete škálovat počet svých rezervovaných jednotek, abyste zvládli větší provoz. Úrovně Standard a Premium poskytují službě API Management maximum výpočetního a celkového výkonu. Tento kurz můžete dokončit pomocí libovolné úrovně. Další informace o úrovních služby API Management najdete v článku [Ceny služby API Management][Ceny služby API Management].
> 
> 

Kliknutím na zaškrtávací políčko vytvoříte instanci služby.

![Nová služba API Management][api-management-instance-created]

Po vytvoření instance služby je dalším krokem vytvoření nebo import rozhraní API.

## <a name="create-api"> </a>Import rozhraní API
Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace. Operace rozhraní API se odesílají přes proxy servery do existujících webových služeb.

Rozhraní API můžete vytvořit (a operace přidat) ručně, nebo je můžete importovat. V tomto kurzu naimportujeme rozhraní API pro ukázkovou webovou službu kalkulačky, kterou poskytuje Microsoft a hostuje ji na Azure.

> [!NOTE]
> Pokyny k vytváření rozhraní API a ručnímu přidávání operací najdete v článcích [Vytvoření rozhraní API](api-management-howto-create-apis.md) a [Přidání operací do rozhraní API](api-management-howto-add-operations.md).
> 
> 

Rozhraní API se konfigurují na portálu vydavatele, který je přístupný prostřednictvím portálu Azure Classic. Pokud se chcete dostat na portál vydavatele, klikněte na portálu Azure Classic služby API Management na **Spravovat**.

![Portál vydavatele][api-management-management-console]

Pokud chcete importovat rozhraní API kalkulačky, klikněte v nabídce **API Management** na levé straně na **Rozhraní API** a potom na **Importovat rozhraní API**.

![Tlačítko Importovat rozhraní API][api-management-import-api]

Pokud chcete konfigurovat rozhraní API kalkulačky, proveďte následující kroky:

1. Klikněte na **Z adresy URL**, do textového pole **Adresa URL dokumentu se specifikací** zadejte **http://calcapi.cloudapp.net/calcapi.json** a klikněte na přepínač **Swagger**.
2. Do textového pole **Přípona adresy URL webového rozhraní API** zadejte **calc**.
3. Klikněte do pole **Produkty (volitelné)** a zvolte **Starter**.
4. Kliknutím na **Uložit** naimportujte rozhraní API.

![Přidání nového rozhraní API][api-management-import-new-api]

> [!NOTE]
> **API Management** aktuálně podporuje import dokumentu Swagger ve verzi 1.2 a 2.0. I když [specifikace Swagger 2.0](http://swagger.io/specification) uvádí, že vlastnosti `host`, `basePath` a `schemes` jsou volitelné, váš dokument Swagger 2.0 **MUSÍ** tyto vlastnosti obsahovat, jinak nepůjde importovat. 
> 
> 

Po importu rozhraní API se na portálu vydavatele zobrazí souhrnná stránka rozhraní.

![Souhrn rozhraní API][api-management-imported-api-summary]

Část rozhraní API obsahuje několik karet. Karta **Souhrn** zobrazuje základní metriky a informace o rozhraní API. Karta [Nastavení](api-management-howto-create-apis.md#configure-api-settings) slouží k zobrazení a úpravě konfigurace rozhraní API. Karta [Operace](api-management-howto-add-operations.md) slouží ke správě operací rozhraní API. Kartu **Zabezpečení** můžete použít ke konfiguraci ověřování brány pro back-endový server pomocí základního ověření nebo pomocí [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md) a ke konfiguraci [autorizace uživatelů pomocí standardu OAuth 2.0](api-management-howto-oauth2.md).  Karta **Problémy** slouží k zobrazení problémů nahlášených vývojáři, kteří používají vaše rozhraní API. Karta **Produkty** slouží ke konfiguraci produktů, které toto rozhraní API obsahují.

Ve výchozím nastavení každá instance služby API Management obsahuje dva ukázkové produkty:

* **Starter**
* **Unlimited**

V tomto kurzu bylo rozhraní API základní kalkulačky k produktu Starter přidáno při importu rozhraní.

Aby bylo možné rozhraní API volat, musí se vývojáři nejdřív přihlásit k odběru produktu, který jim poskytne k rozhraní přístup. Vývojáři se můžou přihlásit k odběru produktů na portálu pro vývojáře, popřípadě můžou správci přihlásit vývojáře k odběru produktů na portálu vydavatele. Vzhledem k tomu, že jste v předchozích krocích tohoto kurzu vytvořili instanci služby API Management, stali se z vás správci a jste ve výchozím nastavení přihlášeni k odběru všech produktů.

## <a name="call-operation"> </a>Volání operace z portálu pro vývojáře
Operaci můžete volat přímo z portálu pro vývojáře, který nabízí pohodlný způsob pro zobrazení a testování operací v rozhraní API. V tomto kroku kurzu budete v rozhraní API základní kalkulačky volat operaci **Add two integers** (Sečíst dvě celá čísla). V nabídce v pravé horní části portálu vydavatele klikněte na **Portál pro vývojáře**.

![portálu pro vývojáře][api-management-developer-portal-menu]

Pokud chcete zobrazit dostupné operace, klikněte v horní nabídce na **Rozhraní API** a potom na **Basic Calculator** (Základní kalkulačka).

![portálu pro vývojáře][api-management-developer-portal-calc-api]

Všimněte si ukázkových popisů a parametrů naimportovaných společně s rozhraním API a operacemi, které poskytují dokumentaci vývojářům, kteří budou tuto operaci používat. Tyto popisy můžete přidat i v případě, kdy operace přidáváte ručně.

Pokud chcete volat operaci **Add two integers** (Sečíst dvě celá čísla), klikněte na **Vyzkoušet**.

![Vyzkoušet][api-management-developer-portal-calc-api-console]

Hodnoty parametrů můžete zadat, nebo můžete použít výchozí hodnoty. Potom klikněte na **Odeslat**.

![Metoda HTTP Get][api-management-invoke-get]

Po vyvolání operace portál pro vývojáře zobrazí **Stav odpovědi**, **Hlavičky odpovědi** a jakýkoli **Obsah odpovědi**.

![Odpověď][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Zobrazení analýzy
Pokud chcete zobrazit analýzu základní kalkulačky, výběrem možnosti **Spravovat** v nabídce v pravé horní části portálu pro vývojáře přepněte zpět na portál vydavatele.

![Spravovat][api-management-manage-menu]

Výchozím zobrazením portálu vydavatele je **řídicí panel**, který nabízí přehled o instanci služby API Management.

![Řídicí panel][api-management-dashboard]

Pokud chcete zobrazit konkrétní metriky využití rozhraní API za určité časové období, najeďte myší na graf pro **Basic Calculator**.

> [!NOTE]
> Pokud na grafu nevidíte žádné čáry, přepněte zpět na portál pro vývojáře, proveďte několik volání do rozhraní API, chvíli počkejte a pak se vraťte k řídicímu panelu.
> 
> 

Kliknutím na **Zobrazit podrobnosti** zobrazíte souhrnnou stránku rozhraní API včetně větší verze zobrazených metrik.

![Analýza][api-management-mouse-over]

![Souhrn][api-management-api-summary-metrics]

Podrobné metriky a sestavy zobrazíte kliknutím na tlačítko **Analýza**, které najdete v nabídce **API Management** na levé straně.

![Přehled][api-management-analytics-overview]

Část **Analýza** obsahuje následující čtyři karty:

* **Rychlý přehled** poskytuje metriky celkového využití a stavu a také nejlepší vývojáře, nejlepší produkty, nejlepší rozhraní API a nejlepší operace.
* **Využití** poskytuje hlubší pohled na volání a šířku pásma rozhraní API, včetně zeměpisného rozlišení.
* **Stav** se zaměřuje na stavové kódy, úspěšnost mezipaměti, doby odezvy a doby odezvy rozhraní API a služby.
* **Aktivita** obsahuje sestavy, které podrobně uvádějí konkrétní aktivitu podle vývojáře, produktu, rozhraní API a operace.

## <a name="next-steps"> </a>Další kroky
* Zjistěte, jak můžete [chránit rozhraní API omezením četnosti ](api-management-howto-product-with-rules.md).

[Bezplatná zkušební verze Azure]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Vytvoření instance služby API Management]: #create-service-instance
[Vytvoření rozhraní API]: #create-api
[Přidání operace]: #add-operation
[Přidání nového rozhraní API do produktu]: #add-api-to-product
[Přihlášení k odběru produktu, který obsahuje rozhraní API]: #subscribe
[Volání operace z portálu pro vývojáře]: #call-operation
[Zobrazení analýzy]: #view-analytics
[Další kroky]: #next-steps


[Správa účtů pro vývojáře ve službě API Management]: api-management-howto-create-or-invite-developers.md
[Konfigurace nastavení pro rozhraní API]: api-management-howto-create-apis.md#configure-api-settings
[Konfigurace oznámení a e-mailových šablon ve službě Azure API Management]: api-management-howto-configure-notifications.md
[Odezvy]: api-management-howto-add-operations.md#responses
[Vytvoření a publikování produktu]: api-management-howto-add-products.md
[Ceny služby API Management]: http://azure.microsoft.com/pricing/details/api-management/

[Portál Azure Classic]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png



<!--HONumber=ago16_HO5-->


