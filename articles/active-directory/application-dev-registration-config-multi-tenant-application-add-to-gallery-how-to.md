---
title: "Postup přidání víceklientské aplikace pro galerii aplikací Azure AD | Microsoft Docs"
description: "Vysvětluje, jak můžete vytvořit seznam vlastní vyvinuté víceklientské aplikace v galerii aplikací Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 208f0d40bd7a8e8f35f16e1fcb09c305d833dbb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Postup přidání víceklientské aplikace pro galerii aplikací Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>Co je Azure AD Application Gallery?

Azure AD Application Gallery je skvělým způsobem, jak dostat aplikaci před miliony Azure Active Directory zákazníky oslovit vaší aplikace na webu Marketplace a rozšíří dopad. Níže uvedených pokynů vysvětlují, jak můžete vytvořit seznam aplikace v galerii aplikací Azure AD.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Pokud vaše aplikace podporuje SAML nebo OpenIDConnect
Pokud máte více klientů aplikace, kterou chcete zobrazit seznam v galerii aplikací Azure AD, musíte nejprve provedete jistotu, že vaše aplikace podporuje jednu z následujících jeden přihlašování technologií:

1. **OpenID Connect** -přímá integrace s Azure AD pomocí OpenID Connect pro ověřování a Azure AD souhlas rozhraní API pro konfiguraci. Pokud právě začínáte integrační a aplikace nepodporuje SAML, jedná se o doporučujeme režim.
2. **SAML** – aplikace již má možnost konfigurace poskytovatelů identit třetích stran pomocí protokolu SAML.

Pokud vaše aplikace podporuje jeden z těchto režimů jednoho přihlášení a chcete uvedení víceklientské aplikace v galerii aplikací Azure AD, můžete podle kroků v dokumentu níže. Abyste mohli rychle začít e-mailovou zprávu na  **waadpartners@microsoft.com** .

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Pokud vaše aplikace nepodporuje SAML nebo OpenIDConnect
I v případě, že vaše aplikace nepodporuje jeden z těchto režimů, jsme můžete stále integrovat do naší Galerie pomocí technologie naše heslo jednotné přihlašování. Pokud chcete prozkoumat tuto možnost, můžete odeslat e-mail na  **waadpartners@microsoft.com** .

## <a name="next-steps"></a>Další kroky
[Postup uvedení aplikace v galerii aplikací Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
