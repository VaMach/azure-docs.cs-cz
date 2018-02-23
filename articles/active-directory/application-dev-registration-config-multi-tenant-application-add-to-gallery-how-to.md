---
title: "Postup přidání víceklientské aplikace pro galerii aplikací Azure AD | Microsoft Docs"
description: "Vysvětluje, jak můžete vytvořit seznam vlastní vyvinuté víceklientské aplikace v galerii aplikací Azure AD"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Postup přidání víceklientské aplikace pro galerii aplikací Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>Co je Azure AD Application Gallery?

Azure AD je Cloudová služba Identity. [Galerie aplikace Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) je běžné úložiště, kde jsou všechny konektory aplikace publikována pro jednotné přihlašování a zřizování uživatelů. Podívejte se pro různé konektory aplikací SaaS, které jsou publikovány zde vzájemné zákazníkům, kteří používají Azure AD jako zprostředkovatele Identity. Správce IT přidá konektor z Galerie aplikací a konfiguruje a použít jej pro jednotné přihlašování a zřizování. Azure AD podporuje všechny hlavní federační protokoly jako SAML 2.0, OpenID Connect, OAuth a WS-Fed pro jednotné přihlašování. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Pokud vaše aplikace podporuje SAML nebo OpenIDConnect
Pokud máte více klientů aplikace, kterou chcete zobrazit seznam v galerii aplikací Azure AD, musíte nejprve provedete jistotu, že vaše aplikace podporuje jednu z následujících jeden přihlašování technologií:

1. **OpenID Connect** – vytvoření víceklientské aplikace ve službě Azure AD a implementaci [framework souhlasu Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) pro vaši aplikaci. Poslat žádost o přihlášení k běžné koncový bod, tak, aby každý zákazník může poskytnout souhlas k aplikaci. Můžete řídit přístup uživatelů zákazníka na základě ID klienta a uživatele (UPN) přijaté v tokenu. Odešlete prosím aplikaci, jak je uvedeno v tomto [článku](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

2. **SAML** – Pokud vaše aplikace podporuje SAML 2.0 potom jsme můžete uvedení aplikace v galerii a jsou uvedeny pokyny [sem](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Pokud vaše aplikace podporuje jeden z těchto režimů jednoho přihlášení a chcete uvedení víceklientské aplikace v galerii aplikací Azure AD, můžete provést kroky uvedené v [to](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) článku. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Pokud vaše aplikace nepodporuje SAML nebo OpenIDConnect
I v případě, že vaše aplikace nepodporuje jeden z těchto režimů, jsme můžete stále integrovat do naší Galerie pomocí technologie naše heslo jednotné přihlašování.

**Heslo jednotného přihlašování k** – vytvoření webové aplikace, který má přihlašovací stránku HTML konfigurace [založené na heslech jednotné přihlašování](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). Jednotné přihlašování založené na heslech, také označuje jako heslo vaulting, můžete spravovat přístup uživatelů a hesla k webovým aplikacím, které nepodporují federaci identit. Je také užitečné v případech, kdy je potřeba několik uživatelé sdílet jeden účet, například k účtům aplikace sociálních médií vaší organizace. 

Pokud chcete uvedení aplikace potom pomocí této technologie, odešlete prosím žádost jak je popsáno v [to](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) článku.

## <a name="escalations"></a>Eskalací

Pro všechny eskalací vyřadit e-mail na [týmu Integrace Azure AD jednotného přihlašování k](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) a jsme se vám ozvat co nejdříve.

## <a name="next-steps"></a>Další postup
[Postup uvedení aplikace v galerii aplikací Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
