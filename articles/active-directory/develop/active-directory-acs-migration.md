---
title: "Migrace ze služby Řízení přístupu Azure | Microsoft Docs"
description: "Možnosti pro přesun aplikace a služby ze služby Řízení přístupu Azure"
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
ms.openlocfilehash: 8c719f4970303c3ff0dd7e80fbd700cb401d2cc5
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="migrate-from-the-azure-access-control-service"></a>Migrace ze služby Řízení přístupu Azure

Azure řízení přístupu, což je služba Azure Active Directory (Azure AD), bude vyřazeno v listopadu 2018. Aplikace a služby, které používají řízení přístupu musí být plně migrovat na jiný mechanismus ověřování té. Tento článek popisuje doporučení pro aktuální zákazníci při plánování přestat používat použití řízení přístupu. Pokud nepoužijete aktuálně řízení přístupu, nemusíte provádět žádnou akci.


## <a name="overview"></a>Přehled

Řízení přístupu je Cloudová služba ověřování, která nabízí snadný způsob, jak ověřování a autorizaci uživatelů pro přístup k webovým aplikacím a službám. To umožňuje mnoho funkcí ověřování a autorizaci se promítnou z vašeho kódu. Řízení přístupu slouží především vývojáři a architektům klienti rozhraní Microsoft .NET, webové aplikace ASP.NET a webových služeb Windows Communication Foundation (WCF).

Případy použití pro řízení přístupu můžete rozdělit do tří kategorií:

- Ověřování pro určité cloudové služby společnosti Microsoft, včetně Azure Service Bus a Dynamics CRM. Klientské aplikace získat tokeny z řízení přístupu k ověření na tyto služby k provádění různých akcí.
- Přidání ověřování pro webové aplikace, hotových (např. Sharepointu) a vlastní. Pomocí řízení přístupu "pasivní" ověřování webové aplikace může podporovat přihlášení pomocí účtu Microsoft (dříve Live ID) a s účty z Google, Facebook, Yahoo, Azure AD a služby Active Directory Federation Services (AD FS).
- Zabezpečení vlastních webových služeb s tokeny vydané pomocí řízení přístupu. Webové služby pomocí "aktivní" ověřování můžete zajistit, že umožňují přístup pouze k známí klienti ověřené pomocí řízení přístupu.

Každá z těchto případy a jejich doporučená migrace, které jsou popsané strategie použít v následujících částech. 

> [!WARNING]
> Ve většině případů se nevyžadují změny významné kódu při migraci stávající aplikace a služby do novější technologie. Doporučujeme, abyste ihned začít plánovat a provádění migrace předejdete všechny potenciální výpadků nebo výpadek.

Řízení přístupu má následující komponenty:

- Zabezpečené tokenu služby (STS), která přijímá žádosti o ověření a vydává tokeny zabezpečení na oplátku.
- Portál Azure classic, kde můžete vytvořit, odstranit a povolit nebo zakázat obory názvů řízení přístupu.
- Samostatné řízení přístupu portálu pro správu, kde můžete přizpůsobit a nakonfigurovat obory názvů řízení přístupu.
- Služba správy, který můžete použít k automatizaci funkcí portálů.
- Token transformace pravidlo modul, který můžete použít k definování složitých logických k manipulaci s formátem výstupních tokenů, které vydává řízení přístupu.

Pokud chcete použít tyto součásti, je nutné vytvořit jeden nebo více oborů názvů řízení přístupu. A *obor názvů* je řízení přístupu, který vašim aplikacím a službám komunikovat s vyhrazenou instancí. Obor názvů je izolován od ostatních zákazníků řízení přístupu. Zákazníci navzájem řízení přístupu používat vlastní obory názvů. Obor názvů v řízení přístupu má vyhrazenou adresu URL, které vypadá takto:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

Veškerá komunikace s služby tokenů zabezpečení a operace správy se provádějí na této adrese URL. Můžete použít různé cesty pro jiné účely. Pokud chcete zjistit, jestli služby nebo aplikace pomocí řízení přístupu, monitorování pro všechny přenosy na https://\<obor názvů\>. accesscontrol.windows.net. Přenosy dat pro tuto adresu URL se zpracovává souborem řízení přístupu a musí být zastaveny. 

Výjimkou jsou všechny přenosy do https://accounts.accesscontrol.windows.net. Data na tuto adresu URL již zpracovává jiný službou a nemá vliv vyřazení řízení přístupu. 

Musí také přihlášení k portálu Azure classic a zkontrolujte všechny obory názvů řízení přístupu v odběry, které vlastníte. Obory názvů řízení přístupu jsou uvedené na **obory názvů řízení přístupu** v části **služby Active Directory** služby.

Další informace o řízení přístupu najdete v tématu [2.0 služby Řízení přístupu (archivovat)](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Plán vyřazování z provozu

Od listopadu 2017 všechny součásti řízení přístupu jsou plně podporované a funkční. Pouze omezení je, že jste [nelze vytvořit nové obory názvů řízení přístupu prostřednictvím portálu Azure classic](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Tady je plán pro místo začne součásti řízení přístupu:

- **2017 listopadu**: Správce Azure AD na základě zkušeností uživatelů portálu Azure classic [je vyřazeno](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). V tomto okamžiku je k dispozici v nové, vyhrazené URL správu oboru názvů pro řízení přístupu: http://manage.windowsazure.com?restoreClassic=true. Zobrazit existující obory, povolení a zakázání obory názvů a k odstranění obory názvů, pokud zvolíte možnost, použijte tuto adresu URl.
- **2018 duben**: obor názvů řízení přístupu již není k dispozici na adrese URL http://manage.windowsazure.com?restoreClassic=true vyhrazené. V tomto okamžiku nelze zakázat nebo povolit, odstranit nebo výčet obory řízení přístupu. Portálu pro správu řízení přístupu však budou plně funkční a umístěné na https://\<obor názvů\>. accesscontrol.windows.net. Všechny součásti řízení přístupu i nadále fungovat normálně.
- **2018 listopadu**: všechny řízení přístupu součásti jsou trvale vypnout. To zahrnuje portálu pro správu řízení přístupu, služba správy, služba tokenů zabezpečení a tokenu transformační pravidlo modul. V tomto okamžiku všechny požadavky, které byly odeslány na řízení přístupu (umístěné v \<obor názvů\>. accesscontrol.windows.net) nezdaří. Jste měli migrovali všechny existující aplikace a služby do jiných technologií dobře před tímto časem.


## <a name="migration-strategies"></a>Strategie migrace

Následující části popisují základní doporučení pro migraci z řízení přístupu na dalších technologiích společnosti Microsoft.

### <a name="clients-of-microsoft-cloud-services"></a>Klienti cloudové služby společnosti Microsoft

Jednotlivých cloudových služeb společnosti Microsoft, které přijímá tokeny, které jsou vydány řízení přístupu nyní podporuje aspoň jeden alternativní způsob ověřování. Správný ověřovací mechanismus se liší u každé služby. Doporučujeme, abyste naleznete konkrétní dokumentace pro každou službu oficiální pokyny. Pro usnadnění práce každou sadu dokumentace je k dispozici zde:

| Služba | Doprovodné materiály |
| ------- | -------- |
| Azure Service Bus | [Migrace na sdílených přístupových podpisů](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Předávání přes Azure Service Bus | [Migrace na sdílených přístupových podpisů](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Redis Cache | [Migrace na Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Služby Azure DataMarket | [Migrace na rozhraní API kognitivní služby](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migrace s funkcí Logic Apps služby Azure App Service](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Migrace na ověřování Azure AD](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Upgrade agenta Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-that-use-passive-authentication"></a>Webové aplikace, které používají pasivní ověřování

Pro webové aplikace, které používají řízení přístupu pro ověřování uživatelů řízení přístupu nabízí následující funkce a možnosti pro vývojáře webových aplikací a architektům:

- Těsná integrace s Windows Identity Foundation (WIF).
- Federace se Google, Facebook, Yahoo, Azure Active Directory a AD FS účty a účty Microsoft.
- Podpora pro následující protokoly pro ověřování: OAuth 2.0 koncept 13, WS-Trust a Web Services Federation (WS-Federation).
- Podpora pro následující formáty tokenu: JSON Web Token (JWT), SAML 1.1, SAML 2.0 a jednoduché webové tokenu (SWT).
- Prostředí pro zjišťování domovské sféry, integrované do WIF, který umožňuje uživatelům vyberte typ účtu, který bude používat k přihlášení. Toto prostředí je hostitelem webové aplikace a je plně přizpůsobitelná.
- Token transformaci, která umožňuje bohaté přizpůsobení deklarací přijatých webovou aplikaci z řízení přístupu, včetně:
    - Předávání deklarací identity od zprostředkovatelů identity.
    - Přidání další vlastní deklarace.
    - Jednoduché pak v případě logika vystavovat deklarace identity za určitých podmínek.

Bohužel není jedna služba, která nabízí všechny tyto funkce ekvivalentní. Byste měli zvážit, jaké funkce řízení přístupu je třeba a pak vybrat mezi ověřováním prostřednictvím [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C), nebo jiné cloudové ověřování Služba.

#### <a name="migrate-to-azure-active-directory"></a>Migrace na Azure Active Directory

Cesta ke zvážení je integrace aplikace a služby přímo s Azure AD. Azure AD je poskytovatel cloudových identit Microsoft pracovní nebo školní účty. Azure AD je zprostředkovatel identity pro Office 365, Azure a mnoho dalšího. Poskytuje podobné federovaný možnosti ověřování pro řízení přístupu, ale nepodporuje všechny funkce řízení přístupu. 

Primární příkladem je federation s poskytovateli sociální identity, jako je Facebook, Google a Yahoo. Pokud vaši uživatelé přihlásit pomocí těchto typů přihlašovacích údajů, není řešení pro vás Azure AD. 

Azure AD nepodporuje ani nemusí být přesné stejné ověřovací protokoly jako řízení přístupu. Například i když řízení přístupu a Azure AD podporovat OAuth, existují malé rozdíly v každé implementace. Různé implementace vyžadují, abyste upravit kód jako součást migrace.

Azure AD poskytuje však několik výhod potenciální zákazníkům řízení přístupu. Je nativně podporuje Microsoft pracovní nebo školní účty hostovaná v cloudu, které se běžně používají zákazníci řízení přístupu. 

Klient služby Azure AD můžete také federovaný na jeden nebo více instancí místní služby Active Directory pomocí služby AD FS. Tímto způsobem, vaše aplikace může ověřit cloudových uživatelů a uživatelů, kteří jsou hostované na místě. Také podporuje protokol WS-Federation, takže je relativně jednoduché k integraci s webovou aplikaci pomocí WIF.

Následující tabulka porovnává funkce řízení přístupu, které jsou relevantní pro webové aplikace se tyto funkce, které jsou k dispozici ve službě Azure AD. 

Na vysoké úrovni *Azure Active Directory je pravděpodobně nejlepší volbou pro migraci Pokud umožníte uživatelům přihlášení v pouze s jejich Microsoft pracovní nebo školní účty*.

| Schopnost | Podpora řízení přístupu | Podpora Azure AD |
| ---------- | ----------- | ---------------- |
| **Typy účtů** | | |
| Microsoft pracovní nebo školní účty | Podporuje se | Podporuje se |
| Účty z Windows Server Active Directory a AD FS |-Podporované prostřednictvím federaci se klient služby Azure AD <br />-Podporované prostřednictvím přímé federační službou AD FS | Podporuje jen přes federaci se klient služby Azure AD | 
| Účty z jiných podnikových systémů správy identit |-Možný prostřednictvím federaci se klient služby Azure AD <br />-Podporované prostřednictvím přímé federation | Možný prostřednictvím federaci se klient služby Azure AD |
| Účty Microsoft pro osobní použití | Podporuje se | Podporované přes Azure AD v2.0 protokolu OAuth, ale nikoli prostřednictvím žádné jiné protokoly | 
| Facebook, Google, Yahoo účty | Podporuje se | Nepodporuje se jakékoli |
| **Protokoly a kompatibility SDK** | | |
| WIF | Podporuje se | Podporované, ale omezená pokyny jsou k dispozici |
| WS-Federation | Podporuje se | Podporuje se |
| OAuth 2.0 | Podpora pro koncept 13 | Podpora pro RFC 6749, většina moderních specifikace |
| WS-Trust | Podporuje se | Nepodporuje se |
| **Token formáty** | | |
| TOKEN JWT | Podporované ve verzi Beta | Podporuje se |
| SAML 1.1 | Podporuje se | Podporuje se |
| SAML 2.0 | Podporuje se | Podporuje se |
| SWT | Podporuje se | Nepodporuje se |
| **Přizpůsobení** | | |
| Přizpůsobitelné domovské sféry zjišťování a účet výdej uživatelského rozhraní | Zaváděná kód, který lze začlenit do aplikace | Nepodporuje se |
| Nahrát vlastní podpisové certifikáty tokenu | Podporuje se | Podporuje se |
| Přizpůsobení deklarací identity v tokenech |-Předávat vstupní deklarace identity od zprostředkovatelů identity<br />-Získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />-Vystavovat deklarace identity výstup založené na hodnotách vstupních deklarací identity<br />-Vystavovat deklarace identity výstup s konstantní hodnoty |-Nelze předat prostřednictvím deklaracích identity od zprostředkovatelů federovaných identit<br />-Nelze získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />-Nemůžou vystavovat výstup deklarace identity založené na hodnotách vstupních deklarací identity<br />-Mohou vystavovat deklarace identity výstup s konstantní hodnoty<br />-Můžete vydat výstup deklarace identity založené na vlastnosti uživatelů se synchronizují do Azure AD |
| **Automation** | | |
| Automatizovat úkoly konfigurace a správy | Podporované prostřednictvím služby pro správu řízení přístupu | Podporu pro Microsoft Graph a Azure AD Graph API |

Pokud se rozhodnete, že Azure AD je nejlepší cesta migrace k vašim aplikacím a službám, by měl být vědomi dva způsoby, jak integrovat vaši aplikaci s Azure AD.

Použití WS-Federation nebo WIF při integraci s Azure AD, doporučujeme následující přístupu popsané v [konfigurace federované jednotné přihlašování pro aplikace bez Galerie](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Článek odkazuje ke konfiguraci Azure AD pro na základě SAML jednotné přihlašování, ale funguje i pro konfiguraci protokolu WS-Federation. Tento způsob vyžaduje licenci Azure AD Premium. Tento přístup má dvě výhody:

- Zobrazí se úplná flexibilita při přizpůsobení tokenu Azure AD. Deklarace identity, které jsou vydané službou Azure Active Directory tak, aby odpovídaly deklarace identity, které jsou vydány řízení přístupu můžete přizpůsobit. To zahrnuje zejména uživatelské ID nebo název identifikátor deklarace identity. Nadále přijímat identifikátory konzistentní uživatele pro uživatele po změně technologie, zkontrolujte, zda uživatel ID vydán Azure AD odpovídají certifikátů vydávaných řízení přístupu.
- Můžete nakonfigurovat certifikát pro podpis tokenu specifický pro vaše aplikace a s životnost, kterou řídíte.

<!--

Possible nameIdentifiers from Access Control (via AAD or AD FS):
- AD FS - Whatever AD FS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Tento postup vyžaduje licenci Azure AD Premium. Pokud jste zákazník řízení přístupu a chcete, aby premium licenci pro nastavení jednotného přihlašování pro aplikace, kontaktujte nás. Jsme bude radostí zajistit vývojáře licence k použití.

Alternativní způsob je podle [této ukázce kódu](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), což dává mírně odlišné pokyny pro nastavení služby WS-Federation. Tato ukázka kódu nepoužívá WIF, ale místo toho middlewaru ASP.NET 4.5 OWIN. Ale pokyny pro registraci aplikace jsou platné pro aplikace využívající technologii WIF a nevyžadují licenci Azure AD Premium. 

Pokud zvolíte tuto metodu, musíte porozumět [podepisování výměna klíče ve službě Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Tento postup používá Azure AD globálního podpisového klíče problém tokeny. Ve výchozím nastavení WIF se automaticky neaktualizuje podpisové klíče. Pokud Azure AD otočí jeho globální podpisové klíče, je nutné připravit pro přijetí změn implementaci WIF.

Pokud můžete integrovat se službou Azure AD prostřednictvím protokolů OpenID Connect a OAuth, doporučujeme, abyste tak. Máme rozsáhlou dokumentaci a pokyny o tom, jak integrovat Azure AD do vaší webové aplikace, které jsou k dispozici v našem [Příručka pro vývojáře Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrate-to-azure-active-directory-b2c"></a>Migrace na Azure Active Directory B2C

Jiné cesty migrace vzít v úvahu je Azure AD B2C. Azure AD B2C je Cloudová služba ověřování, jako je řízení přístupu, umožňuje vývojářům využívající správu logiku v cloudové službě jejich ověřování a identita. Je služba placené (s vrstvami volné a premium), která je určená pro určených aplikací, které by mohly mít až miliony aktivní uživatelé.

Řízení přístupu, jako je jedním z nejvíce atraktivní funkce Azure AD B2C je, že podporuje mnoho různých typů účtů. S Azure AD B2C je uživatelé přihlásit pomocí svého účtu Microsoft nebo sítě Facebook, Google, LinkedIn, GitHub nebo Yahoo účty a další. Azure AD B2C podporuje také "místní účty," nebo uživatelského jména a hesla, které uživatelé vytvářejí speciálně pro vaši aplikaci. Azure AD B2C taky poskytuje bohaté rozšiřitelnosti, který můžete použít k přizpůsobení přihlášení toky. 

Ale Azure AD B2C nepodporuje šířky ověřovací protokoly a formáty token tuto řízení přístupu může vyžadovat zákazníků. Také nemůžete použít Azure AD B2C získat tokeny a dotazů pro další informace o uživateli od zprostředkovatele identity, Microsoft nebo jinak.

Následující tabulka porovnává funkce řízení přístupu, které jsou relevantní pro webové aplikace s těmi, které jsou k dispozici v Azure AD B2C. Na vysoké úrovni *Azure AD B2C je pravděpodobně správná volba pro migraci Pokud vaše aplikace je příjemce, kterým čelí, nebo pokud ji podporuje mnoho různých typů účtů.*

| Schopnost | Podpora řízení přístupu | Podpora Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Typy účtů** | | |
| Microsoft pracovní nebo školní účty | Podporuje se | Podporované pomocí vlastních zásad  |
| Účty z Windows Server Active Directory a AD FS | Podporované prostřednictvím přímé federační službou AD FS | Podporované prostřednictvím SAML federace pomocí vlastních zásad |
| Účty z jiných podnikových systémů správy identit | Podporované prostřednictvím přímé federační prostřednictvím služby WS-Federation | Podporované prostřednictvím SAML federace pomocí vlastních zásad |
| Účty Microsoft pro osobní použití | Podporuje se | Podporuje se | 
| Facebook, Google, Yahoo účty | Podporuje se | Facebook a Google podporované nativně, Yahoo podporován prostřednictvím OpenID Connect federace pomocí vlastních zásad |
| **Protokoly a kompatibility SDK** | | |
| Technologie Windows Identity Foundation (WIF) | Podporuje se | Nepodporuje se |
| WS-Federation | Podporuje se | Nepodporuje se |
| OAuth 2.0 | Podpora pro koncept 13 | Podpora pro RFC 6749, většina moderních specifikace |
| WS-Trust | Podporuje se | Nepodporuje se |
| **Token formáty** | | |
| TOKEN JWT | Podporované ve verzi Beta | Podporuje se |
| SAML 1.1 | Podporuje se | Nepodporuje se |
| SAML 2.0 | Podporuje se | Nepodporuje se |
| SWT | Podporuje se | Nepodporuje se |
| **Přizpůsobení** | | |
| Přizpůsobitelné domovské sféry zjišťování a účet výdej uživatelského rozhraní | Zaváděná kód, který lze začlenit do aplikace | Plně přizpůsobitelná uživatelského rozhraní pomocí vlastních šablon stylů CSS |
| Nahrát vlastní podpisové certifikáty tokenu | Podporuje se | Vlastní podpisové klíče, ne certifikáty podporované pomocí vlastních zásad |
| Přizpůsobení deklarací identity v tokenech |-Předávat vstupní deklarace identity od zprostředkovatelů identity<br />-Získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />-Vystavovat deklarace identity výstup založené na hodnotách vstupních deklarací identity<br />-Vystavovat deklarace identity výstup s konstantní hodnoty |-Může předávat deklarace identity od zprostředkovatelů identity; vlastní zásady, které jsou požadované pro některé deklarace identity<br />-Nelze získání tokenu přístupu z zprostředkovatele identity jako deklaraci<br />-Mohou vystavovat deklarace identity výstup založené na hodnotách vstupních deklarací identity pomocí vlastních zásad<br />-Mohou vystavovat deklarace identity výstup s konstantní hodnoty pomocí vlastních zásad |
| **Automation** | | |
| Automatizovat úkoly konfigurace a správy | Podporované prostřednictvím služby pro správu řízení přístupu |– Vytváření uživatelů povoleno přes Azure AD Graph API<br />-Nelze vytvořit klienty B2C, aplikací nebo zásady prostřednictvím kódu programu |

Pokud se rozhodnete, že Azure AD B2C je nejlepší cesta migrace k vašim aplikacím a službám, začněte v následujících zdrojích informací:

- [Dokumentace k Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Vlastní zásady služby Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Ceny služby Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Další možnosti migrace

Pokud Azure AD a Azure AD B2C nesplňují požadavky webové aplikace, kontaktujte nás. Pomůžeme vám identifikovat nejlepší cestu migrace.

<!--

#### Migrate to Ping Identity or Auth0

In some cases, you might find that Azure AD and Azure AD B2C aren't sufficient to replace Access Control in your web applications without making major code changes. Some common examples might include:

- Web applications that use WIF or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- Web applications that perform direct federation to an enterprise identify provider over the WS-Federation protocol.
- Web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by Access Control.
- Web applications with complex token transformation rules that Azure AD or Azure AD B2C can't reproduce.

In these cases, you might want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options. Each of the following options offer capabilities similar to Access Control:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of Access Control](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of Access Control and Auth0.
- Enterprise customers also should consider [Ping Identity](https://www.pingidentity.com). If you contact us, we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity and Auth0 is to ensure that all Access Control customers have a migration path for their apps and services that minimizes the amount of work required to move from Access Control.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Webové služby, které používají ověřování active

Pro webové služby, které jsou zabezpečené s tokeny vydané pomocí řízení přístupu řízení přístupu nabízí následující funkce a možnosti:

- Možnost registrovat jednu nebo více *identity služby* v oboru názvů řízení přístupu. Identita služby slouží k žádosti o tokeny.
- Podpora pro ZABALENÍ OAuth a OAuth 2.0 koncept 13 protokoly žádosti o tokeny, pomocí následujících typů přihlašovacích údajů:
    - Jednoduché heslo, které se vytvoří identitu služby
    - A podepsaný pomocí symetrického klíče nebo X509 SWT certifikátu
    - Token SAML vydaný Poskytovatel důvěryhodné identity (obvykle, instance služby AD FS)
- Podpora pro následující formáty tokenu: JWT, SAML 1.1, SAML 2.0 a SWT.
- Pravidla transformace jednoduché tokenu.

Identita služby v řízení přístupu jsou obvykle používány k implementaci serveru a serverem ověřování.  

#### <a name="migrate-to-azure-active-directory"></a>Migrace na Azure Active Directory

Naše doporučení pro tento typ tok ověřování je pro migraci do [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD je poskytovatel cloudových identit Microsoft pracovní nebo školní účty. Azure AD je zprostředkovatel identity pro Office 365, Azure a mnoho dalšího. 

Můžete také použít Azure AD pro ověřování serveru na server pomocí Azure AD implementace udělení pověření klienta OAuth. Následující tabulka porovnává funkce řízení přístupu v ověřování serveru na server s těmi, které jsou k dispozici ve službě Azure AD.

| Schopnost | Podpora řízení přístupu | Podpora Azure AD |
| ---------- | ----------- | ---------------- |
| Postup registrace webové služby | Vytvoření předávající strany v portálu pro správu řízení přístupu | Vytvoření webové aplikace služby Azure AD na portálu Azure |
| Postup registrace klienta | Vytvoření identity služby v portálu pro správu řízení přístupu | Vytvořit jinou webovou aplikaci Azure AD na portálu Azure |
| Protokol použitý |-Protokol OAuth WRAP<br />-Udělení pověření klienta 13 koncept OAuth 2.0 | Udělení pověření klienta OAuth 2.0 |
| Metody ověřování klienta |-Jednoduché heslo<br />-Podepsaný SWT<br />-SAML token od zprostředkovatele federovaných identit |-Jednoduché heslo<br />-Podepsaný token JWT |
| Token formáty |-JWT<br />-SAML 1.1<br />-SAML 2.0<br />-SWT<br /> | Pouze JWT |
| Token transformace |-Přidat vlastní deklarace<br />-Jednoduché pak v případě deklarace identity logiku vystavení | Přidat vlastní deklarace | 
| Automatizovat úkoly konfigurace a správy | Podporované prostřednictvím služby pro správu řízení přístupu | Podporu pro Microsoft Graph a Azure AD Graph API |

Pokyny týkající se implementuje scénáře serveru na server najdete v následujících materiálech:

- Část Service-to-Service [Příručka pro vývojáře Azure AD](https://aka.ms/aaddev)
- [Ukázka kódu démon pomocí jednoduché heslo pověření klienta](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Ukázka kódu démon pomocí pověření certifikátu klienta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="other-migration-options"></a>Další možnosti migrace

Pokud Azure AD nevyhovuje potřebám vaší webové služby, nechte komentář. Pomůžeme vám identifikovat nejlepší plán pro váš konkrétní případ.

<!--

#### Migrate to Ping Identity or Auth0

In some cases, you might find that the Azure AD client credentials and the OAuth grant implementation aren't sufficient to replace Access Control in your architecture without major code changes. Some common examples might include:

- Server-to-server authentication using token formats other than JWTs.
- Server-to-server authentication using an input token provided by an external identity provider.
- Server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you might consider migrating your web application to another cloud authentication service. We recommend exploring the following options. Each of the following options offer capabilities similar to Access Control:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of Access Control](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of Access Control and Auth0.
- Enterprise customers should also consider [Ping Identity](https://www.pingidentity.com). If you contact us, we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity and Auth0 is to ensure that all Access Control customers have a migration path for their apps and services that minimizes the amount of work required to move from Access Control.

-->

## <a name="questions-concerns-and-feedback"></a>Dotazy, otázky a zpětné vazby

Chápeme, že mnoho zákazníků řízení přístupu nebude najít migraci po přečtení tohoto článku. Může být nutné některé pomoc nebo pokynů k určení správného plánu. Pokud chcete popisují scénáře migrace a dotazy, uveďte poznámky na této stránce.
