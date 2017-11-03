---
title: "Azure Active Directory B2C: Předdefinované zásady | Microsoft Docs"
description: "Téma na rozhraní rozšiřitelných zásad služby Azure Active Directory B2C a o tom, jak vytvořit různé typy zásad"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: daad3af089afdf76b930053728bb11a5cf4c2a92
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Předdefinované zásady


Rozhraní rozšiřitelných zásad služby Azure Active Directory (Azure AD) B2C je základní sílu služby. Zásady, jako plně popisují činnosti identity příjemce registrace, přihlášení nebo úpravy profilu. Například registrační zásadě můžete řídit chování konfigurací následujícího nastavení:

* Typy účtů (jako je Facebook sociálních účty) nebo místní účty například e-mailové adresy, které mohou příjemci použít se přihlásit k aplikaci
* Atributy (například křestní jméno, poštovní směrovací číslo a velikosti), které se mají shromažďovat od příjemce během registrace
* Použití Azure Multi-Factor Authentication
* Vzhledu a chování registrace všechny stránky
* Informace o (manifesty jako deklarace do tokenu), aplikace obdrží, kdy zásady spustit dokončení

Můžete vytvořit několik zásad různých typů ve vašem klientovi a podle potřeby používat ve svých aplikacích. Zásady můžete opětovně použít napříč aplikací. Tato možnost umožňuje vývojářům definování a úprava činnosti identity uživatelů s minimální nebo žádný změny svůj kód.

Zásady jsou k dispozici pro použití prostřednictvím rozhraní jednoduché developer. Vaše aplikace aktivuje zásadu pomocí standardní požadavek ověřování HTTP (předání parametru zásad v žádosti o) a obdrží token přizpůsobené jako odpověď. Jediným rozdílem mezi požadavky, které vyvolají registrační zásadě a požadavky, které vyvolají zásad přihlašování je například název zásady, který se používá v parametru řetězce dotazu "p":

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Další informace o rozhraní zásad najdete v tématu [tomto příspěvku na blogu o Azure AD B2C na blogu Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-or-sign-in-policy"></a>Vytvoření zásady registrace nebo přihlášení

Tato zásada zpracuje obě příjemce s konfigurací jedné možnosti registrace a přihlášení. Příjemci knihovny jsou vedla dolů správné cesty (registrace nebo přihlášení) v závislosti na kontextu. Také popisuje obsah tokeny, které aplikace se zobrazí po úspěšné zápisy nebo přihlášení.  Ukázka kódu pro zásady registrace nebo přihlášení je [k dispozici zde](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Je doporučeno použití této zásady prostřednictvím zásad přihlašování a registrační zásadě.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Vytvořit zásadu registrace

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Vytvoření zásady přihlášení

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Vytvoření profilu úpravy zásad

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Vytvoření zásady resetování hesel

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Jak lze propojit zásady registrace nebo přihlášení se zásady resetování hesel?
Při vytváření zásad registrace nebo přihlášení (s místní účty), uvidíte **zapomněli jste heslo?** odkaz na první stránce prostředí. Kliknutím na tento odkaz není automaticky aktivační událost heslo resetovat zásady. 

Místo toho kód chyby  **`AADB2C90118`**  se vrátí do vaší aplikace. Aplikace je potřeba zpracovat tomto kódu chyby vyvoláním zásad resetování hesel konkrétní. Další informace najdete v tématu [vzorku, který ukazuje přístup propojení zásad](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Použít zásadu registrace nebo přihlášení nebo registraci zásady a zásady přihlášení?
Doporučujeme použití registrace nebo přihlášení zásady prostřednictvím registrace zásad a přihlášení.  

Zásady registrace nebo přihlášení k dispozici další možnosti než zásady přihlášení. Také umožňuje používat přizpůsobení uživatelského rozhraní stránky a má lepší podporu pro lokalizaci. 

Zásady přihlášení se doporučuje, pokud nepotřebujete k lokalizaci zásad, pouze nutnost branding schopnosti menší přizpůsobení a mají heslo resetovat integrovaný do ní.

## <a name="next-steps"></a>Další kroky
* [Token, relace a jednotné přihlašování](active-directory-b2c-token-session-sso.md)
* [Zakázání e-mailu ověření během registrace příjemce](active-directory-b2c-reference-disable-ev.md)

