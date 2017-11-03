---
title: "Omezení a známé problémy v Azure API Management API import | Microsoft Docs"
description: "Podrobnosti o známých problémech a omezení při importu do Azure API Management pomocí otevřené rozhraní API, WSDL nebo WADL formáty."
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: 4cb6ad53b59b81f906a85027f4ff988bbb78706a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>Omezení import rozhraní API a známé problémy
## <a name="about-this-list"></a>O tomto seznamu
Při každé úsilí zajistit, že import rozhraní API do Azure API Management je jako plynulé a bez problémů, co jsme příležitostně ukládat omezení nebo identifikovat problémy, které budou muset být opravena, aby bylo možné úspěšně naimportovat. Tento článek tyto dokumenty organizuje formát import rozhraní API.

## <a name="open-api"></a>Otevřete rozhraní API/Swagger
Obecně platí, pokud obdržíte chyby import dokumentu otevřené rozhraní API, zkontrolujte, zda jste ověřili jeho – buď pomocí návrháře nového portálu Azure (návrhu - Front-endu - otevřené rozhraní API specifikace Editor), nebo se 3. stran nástroj <a href="http://www.swagger.io">Swagger Editor</a>.

* **Název hostitele** jsme vyžadují atribut název hostitele.
* **Základní cesta** jsme vyžadují atribut základní cesta.
* **Schémata** vyžadujeme schéma pole. 

## <a name="wsdl"></a>WSDL
WSDL soubory se používají ke generování SOAP průchozí rozhraní API nebo sloužit jako back-end SOAP REST API.

* **WSDL: import** aktuálně nepodporujeme rozhraní API pomocí tohoto atributu. Zákazníci měli sloučit importované elementy do jednoho dokumentu.
* **Zprávy s více částmi** aktuálně nejsou podporované.
* **WCF wsHttpBinding** SOAP služby vytvořené pomocí Windows Communication Foundation by měl používat basicHttpBinding – wsHttpBinding není podporován.
* **MTOM** služby používající MTOM <em>může</em> fungovat. V tuto chvíli není nabídnuta oficiální podporu.
* **Rekurze** typy, které jsou definované rekurzivně (například odkazovat na pole sami) nejsou podporovány.

## <a name="wadl"></a>WADL
Aktuálně neexistují žádné známé problémy WADL importu.


[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
