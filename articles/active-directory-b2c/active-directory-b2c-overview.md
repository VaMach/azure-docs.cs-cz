<properties
    pageTitle="Azure Active Directory B2C ve verzi Preview: Přehled | Microsoft Azure"
    description="Vývoj aplikací určených uživatelům pomocí Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/06/2016"
    ms.author="swkrish"/>

# Azure Active Directory B2C ve verzi Preview: Registrace a přihlašování uživatelů aplikace

Azure Active Directory B2C je komplexní cloudové řešení pro správu identit pro webové a mobilní aplikace určené uživatelům. Jedná se o vysoce dostupnou globální službu, která je škálovatelná pro stovky milionů uživatelských identit. Azure Active Directory B2C je postavené na platformě zabezpečené na podnikové úrovni a chrání tak vaše aplikace, firmu i uživatele.

V minulosti by si vývojáři aplikací napsali vlastní kód pro registraci a přihlašování uživatelů do svojí aplikace. A použili by místní databáze nebo systémy k ukládání uživatelských jmen a hesel. Azure Active Directory B2C nabízí vývojářům lepší způsob, jak integrovat správu identit uživatelů do svých aplikací s pomocí bezpečné, na standardech postavené platformy a bohaté škály rozšiřitelných zásad. Při použití Azure Active Directory B2C se vaši uživatelé mohou zaregistrovat do vaší aplikace pomocí svých účtů na sociálních sítích (Facebook, Google, Amazon, LinkedIn) nebo pomocí místních účtů – nově vytvořených přihlašovacích údajů (emailová adresa a heslo, nebo uživatelské jméno a heslo), 

Azure Active Directory B2C je ve verzi Preview. Během této doby od vás uvítáme zpětnou vazbu a zkušenosti s používáním služby. Na základě zpětné vazby můžeme provést zásadní změny v zájmu zkvalitnění služby .  Během této doby byste neměli vydávat produkční aplikace používající verzi Preview. Sdělte nám své myšlenky pomocí [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/).

## Začínáme

Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).

Aplikaci můžete napsat s využitím služby Azure Active Directory B2C tak, že buď zvolíte přímé zasílání zpráv protokolu pomocí protokolů [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) nebo [Open ID Connect](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), nebo použijte naše knihovny, které udělají tuto práci za vás. V následující tabulce vyberte oblíbenou platformu a můžete začít.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## Co je nového

Pravidelně kontrolujte tuto stránku, abyste se dovědět o budoucích změnách Azure Active Directory B2C ve verzi Preview. O všech aktualizacích budeme také tweetovat na @AzureAD.

- Zjistěte více o našem [rozhraní rozšiřitelných zásad](active-directory-b2c-reference-policies.md) a o typech zásad, které můžete vytvářet a používat ve svých aplikacích.
- Současná [omezení a limity ve verzi Preview](active-directory-b2c-limitations.md),

## Články s návody

Další informace o použití konkrétních funkcí Azure Active Directory B2C ve verzi Preview:

- Nastavení [Facebooku](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [účtu Microsoft](active-directory-b2c-setup-msa-app.md), účtů [Amazon](active-directory-b2c-setup-amzn-app.md), a [inkedIn](active-directory-b2c-setup-li-app.md) pro použití v aplikacích určených uživatelům.
- [Použití vlastních atributů ke sběru informací o uživatelích](active-directory-b2c-reference-custom-attr.md).
- [Povolení Azure Multi-Factor Authentication v aplikacích určených uživatelům](active-directory-b2c-reference-mfa.md).
- [Nastavení samoobslužného resetování hesla pro uživatele](active-directory-b2c-reference-sspr.md).
- [Přizpůsobení vzhledu a chování registrace a přihlášení a dalších stránek určených uživatelům](active-directory-b2c-reference-ui-customization.md), které obsluhuje Azure Active Directory B2C.
- [Využijte Azure Active Directory Graph API k programovému vytváření, čtení, aktualizaci a mazání uživatelů](active-directory-b2c-devquickstarts-graph-dotnet.md) ve svém klientu Azure Active Directory B2C.

## Další kroky

Následující odkazy budou užitečné při prozkoumávání služby do hloubky:

- Viz [Informace o cenách Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Získejte pomoc na Stack Overflow pomocí značek [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) nebo [adal](http://stackoverflow.com/questions/tagged/adal).
- Sdělte nám své myšlenky o verzi Preview pomocí [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/) – Chceme je slyšet! Použijte frázi "AzureADB2C:" v názvu svého příspěvku, abychom ho našli.
- Azure Active Directory B2C podporuje standardní protokoly, OpenID Connect a OAuth 2.0 pomocí modelu registrace aplikace, kterému říkáme „Model Aplikace v2.0“.
  - [Příručka k protokolům Modelu aplikace v2.0](active-directory-b2c-reference-protocols.md)
  - [Příručka k tokenům Modelu aplikace v2.0](active-directory-b2c-reference-tokens.md)
- Přečtěte si [Nejčastější dotazy k Azure Active Directory B2C](active-directory-b2c-faqs.md).
- [Podávejte požadavky na podporu pro Azure Active Directory B2C](active-directory-b2c-support.md).

## Získejte bezpečnostní aktualizace našich produktů

Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.



<!--HONumber=Jun16_HO2-->


