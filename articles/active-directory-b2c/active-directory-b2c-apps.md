<properties
    pageTitle="Azure AD B2C ve verzi Preview | Microsoft Azure"
    description="Typy aplikací, které můžete sestavit v Azure Active Directory B2C ve verzi Preview."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dastrock"/>

# Azure Active Directory B2C ve verzi Preview: Typy aplikací

Azure Active Directory (Azure AD) B2C podporuje ověřování pro celou řadu architektur moderních aplikací. Všechny jsou založeny na standardních oborových protokolech [OAuth 2.0](active-directory-b2c-reference-protocols.md) nebo [OpenID Connect](active-directory-b2c-reference-protocols.md). Tento dokument stručně popisuje typy aplikací, které můžete sestavit, nezávisle na jazyce nebo platformě, kterým dáváte přednost. Také pomáhá pochopit scénáře vysoké úrovně před tím, než [začnete sestavovat aplikace](active-directory-b2c-overview.md#getting-started).

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Základy
Každá aplikace používající Azure AD B2C musí být zaregistrovaná v [adresáři B2C](active-directory-b2c-get-started.md) přes [portál Azure](https://portal.azure.com/). Proces registrace aplikace shromáždí a přiřadí vaší aplikaci několik hodnot:

- **ID aplikace**, které jednoznačně identifikuje vaši aplikaci.
- **Identifikátor URI přesměrování**, který lze použít k cílení odpovědí zpět do aplikace.
- Další hodnoty v závislosti na scénáři. Pro další informace si přečtěte, jak [zaregistrovat aplikaci](active-directory-b2c-app-registration.md).

Po registraci aplikace komunikuje s Azure AD pomocí zasílání požadavků do koncového bodu Azure AD v2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Každý požadavek zaslaný do Azure AD B2C určuje **zásadu**. Zásady řídí chování Azure AD. Pomocí těchto koncových bodů můžete vytvořit vysoce přizpůsobitelnou sadu činností koncového uživatele. Mezi běžné zásady patří zásady registrace, přihlášení a úpravy profilu. Pokud nejste obeznámeni se zásadami, měli byste si před pokračováním přečíst o [rozšiřitelném rozhraní zásad](active-directory-b2c-reference-policies.md) Azure AD B2C.

Interakce každé aplikace s koncovým bodem v2.0 probíhá podle podobného vzoru:

1. Aplikace uživatele odkáže na koncový bod v2.0, aby se spustila [zásada](active-directory-b2c-reference-policies.md).
2. Uživatel vykoná zásadu podle její definice.
4. Aplikace obdrží z koncového bodu v2.0 nějaký druh tokenu zabezpečení.
5. Aplikace použije token zabezpečení pro přístup k chráněným informacím nebo prostředkům.
6. Server prostředků ověří token zabezpečení, aby ověřil, zda lze udělit přístup.
7. Aplikace token zabezpečení pravidelně aktualizuje.

<!-- TODO: Need a page for libraries to link to -->
Tento postup se může mírně lišit v závislosti na typu aplikace, kterou sestavujete. Opensourcové knihovny za vás mohou řešit detaily.

## Webové aplikace
U webových aplikací (včetně .NET, PHP, Javy, Ruby, Pythonu a Node.js) hostovaných na serveru a přístupných prostřednictvím prohlížeče Azure AD B2C podporuje [OpenID Connect](active-directory-b2c-reference-protocols.md) pro všechny činnosti koncového uživatele. To zahrnuje přihlášení, registraci a správu profilů. V implementaci OpenID Connect v Azure AD B2C zahajuje webová aplikace tyto činnosti koncového uživatele zasláním požadavků na ověření do Azure AD. Výsledkem požadavku je `id_token`. Tento token zabezpečení představuje identitu uživatele. Poskytuje také informace o uživateli ve formě deklarací identity:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Další informace o typech tokenů a deklaracích identity přístupných aplikaci najdete v tématu [Odkaz tokenu B2C](active-directory-b2c-reference-tokens.md).

Ve webové aplikaci se každé spuštění [zásady](active-directory-b2c-reference-policies.md) skládá z těchto kroků:

![Obrázek plaveckých drah webové aplikace](./media/active-directory-b2c-apps/webapp.png)

Ověření `id_token` pomocí veřejného podpisového klíče přijatého z Azure AD je dostačující k ověření identity uživatele. Zároveň se nastaví soubor cookie relace, který lze použít k identifikaci uživatele pro požadavky na dalších stránkách.

Chcete-li vidět tento scénář v akci, vyzkoušejte některý z příkladů kódu pro přihlášení k webové aplikaci v [oddílu Začínáme](active-directory-b2c-overview.md#getting-started).

Kromě usnadnění snadného přihlášení může webová aplikace vyžadovat přístup k back-endu webové služby. V takovém případě může webová aplikace provádět mírně odlišný [tok OpenID Connect](active-directory-b2c-reference-oidc.md) a získávat tokeny pomocí autorizačních kódů a obnovovacích tokenů. Tento scénář je znázorněn v následujícím [oddílu Webová rozhraní API](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## Webová rozhraní API
Azure AD B2C můžete použít k zabezpečení webových služeb, jako jsou vaše RESTful webová rozhraní API. Webová rozhraní API mohou k zabezpečení svých dat využít OAuth 2.0. Příchozí požadavky HTTP lze také ověřovat pomocí tokenů. Volající webového rozhraní API připojí token v hlavičce autorizace požadavku HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Webové rozhraní API pak může pomocí tokenu ověřit identitu volajícího a extrahovat informace o volajícím z deklarací identity zakódovaných v tokenu. Další informace o typech tokenů a deklaracích identity přístupných aplikaci najdete v tématu [Odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C ve verzi Preview v současné době podporuje pouze webová rozhraní API, ke kterým přistupují vlastní, dobře známí klienti. Kompletní aplikace může zahrnovat aplikaci pro iOS, aplikaci pro Android a back-end v podobě webové rozhraní API. Tato architektura je plně podporovaná. Povolení přístupu partnerského klienta, jako například další aplikace pro iOS, ke stejnému webovému rozhraní API není v současné době podporováno. Všechny komponenty výsledné aplikace musí sdílet jednotné ID aplikace.

Webové rozhraní API může přijímat tokeny z řady typů klientů, včetně webových aplikací, počítačových a mobilních aplikací, jednostránkových aplikací, démonů na straně serveru a dalších webových rozhraní API. Zde je příklad celého toku u webové aplikace, která volá webové rozhraní API:

![Obrázek plaveckých drah webového rozhraní API webové aplikace](./media/active-directory-b2c-apps/webapi.png)

Pro další informace o kódech autorizace a obnovovacích tokenech a návod, jak získat tokeny, si přečtěte o [protokolu OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

Chcete-li zjistit, jak zabezpečit webové rozhraní API pomocí Azure AD B2C, podívejte se na kurzy o webovém rozhraní API v [oddílu Začínáme](active-directory-b2c-overview.md#getting-started).

## Mobilní a nativní aplikace
Aplikace nainstalované na zařízeních, jako například mobilní aplikace a aplikace počítače, často potřebují přístup k back-endovým službám nebo webovým rozhraním API jménem uživatele. Do nativních aplikací můžete přidat vlastní činnosti správy identity a bezpečně volat back-endové služby pomocí Azure AD B2C a [toku kódu autorizace OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

V tomto toku aplikace spouští [zásady](active-directory-b2c-reference-policies.md) a přijímá `authorization_code` z Azure AD poté, co uživatel vykoná zásadu. `authorization_code` představuje oprávnění aplikace volat back-endové služby jménem aktuálně přihlášeného uživatele. Aplikace pak může na pozadí vyměnit `authorization_code` za `id_token` a `refresh_token`.  Aplikace může použít `id_token` k prokázání identity back-endovému webovému rozhraní API v požadavcích HTTP. Může také použít `refresh_token` k získání nového `id_token` po vypršení platnosti toho starého.

> [AZURE.NOTE]
    Azure AD B2C ve verzi Preview v současné době podporuje pouze tokeny typu ID, které se používají pro přístup k vlastní back-endové webové službě aplikace. Kompletní aplikace může zahrnovat aplikaci pro iOS, aplikaci pro Android a back-end v podobě webové rozhraní API. Tato architektura je plně podporovaná. Povolení přístupu aplikace pro iOS k partnerskému webovému rozhraní API pomocí přístupových tokenů OAuth 2.0 není v současné době podporováno. Všechny komponenty výsledné aplikace musí sdílet jednotné ID aplikace.

![Obrázek plaveckých drah nativní aplikace](./media/active-directory-b2c-apps/native.png)

## Současná omezení verze Preview
Azure AD B2C ve verzi Preview v současné době nepodporuje následující typy aplikací, jejich podpora je však plánovaná tak, aby byla včas k dispozici pro obecnou dostupnost. Další omezení a limity týkající se Azure AD B2C ve verzi Preview jsou popsány v tématu [Omezení a limity](active-directory-b2c-limitations.md).

### Jednostránkové aplikace (JavaScript)
Řada moderních aplikací využívá jednostránkový front-end napsaný primárně v JavaScriptu. Často používají nějaké rozhraní, například AngularJS, Ember.js nebo Durandal. Obecně dostupná služba Azure AD podporuje tyto aplikace pomocí implicitního toku OAuth 2.0. Tento tok však zatím není dostupný v Azure AD B2C. Brzy by měl být.

### Démoni nebo serverové aplikace
Aplikace, které obsahují dlouho běžící procesy nebo které pracují bez přítomnosti uživatele také potřebují způsob, jak přistupovat k zabezpečeným prostředkům, jako jsou webová rozhraní API. Tyto aplikace se mohou prokázat a získat tokeny pomocí identity aplikace (místo uživatelovy delegované identity) a pomocí toku přihlašovacích údajů klienta OAuth 2.0. 

Azure AD B2C v současné době nepodporuje tento tok. Tyto aplikace mohou získat tokeny pouze poté, co došlo k interaktivnímu uživatelskému toku. Očekáváme, že tok přihlašovacích údajů klienta bude přidán v blízké budoucnosti.

### Řetězení webových rozhraní API (tok on-behalf-of)
Mnoho architektur zahrnuje webové rozhraní API, které potřebuje volat podřízené webové rozhraní API, přičemž obě jsou zabezpečené pomocí Azure AD B2C. Tento scénář je častý u nativních klientů s back-endem v podobě webového rozhraní API. To poté zavolá online službu Microsoftu, jako je například Azure AD Graph API.

Tento scénář zřetězených webových rozhraní API může být podporován pomocí udělení přihlašovacích údajů nosiče OAuth 2.0 JWT, označovaného také jako tok on-behalf-of.  Nicméně tok on-behalf-of není v současné době v Azure AD B2C ve verzi Preview implementován.



<!--HONumber=Jun16_HO2-->


