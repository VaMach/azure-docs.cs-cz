---
title: "Vlastní konektor časté otázky – Azure Logic Apps | Microsoft Docs"
description: "Nejčastější dotazy o požadavcích, aktivační události, a tak dále o vytváření vlastních konektorů"
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
ms.openlocfilehash: 225e56de3985acae871ddec447b763e7de61cb80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="faq-custom-connectors"></a>Nejčastější dotazy: Vlastní konektory

## <a name="requirements"></a>Požadavky

**Otázka:** můžete vytvořit konektor bez rozhraní REST API? </br>
**Odpověď:** Ne, pokud chcete vytvořit konektor, je nutná podpora stabilní HTTP REST API pro vaši službu. 

**Otázka:** I jaké nástroje můžete použít k vytvoření konektoru? </br>
**Odpověď:** Azure má funkce a služby, které můžete použít pro vystavení jakoukoli službu, jako rozhraní API, například Azure App Service pro hostování API Management a další.

**Otázka:** jaké typy ověřování jsou podporovány? </br>
**Odpověď:** těchto standardů podporovaných ověřování můžete použít:

* [OAuth 2.0](https://oauth.net/2/), včetně [Azure Active Directory](https://azure.microsoft.com/develop/identity/) nebo konkrétní služby, jako je Dropbox, Githubu a SalesForce
* Obecné OAuth 2.0
* [Základní ověřování](https://swagger.io/docs/specification/authentication/basic-authentication/)
* [Klíč rozhraní API](https://swagger.io/docs/specification/authentication/api-keys/)

## <a name="triggers"></a>Triggery

**Otázka:** můžete vytvořit aktivační události bez webhooky? </br>
**Odpověď:** Ne, vlastní konektory pro Azure Logic Apps a Microsoft Flow podporu pouze na základě webhooku aktivační události. Pokud chcete požádat o další vzory pro implementaci, obraťte se na [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com) s dalšími podrobnostmi o rozhraní API.

## <a name="certification"></a>Certifikace

**Q**: nejsem partnera Microsoftu nebo nezávislého výrobce softwaru (ISV). Vytvoření konektory </br>
**A**: Ano, můžete zaregistrovat tyto konektory pro interní použití ve vaší organizaci, ale pokud chcete k certifikaci a veřejně uvolněte konektor, musí buď vlastní základní služby nebo existuje explicitní práva na používání rozhraní API.

## <a name="other"></a>Ostatní

**Otázka:** Moje rozhraní API použijte dynamické hostitele. Jak implementovat je pomocí OpenAPI? </br>
**Odpověď:** vlastní konektory nepodporují dynamické hostitele. Místo toho použijte statické hostitele pro účely vývoje a testování. Pokud chcete k certifikaci vašeho konektoru, požádejte o dynamické implementace kontaktu společnosti Microsoft.

**Otázka:** podporují Postman kolekce V2? </br>
**Odpověď:** Ne, je momentálně podporován pouze Postman kolekce V1.

**Otázka:** podporují OpenAPI 3.0? </br>
**Odpověď:** Ne, je momentálně podporován pouze OpenAPI 2.0.