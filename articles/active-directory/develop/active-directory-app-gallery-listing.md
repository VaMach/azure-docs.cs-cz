---
title: "Výpis aplikace v galerii aplikací Azure Active Directory"
description: "Jak zobrazit aplikace, která podporuje jednotné přihlašování v galerii Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Výpis aplikace v galerii aplikací Azure Active Directory


##  <a name="what-is-azure-ad-app-gallery"></a>Co je Galerie aplikací Azure AD?

Azure AD je Cloudová služba Identity. [Galerie aplikace Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) je běžné úložiště, kde jsou všechny konektory aplikace publikována pro jednotné přihlašování a zřizování uživatelů. Podívejte se pro různé konektory aplikací SaaS, které jsou publikovány zde vzájemné zákazníkům, kteří používají Azure AD jako zprostředkovatele Identity. Správce IT přidá konektor z Galerie aplikací a konfiguruje a použít jej pro jednotné přihlašování a zřizování. Azure AD podporuje všechny hlavní federační protokoly jako SAML 2.0, OpenID Connect, OAuth a WS-Fed pro jednotné přihlašování. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Jaké jsou výhody uvedení aplikace v galerii?

*  Poskytovat nejlepší možný jeden přihlašování zákazníků.

*  Jednoduché a minimální konfigurace aplikace.

*  Zákazníci můžete hledat aplikace a najít v galerii. 

*  Každý zákazník pomocí této integrace bez ohledu na Azure AD SKU Free, Basic nebo Premium.

*  Krok v kroku konfigurace kurzu vzájemné zákazníků.

*  Povolte zřizování uživatelů pro stejnou aplikaci používáte SCIM.


##  <a name="what-are-the-pre-requisites"></a>Jaké jsou požadavky?

Pro zobrazení seznamu aplikace v galerii Azure AD, nejprve aplikace musí implementovat jednoho z protokolů federační podporovaný službou Azure AD. Přečtěte si podmínky a ujednání galerii aplikací Azure AD odsud. Pokud používáte: 

*   **OpenID Connect** – vytvoření víceklientské aplikace ve službě Azure AD a implementaci [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) pro vaši aplikaci. Poslat žádost o přihlášení k běžné koncový bod, tak, aby každý zákazník může poskytnout souhlas k aplikaci. Můžete řídit přístup uživatelů zákazníka na základě ID klienta a uživatele (UPN) přijaté v tokenu. Při integraci aplikace s Azure AD, postupujte podle [vývojáře pokyny](active-directory-authentication-scenarios.md).

*   **SAML 2.0 nebo WS-Fed** – aplikace by měl mít schopnost provést integraci SAML/WS-Fed jednotné přihlašování v režimu SP nebo deklarací identity. Jakékoli aplikaci, která podporuje SAML 2.0, můžete přímo integrovat klient služby Azure AD pomocí [pokyny k přidání vlastní aplikace](../active-directory-saas-custom-apps.md).

*   **Heslo jednotného přihlašování k** – vytvoření webové aplikace, který má přihlašovací stránku HTML konfigurace [založené na heslech jednotné přihlašování](../active-directory-appssoaccess-whatis.md). Jednotné přihlašování založené na heslech, také označuje jako heslo vaulting, můžete spravovat přístup uživatelů a hesla k webovým aplikacím, které nepodporují federaci identit. Je také užitečné v případech, kdy je potřeba několik uživatelé sdílet jeden účet, například k účtům aplikace sociálních médií vaší organizace. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Proces odesílání požadavku na portálu

Jakmile testování, svoji integraci aplikace funguje s Azure AD, budete muset odeslat žádost o přístup na našem [aplikace sítě portál](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Pokud máte účet Office 365, který můžete použít k přihlášení k tomuto portálu jinak, použijte ID služeb Microsoft (Live ID, Outlook, Hotmail atd) k přihlášení. Zobrazí následující stránka požádat o přístup. Zadejte do textového pole obchodního oprávnění a klikněte na **požádat o přístup**. Náš tým bude zkontrolujte všechny podrobnosti a získáte přístup odpovídajícím způsobem. Potom můžete přihlásit na portál a odeslat žádost o podrobné pro aplikaci.

Pokud jste čelí jakýkoli problém týkající se přístupu, obraťte se na [týmu Integrace Azure AD jednotného přihlašování k](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Žádost o přístup na portál služby SharePoint](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Časové osy
    
*   Proces výpis SAML 2.0 a WS-Fed aplikaci do Galerie - **7 – 10 pracovních dnů**

   ![Časová osa uvedení aplikace saml do Galerie](./media/active-directory-app-gallery-listing/timeline.png)

*   Proces výpis OpenID Connect aplikace do Galerie - **2 až 5 pracovních dní**

   ![Časová osa uvedení aplikace saml do Galerie](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Eskalací

Pro všechny eskalací vyřadit e-mail na [týmu Integrace Azure AD jednotného přihlašování k](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) a jsme se vám ozvat co nejdříve.

