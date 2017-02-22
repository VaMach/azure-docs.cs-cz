---
title: "Úvod k API Apps | Dokumentace Microsoftu"
description: "Zjistěte, jak Azure App Service pomáhá nasazovat, hostovat a používat rozhraní RESTful API."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 62b1541414543fc947886374424dadee09759323


---
# <a name="api-apps-overview"></a>Přehled API Apps
API Apps v Azure App Service nabízejí funkce k usnadnění vývoje, hostování a používání rozhraní API v cloudu i lokálně. Aplikace API poskytují zabezpečení na úrovni potřebné ve velkých firmách, snadné řízení přístupu, hybridní připojení, automatické generování sad SDK a bezproblémovou integraci s [Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

[Azure App Service](../app-service/app-service-value-prop-what-is.md) je plně spravovaná platforma pro webové, mobilní a integrační scénáře. API Apps jsou jedním ze čtyř typů aplikací, které služba [Azure App Service](../app-service/app-service-value-prop-what-is.md) nabízí.

![Typy aplikací v Azure App Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Proč používat API Apps?
Toto jsou některé klíčové funkce API Apps:

* **Použití existujícího rozhraní API bez nutnosti jeho přizpůsobení:** Abyste mohli využívat výhod API Apps, nemusíte kód svých stávajících rozhraní API nijak upravovat. Kód stačí do aplikace API jen nasadit. Vaše rozhraní API může používat libovolný jazyk nebo rozhraní podporované službou App Service, včetně technologií ASP.NET, C#, Java, PHP, Node.js a Python.
* **Snadné požívání:** Integrovaná podpora [metadat rozhraní API Swaggeru](http://swagger.io/) umožňuje snadné použití vašich rozhraní API pro nejrůznější klienty.  Automaticky generuje kód klienta pro vaše rozhraní API v různých jazycích, včetně C#, Javy a JavaScriptu. Můžete snadno konfigurovat [CORS](app-service-api-cors-consume-javascript.md), aniž byste měnili kód. Další informace najdete v tématech [Metadata App Service API Apps pro zjišťování rozhraní API a generování kódu](app-service-api-metadata.md) a [Využití aplikace API z JavaScriptu pomocí CORS](app-service-api-cors-consume-javascript.md). 
* **Snadné řízení přístupu:** Aplikace API jsou chráněny před neověřeným přístupem, a to bez jediné změny v kódu. Integrované služby ověřování zajišťují přístup k rozhraní API pro jiné služby nebo klienty představující uživatele. Mezi podporované zprostředkovatele identity patří Azure Active Directory, Facebook, Twitter, Google a účet Microsoft. Klienti mohou používat knihovnu ADAL (Active Directory Authentication Library) nebo sadu SDK pro Mobile Apps. Další informace najdete v tématu [Ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md).
* **Integrace nástroje Visual Studio:** Vyhrazené nástroje v nástroji Visual Studio zjednodušují práci při vytváření, nasazování, využívání, ladění a správě aplikací API. Další informace najdete v tématu [Uvedení sady Azure SDK 2.8.1 pro .NET](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).
* **Integrace s Logic Apps:** Aplikace API, které vytvoříte, mohou být využívány službou [App Service Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).  Další informace najdete v tématech [Používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md) a [Nová verze schématu 2015-08-01-preview](../logic-apps/logic-apps-schema-2015-08-01.md).

Kromě toho může aplikace API využívat funkce služeb [Web Apps](../app-service-web/app-service-web-overview.md) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md). A platí to i naopak: pokud používáte webovou aplikaci nebo mobilní aplikaci k hostování rozhraní API, může využívat funkce aplikací API, například metadata Swagger ke generování klientského kódu a CORS pro přístup k prohlížeči mezi doménami. Jediný rozdíl mezi všemi třemi typy aplikací (API, webová aplikace a mobilní aplikace) je název a ikona, které se pro ně používají na Portálu Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Jaký je rozdíl mezi funkcí API Apps a službou Azure API Management?
API Apps a [Azure API Management](../api-management/api-management-key-concepts.md) jsou doplňkové služby:

* API Management se týká správy rozhraní API. Front-end služby API Management se do rozhraní API přidává za účelem monitorování a omezování využití, zpracování vstupu a výstupu, konsolidace několika rozhraní API do jednoho koncového bodu apod. Spravovaná rozhraní API je možné hostovat kdekoli.
* Účelem API Apps je hostování rozhraní API. Tato služba obsahuje funkce, které usnadňují vývoj a využívání rozhraní API, ale neprovádí monitorování, omezování, manipulaci nebo konsolidaci tak jako služba API Management. Pokud nepotřebujete funkce API Management, můžete rozhraní API hostovat v aplikacích API bez použití služby API Management.

Zde je diagram, který znázorňuje použití služby API Management pro rozhraní API hostovaná v aplikacích API nebo jinde.

![Azure API Management a API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Některé funkce mají služba API Management a funkce API Apps podobné.  Obě služby mohou například automatizovat podporu CORS. Pokud používáte obě služby současně, budete pro CORS používat službu API Management, protože funguje jako front-end pro vaše aplikace API. 

## <a name="getting-started"></a>Začínáme
Pokud chcete začít pracovat s aplikacemi API a nasadit do některé aplikace API ukázkový kód, projděte si kurz pro vámi preferované rozhraní:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Pokud se chcete v souvislosti s aplikacemi API na něco zeptat, otevřete vlákno ve [fóru pro API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 




<!--HONumber=Feb17_HO3-->


