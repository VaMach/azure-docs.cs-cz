---
title: Azure Active Directory Graph API | Microsoft Docs
description: "Přehled a rychlé spuštění Průvodce pro rozhraní Graph API, která umožňuje programový přístup ke službě Azure AD prostřednictvím koncových bodů rozhraní REST API."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: 815b9f75864ba3a08f623af12417391fba430f7d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Důrazně doporučujeme pro přístup k prostředkům Azure Active Directory použít [Microsoft Graph](https://graph.microsoft.io/) místo Azure AD Graph API. Náš vývojový program se nyní soustředí na Microsoft Graph a pro Azure AD Graph API nejsou plánovaná žádná další vylepšení. Existuje velmi omezený počet scénářů, pro které může být Azure AD Graph API stále vhodné. Další informace najdete v příspěvku [Microsoft Graph nebo Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blogu na webu Office Dev Center.
> 
> 

Azure Active Directory Graph API zajišťují programový přístup ke službě Azure AD prostřednictvím koncových bodů rozhraní REST API. Aplikace můžete použít rozhraní Graph API k provedení vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty. Například rozhraní Graph API podporuje následující běžných operací pro objekt uživatele:

* Vytvoření nového uživatele v adresáři
* Získat podrobné vlastnosti uživatele, jako je například jejich skupin
* Aktualizovat vlastnosti uživatele, například jejich umístění a telefonní číslo, nebo změnit své heslo
* Zkontrolovat členství ve skupinách uživatele pro přístup na základě rolí
* Zakažte účet uživatele nebo zcela odstranit

Kromě uživatelských objektů můžete provést podobnými operacemi na jiné objekty, jako jsou skupiny a aplikace. Chcete-li zavolat rozhraní Graph API v adresáři, aplikace musí být zaregistrované v Azure AD a nakonfigurovat tak, aby přístup k adresáři. Dosahuje se obvykle prostřednictvím k toku souhlasu uživatele nebo správce.

Chcete-li začít používat Azure Active Directory Graph API, přečtěte si téma [Průvodce rychlým zahájením Graph API](active-directory-graph-api-quickstart.md), nebo zobrazit [interaktivní referenční dokumentace rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funkce
Rozhraní Graph API poskytuje následující funkce:

* **Koncové body REST API**: rozhraní Graph API je služba RESTful skládá z koncových bodů, které jsou přístupné pomocí standardní požadavky HTTP. Rozhraní Graph API podporuje typy obsahu XML nebo Javascript Object Notation (JSON) pro požadavky a odpovědi. Další informace najdete v tématu [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Ověřování s Azure AD**: každý požadavek pro rozhraní Graph API musí být ověřeny připojením JSON Web Token (JWT) v hlavičce autorizace požadavku. Tento token se získávají pomocí vytváření požadavku na koncový bod tokenu Azure AD a poskytnutí platné přihlašovací údaje. Můžete použít tok přihlašovacích údajů klienta OAuth 2.0 nebo tok k získání tokenu pro volání grafu poskytování autorizačních kódů. Další informace najdete [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Na základě rolí autorizace (RBAC)**: skupiny zabezpečení slouží k provedení RBAC v rozhraní Graph API. Například, pokud chcete určit, zda má uživatel přístup ke konkrétní prostředek, můžete volat aplikace [zkontrolovat členství ve skupině (přenosné)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) operaci, která vrátí hodnotu true nebo false.
* **Rozdílovou dotazu**: Pokud chcete kontrolovat změny v adresáři mezi dvěma časových období bez nutnosti provádět časté dotazy na rozhraní Graph API, musíte provést žádost rozdílové dotazu. Tento typ požadavku vrátí pouze změny provedené mezi předchozí požadavek rozdílové dotazu a aktuální žádost. Další informace najdete v tématu [Azure AD Graph API rozdílové dotazu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Rozšíření adresáře**: Pokud vyvíjíte aplikaci, která potřebuje číst nebo zapisovat jedinečné vlastnosti u objektů adresáře, můžete zaregistrovat a používání rozšíření hodnoty pomocí rozhraní Graph API. Například pokud vaše aplikace vyžaduje Skype ID vlastnosti pro každého uživatele, můžete zaregistrovat nové vlastnosti v adresáři a bude k dispozici na každý objekt uživatele. Další informace najdete v tématu [Azure AD Graph API rozšíření schématu služby Directory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Zabezpečené obory oprávnění**: rozhraní AAD Graph API zpřístupní obory oprávnění, které umožňují zabezpečit souhlas přístup k datům AAD a podporují celou řadu typů aplikace klienta, včetně:
  
  * ty s uživatelským rozhraním, které jsou uvedeny delegovaná přístup k datům prostřednictvím autorizace z přihlášeného uživatele (delegovaný)
  * ty, které používají aplikace definovat řízení přístupu na základě rolí například klienti služby nebo démon (role aplikace)
    
    Obě delegovat a obory oprávnění role aplikace představují oprávnění vystavené rozhraní Graph API a může požadovat klientské aplikace prostřednictvím oprávnění k registraci aplikací [funkce na portálu Azure](https://portal.azure.com). Klienty můžete ověřit obory oprávnění uděleno jim zkontrolováním deklarace oboru (spojovací bod "služby") přijaté v tokenu přístupu pro přidělená oprávnění a rolí ("role") deklarací identity pro oprávnění role aplikace. Další informace o [Azure AD Graph API oprávnění obory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Scénáře
Rozhraní Graph API umožňuje mnoho scénářů s aplikací. Nejobvyklejší jsou následující scénáře:

* **Obchodní (jednoho klienta) aplikace**: V tomto scénáři funguje vývojář enterprise pro organizaci, která má předplatné služeb Office 365. Vývojář se vytváření webové aplikace, která komunikuje se službou Azure AD k provádění úloh takové přiřazení licence pro uživatele. Tato úloha vyžaduje přístup k rozhraní Graph API, takže registry vývojáře jedné klienta aplikace v Azure AD a nakonfiguruje oprávnění čtení a zápisu pro rozhraní Graph API. Pak aplikace je nakonfigurovaná k použití svoje vlastní přihlašovací údaje nebo těch, které aktuálně přihlášení uživatele k získání tokenu pro volání rozhraní Graph API.
* **Software jako služba aplikace (víceklientské)**: V tomto scénáři je nezávislý dodavatel softwaru (ISV) vývoj hostované víceklientské webové aplikace, která poskytuje funkce správy uživatele pro jiné organizace, které používají Azure AD. Tyto funkce vyžadují přístup k objektům adresář, a proto aplikace potřebuje k volání rozhraní Graph API. Vývojář zaregistruje aplikaci ve službě Azure AD, nakonfiguruje se vyžadují pro čtení a oprávnění pro rozhraní Graph API pro zápis a pak umožňuje externí přístup, takže se můžete k používání aplikace v jejich adresář souhlas jiných organizací. Při ověření uživatele v jiné organizaci k aplikaci poprvé, zobrazí se dialogové okno souhlasu s oprávněními, které aplikace požaduje.  Udělení souhlasu pak získáte aplikace ty požadovaná oprávnění k rozhraní Graph API v adresáři uživatele. Další informace o rozhraní souhlasu najdete v tématu [přehled rozhraní souhlas](active-directory-integrating-applications.md).

## <a name="see-also"></a>Viz také
[Průvodce rychlým zahájením Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Dokumentace k AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Příručka pro vývojáře pro službu Azure Active Directory](active-directory-developers-guide.md)

