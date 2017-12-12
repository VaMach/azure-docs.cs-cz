---
title: "Jak sestavit aplikaci, která se můžete přihlásit žádné uživatele Azure AD"
description: "Podrobné pokyny pro vytváření aplikace, které můžou přihlásit uživatel jakéhokoli klienta Azure Active Directory, také známé jako aplikaci na více klientů."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: f6d8d2c07c2860059c4e9deb75d0bc4a876e057b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Jak se přihlásit žádné uživatele Azure Active Directory (AD) pomocí vzoru víceklientské aplikace
Pokud nabízíte softwaru jako aplikace služby k mnoha organizacích, můžete nakonfigurovat aplikaci tak, aby přijímal přihlášení jakéhokoli klienta Azure AD.  Ve službě Azure AD tato konfigurace se nazývá provedení víceklientské vaší aplikace.  Uživatelé v jakékoli klientovi Azure AD bude moci přihlásit k aplikaci po souhlas používat svůj účet s vaší aplikací.  

Pokud máte existující aplikaci, která má svůj vlastní účet systému nebo jiných druhů přihlášení od jiných poskytovatelů cloudu podporuje, klepněte přidání Azure AD přihlášení jakéhokoli klienta je jednoduché. Právě svou aplikaci zaregistrovat, přidejte kód přihlášení přes OAuth2, OpenID Connect nebo SAML a umístí tlačítko "Přihlášení v with Microsoft" vaší aplikace. Klikněte na následující tlačítko Další informace o branding vaší aplikace.

[![Přihlaste se tlačítko][AAD-Sign-In]][AAD-App-Branding]

Tento článek předpokládá, že jste již obeznámeni s vytvoření jednoho klienta aplikace pro Azure AD.  Pokud si nejste, head zpět do [domovské stránce průvodce vývojáře] [ AAD-Dev-Guide] a vyzkoušet některý z našich rychlé zahájení práce!

Existují čtyři jednoduché kroky k převedení aplikace do více klientů aplikace Azure AD:

1. Aktualizace registrace vaší aplikace na více klientů
2. Aktualizujte kód a odesílat požadavky / Common koncový bod 
3. Aktualizujte kód pro zpracování více hodnot vystavitele
4. Rady pro pochopení souhlasu uživatele a správce a proveďte změny správný kód

Podívejme se na každý krok podrobně. Můžete také přejít přímo na [tento seznam víceklientské ukázky][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Aktualizace registrace, která má být více klientů
Ve výchozím nastavení jsou webové aplikaci nebo API registrace ve službě Azure AD jednoho klienta.  Můžete provést registrace víceklientské tak, že najdete na stránce Vlastnosti registrace vaší aplikace v "Více nevyužívá dělené tabulky" přepínač [portál Azure] [ AZURE-portal] a jeho nastavení na hodnotu "Ano".

Všimněte si také, než aplikace můžete provedeny více klientů Azure AD vyžaduje identifikátor ID URI aplikace aplikace být globálně jedinečný. Identifikátor ID URI aplikace je jedním ze způsobů, které aplikace je definována ve zprávách protokolu.  Aplikace pomocí jednoho klienta je dostačující pro identifikátor ID URI aplikace být jedinečný v rámci tohoto klienta.  Pro více klientů aplikace musí být globálně jedinečné, Azure AD můžete najít aplikaci přes všechny klienty.  Globální jedinečnosti se vynucuje tím, že identifikátor ID URI aplikace tak, aby měl název hostitele, který odpovídá ověřené domény klienta Azure AD.  Například, pokud název vašeho klienta se contoso.onmicrosoft.com pak platný identifikátor ID URI aplikace by být `https://contoso.onmicrosoft.com/myapp`.  Pokud váš klient měli ověřené domény `contoso.com`, pak by také být platný identifikátor ID URI aplikace `https://contoso.com/myapp`.  Pokud je identifikátor ID URI aplikace není postupujte podle tohoto vzoru, nastavení aplikace, protože víceklientské selže.

Registrace nativního klienta jsou víceklientské ve výchozím nastavení.  Není nutné provádět žádnou akci, aby Nativní klient aplikace registrace více klientů.

## <a name="update-your-code-to-send-requests-to-common"></a>Aktualizujte kód a odesílat žádosti do/Common
Žádostí o přihlášení k aplikaci jednoho klienta, se odešlou do klienta přihlášení koncový bod. Například pro contoso.onmicrosoft.com bude koncový bod:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Požadavky odeslané na koncový bod klienta můžete přihlásit uživatele (nebo Hosté) v něm k aplikacím v něm.  S více klienty aplikací aplikace neví, předem jaké klienta uživatele je, aby nemohli odesílat žádosti do koncového bodu klienta.  Místo toho jsou odesílány požadavky na koncový bod, který multiplexes napříč všechny klienty Azure AD:

    https://login.microsoftonline.com/common

Pokud Azure AD přijme požadavek na / Common koncový bod, se uživatel přihlásí a v důsledku zjišťuje klienta, které je uživatel z.  / Společný koncový bod funguje se všemi ověřovací protokoly podporovaný službou Azure AD: OpenID Connect, OAuth 2.0, SAML 2.0 a WS-Federation.

Odpověď přihlášení do aplikace se pak obsahuje token reprezentující uživatele.  Hodnota vystavitele v tokenu informuje co klienta uživatele je z aplikace.  Pokud vrátí odpověď z / Common koncový bod, odpovídá hodnotě vystavitele v tokenu uživatele klienta.  Je důležité si uvědomit, / Common koncový bod není klienta a není vystavitele, je právě multiplexor.  Při použití/Common, logiku aplikace k ověření tokeny musí aktualizovat, aby vzít v úvahu. 

Jak už bylo zmíněno dříve, musí aplikace víceklientské zadat konzistentního prostředí přihlášení pro uživatele, následující aplikace Azure AD branding pokyny. Klikněte na následující tlačítko Další informace o branding vaší aplikace.

[![Přihlaste se tlačítko][AAD-Sign-In]][AAD-App-Branding]

Podívejme se na používání / Common koncový bod a implementaci kódu podrobněji.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Aktualizujte kód pro zpracování více hodnot vystavitele
Webové aplikace a webové rozhraní API přijímat a ověřovat tokeny z Azure AD.  

> [!NOTE]
> Nativní klientské aplikace požadovat a přijímat tokeny z Azure AD, se to provést tak o jejich odeslání do rozhraní API, kde se ověří.  Nativní aplikace neověřují tokeny a musí je považovat za neprůhledné.
> 
> 

Podívejme se na tom, jak aplikaci ověří tokeny obdrží z Azure AD.  Jednoho klienta aplikace obvykle přebírá hodnotu koncového bodu jako:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

a používejte ho vytvořit adresu URL metadat (v tomto případě OpenID Connect) jako:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

ke stažení dvě zásadní informace používané k ověření tokeny: klient je podepisování klíče a hodnoty vystavitele.  Každý klient Azure AD má jedinečný vystavitele hodnotu ve formátu:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

kde je hodnota GUID přejmenování bezpečná verze klienta ID klienta.  Pokud kliknete na odkaz na předchozí metadata pro `contoso.onmicrosoft.com`, zobrazí se tato hodnota vystavitele v dokumentu.

Pokud aplikace jednoho klienta ověří token, ověří podpis tokenu proti podpisové klíče z dokumentu metadat. Tento test umožňuje Ujistěte se, že hodnota vystavitele v tokenu odpovídá ten, který byl nalezen v dokumentu metadat.

Od / Common koncový bod neodpovídá klienta a není vystavitele, při zkontrolujte hodnotu vystavitele metadat pro/běžné má podle šablony URL namísto skutečné hodnoty:

    https://sts.windows.net/{tenantid}/

Proto nelze ověřit tokeny pouhým odpovídající hodnotě vystavitele v metadatech s víceklientské aplikace `issuer` hodnotu v tokenu.  Víceklientské aplikace potřebuje logiku rozhodnout vystavitele hodnot, které jsou platné a které jsou není, u klienta na základě ID část hodnoty vystavitele.  

Například víceklientské aplikace umožňuje pouze přihlášení z konkrétní klientů, kteří zaregistrovali pro své služby, potom je nutné zkontrolovat hodnotu vystavitele nebo `tid` hodnoty v tokenu a ujistěte se, že klienta je v jejich seznamu odběratelů deklarace identity.  Pokud aplikace víceklientské pouze se zabývá jednotlivce a není rozhodnutí žádné přístupu na základě na klienty, potom ji můžete ignorovat hodnotě vystavitele zcela.

V víceklientské ukázky [související obsahu](#related-content) část na konci tohoto článku, ověření vystavitele zakázána povolit všechny klienta Azure AD pro přihlášení.

Nyní Podíváme se na uživatelské prostředí pro uživatele, kteří se přihlašují k víceklientským aplikacím.

## <a name="understanding-user-and-admin-consent"></a>Principy uživatelů a správce souhlasu
Pro uživatele k přihlášení k aplikaci ve službě Azure AD musí být v klientovi uživatele reprezentován aplikace.  To umožňuje organizace například použít jedinečné zásady po přihlášení uživatelé z jejich klienta pro aplikaci.  Pro jednoho klienta aplikace je tato registrace jednoduché. je ten, který se stane, když zaregistrujete aplikaci v [portál Azure][AZURE-portal].

Aplikace pomocí víceklientské žije počáteční registrace pro aplikaci v klientovi Azure AD, používá vývojář.  Když uživatel z jiné klienta přihlásí k aplikaci poprvé, vyzve Azure AD je k souhlas oprávnění požadované aplikací.  Pokud budou souhlasit, pak volat znázornění aplikace *instanční objekt* je vytvořen v klientovi uživatele a přihlášení může pokračovat. Delegování je vytvořen také v adresáři, který zaznamenává souhlasu uživatele k aplikaci. V tématu [objekty aplikací a hlavní objekty služeb] [ AAD-App-SP-Objects] podrobnosti o aplikace aplikace a ServicePrincipal objekty, a jak se vztahují k sobě navzájem.

![Souhlas Jednoúrovňové aplikace][Consent-Single-Tier] 

Toto prostředí souhlasu je ovlivňován oprávnění požadované aplikací.  Azure AD podporuje dva druhy oprávnění jenom aplikace a delegovaným:

* Delegovaná oprávnění uděluje aplikace, které může provádět možnost tak, aby fungoval jako přihlášeného uživatele pro podmnožinu akcí uživatele.  Například můžete udělit aplikaci delegovaná oprávnění ke čtení kalendáře přihlášeného uživatele.
* Jen aplikace je povoleno přímo na identitu aplikace.  Například můžete udělit aplikaci jen aplikace oprávnění ke čtení seznamu uživatelů v klientovi, bez ohledu na to, kdo je přihlášený do aplikace.

Některá oprávnění může být souhlas regulární uživatelem, zatímco jiné vyžadují souhlas správce klienta. 

### <a name="admin-consent"></a>Správce souhlasu
Oprávnění jen aplikace vždy vyžadovat souhlas správce klienta.  Pokud vaše aplikace požaduje oprávnění jen aplikace a uživatel se pokusí přihlásit k aplikaci, se zobrazí chybová zpráva, že uživatel není možné vyjádřit souhlas.

Některé přidělená oprávnění také vyžadovat souhlas správce klienta.  Například možnost ke zpětnému zápisu do Azure AD jako přihlášeného uživatele vyžaduje souhlas správce klienta.  Jako oprávnění jen pro aplikace Pokud se pokusí přihlásit k aplikaci, která požaduje delegované oprávnění, která vyžaduje souhlas správce běžného uživatele vaší aplikace obdrží chybu.  Zda vyžaduje oprávnění správce souhlasu je určen podle vývojáře, která publikuje prostředek a může být v dokumentaci k prostředku.  Odkazy na témata s popisem dostupná oprávnění pro Azure AD Graph API a rozhraní Microsoft Graph API jsou v [související obsahu](#related-content) tohoto článku.

Pokud vaše aplikace používá oprávnění, která vyžadovat souhlas správce, musíte mít gesto například tlačítko nebo odkaz, kde může správce zahájit akci.  Vaše aplikace odešle pro tato akce je obvykle požadavek ověřování OAuth2/OpenID Connect, ale který také obsahuje požadavek `prompt=admin_consent` parametr řetězce dotazu.  Jakmile se dá souhlas správce a instanční objekt se vytvoří v klientovi zákazníka, následných žádostí o přihlášení není nutné `prompt=admin_consent` parametr. Vzhledem k tomu, že správce určil, že požadovaná oprávnění jsou přijatelné, požádejte ho o souhlas od tohoto okamžiku žádné jiných uživatelů v klientovi.

`prompt=admin_consent` Parametru lze také aplikace, které žádostí o oprávnění, které nevyžadují souhlas správce. Důvodem je, pokud hasicí vyžaduje prostředí, kde klienta správce "zaregistruje" jeden čas a žádné jiné uživateli zobrazí výzva k vyjádření souhlasu od tohoto okamžiku na.

Pokud aplikace vyžaduje souhlas správce a správce přihlášení ale `prompt=admin_consent` parametr neposílají, správce úspěšně souhlasí aplikace **pouze ke svému uživatelskému účtu**.  Běžní uživatelé nejsou stále moci přihlásit a souhlas k aplikaci.  Tato funkce je užitečná, pokud chcete poskytnout možnost prozkoumat aplikace před povolením přístupu jiných uživatelů správce klienta.

Správce klienta můžete zakázat možnost pro regulární uživatele o souhlas pro aplikace.  Pokud tato možnost je vypnuta, je vyžadováno pro aplikaci, aby byl nastavený v klientovi vždy souhlas správce.  Pokud chcete testování vaší aplikace pomocí běžných uživatelských souhlasu zakázaná, můžete najít konfigurace přepínače v klientovi Azure AD oddílu konfigurace [portál Azure][AZURE-portal].

> [!NOTE]
> Některé aplikace mají prostředí, kde jsou běžní uživatelé moct souhlas původně a později aplikace může zahrnovat oprávnění správce a požadavku, která vyžadovat souhlas správce.  Neexistuje žádný způsob, jak to provést pomocí jedné aplikace registrace ve službě Azure AD ještě dnes.  Nadcházející endpoint modelu nasazení Azure AD Resource Manager umožňuje aplikacím a požádat o oprávnění za běhu, místo v době registrace, který umožňuje tento scénář.  Další informace najdete v tématu [modelu nasazení Azure AD aplikace modelu Resource Manager Příručka vývojáře][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Souhlasu a vícevrstvé aplikace
Aplikace může mít několik vrstev, každý reprezentován vlastní registraci ve službě Azure AD.  Nativní aplikace, která volá webové rozhraní API nebo webovou aplikaci, například volání webového rozhraní API.  V obou případech klienta (nativní aplikaci nebo webové aplikace) vyžaduje oprávnění k volání prostředků (webového rozhraní API).  Klient se úspěšně dá souhlas, do klienta zákazníka všechny prostředky, ke kterým se vyžaduje oprávnění již musí existovat v klientovi zákazníka.  Pokud není tato podmínka splněná, Azure AD vrátí chybu prostředek musí být nejprve přidán.

**Několik vrstev v jednoho klienta**

Může to být problém, pokud vaše logické aplikace se skládá ze dvou nebo více aplikací registrace, například klienta a prostředků.  Jak můžete získat prostředek do klienta zákazníka první?  Azure AD se popisuje tento případ povolením klienta a prostředku se dá souhlas v jediném kroku. Uživateli se zobrazí celkový součet oprávnění požadovaná klientem a prostředků na stránce souhlasu.  Chcete-li toto chování, registrace aplikace prostředku musí obsahovat ID klienta aplikace jako `knownClientApplications` v manifestu aplikace.  Například:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Tuto vlastnost lze aktualizovat prostřednictvím prostředek [manifestu aplikace][AAD-App-Manifest]. Tento postup je znázorněn v vícevrstvé nativního klienta volání ukázkové webové rozhraní API v [související obsahu](#related-content) na konci tohoto článku. Následující diagram přehledně souhlasu pro vícevrstvé aplikace s zaregistrovaný v jednoho klienta:

![Souhlas známé klienta vícevrstvé aplikace][Consent-Multi-Tier-Known-Client] 

**Několik vrstev v několika klientech**

Podobný případ se stane, když jsou v jiných klientů zaregistrované různých vrstev aplikace.  Představte si třeba v případě vytváření nativní klientskou aplikaci, která volá Online rozhraní API sady Office 365 Exchange.  K vývoji nativní aplikaci a novějším pro nativní aplikace spuštěna v klientovi zákazníka, musí být objekt služby Exchange Online.  V takovém případě se developer a zákazník musí zakoupit Exchange Online pro službu objektu vytvořeny v svým klientům.  

V případě rozhraní API vytvořené v organizaci než Microsoft musí vývojář rozhraní API poskytují způsob, jak zákazníkům o souhlas aplikaci do klientů svým zákazníkům. Doporučený návrh je pro vývojáře 3. stran k vytvoření rozhraní API, tak, aby také může fungovat jako webový klient k implementaci registrace:

1. Postupujte podle předchozích částech k zajištění, že požadavky na registraci nebo kód víceklientské aplikace implementuje rozhraní API
2. Kromě vystavení rozhraní API obory nebo role, ujistěte se, zahrnuje registrace "přihlášení a čtení profilu uživatele" oprávnění Azure AD (poskytuje se ve výchozím nastavení)
3. Implementace sign v nebo registrace-množství stránky v webovému klientovi, následující [souhlas správce](#admin-consent) pokyny uvedenými výše 
4. Když uživatel souhlasí k aplikaci, odkazy service principal a souhlasu delegování jsou vytvořené v jejich klienta a nativní aplikace mohou získat tokeny pro rozhraní API

Následující diagram přehledně souhlasu pro vícevrstvé aplikace s zaregistrovaný v jiných klientů:

![Souhlas vícevrstvé aplikace s více stran][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Odvolání souhlasu
Uživatelé a správci můžete odvolat souhlas k vaší aplikaci kdykoli:

* Uživatelé odvolat přístup k jednotlivých aplikací odstraněním těchto z jejich [přístup k aplikacím panely] [ AAD-Access-Panel] seznamu.
* Správci odvolat přístup k aplikacím je odstranit z Azure AD pomocí části Správa Azure AD [portál Azure][AZURE-portal].

Pokud správce souhlasí k aplikaci pro všechny uživatele v klientovi, uživatelé nelze odvolat přístup jednotlivě.  Pouze správce můžete odvolat přístup a jenom pro celou aplikaci.

### <a name="consent-and-protocol-support"></a>Souhlasu a podpora protokolu
Souhlas se podporuje ve službě Azure AD pomocí OAuth, OpenID Connect, WS-Federation a protokoly SAML.  Protokoly SAML a WS-Federation nepodporují `prompt=admin_consent` parametr, tak je možné pomocí OAuth a OpenID Connect jenom souhlas správce.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Víceklientské aplikace a ukládání do mezipaměti přístupové tokeny
Víceklientským aplikacím můžete také získat přístupové tokeny k volání rozhraní API, které jsou chráněné službou Azure AD.  Běžnou chybou při Active Directory Authentication Library (ADAL) pomocí víceklientské aplikace, je nejdřív požádat o token pro uživatele s využitím/Common, obdrží odpověď a potom požádat o další token pro tomuto uživateli také pomocí/Common.  Vzhledem k tomu, že odpověď z Azure AD pochází z klienta, není nebo běžné, ADAL ukládá do mezipaměti token, že je z klienta. Další volání/Common získat přístupový token pro uživatele neúspěšných přístupů do položky mezipaměti a uživatel je vyzván se znovu přihlásit.  Abyste se vyhnuli chybí mezipaměti, zkontrolujte, zda koncový bod klienta jsou provedeny následující volání pro již přihlášeného uživatele.

## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak sestavit aplikaci, která se může přihlásit uživatel jakéhokoli klienta Azure Active Directory. Po povolení jednotného přihlašování mezi aplikací a Azure Active Directory, můžete také aktualizovat aplikaci pro přístup k rozhraní API vystavené zdroje společnosti Microsoft, jako je Office 365. Přizpůsobené rozhraní můžete tak nabízet ve vaší aplikaci, například zobrazení kontextové informace pro uživatele, jako je jejich profilový obrázek nebo jejich další událost kalendáře. Další informace o volání rozhraní API služby Azure Active Directory a Office 365, jako je Exchange, SharePoint, OneDrive, OneNote, Planner, Excel a další, navštivte: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>Související obsah
* [Ukázky víceklientské aplikace][AAD-Samples-MT]
* [Branding pokyny pro aplikace][AAD-App-Branding]
* [Průvodce Azure AD vývojáře][AAD-Dev-Guide]
* [Objekty aplikací a hlavní objekty služeb][AAD-App-SP-Objects]
* [Integrace aplikací s Azure Active Directory][AAD-Integrating-Apps]
* [Přehled rozhraní souhlasu][AAD-Consent-Overview]
* [Obory oprávnění Microsoft Graph API][MSFT-Graph-permision-scopes]
* [Obory oprávnění rozhraní Azure AD Graph API][AAD-Graph-Perm-Scopes]

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














