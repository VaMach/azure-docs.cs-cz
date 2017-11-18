---
title: "Migrace ze služby Řízení přístupu Azure (ACS)"
description: "Možnosti pro přesun aplikace a služby z služby Řízení přístupu Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="migrating-from-azure-access-control-service-acs"></a>Migrace ze služby Řízení přístupu Azure (ACS)

Microsoft Azure Active Directory řízení přístupu (také označované jako služby Řízení přístupu nebo služby ACS) se postupně vyřazuje z provozu v listopadu 2018.  Aplikace a služby aktuálně používá ACS muset plně migrovat na jiný mechanismus ověřování před tímto datem. Tento dokument popisuje doporučení pro aktuální zákazníky jako jejich plánování přestat používat jejich použití služby ACS. Pokud nepoužíváte aktuálně služby ACS, není nutné provádět žádnou akci.


## <a name="brief-acs-overview"></a>Stručný přehled služby ACS

Služby ACS je Cloudová služba ověřování, která poskytuje snadný způsob ověřování a autorizace uživatelů pro přístup do webové aplikace a služby současně mnoho funkcí ověřování a autorizaci se promítnou z vašeho kódu. Používá se především ve vývojáři & architekty klientů .NET, webové aplikace ASP.NET a webové služby WCF.

Případy použití pro služby ACS lze rozdělit do tří kategorií:

- Ověřování pro určité cloudových služeb společnosti Microsoft, včetně Azure Service Bus, Dynamics CRM a další. Klientské aplikace mohou získat tokeny ze serveru ACS, který můžete použít k ověřování na tyto služby k provádění různých akcí.
- Přidání ověřování do webové aplikace, z různých vlastním & z různých předem zabalené (např. Sharepointu). Pomocí služby ACS "pasivní" ověřování, webové aplikace může podporovat přihlášení s účty z Google, Facebook, Yahoo, účtu (Live ID) společnosti Microsoft, Azure Active Directory a AD FS.
- Zabezpečení vlastních integrované webové služby s tokeny vydané službou ACS. "Aktivní" ověřování pomocí, webové služby může zajistit jejich pouze povolit přístup ze známé klienty, kteří mají k ověření pomocí služby ACS.

Každá z těchto případů použití a jejich doporučená migrace strategie je popsána v následujících částech. Ve většině případů velká jsou změny významné kódu potřebné k migraci stávající aplikace a služby na novější technologie. Doporučuje se, že začnete plánování a provádění migrace okamžitě, aby se zabránilo potenciální výpadků nebo výpadek.

> [!WARNING]
> Ve většině případů kód významné změny jsou potřebné k migraci stávající aplikace a služby na novější technologie. Doporučuje se, že začnete plánování a provádění migrace okamžitě, aby se zabránilo potenciální výpadků nebo výpadek.

Z pohledu architektury ACS zcela skládá z následujících součástí:

- Zabezpečené tokenu služby (STS), který přijímá požadavky na ověření & na oplátku vydává tokeny zabezpečení.
- Klasický portál Azure, která se používá k vytváření, odstraňování a povolení nebo zákaz obory názvů služby ACS.
- Samostatné služby ACS portálu pro správu, který se používá k přizpůsobení & konfigurace chování oboru názvů služby ACS.
- Služba správy, který můžete použít k automatizaci funkcí portálů.
- Token transformace pravidlo modul, který můžete použít k definování komplexní logiku pro manipulaci s výstupní formát tokeny vydané službou ACS.

Pokud chcete použít tyto součásti, musíte vytvořit jeden nebo více ACS **obory názvů**. Obor názvů je vyhrazenou instancí služby ACS, který aplikacím a službám komunikovat se službou; je izolovaný od všech dalších ACS zákazníků, kteří používají svá vlastní obory názvů. Obor názvů v rámci služby ACS má vyhrazenou adresu URL, jako je třeba:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

Veškerá komunikace s služby tokenů zabezpečení a operace správy se provádějí na této adrese URL různé cesty pro jiné účely. Pokud chcete zjistit, pokud vaše aplikace nebo služby pomocí služby ACS, monitorování pro všechny přenosy do `https://{namespace}.accesscontrol.windows.net`.  Přenosy dat tato adresa URL je provoz, který je zpracovávány službou ACS a musí být zastaveny.  Jedinou výjimkou je přenosy dat do `https://accounts.accesscontrol.windows.net` -provoz tato adresa URL již zpracovává jiný službou a nemá vliv vyřazení služby ACS.  Také byste měli být nezapomeňte přihlásit k portálu Azure classic a zkontrolujte všechny obory názvů služby ACS v odběry, které vlastníte.  Obory názvů služby ACS, které jsou uvedeny v **služby Active Directory** služby, v části **obory názvů řízení přístupu** kartě.

Další informace o ACS najdete v tématu [to archivovat dokumentaci na webu MSDN](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Plán vyřazování z provozu

Od listopadu 2017 všechny součásti služby ACS jsou plně podporované & provozu. Pouze omezení je, že [nelze vytvořit nové obory názvů služby ACS prostřednictvím portálu Azure classic](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Časovou osu pro vyřazení těchto součástí zahrnuje tento plán:

- **2017 listopadu**: Správce Azure AD na základě zkušeností uživatelů na portálu Azure classic [je vyřazeno](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). V tomto okamžiku správu oboru názvů služby ACS bude k dispozici v této nové vyhrazenou adresu URL: `http://manage.windowsazure.com?restoreClassic=true`. Toto je vám umožní zobrazit existující obory, povolit nebo zakázat, je, a je zcela v případě, že chcete odstranit.
- **2018 duben**: Správa oboru názvů služby ACS je již nebude k dispozici na tuto vyhrazenou adresu URL. V tuto chvíli nemůžete se moct zapnout/vypnout, odstraňovat nebo výčet obory služby ACS. Portálu pro správu služby ACS, ale je plně funkční a v umístění `https://{namespace}.accesscontrol.windows.net`. Všechny součásti služby ACS i nadále fungovat normálně také.
- **2018 listopadu**: všechny ACS součásti jsou trvale vypnout. To zahrnuje portálu pro správu služby ACS, služba pro správu, služby tokenů zabezpečení a tokenu transformační pravidlo modul. V tomto okamžiku všechny požadavky, které byly odeslány na služby ACS (umístěné v `{namespace}.accesscontrol.windows.net`) nezdaří. Jste by měl migrovali všechny existující aplikace a služby do jiných technologií dobře před toto časové období.


## <a name="migration-strategies"></a>Strategie migrace

Následující části popisují vysoké úrovně doporučení pro migraci z služby ACS na dalších technologiích společnosti Microsoft.

### <a name="clients-of-microsoft-cloud-services"></a>Klienti cloudové služby společnosti Microsoft

Jednotlivých cloudových služeb Microsoftu, které přijímají tokeny vydané službou ACS nyní podporuje aspoň jeden alternativní způsob ověřování. Proto doporučujeme, která odkazuje na konkrétní dokumentace každé služby oficiální pokyny, se liší u každé služby správný ověřovací mechanismus. Pro usnadnění práce každou sadu dokumentace je k dispozici zde:

| Služba | Doprovodné materiály |
| ------- | -------- |
| Azure Service Bus | [Migrace na sdílených přístupových podpisů](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Relay | [Migrace na sdílených přístupových podpisů](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Cache | [Migrace na Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Trhu dat Azure | [Migrace na rozhraní API kognitivní služby](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migrace na Azure Logic Apps](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Migrace na ověřování Azure AD](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Upgrade aplikace Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-using-passive-authentication"></a>Webové aplikace pomocí pasivní ověřování

Pro webové aplikace pomocí služby ACS pro ověřování uživatelů ACS poskytuje následující funkce a možnosti pro vývojáře & architekty webových aplikací:

- Těsná integrace s Windows Identity Foundation (WIF).
- Federaci se Google, Facebook, Yahoo, účet (Live ID) společnosti Microsoft, Azure Active Directory a AD FS.
- Podpora pro následující protokoly pro ověřování: OAuth 2.0 koncept 13 a Ws-Trust, Ws-Federation.
- Podpora pro následující formáty tokenu: webového tokenu JSON (JWT), SAML 1.1, SAML 2.0 a jednoduché webové token (SWT).
- Zjišťování domovské sféry prostředí, integrované do WIF, který povolen uživatelům vyberte typ účtu, který bude používat k přihlášení. Toto prostředí se hostuje webovou aplikací a plně přizpůsobit.
- Token transformace, který povolen bohaté přizpůsobení deklarací přijatých webovou aplikací ze serveru ACS, včetně:
    - předávání prostřednictvím deklarací identity od zprostředkovatelů identity
    - Přidání další vlastní deklarace
    - jednoduché Pokud potom logiku vystavovat deklarace identity za určitých podmínek

Není bohužel není jedna služba, která poskytuje všechny tyto funkce ekvivalentní.  Byste měli zvážit, jaké funkce služby ACS je třeba a pak vybrat mezi ověřováním prostřednictvím [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD), [ **Azure AD B2C** ](https://azure.microsoft.com/services/active-directory-b2c/), nebo jiné cloudové služby ověřování.

#### <a name="migrating-to-azure-active-directory"></a>Migrace na Azure Active Directory

Jednu cestu vzít v úvahu je přímo integruje do vaší aplikace a služby Azure Active Directory. Azure AD je na základě identity poskytovatele cloudové služby Microsoft pracovní a školní účty - je zprostředkovatel identity pro Office 365, Azure a mnoho dalšího. Poskytuje podobné federovaný možnosti ověřování služby ACS, ale nepodporuje všechny funkce služby ACS. Primární příkladem je federation s poskytovateli sociální identity, jako je Facebook, Google a Yahoo. Pokud vaši uživatelé přihlásit pomocí těchto typů přihlašovacích údajů, Azure AD není pro vás. Zajišťuje také nemusí podporovat přesný stejné ověřovací protokoly, jako ACS - při obě služby ACS & Azure AD podpory OAuth, například existují malé rozdíly v každé implementace, která vyžadují, abyste upravit kód jako součást migrace.

Azure AD ale poskytnout několik výhod potenciální zákazníky služby ACS. Ho nativně podporuje Microsoft pracovní a školní účty hostovaná v cloudu, které se běžně používají zákazníci služby ACS.  Klient služby Azure AD můžete také federovaný na jeden nebo více instancí v místní službě Active Directory přes službu AD FS, umožní aplikaci ověřit cloudových uživatelů i uživatelé hostované na místní.  Také podporuje protokol Ws-Federation, takže je relativně jednoduché k integraci s webovou aplikaci pomocí Windows Identity Foundation (WIF).

Následující tabulka porovnává funkce služby ACS, které jsou relevantní pro webové aplikace s jsou k dispozici ve službě Azure AD. Na vysoké úrovni **Azure Active Directory je pravděpodobně správná volba pro migraci Pokud necháte pouze uživatelé, přihlaste se pomocí svého pracovního Microsoft & školní účty**.

| Schopnost | Podpora služby ACS | Podpora Azure AD |
| ---------- | ----------- | ---------------- |
| **Typy účtů** | | |
| Microsoft pracovní a školní účty | Podporuje se | Podporuje se |
| Účty ze služby Windows Server Active Directory a AD FS | Podporu pro federaci se klient služby Azure AD <br /> Podporované prostřednictvím přímé federace se službou AD FS | Podporuje jen přes federaci se klient služby Azure AD | 
| Účty z jiných podnikových systémů správy identit | Možný prostřednictvím federaci se klient služby Azure AD <br />Podporované prostřednictvím přímé federation | Možný prostřednictvím federaci se klient služby Azure AD |
| Účty Microsoft pro osobní použití (Windows Live ID) | Podporuje se | Podporované přes Azure AD v2.0 protokolu OAuth, ale nikoli prostřednictvím žádné jiné protokoly. | 
| Facebook, Google, Yahoo účty | Podporuje se | Jakékoli není podporováno. |
| **Protokoly & kompatibility SDK** | | |
| Technologie Windows Identity Foundation (WIF) | Podporuje se | Podporované, ale omezená pokyny, které jsou k dispozici. |
| WS-Federation | Podporuje se | Podporuje se |
| OAuth 2.0 | Podpora pro koncept 13 | Podpora pro RFC 6749, specifikace většina moderních. |
| WS-Trust | Podporuje se | Nepodporuje se |
| **Token formáty** | | |
| Tokeny webové JSON (Jwt) | Podporované ve verzi Beta | Podporuje se |
| Tokeny SAML 1.1 | Podporuje se | Podporuje se |
| Tokeny SAML 2.0 | Podporuje se | Podporuje se |
| Jednoduchých webových tokenů (SWT) | Podporuje se | Nepodporuje se |
| **Přizpůsobení** | | |
| Přizpůsobitelné domovské sféry nebo účet zjišťování Výběr uživatelského rozhraní | Zaváděná kód, který lze začlenit do aplikace | Nepodporuje se |
| Nahrát vlastní podpisových certifikátů tokenu | Podporuje se | Podporuje se |
| Přizpůsobení deklarací identity v tokenech | Atribut PassThrough vstupních deklarací identity od zprostředkovatelů identity<br />Získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />Problém výstup deklarace identity založené na hodnotách vstupních deklarací identity<br />Vystavovat deklarace identity výstup s konstantní hodnoty | Nelze průchozí deklaracích identity od zprostředkovatelů federovaných identit<br />Nelze získat přístupový token od zprostředkovatele identity jako deklaraci<br />Nelze vystavit deklarace výstupu založené na hodnotách vstupních deklarací identity<br />Mohou vystavovat deklarace identity výstup s konstantní hodnoty<br />Můžete vydat výstup deklarace identity založené na vlastnosti uživatelů synchronizována do Azure AD |
| **Automation** | | |
| Automatizovat úlohy konfigurace nebo správy | Podporu pro správu služby ACS. | Podporované prostřednictvím Microsoft Graph & Azure AD Graph API. |

Pokud se rozhodnete, že Azure AD je správná cesta dál pro vaše aplikace a služby, by měl být vědomi dva způsoby, jak integrovat vaši aplikaci s Azure AD.

Použití služby Ws-Federation/WIF při integraci s Azure AD, doporučujeme následující [metody uvedené v tomto článku](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Článek odkazuje ke konfiguraci Azure AD pro na základě SAML jednotné přihlašování, ale funguje i konfigurace Ws-Federation. Tento způsob vyžaduje licenci Azure AD Premium, ale má dvě výhody:

- Zobrazí se úplná flexibilita při přizpůsobení tokenu Azure AD. To umožňuje přizpůsobit deklarace identity vystavené službou Azure Active Directory tak, aby odpovídaly certifikátů vydávaných služby ACS, zejména včetně uživatelské ID nebo název identifikátor deklarace identity. Musíte zajistit, aby vydán Azure AD odpovídají těch, které vydal služby ACS, tak, aby i nadále zobrazovat identifikátory konzistentní uživatele pro uživatele i po provedení změny technologie ID uživatelů.
- Můžete nakonfigurovat token podpisový certifikát specifický pro vaši aplikaci, jejichž doba platnosti řídíte.

<!--

Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Použijte tento postup vyžaduje licenci Azure AD Premium. Pokud jste zákazník služby ACS a vyžadují licenci premium pro nastavení jednotného přihlašování pro aplikace, oslovení nám a My s radostí zajistit vývojáře licencí pro vaše použití.

Alternativní způsob je podle [této ukázce kódu](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), což dává mírně odlišné pokyny o tom, jak nastavit Ws-Federation. Tato ukázka kódu nepoužívá WIF, ale místo toho middlewaru ASP.NET 4.5 OWIN. Ale pokyny pro registraci aplikace jsou platné pro aplikace využívající technologii WIF a nevyžadují licenci Azure AD Premium.  Pokud zvolíte tuto metodu, heneed pochopit [podepisování výměna klíče ve službě Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Tento postup používá Azure AD globálního podpisového klíče problém tokeny. Ve výchozím nastavení WIF se automaticky neaktualizuje podpisové klíče. Pokud Azure AD otočí jeho globální podpisové klíče, je nutné připravit pro přijetí změn implementaci WIF.

Pokud budete moci integraci se službou Azure AD prostřednictvím protokolů OpenID Connect a OAuth, doporučujeme, abyste tak.  Máme rozsáhlou dokumentaci a pokyny, jak integrovat Azure AD do vaší webové aplikace, které jsou k dispozici v našem [Příručka pro vývojáře Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrating-to-azure-ad-b2c"></a>Migrace na Azure AD B2C

Jiné cesty migrace vzít v úvahu je Azure AD B2C.  B2C je cloudové ověřování služby, podobně jako služby ACS, mohou vývojáři využít svých logiku správu ověřování a identita cloudové služby k.  Je placené služby (s vrstvami volné & premium) určený pro příjemce čelí aplikace, které může mít až miliony aktivních uživatelů.

Jako služby ACS, jedna z funkcí nejvíce atraktivní B2C je, že se podporuje mnoho různých typů účtů. S B2C, můžete přihlášení uživatelů s jejich Facebook, Google, Microsoft, LinkedIn, Githubu, Yahoo účty a další. B2C podporuje kromě "místní účty", nebo uživatelského jména a hesla, které uživatelé vytvářejí speciálně pro vaši aplikaci. Azure AD B2C taky poskytuje bohaté rozšiřitelnosti, který můžete použít k přizpůsobení toky vaše přihlášení. Ale nepodporuje šířky ověřovací protokoly & tokenu formáty, které mohou vyžadovat zákazníci služby ACS. Je také nelze použít na získat tokeny & dotaz na další informace o uživateli od zprostředkovatele identity, Microsoft nebo jinak.

Následující tabulka porovnává funkce služby ACS, které jsou relevantní pro webové aplikace s těmi, které jsou k dispozici v Azure AD B2C. Na vysoké úrovni **Azure AD B2C je pravděpodobně správná volba pro migraci Pokud vaše aplikace je příjemce, kterým čelí, nebo pokud ji podporuje mnoho různých typů účtů.**

| Schopnost | Podpora služby ACS | Podpora Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Typy účtů** | | |
| Microsoft pracovní a školní účty | Podporuje se | Podporované pomocí vlastních zásad  |
| Účty ze služby Windows Server Active Directory a AD FS | Podporované prostřednictvím přímé federace se službou AD FS | Podporované pomocí SAML federace pomocí vlastních zásad |
| Účty z jiných podnikových systémů správy identit | Podporovaných prostřednictvím přímé federační přes Ws-Federation | Podporované pomocí SAML federace pomocí vlastních zásad |
| Účty Microsoft pro osobní použití (Windows Live ID) | Podporuje se | Podporuje se | 
| Facebook, Google, Yahoo účty | Podporuje se | Facebook & Google podporované nativně, Yahoo podporované prostřednictvím OpenID Connect federace pomocí vlastních zásad |
| **Protokoly & kompatibility SDK** | | |
| Technologie Windows Identity Foundation (WIF) | Podporuje se | Není podporováno. |
| WS-Federation | Podporuje se | Není podporováno. |
| OAuth 2.0 | Podpora pro koncept 13 | Podpora pro RFC 6749, specifikace většina moderních. |
| WS-Trust | Podporuje se | Není podporováno. |
| **Token formáty** | | |
| Tokeny webové JSON (Jwt) | Podporované ve verzi Beta | Podporuje se |
| Tokeny SAML 1.1 | Podporuje se | Nepodporuje se |
| Tokeny SAML 2.0 | Podporuje se | Nepodporuje se |
| Jednoduchých webových tokenů (SWT) | Podporuje se | Nepodporuje se |
| **Přizpůsobení** | | |
| Přizpůsobitelné domovské sféry nebo účet zjišťování Výběr uživatelského rozhraní | Zaváděná kód, který lze začlenit do aplikace | Plně přizpůsobitelná uživatelského rozhraní pomocí vlastních šablon stylů CSS. |
| Nahrát vlastní podpisových certifikátů tokenu | Podporuje se | Vlastní podpisové klíče, ne certifikáty podporované pomocí vlastních zásad. |
| Přizpůsobení deklarací identity v tokenech | Atribut PassThrough vstupních deklarací identity od zprostředkovatelů identity<br />Získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />Problém výstup deklarace identity založené na hodnotách vstupních deklarací identity<br />Vystavovat deklarace identity výstup s konstantní hodnoty | Můžete průchodu deklarace identity od zprostředkovatelů identity. Vlastní zásady, vyžaduje se pro některé deklarace identity.<br />Nelze získat přístupový token od zprostředkovatele identity jako deklaraci<br />Můžete vydat výstup deklarace identity založené na hodnotách vstupních deklarací identity pomocí vlastních zásad<br />Mohou vystavovat deklarace identity výstup s konstantní hodnoty pomocí vlastních zásad |
| **Automation** | | |
| Automatizovat úlohy konfigurace nebo správy | Podporu pro správu služby ACS. | Vytváření uživatelů povoleno přes Azure AD Graph API. Nelze vytvořit klienty B2C, aplikací nebo zásady prostřednictvím kódu programu. |

Pokud se rozhodnete, že Azure AD B2C je správná cesta dál pro vaše aplikace a služby, by měl začínat obráceným v následujících zdrojích informací:

- [Dokumentace k Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Vlastní zásady Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C ceny](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Další možnosti migrace

Pokud ani Azure AD ani Azure AD B2C vyhovovat potřebám vaší webové aplikace, kontaktujte nás a jsme vám může pomoct identifikovat nejlepší cestu dál.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD nor Azure AD B2C is sufficient to replace ACS in your web applications without making major code changes.  Some common examples might include:

- web applications using WIF and/or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- web applications performing direct federation to an enterprise IDP over the Ws-Federation protocol.
- web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by ACS.
- web applications with complex token transformation rules that Azure AD or Azure AD B2C cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: AAD only, use AAD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-using-active-authentication"></a>Webové služby pomocí ověřování active

Pro webové služby Zabezpečené s tokeny vydané službou ACS ACS poskytuje následující funkce a možnosti:

- Možnost registrovat jednu nebo více **identity služby** v oboru názvů služby ACS, která slouží k žádosti o tokeny.
- Podpora pro protokoly OAuth ZABALENÍ & OAuth 2.0 koncept 13 žádosti o tokeny, pomocí následujících typů přihlašovacích údajů:
    - Jednoduché heslo vytvořit identitu služby
    - A podepsaný pomocí symetrického klíče nebo X509 SWT certifikátu
    - Token SAML vydaný Poskytovatel důvěryhodné identity (obvykle instance služby AD FS)
- Podpora pro následující formáty tokenu: webového tokenu JSON (JWT), SAML 1.1, SAML 2.0 a jednoduché webové token (SWT).
- Pravidla transformace jednoduché tokenu

Identita služby v rámci služby ACS se obvykle používají pro implementaci serveru (S2S) jako ověřování.  

#### <a name="migrating-to-azure-active-directory"></a>Migrace na Azure Active Directory

Naše doporučení pro tento typ tok ověřování je pro migraci do [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD). Azure AD je na základě identity poskytovatele cloudové služby Microsoft pracovní a školní účty - je zprostředkovatel identity pro Office 365, Azure a mnoho dalšího. Ale můžou používat i k serveru na server ověřování pomocí služby Azure AD implementace udělení pověření klienta OAuth.  Následující tabulka porovnává funkce služby ACS v ověřování serveru na server s jsou k dispozici ve službě Azure AD.

| Schopnost | Podpora služby ACS | Podpora Azure AD |
| ---------- | ----------- | ---------------- |
| Postup registrace webové služby | Vytvoření předávající strany v portálu pro správu služby ACS. | Vytvoření webové aplikace služby Azure AD na portálu Azure. |
| Postup registrace klienta | Vytvoření identity služby v portálu pro správu služby ACS. | Vytvořte jinou webovou aplikaci Azure AD na portálu Azure. |
| Protokol použitý | Protokol OAuth ZABALENÍ<br />Udělení pověření klienta OAuth 2.0 koncept 13 | Udělení pověření klienta OAuth 2.0 |
| Metody ověřování klienta | Jednoduché heslo<br />Podepsaný SWT<br />Token SAML z federované IDP | Jednoduché heslo<br />Podepsaný token JWT |
| Token formáty | TOKEN JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | Pouze JWT |
| Token transformace | Přidat vlastní deklarace<br />Vystavení logiku jednoduché pak v případě deklarace identity | Přidat vlastní deklarace | 
| Automatizovat úlohy konfigurace nebo správy | Podporu pro správu služby ACS. | Podporované prostřednictvím Microsoft Graph & Azure AD Graph API. |

Pokyny pro scénáře implementující serveru na server najdete v následujících zdrojích informací:

- [Service-to-Service části příručky pro vývojáře Azure AD](https://aka.ms/aaddev).
- [Ukázka kódu démon pomocí jednoduché heslo pověření klienta](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Ukázka kódu démon pomocí přihlašovacích údajů certifikátu klienta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="other-migration-options"></a>Další možnosti migrace

Pokud Azure AD nevyhovuje potřebám vaší webové služby, uveďte poznámky a jsme vám může pomoct identifikovat nejlepší plán pro váš konkrétní případ.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD's client credentials OAuth grant implementation is not sufficient to replace ACS in your architecture without major code changes.  Some common examples might include:

- server-to-server authentication using token formats other than JWTs.
- server-to-server authentication using an input token provided by an external identity provider.
- server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

## <a name="questions-concerns--feedback"></a>Dotazy, otázky a zpětné vazby

Chápeme, že mnoho zákazníků služby ACS nebude po přečtení tohoto článku najít migraci a může být nutné některé pomoc nebo pokynů k určení správného plánu. Pokud chcete popisují scénáře migrace & otázky, nechte komentář na této stránce.
