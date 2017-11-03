---
title: "Přehled vlastních konektorů – Azure Logic Apps | Microsoft Docs"
description: "Přehled o vytváření vlastních konektorů pro podporu a rozšiřování scénáře integrace"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 0515b21bc7c7cc737e14fda016606bbea1aab476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="custom-connectors-overview"></a>Přehled vlastních konektorů

Bez psaní kódu, můžete vytvořit pracovní postupy nebo aplikace s [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), [Microsoft Flow](https://flow.microsoft.com), nebo [Microsoft PowerApps](https://powerapps.microsoft.com). Můžete integrovat vaše data a obchodních procesů, tyto služby nabízejí [100 + konektory](../connectors/apis-list.md), ne jenom pro Microsoft službami a produkty, jako je Azure a SQL Server, ale jiné služby, jako jsou Githubu, Salesforce, Twitter a další . 

Někdy ale můžete chtít volání rozhraní API, služeb a systémů, které nejsou k dispozici jako předem konektory. Pro podporu více šité na míru scénářů pro firmy a produktivitu potřebám vašich uživatelů, můžete vytvořit *vlastní konektory* s vlastní triggery a akce.
Vlastní konektory rozbalte integrace, reach, možnosti rozpoznání a použití pro službu nebo produkt, který může pomoci zvýšit a urychlit přijetí zákazníka.

Tento diagram zobrazuje interakce mezi Web API, vlastní konektoru Logic Apps vytvořené pro toto rozhraní API a aplikace logiky, která funguje s toto rozhraní API pomocí vlastní konektor:

![Koncepční přehled webového rozhraní API, vlastní konektor a aplikace logiky](./media/custom-connector-overview/custom-connector-conceptual.png)

Tento přehled popisuje obecné úlohy vysoké úrovně pro vytváření, zabezpečení, registrace a pomocí a volitelně sdílení nebo certifikuje vaší konektory:

![Vlastní konektor vytváření kroky](./media/custom-connector-overview/authoring-steps.png)

## <a name="prerequisites"></a>Požadavky

Chcete-li sestavit vaše konektor od začátku do konce, je třeba tyto položky:

* Předplatné Azure. Pokud nemáte předplatné, můžete začít s [bezplatný účet Azure](https://azure.microsoft.com/free/). Jinak, zaregistrujte si [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Rozhraní RESTful API s nějaký typ ověřený přístup. Další informace najdete v tématu [vytvořit vlastní konektory z rozhraní Web API](../logic-apps/custom-connector-build-web-api-app-tutorial.md). V případě vzorů, které jsou vhodné pro vaše konektor triggery a akce, najdete v části [vytvořte vlastní rozhraní API, která můžete volat z pracovních aplikace logiky](../logic-apps/logic-apps-create-api-app.md).

* Libovolnou položku zde:

  * [OpenAPI 2.0 souboru](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md), dříve označovaný jako Swagger
  * Adresa URL k definici OpenAPI
  * A [Postman kolekce](../logic-apps/custom-connector-api-postman-collection.md) pro vaše rozhraní API 

  Pokud nemáte žádné z těchto položek, poskytujeme pokyny pro vás.

* Volitelné: Obrázek, který chcete použít jako ikona pro vaše vlastní konektor

## <a name="1-build-your-restful-api"></a>1. Sestavení rozhraní RESTful API.

Technicky je konektor obálku kolem REST API, které je založena na OpenAPI (dříve Swagger) specifikaci a umožňuje službě základní komunikují s Logic Apps, tok nebo PowerApps. Proto nejprve je třeba plně funkční rozhraní API, než vytvoříte vlastní konektor. 

Můžete použít libovolný jazyk a platformy pro vaše rozhraní API. Pro technologiích společnosti Microsoft doporučujeme jeden z těchto platforem:

* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/)
* [Aplikace Azure API](https://azure.microsoft.com/services/app-service/api/)

Například tento kurz ukazuje [jak vytvořit vlastní konektor z webového rozhraní API](../logic-apps/custom-connector-build-web-api-app-tutorial.md). V případě vzorů, které jsou vhodné pro vaše konektor triggery a akce, najdete v části [vytvořte vlastní rozhraní API, která můžete volat z pracovních aplikace logiky](../logic-apps/logic-apps-create-api-app.md).

## <a name="2-secure-your-api"></a>2. Zabezpečení API

Těchto standardů ověřování můžete použít pro konektory a rozhraní API:

   * [OAuth 2.0](https://oauth.net/2/), včetně [Azure Active Directory](https://azure.microsoft.com/develop/identity/) nebo konkrétní služby, jako je Dropbox, Githubu a SalesForce
   * Obecné OAuth 2.0
   * [Základní ověřování](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [Klíč rozhraní API](https://swagger.io/docs/specification/authentication/api-keys/)

Ověřování Azure Active Directory (Azure AD) pro vaše rozhraní API na portálu Azure můžete nastavit, takže není nutné implementovat ověřování prostřednictvím kódu. Nebo můžete požadovat a vynutit ověřování prostřednictvím kódu vaše rozhraní API. 

Další informace postupujte podle příslušné kurzy:

* [Služba Logic Apps: Zabezpečené vlastní konektor](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Tok: Zabezpečené vlastní konektor](https://ms.flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/)
* [PowerApps: Vlastní konektor zabezpečení](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/)

## <a name="3-describe-your-api"></a>3. Popisují vaše rozhraní API 

Za předpokladu, že vaše rozhraní API má nějaký typ ověřený přístup, budete muset popisují vaše rozhraní API rozhraní a operace tak, aby Logic Apps, tok nebo PowerApps může komunikovat s vaše rozhraní API.
Použijte jednu z těchto oborový standard definice:

* [OpenAPI 2.0 souboru](https://swagger.io/) můžete spustit s existující soubor OpenAPI sestavení.

  Pokud jste ještě OpenAPI, navštivte [Začínáme s Swagger](http://swagger.io/getting-started/) na webu swagger.io.
  Příklad OpenAPI souboru najdete v článku [dokumentaci k rozhraní API Analytics Text](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/export?DocumentFormat=Swagger&ApiName=Azure). 

* Postman kolekce, které automaticky generuje soubor OpenAPI za vás. Když již nemají soubor OpenAPI a nechcete, aby si ji vytvořit, můžete snadno vytvořit vlastní konektor pomocí Postman kolekce.

  Pokud jste ještě Postman, [nainstalujte aplikaci Postman](https://www.getpostman.com/apps) z jejich lokality. Další informace najdete v tématu [popisují vlastní konektory s Postman](../logic-apps/custom-connector-api-postman-collection.md).

> [!IMPORTANT]
> Velikost vašeho souboru musí být menší než 1 MB.

Na pozadí Logic Apps, toku a PowerApps nakonec použít OpenAPI, analyzuje kolekci Postman a překládá kolekci do souboru definice OpenAPI. I když OpenAPI 2.0 a Postman kolekce pomocí různých formátech, jsou obě bez ohledu na jazyk, strojově čitelným dokumenty, které popisují vaše rozhraní API operace a parametry. Tyto dokumenty můžou Generovat z různých nástrojů, na základě jazyka a platformy používá vaše rozhraní API. Můžete také vytvořit soubor OpenAPI při registraci vaší konektor.

Například můžete vytvořit soubor OpenAPI nebo kolekci Postman z žádný koncový bod REST API, včetně:

* Veřejně dostupné konektory, například [Spotify](https://developer.spotify.com/), [Slack](https://api.slack.com/), [Rackspace](http://docs.rackspace.com/)a tak dále

* Rozhraní API, které jste vytvořili a nasadili pro všechny poskytovatele cloudových hostitelských služeb, jako je například Azure, Heroku, Google Cloud a další

* Vlastní rozhraní API obchodní, kde je nasazena do vaší sítě, ale jenom v případě toto rozhraní API je vystaven na veřejného Internetu

## <a name="4-register-your-connector"></a>4. Zaregistrovat vaše konektoru

Proces registrace pomáhá Logic Apps, tok, nebo PowerApps pochopit charakteristiky vaše rozhraní API, včetně popisu, vyžaduje ověřování a operace, včetně parametrů a výstupy pro každou operaci. Když spustíte proces registrace, můžete zadat soubor OpenAPI nebo Postman kolekce, která automaticky vyplnit pole metadat v Průvodci registrací. Kdykoli můžete upravit hodnoty těchto polí.

![Popisují vaše rozhraní API](./media/custom-connector-overview/choose-api-definition-file.png)

K registraci vašeho konektoru, postupujte podle příslušné kurzu:

* [Služba Logic Apps: Registrace vaší konektoru](../logic-apps/logic-apps-custom-connector-register.md)
* [Postup: Registrace vaší konektoru](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registrace vaší konektoru](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)

## <a name="5-use-your-connector-in-a-logic-app-flow-or-app"></a>5. Použijte váš konektor v aplikaci logiky, tok nebo aplikace 

Vaše konektor můžete použít stejným způsobem, že používáte konektorů spravovaných společností Microsoft. Například v pracovním postupu aplikace logiky, přidejte vlastní konektor, můžete vytvořit připojení k vašemu rozhraní API a volání žádné operace, které poskytuje rozhraní API stejným způsobem, který volání operací pro konektorů spravovaných společností Microsoft.

## <a name="6-share-your-connector"></a>6. Sdílet vaše konektoru 

Vaše konektoru můžete sdílet s uživateli ve vaší organizaci stejným způsobem, že můžete sdílet prostředky v Logic Apps, tok nebo PowerApps. I když sdílení je volitelný a může mít scénáře, ve které chcete sdílet vaše konektory s dalšími uživateli.

Registrovaná, ale necertifikované vlastní konektory fungovat jako konektorů spravovaných společností Microsoft, ale jsou viditelné a dostupné *pouze* konektoru autora a uživatelé, kteří mají stejné klienta Azure Active Directory a předplatné Azure pro logic apps v oblasti, kde jsou tyto aplikace nasazené. Další krok popisuje, jak můžete sdílet vaše konektor s externími uživateli mimo tyto hranice, například s Logic Apps, toku a PowerApps uživateli.

> [!IMPORTANT]
> Pokud sdílíte konektor, jiné můžou začít závisí na konektor. 
> ***Odstraněním vašeho konektoru odstraníte všechna připojení pro tento konektor.***

* [Služba Logic Apps: Sdílet vaše konektoru](../logic-apps/logic-apps-custom-connector-register.md)
* [Tok: Sdílet vaše konektoru](https://ms.flow.microsoft.com/documentation/register-custom-api/#share-your-custom-connector)
* [PowerApps: Sdílet vaše konektoru](https://powerapps.microsoft.com/tutorials/register-custom-api/#share-your-custom-connector)

## <a name="7-certify-your-connector"></a>7. Certifikovat vašeho konektoru

Volitelně sdílet vaše konektoru se všemi uživateli v Logic Apps, toku a PowerApps, odeslání vašeho konektoru pro certifikaci Microsoft. Tento proces zkontroluje váš konektor, zkontroluje technické a obsahu dodržování předpisů a ověřuje funkce Logic Apps, toku a PowerApps předtím, než Microsoft můžete publikovat vaše konektor. Další informace [odeslání vašeho konektoru pro certifikaci Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Získat podporu

* Podporu pro vývoj a registrace, e-mailu [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft aktivně sleduje tento účet pro vývojáře otázky a problémy a směruje je do příslušné team. 

* Nejčastější dotazy, najdete v článku [vlastní konektor – nejčastější dotazy](../logic-apps/custom-connector-faq.md)

## <a name="next-steps"></a>Další kroky

* [Sestavit vlastní konektor z webového rozhraní API](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Nastavení ověřování pro vlastní konektory](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Popisují vlastní konektory s kolekcí Postman](../logic-apps/custom-connector-api-postman-collection.md)
* [Odeslat vlastní konektory pro certifikaci Microsoft](../logic-apps/custom-connector-submit-certification.md)
