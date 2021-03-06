---
title: "Výpis aplikace v galerii aplikací Azure Active Directory | Microsoft Docs"
description: "Jak zobrazit aplikace, která podporuje jednotné přihlašování v galerii aplikací Azure Active Directory"
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
ms.openlocfilehash: 502fb555bd3b381c9be0ff04e210cc07f9bf6cd8
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Uvedení aplikace v galerii aplikací služby Azure Active Directory


##  <a name="what-is-the-azure-ad-application-gallery"></a>Co je galerii aplikací Azure AD?

Azure Active Directory (Azure AD) je služba cloudových identit. [Galerii aplikací Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) je v obchodě s aplikacemi Azure Marketplace, kde jsou všechny konektory aplikace publikována pro jednotné přihlašování a zřizování uživatelů. Zákazníci, kteří používají Azure AD jako zprostředkovatele identity najít jiné konektory aplikací SaaS publikované. Správci IT přidejte konektory z Galerie aplikace a potom nakonfigurovat a používat konektory pro jednotné přihlašování a zřizování. Azure AD podporuje všechny hlavní federační protokoly pro jednotné přihlašování, včetně SAML 2.0, OpenID Connect, OAuth a WS-Fed. 

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Jaké jsou výhody uvedení aplikace v galerii?

*  Zákazníci najít optimální jeden přihlašování prostředí.

*  Konfigurace aplikace je jednoduchý a minimální. 

*  Rychlé vyhledávání vyhledá aplikace v galerii.

*  Uvolněte, Basic, a všechny Azure AD Premium zákazníci mohou využít této integrace. 

*  Vzájemné zákazníkům získat podrobné konfigurace kurzu. 

*  Zákazníci, kteří používají SCIM můžete použít zřizování pro stejnou aplikaci.


##  <a name="prerequisites-implement-federation-protocol"></a>Předpoklady: Protokol federační implementace

Pro zobrazení seznamu aplikace v galerii aplikací Azure AD, musíte nejprve implementovat jednu z následujících protokolů federační podporovaný službou Azure AD. Přečtěte si podmínky a ujednání galerii aplikací Azure AD odsud. 

*   **OpenID Connect**: vytvoření víceklientské aplikace ve službě Azure AD a implementaci [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) pro vaši aplikaci. Poslat žádost o přihlášení k běžné koncový bod, tak, aby každý zákazník může poskytnout souhlas k aplikaci. Můžete řídit přístup uživatele na základě ID klienta a uživatele (UPN) přijaté v tokenu. Při integraci aplikace s Azure AD, postupujte podle [vývojáře pokyny](active-directory-authentication-scenarios.md).

*   **SAML 2.0** nebo **WS-Fed**: aplikace musí mít možnost provést integraci SAML/WS-Fed jednotné přihlašování v režimu SP nebo deklarací identity. Pokud vaše aplikace podporuje SAML 2.0, můžete integrovat přímo do klienta služby Azure AD pomocí [pokyny k přidání vlastní aplikace](../active-directory-saas-custom-apps.md).

*   **Heslo jednotného přihlašování k**: vytvoření webové aplikace, který má přihlašovací stránku HTML konfigurace [založené na heslech jednotné přihlašování](../active-directory-appssoaccess-whatis.md). Jednotné přihlašování založené na heslech, také označuje jako heslo vaulting, můžete spravovat přístup uživatelů a hesla k webovým aplikacím, které nepodporují federaci identit. Je také užitečné v případech, ve kterých několik uživatelé muset sdílet jeden účet, například k účtům aplikace sociálních médií vaší organizace. 

## <a name="submit-the-request-in-the-portal"></a>Odešlete žádost na portálu

Po otestujete, že vaše integraci aplikací funguje s Azure AD, odešlete žádost o přístup na našem [aplikace sítě portál](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Pokud máte účet Office 365, použijte k přihlášení k tomuto portálu. Pokud ne, přihlaste se pomocí účtu Microsoft (například aplikaci Outlook nebo Hotmail).

Po přihlášení, zobrazí se následující stránka. Zadejte obchodního oprávnění pro přístup do textového pole, která potřebuje a pak vyberte **požádat o přístup**. Náš tým zkontroluje podrobnosti a zajišťuje přístup odpovídajícím způsobem. Potom můžete přihlásit na portál a odeslat žádost o podrobné pro aplikaci.

Pokud máte problémy týkající se přístupu, obraťte se [týmu Integrace Azure AD jednotného přihlašování k](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Žádost o přístup na portál služby SharePoint](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Časové osy
    
Časová osa pro proces výpis SAML 2.0 nebo WS-Fed aplikace v galerii je 7 – 10 pracovních dnů.

   ![Časová osa uvedení aplikace saml do Galerie](./media/active-directory-app-gallery-listing/timeline.png)

Časová osa pro proces výpis OpenID Connect aplikace v galerii je 2 až 5 pracovních dní.

   ![Časová osa uvedení aplikace saml do Galerie](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Eskalací

Pro všechny eskalací odesílat e-mailu [týmu Integrace Azure AD jednotného přihlašování k](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) a budete odpovíte co nejdříve.

