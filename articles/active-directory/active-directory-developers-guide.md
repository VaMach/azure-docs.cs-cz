<properties
   pageTitle="Příručka pro vývojáře pro službu Azure Active Directory | Microsoft Azure"
   description="Tento článek poskytuje komplexního průvodce prostředky služby Azure Active Directory určenými pro vývojáře."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/06/2016"
   ms.author="mbaldwin"/>


# Příručka pro vývojáře pro službu Azure Active Directory

## Přehled
Azure Active Directory (AD) jako platforma Správy identit jako služby (IDMaaS) poskytuje vývojářům účinný způsob, jak integrovat do svých aplikací správu identity. Následující články poskytují přehled o implementaci a klíčových funkcích služby Azure AD. Doporučujeme číst je popořadě, nebo pokud jste připraveni se do toho pustit, přejděte na položku [Začínáme](#getting-started).


1. [Výhody integrace služby Azure Active Directory](active-directory-how-to-integrate.md): Zjistěte, proč integrace se službou Azure AD nabízí nejlepší řešení pro zabezpečené přihlašování a autorizaci.

1. [Scénáře ověřování služby Active Directory](active-directory-authentication-scenarios.md): Využijte výhody zjednodušeného ověřování ve službě Azure AD, které vám umožní přihlášení do vaší aplikace.

1. [Integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md): Zjistěte, jak přidávat, aktualizovat a odebírat aplikace ze služby Azure AD, a přečtěte si pokyny k přizpůsobení prostředí značce pro integrované aplikace.

1. [Azure Active Directory Graph API](active-directory-graph-api.md): Použijte rozhraní Azure AD Graph API k programovému přístupu ke službě Azure AD prostřednictvím koncových bodů rozhraní REST API. Rozhraní Azure AD Graph API je také přístupné prostřednictvím sjednoceného rozhraní API [Microsoft Graph](https://graph.microsoft.io/), které umožňuje přístup k několika rozhraním API cloudových služeb společnosti Microsoft pomocí jednoho koncového bodu rozhraní REST API a jednoho přístupového tokenu.

1. [Knihovny ověřování služby Azure Active Directory](active-directory-authentication-libraries.md): Snadno ověřte uživatele, aby získali přístupové tokeny, pomocí knihoven ověřování služby Azure AD pro rozhraní .NET, JavaScript, Objective-C, Android a další.


## Začínáme

Tyto kurzy jsou přizpůsobené různým platformám a umožní vám rychle začít s vývojem pro službu Azure Active Directory. Předpokladem je, že [získáte klienta služby Azure Active Directory](active-directory-howto-tenant.md).

### Průvodce rychlým zahájením pro mobilní a počítačové aplikace

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Univerzální pro Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Přímá integrace s protokoly OAuth 2.0](active-directory-protocols-oauth-code.md)|

### Průvodce rychlým zahájením pro webové aplikace

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID Connect](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Přímá integrace s protokoly OpenID](active-directory-protocols-openid-connect-code.md)|

### Průvodce rychlým zahájením pro webové rozhraní API

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### Průvodce rychlým zahájením pro dotazování adresáře

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](active-directory-graph-api-quickstart.md)|

## Postupy

Tyto články popisují, jak provádět určité úlohy pomocí služby Azure Active Directory:

- [Získání tenanta služby Azure AD](active-directory-howto-tenant.md)
- [Přihlášení kteréhokoli uživatele služby Azure AD pomocí vzoru aplikace s víc tenanty](active-directory-devhowto-multi-tenant-overview.md) 
- Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL v zařízeních [Android](active-directory-sso-android.md) a [iOS](active-directory-sso-ios.md)
- [Získání certifikace AppSource pro aplikaci pro službu Azure AD](active-directory-devhowto-appsource-certified.md)
- [Uvedení aplikace v galerii aplikací služby Azure AD](active-directory-app-gallery-listing.md)
- [Odesílání webových aplikací pro Office 365 na řídicí panel prodejce](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Porozumění manifestu aplikace služby Azure Active Directory](active-directory-application-manifest.md)
- [Vysvětlení zásad značky pro tlačítka přihlašování a pořizování aplikace v klientské aplikaci](active-directory-branding-guidelines.md)
- [Náhled: Jak vytvářet aplikace, které uživatele přihlašují pomocí osobních i pracovních či školních účtů](active-directory-appmodel-v2-overview.md)
- [Náhled: Jak vytvářet aplikace, které registrují a přihlašují uživatele](../active-directory-b2c/active-directory-b2c-overview.md)


## Referenční informace

Tyto články poskytují základní referenční informace o rozhraních REST a knihovny pro ověřování, protokolech, chybách, ukázkových kódech a koncových bodech.  

###  Podpora
- [Otázky se značkou](http://stackoverflow.com/questions/tagged/azure-active-directory): Najděte řešení pro službu Azure Active Directory na webu Stack Overflow vyhledáním značek [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) a [adal](http://stackoverflow.com/questions/tagged/adal).

### Kód

- [Knihovny open-source ve službě Azure Active Directory](http://github.com/AzureAD): Nejjednodušší způsob, jak najít zdroj knihovny, je pomocí [seznamu knihoven](active-directory-authentication-libraries.md).

- [Ukázky pro službu Azure Active Directory](https://github.com/azure-samples?query=active-directory): Nejjednodušší způsob, jak se orientovat v seznamu ukázek, je pomocí [rejstříku ukázkových kódů](active-directory-code-samples.md).

- [ADAL pro .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx): Dokumentace knihoven ověřování .NET.

### Graph API

- [Referenční informace o rozhraní Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx): Referenční příručka rozhraní REST API pro Azure Active Directory Graph API. [Zobrazení interaktivního referenčního prostředí rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Obory oprávnění rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): Obory oprávnění OAuth 2.0, které slouží k řízení přístupu aplikace k datům adresáře v klientovi.

### Ověřovací a autorizační protokoly

- [Výměna podpisového klíče ve službě Azure AD](active-directory-signing-key-rollover.md): Získejte další informace o četnosti výměny podpisových klíčů služby Azure AD a o tom, jak aktualizovat klíč ve většině běžných scénářů aplikace.

- [Protokol OAuth 2.0: Použití udělení autorizačního kódu](active-directory-protocols-oauth-code.md): Pomocí udělení autorizačního kódu v protokolu OAuth 2.0 můžete autorizovat přístup k webovým aplikacím a webovým rozhraním API v tenantovi služby Azure Active Directory.

- [Protokol OAuth 2.0: Vysvětlení implicitního udělení](active-directory-dev-understanding-oauth2-implicit-grant.md): Získejte další informace o implicitním udělení autorizace a tom, jestli je pro vaši aplikaci vhodné.

- [Protokol OAuth 2.0: Volání mezi službami za použití přihlašovacích údajů klienta](active-directory-protocols-oauth-service-to-service.md): Tok udělování přihlašovacích údajů klienta protokolu OAuth 2.0 umožňuje webové službě (důvěrnému klientovi) ověření pomocí vlastních přihlašovacích údajů při volání jiné webové služby, takže nemusí používat zosobnění uživatele. V tomto scénáři je klient většinou webová služba střední vrstvy, služba démona nebo webová stránka.

- [Protokol OpenID Connect 1.0: Přihlášení a ověření](active-directory-protocols-openid-connect-code.md): Protokol OpenID Connect 1.0 rozšiřuje OAuth 2.0 pro použití jako ověřovací protokol. Klientská aplikace můžete ke správě procesu přihlašování dostat id_token nebo se může tok autorizačního kódu rozšířit tak, aby přijímal jak id_token, tak autorizační kód.

- [Referenční informace o protokolu SAML 2.0](active-directory-saml-protocol-reference.md): Protokol SAML 2.0 umožňuje aplikacím poskytovat jednotné přihlašování svým uživatelům.

- [Protokol WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory podporuje protokol WS-Federation 1.2 podle specifikace Web Services Federation verze 1.2. Další informace o dokumentu federačních metadat najdete v článku [Federační metadata](active-directory-federation-metadata.md).

- [Podporované typy tokenů a deklarací identity](active-directory-token-and-claims.md): Tento průvodce vám pomůže pochopit a vyhodnotit deklarace identity v  tokenech SAML 2.0 a webových tokenech JSON (JWT).

## Videa

### Sestavení

V těchto prezentacích s přehledem vývoje aplikací pro službu Azure Active Directory vystupují řečníci, kteří pracují přímo v technickém týmu.  Prezentace zahrnují základní témata, včetně IDMaaS, ověřování, federace identity a jednotného přihlašování.

- [Microsoft Identity: Stav sjednocení a budoucí směr](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Správa identit jako služby pro moderní aplikace](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Vývoj moderních webových aplikací pomocí služby Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Vývoj moderních nativních aplikací pomocí služby Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### Azure Friday
[Azure Friday](https://azure.microsoft.com/documentation/videos/azure-friday/) je série videí 1:1, která se opakuje každý pátek a přináší krátké rozhovory (10 – 15 minut) s odborníky na různá témata služby Azure.  Pomocí funkce filtru služeb na stránce zobrazíte všechna videa na téma Azure Active Directory.

- [Azure Identity 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure Identity 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure Identity 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## Sociální

- [Blog týmu služby Active Directory](http://blogs.technet.com/b/ad/): nejnovější vývoj ve světě služby Azure Active Directory.

- [Blog týmu rozhraní API Azure Active Directory Graph](http://blogs.msdn.com/b/aadgraphteam): informace o službě Azure Active Directory, které jsou specifické pro rozhraní Graph API.

- [Cloudová identita](http://www.cloudidentity.net): Nápady na správu identit jako služby z hlavního PM služby Azure Active Directory.  

- [Azure Active Directory na Twitteru](https://twitter.com/azuread): Oznámení služby Azure Active Directory v délce 140 znaků nebo méně.



<!--HONumber=Aug16_HO4-->


