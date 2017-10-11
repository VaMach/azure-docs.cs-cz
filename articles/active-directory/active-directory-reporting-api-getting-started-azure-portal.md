---
title: "Začínáme s Azure AD reporting rozhraní API | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory, vytváření sestav rozhraní API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9944cbd2b1b7c4acb18d37da1394c0bbc170f77d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Začínáme s Azure Active Directory, vytváření sestav rozhraní API

Azure Active Directory nabízí celou řadu sestav. Data z těchto sestav můžou být velmi užitečná pro vaše aplikace, jako jsou systémy SIEM nebo nástroje pro auditování a business intelligence. Rozhraní API pro generování sestav v Azure AD poskytují programový přístup k těmto datům prostřednictvím sady rozhraní API založených na REST. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Tento článek vám poskytne informace, které potřebujete začít s Azure AD reporting rozhraní API.
V další části můžete najít další podrobnosti o používání auditu a přihlaste se rozhraní API. 

Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq). O problémech, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)

## <a name="learning-map"></a>Výuková mapa
1. **Příprava** -mohli otestovat vaši ukázky rozhraní API, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites-azure-portal.md).
2. **Prozkoumejte** -získal první dojem o rozhraní API pro vytváření sestav:
   
   * [Pomocí ukázky pro audit rozhraní API](active-directory-reporting-api-audit-samples.md) 
   * [Pomocí ukázky pro sestavu přihlašovací aktivita rozhraní API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Přizpůsobení** -vytvořit vlastní řešení: 
   
   * [Pomocí auditu referenční dokumentace rozhraní API](active-directory-reporting-api-audit-reference.md) 
   * [Pomocí odkaz na sestavu API přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Další kroky
Pokud jste zvědaví zobrazíte všechny koncové body Azure AD Graph API k dispozici, použijte tento odkaz: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).

