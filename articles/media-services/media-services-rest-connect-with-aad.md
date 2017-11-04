---
title: "Ověřování pomocí služby Azure AD pro přístup k rozhraní API služby Azure Media Services se zbytkem | Microsoft Docs"
description: "Zjistěte, jak získat přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure Active Directory pomocí REST."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: e5d7a5ec1c28a552420aba5e2cd6c8c7bbf4213d
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Používat pro přístup k rozhraní API Azure Media Services se zbytkem ověřování Azure AD

Tým služby Azure Media Services vydala Podpora ověřování Azure Active Directory (Azure AD) pro přístup k Azure Media Services. Oznámeno také plány přestat používat ověřování služby Řízení přístupu Azure pro Media Services přístup. Protože každé předplatné služby Azure a každý účet Media Services je připojený k klient služby Azure AD, Azure AD Podpora ověřování přináší spoustu výhod, zabezpečení. Podrobnosti o této změny a migrace (Pokud používáte sadu Media Services .NET SDK pro aplikaci) naleznete v následujících příspěvcích na blogu a články:

- [Azure Media Services ohlášen podpory pro Azure AD a vyřazení ověřování řízení přístupu](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md)
- [Používat ověřování Azure AD pro přístup k rozhraní API služby Azure Media Services pomocí rozhraní Microsoft .NET](media-services-dotnet-get-started-with-aad.md)
- [Začínáme s ověřováním Azure AD pomocí portálu Azure](media-services-portal-get-started-with-aad.md)

Někteří zákazníci potřebovat pro vývoj řešení pro jejich Media Services v seznamu následující omezení:

*   Používají programovací jazyk, který není rozhraní Microsoft .NET nebo C# nebo běhové prostředí není systému Windows.
*   Knihovny Azure AD, jako je například knihovny ověřování služby Active Directory nejsou k dispozici pro programovací jazyk nebo nelze použít pro jejich prostředí runtime.

Někteří zákazníci mají aplikace vyvinuté pomocí rozhraní REST API pro řízení přístupu, ověřování a přístup k Azure Media Services. U těchto zákazníků potřebujete způsob, jak používat pouze rozhraní REST API pro ověřování Azure AD a následné přístup k Azure Media Services. Budete muset není závislý na žádném z knihovny Azure AD nebo na sadu Media Services .NET SDK. V tomto článku jsme popisují řešení a poskytují ukázkový kód pro tento scénář. Protože kód je všech voláních REST API s žádná závislost na všechny Azure AD nebo knihovny Azure Media Services, kód lze snadno přeložit do dalších programovacích jazyků.

> [!IMPORTANT]
> V současné době Media Services podporuje model řízení přístupu Azure služby ověřování. Řízení přístupu ověřování však bude zastaralá 1. června 2018. Doporučujeme, abyste přenést do Azure AD authentication modelu co nejdříve.


## <a name="design"></a>Návrh

V tomto článku používáme následující návrhu ověřování a autorizace:

*  Povolení protokolu: OAuth 2.0. OAuth 2.0 je standard zabezpečení webové, které pokrývá ověřování a autorizace. Podporuje Google, Microsoft, Facebook a PayPal. Ho byl ratifikován říjen 2012. Společnost Microsoft podporuje pevně OAuth 2.0 a OpenID Connect. Obě tyto standardy jsou podporovány více služeb a knihovny klienta, včetně Azure Active Directory, Open Web Interface pro .NET (OWIN) Katana a knihovny Azure AD.
*  Udělit typu: Typ udělení pověření klienta. Pověření klienta je jeden z typů čtyři grant v OAuth 2.0. Často se používá pro přístup k Azure AD Microsoft Graph API.
*  Režim ověřování: instanční objekt. Další režim ověřování je uživatele nebo interaktivního ověřování.

Celkem čtyři služby nebo aplikace jsou součástí Azure AD ověřování a autorizace tok pro pomocí služby Media Services. Aplikace a služby a toku, jsou popsané v následující tabulce:

|Typ aplikace |Aplikace |Tok|
|---|---|---|
|Klient | Zákazník aplikace nebo řešení | Tuto aplikaci (ve skutečnosti, jeho proxy) je zaregistrován v klientovi Azure AD, ve kterém jsou umístěny předplatné Azure a účet služby média. Objekt služby registrovaná aplikace se pak udělí s vlastníkem nebo přispěvatelem rolí v přístupu k řízení (IAM) účtu služby media. Objekt služby je reprezentována app ID a klienta tajný klíč klienta. |
|Zprostředkovatel identity (IDP) | Azure AD jako poskytovatel identity | Ověření Azure AD jako rozšíření IDP registrované app service hlavní (ID klienta a tajný klíč klienta). Toto ověřování se provádí interně a implicitně. Jako tok přihlašovacích údajů klienta nikoli na uživatele ověření klienta. |
|Zabezpečení tokenu služby (STS) nebo OAuth server | Azure AD jako služba tokenů zabezpečení | Po ověření IDP (nebo OAuth serveru v podmínkách OAuth 2.0) na přístupový token nebo Token pro webové JSON (JWT) vydává Azure AD jako Server služby tokenů zabezpečení nebo OAuth pro přístup k prostředku střední vrstvy: v našem případě koncový bod Media Services REST API. |
|Prostředek | Rozhraní REST API služby Media Services | Každé volání Media Services REST API je autorizovat přístupový token, který je vydán Azure AD jako služba tokenů zabezpečení nebo OAuth server. |

Pokud spustíte ukázkový kód a zachycení token JWT nebo přístupový token, token JWT má následující atributy:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Zde jsou mapování mezi atributy v token JWT a čtyři aplikace nebo služby v předchozí tabulce:

|Typ aplikace |Aplikace |Atribut JWT |
|---|---|---|
|Klient |Zákazník aplikace nebo řešení |AppID: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6". ID klienta aplikace zaregistrujete do služby Azure AD v další části. |
|Zprostředkovatel identity (IDP) | Azure AD jako poskytovatel identity |rozšíření IDP: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/" GUID je klient ID společnosti Microsoft (microsoft.onmicrosoft.com). Každý klient má svou vlastní, jedinečné ID. |
|Zabezpečení tokenu služby (STS) nebo OAuth server |Azure AD jako služba tokenů zabezpečení | iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/". Identifikátor GUID je klient ID společnosti Microsoft (microsoft.onmicrosoft.com). |
|Prostředek | Rozhraní REST API služby Media Services |oblast: "https://rest.media.azure.net". K příjemce nebo cílovou skupinu přístupového tokenu. |

## <a name="steps-for-setup"></a>Kroky pro instalaci

K registraci a nastavit aplikaci Azure Active Directory (AAD) a získat klíče pro volání koncový bod REST API služby Azure Media Services, naleznete v článku [Začínáme s ověřováním Azure AD pomocí portálu Azure](media-services-portal-get-started-with-aad.md)


## <a name="info-to-collect"></a>Informace o shromažďování

Příprava kódování REST, shromažďování těchto datových bodů pro zahrnutí do kódu:

*   Azure AD jako koncový bod služby tokenů zabezpečení: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Z tohoto koncového bodu se požaduje přístupový token JWT. Kromě slouží jako IDP, Azure AD slouží také jako služby tokenů zabezpečení. Azure AD vydá token JWT pro přístup k prostředkům (přístupový token). JWT token má různé deklarace identity.
*   Azure Media Services REST API jako prostředek nebo cílové skupiny: https://rest.media.azure.net.
*   ID klienta: Viz krok 2 v [kroky pro instalaci](#steps-for-setup).
*   Tajný klíč klienta: viz krok 2 v [kroky pro instalaci](#steps-for-setup).
*   Účet Media Services koncový bod REST API v následujícím formátu:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Toto je koncový bod, na které všechny Media Services REST API v aplikaci volání. Například https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Těchto pět parametrů můžete pak umístit do souboru web.config nebo app.config pro použití v kódu.

## <a name="sample-code"></a>Ukázka kódu

Můžete najít ukázkový kód v [ověřování Azure AD pro Azure Media Services přístup: I přes REST API](https://github.com/willzhan/WAMSRESTSoln).

Ukázkový kód má dvě části:

*   Projekt knihovny DLL, který má kód rozhraní REST API pro Azure AD ověřování a autorizaci. Je také metodu pro volání rozhraní REST API Media Services REST API koncový bod s tímto tokenem přístupu.
*   Klient test, konzoly, která zahájí ověřování Azure AD a volá jinou Media Services REST API.

Ukázkový projekt má tři funkce:

*   Ověření Azure AD pomocí pověření klienta udělit pomocí REST API.
*   Azure Media Services přístup pomocí REST API.
*   Azure přístup k úložišti pomocí pouze REST API (jak je použita k vytvoření účtu Media Services, pomocí rozhraní REST API).


## <a name="where-is-the-refresh-token"></a>Kde je token obnovení?

Může požádat některé čtečky: kde je token obnovení? Proč není zde použít token obnovení?

Účelem token obnovení je nechcete aktualizovat přístupový token. Je určený obejít ověřování koncového uživatele a stále získat token platný přístup, když vyprší platnost tokenu starší. Lepší název tokenu obnovení může být něco jako "Nepoužívat zpětný-registrace v token uživatele."

Pokud používáte OAuth 2.0 autorizace udělit tok (uživatelské jméno a heslo, které jednají jménem uživatele), umožňuje obnovovací token získání tokenu pro přístup k obnovené bez zásahu uživatele. Pro tok, který je popsaný v tomto článku poskytování pověření klienta OAuth 2.0 však klient funguje na vlastní účet. Nepotřebujete zásahu uživatele vůbec, a server ověřování není nutné získáte token obnovení. Pokud ladíte **GetUrlEncodedJWT** metoda, zjistíte, že odpověď z koncový bod token má přístupový token, ale žádné token obnovení.

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů do účtu](media-services-dotnet-upload-files.md).
