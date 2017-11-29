---
title: "Ověřování a autorizace ve službě Azure App Service | Microsoft Docs"
description: "Reference konceptu a Přehled ověřování / autorizace funkcí pro Azure App Service"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: f0d2644903181cd2e20166feae4f90ddd4037fa8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Ověřování a autorizace v prostředí Azure App Service
## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / autorizace?
Aplikace služby ověřování / autorizace je funkce, která poskytuje způsob, jak vaše aplikace k přihlášení uživatele, takže není nutné změnit kód na back-end aplikace. Poskytuje snadný způsob, jak chránit vaše aplikace a pracovat s daty na uživatele.

Služby App Service používá federovaných identit, ve kterém poskytovatel identity jiného výrobce uchovává účty a ověřuje uživatele. Aplikace spoléhá na informace o identitě poskytovatele tak, aby aplikace bude obsahovat tyto informace sám sebe. App Service podporuje pět poskytovatelů identit ihned: Azure Active Directory, Facebook, Google, Microsoft Account a Twitter. Aplikace můžete použít libovolný počet těchto poskytovatelů identit zajistit uživatelům s možnostmi pro jak přihlášení. Rozšířit integrovanou podporu, můžete integrovat jiného poskytovatele identity nebo [řešení vlastní identitu][custom-auth].

Pokud chcete začít hned, najdete v jednom z následujících kurzů [přidání ověřování do vaší aplikace pro iOS][iOS] (nebo [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], nebo [Cordova]).

## <a name="how-authentication-works-in-app-service"></a>Princip ověřování ve službě App Service
Aby bylo možné ověřit pomocí jednoho z poskytovatelů identity, musíte nejprve konfigurovat zprostředkovatel identity vědět o vaší aplikaci. Poskytovatel identity bude potom zadejte ID a tajné klíče, které zadáte do služby App Service. Tím se dokončí vztahu důvěryhodnosti, aby služby App Service můžete ověřit kontrolní výrazy uživatele, jako je například ověřování tokenů, od zprostředkovatele identity.

K přihlášení uživatele pomocí jednoho z těchto poskytovatelů, musí být uživatel přesměrován na koncový bod, který se přihlásí uživatele pro tohoto zprostředkovatele. Pokud zákazníci používají webový prohlížeč, může mít všechny neověřené uživatele na koncový bod, který se přihlásí uživatele automaticky nasměrovat služby App Service. Jinak, budete muset nasměrovat zákazníkům `{your App Service base URL}/.auth/login/<provider>`, kde `<provider>` je jedním z následujících hodnot: aad, facebook, google, microsoft nebo twitteru. Mobilní aplikace a API scénáře jsou vysvětlené v částech později v tomto článku.

Uživatelé, kteří pracují s vaší aplikací prostřednictvím webového prohlížeče, bude mít soubor cookie nastavte tak, aby se může zůstat ověřený jako procházení vaší aplikace. U jiných typů klientů, jako je například mobile, webového tokenu JSON (JWT), která by měla zobrazit v `X-ZUMO-AUTH` záhlaví, budou vydané klienta. Klient pro mobilní aplikace sady SDK bude zpracovávat to pro vás. Alternativně tokenu identity Azure Active Directory nebo přístupový token může být přímo součástí `Authorization` záhlaví jako [tokenu nosiče](https://tools.ietf.org/html/rfc6750).

Služby App Service ověří všechny soubor cookie nebo token, který vaše aplikace vystavuje k ověřování uživatelů. Pokud chcete omezit, kdo má přístup k aplikaci, najdete v článku [autorizace](#authorization) později v tomto článku.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobilní ověření u poskytovatele sady SDK
Jakmile je všechno nastavené na back-end, můžete upravit mobilních klientů se přihlásit pomocí služby App Service. Existují dva přístupy tady:

* Pomocí sady SDK, který publikuje danou identitu zprostředkovatele k vytvoření identity a přístup do služby App Service.
* Použijte jeden řádek kódu tak, aby klient SDK pro Mobile Apps můžete přihlásit uživatele.

> [!TIP]
> Většina aplikace by měly používat poskytovatele sady SDK potřebujete jednotnější prostředí po přihlášení uživatelé, používat podporu aktualizace a získat další výhody, které určuje zprostředkovatele.
> 
> 

Pokud používáte poskytovatele sady SDK, mohou uživatelé přihlašují k prostředí, které více úzce se integruje se službou operačního systému, který aplikace běží na. To také umožňuje token zprostředkovatele a některé informace o uživateli na straně klienta, takže je mnohem jednodušší graf rozhraní API přehledná a přizpůsobit činnost koncového uživatele. Čas od času na blogy a fóra, zobrazí se tento označuje jako "tok klienta" nebo "pohybu přesměruje klienta", protože kódu na straně klienta přihlášení uživatelů a kódem klientské má přístup k tokenu zprostředkovatele.

Po získání tokenu zprostředkovatele musí být odeslány do služby App Service pro ověření. Po služby App Service ověří token, služby App Service vytvoří nové služby App Service token, který je vrácen do klienta. Mobilní aplikace klienta SDK obsahuje pomocné metody pro správu této exchange a automaticky připojit token pro všechny požadavky na back-end aplikace. Vývojáři mohou také ponechat odkaz na token zprostředkovatele, pokud se tak rozhodne.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobilní ověřování bez poskytovatele sady SDK
Pokud nechcete nastavit poskytovatele sady SDK, můžete povolit funkce Mobile Apps služby Azure App Service se můžete přihlásit. Mobilní aplikace klienta SDK se jim otevře webové zobrazení k poskytovateli dle vašeho výběru a přihlásit uživatele. Příležitostně na blogy a fóra, zobrazí se tomu taky říká "serveru pohybu" nebo "směrované serveru pohybu" vzhledem k tomu, že server spravuje proces, který se přihlásí uživatele, a klient SDK nikdy obdrží token zprostředkovatele.

Kód do začátku tento tok je obsažena v tomto kurzu ověřování pro každou platformu. Na konci tok tokenu služby App Service má klient SDK a token se automaticky přiloží k všechny požadavky na back-end aplikace.

### <a name="service-to-service-authentication"></a>Ověřování služba-služba
I když můžete uživatelům přístup k vaší aplikaci, můžete také důvěřovat jiná aplikace pro volání vlastní rozhraní API. Například můžete mít jednu webovou aplikaci volat rozhraní API v jiné webové aplikace. V tomto scénáři budete používat přihlašovací údaje pro účet služby místo přihlašovací údaje uživatele k získání tokenu. Je také označován jako účet služby *instanční objekt* v Azure Active Directory slangu a ověřování, který používá uvedený účet je také označován jako scénáři service-to-service.

> [!IMPORTANT]
> Vzhledem k tomu mobilní aplikace běží v zařízení zákazníka, mobilní aplikace se *není* se počítají jako důvěryhodné aplikace a neměli používat k hlavní toku služby. Místo toho měli používat toku uživatele, který byl dříve podrobné.
> 
> 

Pro scénáře service to service služby App Service chrání vaše aplikace pomocí služby Azure Active Directory. Volající aplikace právě musí zajistit Azure Active Directory služby hlavní autorizační token, který byl získán zadáním klienta ID a klienta tajný z Azure Active Directory. Příklad tohoto scénáře, který používá rozhraní API ASP.NET aplikace je vysvětleno v kurzu [hlavní ověřování pro služby pro aplikace API] [apia služba].

Pokud chcete použít ověřování služby App Service pro zpracování scénáři service-to-service, můžete použít klientské certifikáty nebo základní ověřování. Informace o klientské certifikáty v Azure najdete v tématu [jak do konfigurace TLS vzájemné ověřování pro webové aplikace](app-service-web-configure-tls-mutual-auth.md). Informace o základní ověřování v technologii ASP.NET najdete v tématu [filtry ověřování ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/authentication-filters).

Ověřování účtu služby z aplikace služby App Service logic do aplikace API je zvláštní případ, který je podrobně popsán v [používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Princip ověřování ve službě App Service
Máte plnou kontrolu nad požadavků, které můžete přístup k aplikaci. Aplikace služby ověřování / autorizace může být nakonfigurována s žádným z následujících chování:

* Povolit pouze ověřené žádosti k dosažení vaší aplikace.
  
    Pokud prohlížeč odešle požadavek anonymní, přesměruje na stránku služby App Service pro zprostředkovatele identity, který zvolíte, aby uživatelé mohli podepsat. Pokud požadavek pochází z mobilního zařízení, je vrácená odpověď HTTP *401 – Neověřeno* odpovědi.
  
    Tato možnost nemusíte vůbec psaní jakéhokoli kódu ověřování ve vaší aplikaci. Pokud potřebujete jemnějšího autorizace, informace o uživateli je k dispozici pro váš kód.
* Povolit všechny požadavky k dosažení vaší aplikace, ale ověření ověřené žádosti a předávají informace o ověřování v hlavičkách protokolu HTTP.
  
    Tato možnost odkládat údaje autorizačních rozhodnutích do kódu aplikace. Poskytuje větší flexibilitu při zpracování anonymních požadavků, ale budete muset psát kód.
* Povolit všechny požadavky na přístup vaší aplikace a provádět žádnou akci na informace o ověřování v žádosti.
  
    V tomto případě, že ověřování / autorizace funkce je vypnutá. Úlohy ověřování a autorizace jsou zcela až kódu aplikace.

Předchozí chování jsou řízeny **akci provést, když požadavek nebude ověřený** možnost na portálu Azure. Pokud se rozhodnete ** protokolu s *název zprostředkovatele* **, všechny požadavky nutné ověřit. **Povolit požadavku (žádná akce)** odkládat údaje rozhodnutí o autorizaci kódu, ale stále poskytuje informace o ověřování. Pokud chcete mít kódu zpracovat vše, můžete zakázat ověřování / autorizace funkce.

## <a name="working-with-user-identities-in-your-application"></a>Práce s identit uživatelů ve vaší aplikaci
Služby App Service pomocí speciálními záhlavími předá některé informace o uživateli do vaší aplikace. Externí požadavky zakázat tyto hlavičky a se nachází v případě nastavit pouze pomocí aplikace služby ověřování / autorizace. Některé příklad hlavičky zahrnují:

* X-MS-KLIENTA HLAVNÍ NÁZEV
* X-MS-CLIENT-HLAVNÍ ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kód, který je napsán v libovolném jazyce nebo rozhraní můžete získat informace, které se musí z těchto hlavičky. Pro aplikace ASP.NET 4.6 **ClaimsPrincipal** bude automaticky nastavena s příslušnými hodnotami.

Aplikace můžete také získat podrobnosti další uživatele prostřednictvím HTTP GET na `/.auth/me` koncový bod vaší aplikace. Neplatný token, který je součástí požadavku vrátí datové části JSON s podrobnostmi o zprostředkovatele, který se používá, základní zprostředkovatel tokenu a některé další informace o uživateli. Server Mobile Apps sady SDK poskytují pomocné metody pro práci s těmito daty. Další informace najdete v tématu [jak používat Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), a [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentace a další prostředky
### <a name="identity-providers"></a>Zprostředkovatelé identity
Následující kurzy ukazují, jak nakonfigurovat App Service používat různá ověřovací zprostředkovatele:

* [Postup konfigurace aplikace použít Azure Active Directory přihlášení][AAD]
* [Postup konfigurace aplikace používat Facebook přihlášení][Facebook]
* [Postup konfigurace aplikace používat Google přihlášení][Google]
* [Postup konfigurace aplikace používat Account Microsoft přihlášení][MSA]
* [Postup konfigurace aplikace používat přihlášení služby Twitter.][Twitter]

Pokud chcete používat systém identit než ty, které, pokud zde, můžete také použít [náhled podporu vlastního ověřování mobilní aplikace .NET serveru SDK][custom-auth], který může být použit v webové, mobilní aplikace aplikace, nebo rozhraní API.

### <a name="mobile-applications"></a>Mobilní aplikace
Následující kurzy ukazují, jak přidat ověřování do mobilních klientů pomocí toku směrované serveru:

* [Přidání ověřování do vaší aplikace pro iOS][iOS]
* [Přidání ověřování do aplikace pro Android][Android]
* [Přidání ověřování do aplikace pro Windows][Windows]
* [Přidání ověřování do aplikace pro Xamarin.iOS][Xamarin.iOS]
* [Přidání ověřování do aplikace Xamarin.Android][Xamarin.Android]
* [Přidání ověřování do aplikace Xamarin.Forms][Xamarin.Forms]
* [Přidání ověřování do aplikace Cordova][Cordova]

Pokud chcete použít toku přesměruje klienta pro Azure Active Directory, použijte v následujících zdrojích informací:

* [Použít Active Directory Authentication Library pro iOS][ADAL-iOS]
* [Použití knihovny pro ověřování služby Active Directory pro Android][ADAL-Android]
* [Použití knihovny pro ověřování služby Active Directory pro Windows a Xamarin][ADAL-dotnet]

Pokud chcete použít toku přesměruje klienta pro Facebook, použijte v následujících zdrojích informací:

* [Použití sady SDK Facebook pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Pokud chcete použít pro Twitter toku přesměruje klienta, použijte v následujících zdrojích informací:

* [Pomocí služby Twitter prostředků infrastruktury pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Pokud chcete použít pro Google toku přesměruje klienta, použijte v následujících zdrojích informací:

* [Použití Google přihlášení sady SDK pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

<!-- ### API applications
The following tutorials show how to protect your API apps:

* [User authentication for API Apps in Azure App Service][apia-user]
* [Service principal authentication for API Apps in Azure App Service][apia-service] -->

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
