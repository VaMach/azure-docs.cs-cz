---
title: "Jak zjistit, jaké jednotného přihlašování metodu použít | Microsoft Docs"
description: "Pochopit režimy jeden přihlašování podporované službou Azure AD a jak vybrat volbě pro aplikace, které vás zajímají."
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
ms.openlocfilehash: 80f4a965920fec9cb578c1bee235c7857f37431e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Jak zjistit, jaké jednotného přihlašování metoda se má použít

Tento článek vám pomůže lépe porozumět režimy jeden přihlašování podporované službou Azure AD a jak vybrat volbě pro aplikace, které vás zajímají.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací

Následující tabulka popisuje různé jednotné přihlašování a zřizování režimy podporované každou z výše uvedených typů aplikací. Tato tabulka vám pomůže vám pomůže lépe porozumět aplikaci, pro kterou je nutné přidat na podporu dosažení určitého cíle.

  ![Tabulka typů Asie a Tichomoří](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Jak vybrat jediný režim přihlášení

U podporovaných **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.

-   **Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě nejsou připraveny k integraci tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování

-   **Propojené přihlášení** – zvolte [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete publikovat jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Založené na heslech přihlašování** – zvolte [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete uložit tento uživatelské jméno a heslo do přehrány do aplikace později

-   **Na základě SAML přihlašování** – zvolte [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) jednotné přihlašování v režimu, pokud vaše aplikace podporuje protokoly SAML nebo OpenID Connect, nebo chcete být schopni mapování uživatelů na konkrétní aplikaci role na základě pravidel definování v deklaracích identity vaší SAML *(**Poznámka:** tato možnost není k dispozici, když je proxy aplikací nakonfigurovaný pro aplikaci) *

-   **Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete provést jednotné přihlašování k *(**Poznámka:** tato možnost je dostupná pouze v případě proxy aplikací a PingAccess konfigurován pro aplikaci) *

-   **Integrované ověřování systému Windows** – zvolte [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení, kterou chcete provést jednotné přihlašování k aplikaci WIA místní *(* *Poznámka:** tato možnost je dostupná, pouze pokud je nakonfigurován proxy aplikace pro aplikaci) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Režimy jeden přihlašování pro aplikace vyvinuté vlastní

Aplikací mít vlastní vyvinuté pomocí [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše. Mezi ně patří:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení

Pro čtení [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) Další informace o tom, jak vytvořit vyvinuté vlastní aplikaci, která podporuje tyto režimy jednom přihlášení.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Postup nastavení aplikace jediný přihlašování režim

Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

   * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

## <a name="next-steps"></a>Další kroky
[Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

