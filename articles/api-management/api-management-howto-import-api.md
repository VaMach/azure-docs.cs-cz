---
title: "Import rozhraní API do Azure API Management | Microsoft Docs"
description: "Zjistěte, jak importovat rozhraní API a jeho operací do Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Postup importu definice rozhraní API s operacemi v Azure API Management
Ve službě API Management můžete vytvořit nová rozhraní API a operace přidat ručně, nebo rozhraní API může být importován společně s operací v jednom kroku.

Rozhraní API a jejich operace lze importovat z následujících formátů.

* WADL
* Swagger

Tato příručka ukazuje, jak vytvořit nové rozhraní API a importovat jeho operací v jednom kroku. Informace o ruční vytvoření rozhraní API a přidávání operací najdete v tématu [vytvoření rozhraní API] [ How to create APIs] a [přidání operací do rozhraní API][How to add operations to an API].

## <a name="import-api"> </a>Import rozhraní API
Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele. Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně a pak klikněte na **importovat rozhraní API**.

![Rozhraní API pro import][api-management-import-apis]

**Rozhraní API pro Import** okno má tři karty, které odpovídají tři způsoby, jak poskytnout specifikace rozhraní API.

* **Ze schránky** umožňuje specifikace rozhraní API vložit do určené textového pole.
* **Ze souboru** můžete procházet a vyberte soubor, který obsahuje specifikace rozhraní API.
* **Z adresy URL** umožňuje zadat adresu URL do specifikace rozhraní pro rozhraní API.

![Formát importovat rozhraní API][api-management-import-api-clipboard]

Po zadání specifikace rozhraní API, pomocí přepínačů na pravé straně, k označení specifikace formátu. Jsou podporovány následující formáty.

* WADL
* Swagger

Potom zadejte **přípona adresy URL webového rozhraní API**. Připojí se k základní adresu URL služby API management. Základní adresa URL je běžné pro všechna rozhraní API hostované na každou instanci služby API Management. API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API na konkrétní instance služby API management.

Po zadání všech hodnot, klikněte na **Uložit** k vytvoření rozhraní API a související operace. 

> [!NOTE]
> Kurz import základní kalkulačky rozhraní API ve formátu Swagger, najdete v části [Správa vašeho prvního rozhraní API v Azure API Management](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Export rozhraní API
Kromě import nových rozhraní API, můžete exportovat definice vaše rozhraní API na portálu vydavatele. Chcete-li to provést, klikněte na tlačítko **Export rozhraní API** z **kartu Souhrn** z vaší **rozhraní API**.

![Export rozhraní API][api-management-export-api]

Rozhraní API je možné exportovat pomocí WADL nebo Swagger. Vyberte požadovaný formát, klikněte na tlačítko **Uložit**a vyberte umístění, do kterého chcete soubor uložit.

![Formát exportu rozhraní API][api-management-export-api-format]

## <a name="next-steps"> </a>Další kroky
Po vytvoření rozhraní API a operace importu, můžete zkontrolovat a nakonfigurujte jakákoli další nastavení, přidat rozhraní API do produktu a publikovat, aby bylo k dispozici pro vývojáře. Další informace najdete v následujících pokynech.

* [Postup konfigurace nastavení rozhraní API][How to configure API settings]
* [Postup vytvoření a publikování produktu][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
