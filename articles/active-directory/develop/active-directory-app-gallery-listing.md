---
title: "Výpis aplikace v galerii aplikací Azure Active Directory"
description: "Jak zobrazit aplikace, která podporuje jednotné přihlašování v galerii Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: cf25772bd9d92b59401aa5da76e6bbd5fa5ee3e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Výpis aplikace v galerii aplikací Azure Active Directory
Aplikace, která podporuje jednotné přihlašování s Azure Active Directory na seznam [Galerie Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikace se nejdřív musí implementovat jednu z následujících režimů integrace:

* **OpenID Connect** -přímá integrace s Azure AD pomocí OpenID Connect pro ověřování a Azure AD souhlas rozhraní API pro konfiguraci. Pokud právě začínáte integrační a aplikace nepodporuje SAML, jedná se o doporučujeme režim.
* **SAML** – aplikace již má možnost konfigurace poskytovatelů identit třetích stran pomocí protokolu SAML.

Požadavky na výpis pro oba režimy jsou níže.

## <a name="openid-connect-integration"></a>OpenID Connect integrace
K integraci aplikace s Azure AD, následující [vývojáře pokyny](active-directory-authentication-scenarios.md). Potom dokončete níže uvedené otázky a poslat waadpartners@microsoft.com.

* Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné otestovat integrace tým služby Azure AD.  
* Poskytnout pokyny jak tým Azure AD přihlášení a připojit instanci Azure AD k aplikaci pomocí [framework souhlasu Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Zadejte jakékoli další pokyny potřebné pro tento tým služby Azure AD k testování jednotné přihlašování s vaší aplikací. 
* Zadejte následující informace:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (limit 200 znaků):
> 
> Web Application (informativní):
> 
> Web podpory Technical aplikace nebo kontaktní údaje:
> 
> ID aplikace, jak je znázorněno v podrobností o aplikaci v https://portal.azure.com:
> 
> Adresa URL aplikace poskytovateli kde Zákazníci moct zaregistrovat a/nebo zakoupit aplikace:
> 
> Vyberte až tři kategorie pro aplikace uvedené v části (pro Azure Active Directory Marketplace naleznete kategorie k dispozici v):
> 
> Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):
> 
> Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):
> 
> Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):
> 
> 

## <a name="saml-integration"></a>Integrace SAML
Jakékoli aplikaci, která podporuje SAML 2.0 umožňuje integraci přímo se klient služby Azure AD pomocí [těchto pokynů můžete přidat vlastní aplikaci](../active-directory-saas-custom-apps.md). Jakmile testování, svoji integraci aplikace funguje s Azure AD, poslat následující informace, které < mailto:waadpartners@microsoft.com >.

* Zadejte přihlašovací údaje k účtu nebo testovacím klientem s vaší aplikací, které je možné otestovat integrace tým služby Azure AD.  
* Zadejte adresy přihlašování SAML, URL vystavitele (entity ID) a adresa URL odpovědi (služba assertion příjemce) hodnoty pro vaši aplikaci, jak je popsáno [zde](../active-directory-saas-custom-apps.md). Pokud tyto hodnoty se obvykle poskytují jako součást souboru metadat SAML, potom odešlete, také.
* Zadejte stručný popis, jak nakonfigurovat Azure AD jako zprostředkovatele identity do vaší aplikace pomocí SAML 2.0. Pokud vaše aplikace podporuje konfiguraci Azure AD jako zprostředkovatele identity prostřednictvím samoobslužný portál pro správu, potom ověřte, zda pověření výše uvedeného, zahrnují možnost nastavit tuto možnost.
* Zadejte následující informace:

> Název společnosti:
> 
> Web společnosti:
> 
> Název aplikace:
> 
> Popis aplikace (limit 200 znaků):
> 
> Web Application (informativní):
> 
> Web podpory Technical aplikace nebo kontaktní údaje:
> 
> Adresa URL aplikace poskytovateli kde Zákazníci moct zaregistrovat a/nebo zakoupit aplikace:
> 
> Vyberte maximálně tři kategorie pro aplikace uvedené v části (kategorie k dispozici, najdete v tématu [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Připojte malé ikony aplikace (soubor PNG, 45px podle 45px, plná barva pozadí):
> 
> Připojte velkých ikon aplikace (soubor PNG, 215px podle 215px, plná barva pozadí):
> 
> Připojte Logo aplikace (soubor PNG, 150px podle 122px, průhlednou barvu pozadí):
> 
> 

