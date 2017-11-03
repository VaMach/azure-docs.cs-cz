---
title: "Vytvoření rozhraní API v Azure API Management"
description: "Zjistěte, jak vytvořit a nakonfigurovat rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 79630fb8998d47aab8eed38daf7d5fcef3d870d7
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a>Vytvoření rozhraní API v Azure API Management
Rozhraní API ve službě API Management představuje sadu operací, které můžete vyvolat klientské aplikace. Vytvoří se nová rozhraní API na portálu vydavatele a pak požadované operace jsou přidány. Jakmile se operace, které jsou přidány, rozhraní API se přidá do produktu a může být publikována. Po publikování rozhraní API se můžete přihlásit k odběru a používají vývojáři.

Tato příručka obsahuje prvním krokem v procesu: postup vytvoření a konfigurace nové rozhraní API ve službě API Management. Další informace o přidávání operací a publikování produktu najdete v tématu [přidání operací do rozhraní API] [ How to add operations to an API] a [postup vytvoření a publikování produktu][How to create and publish a product].

## <a name="create-new-api"></a>Vytvořit nové rozhraní API
Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele. Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně a pak klikněte na **přidání rozhraní API**.

![Vytvoření rozhraní API][api-management-create-api]

Použití **přidání nového rozhraní API** okno Konfigurace nového rozhraní API.

![Přidání nového rozhraní API][api-management-add-new-api]

Následující pole se používají ke konfiguraci nového rozhraní API.

* **Název webové rozhraní API** poskytuje jedinečný a popisný název pro rozhraní API. Zobrazí se na vývojáře a vydavatele portálech.
* **Adresu URL webové služby** odkazuje na službu HTTP implementace rozhraní API. Rozhraní API správy předá požadavky na tuto adresu.
* **Přípona adresy URL rozhraní API webové** se připojí k základní adresu URL pro službu správy rozhraní API. Základní adresa URL je společný pro všechny rozhraní API hostovaná v instanci služby API Management. API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.
* **Schéma adresy URL rozhraní API webové** určuje protokoly, které lze použít pro přístup k rozhraní API. Ve výchozím nastavení je zadán HTTPs.
* Chcete-li volitelně přidat toto nové rozhraní API pro určitý produkt, klikněte na tlačítko **produkty (volitelné)** rozevíracího seznamu a vyberte produkt. Tento krok můžete přidat rozhraní API k více produktům opakuje vícekrát.

Po nakonfigurování požadované hodnoty, klikněte na tlačítko **Uložit**. Po vytvoření nového rozhraní API na portálu vydavatele zobrazí souhrnná stránka rozhraní API.

![Souhrn rozhraní API][api-management-api-summary]

## <a name="configure-api-settings"></a>Nastavení konfigurace rozhraní API
Můžete použít **nastavení** kartě ověřte a úpravě konfigurace rozhraní API. **Název webové rozhraní API**, **adresu URL webové služby**, a **přípona adresy URL webového rozhraní API** původně nastaveny při vytvoření rozhraní API a nelze jej změnit zde. **Popis** poskytuje volitelný popis a **schéma adresy URL webového rozhraní API** určuje protokoly, které lze použít pro přístup k rozhraní API.

![Nastavení rozhraní API][api-management-api-settings]

Ke konfiguraci ověřování brány pro back-end službu implementace rozhraní API, vyberte **zabezpečení** kartě. **s přihlašovacími údaji** rozevíracího seznamu můžete použít ke konfiguraci **HTTP basic** nebo **klientské certifikáty** ověřování. Pokud chcete používat základní ověřování protokolu HTTP, jednoduše zadejte požadované pověření. Informace o používání ověřování certifikátu klienta najdete v tématu [jak zabezpečit back endové služby pomocí klienta pro ověřování pomocí certifikátu ve službě Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].

**Zabezpečení** můžete také použít ke konfiguraci **autorizace uživatelů** pomocí OAuth 2.0. Další informace najdete v tématu [autorizace vývojářským účtům pomocí OAuth 2.0 ve službě Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].

![Nastavení základního ověřování][api-management-api-settings-credentials]

Klikněte na tlačítko **Uložit** uložit změny provedené nastavení rozhraní API.

## <a name="next-steps"></a>Další kroky
Po vytvoření rozhraní API a nakonfigurované nastavení jsou další kroky pro přidání operací do rozhraní API, přidat rozhraní API do produktu a publikujete ji tak, aby se k dispozici pro vývojáře. Další informace naleznete v následujících článcích.

* [Přidání operací do rozhraní API][How to add operations to an API]
* [Postup vytvoření a publikování produktu][How to create and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
