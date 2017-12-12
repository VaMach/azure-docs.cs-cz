---
title: "Azure Active Directory auditu referenční dokumentace rozhraní API | Microsoft Docs"
description: "Jak začít pracovat s rozhraním API auditování Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9294d478ef35cac20bd54c2171eecda7c932e7a6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory auditu referenční dokumentace rozhraní API
Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům auditování pomocí kódu nebo související nástroje.
Obor tohoto tématu je k poskytování referenční informace o **audit rozhraní API**.

Přejděte na téma:

* [Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace

* [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.


Pro:

- Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](active-directory-reporting-faq.md) 

- Problémy prosím [souboru lístek podpory](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-the-data"></a>Kdo má přístup k datům?
* Uživatelé v roli Správce zabezpečení nebo Čtenář zabezpečení
* Globální správci
* Jakékoli aplikaci, která má oprávnění pro přístup k rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)

## <a name="prerequisites"></a>Požadavky
Chcete-li získat přístup k této sestavě prostřednictvím rozhraní API pro vytváření sestav, musíte mít:

* [Lepší edici nebo Azure Active Directory volné](active-directory-editions.md)
* Byla dokončena [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Přístup k rozhraní API
Můžete buď přístup toto rozhraní API prostřednictvím [Explorer grafu](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell. Aby PowerShell správně interpretovat syntaxe filtru OData, který se používá při voláních REST grafu AAD, je nutné použít backtick (neboli: čárka) znak "řídicí" znak $. Backtick znak, který slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell provést literálu výklad znak $, a zamezit tak složitá jako název proměnné prostředí PowerShell (ie: $filter).

Graf Explorer je aktivní v tomto tématu. V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>Koncový bod rozhraní API
Toto rozhraní API pomocí následující identifikátor URI se můžete dostat:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Neexistuje žádné omezení počtu záznamů vrácených rozhraní API auditu Azure AD (pomocí stránkování OData).
Pro uchování omezení pro vytváření sestav dat, podívejte se na [Reporting zásady uchovávání informací](active-directory-reporting-retention.md).

Toto volání se vrátí data v dávkách. Má každé dávky nesmí být delší než 1 000 záznamů.  
Chcete-li získat další dávky záznamů, použijte odkaz na další. Získáte informace o skiptoken z první sady vrácené záznamy. Token přeskočit bude na konci výsledek nastaveno.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Podporované filtry
Počet záznamů, které se vrátí pomocí rozhraní API můžete zúžit volání v podobě filtru.  
Pro přihlášení rozhraní API související data, jsou podporovány následující filtry:

* **$top =\<počet vrácených\>**  – Pokud chcete omezit počet vrácených záznamů. Toto je náročná operace. Tento filtr byste neměli používat, pokud chcete vrátit tisíce objektů.     
* **$filter =\<údajů filtru\>**  – Pokud chcete zadat typ záznamy, na kterých vám nejvíc záleží na základě pole podporovaný filtr

## <a name="supported-filter-fields-and-operators"></a>Pole podporovaný filtr a operátory
Pokud chcete zadat typ záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci následující pole filtru:

* [Datum](#activitydate) -definuje datum nebo rozsah dat
* [kategorie](#category) – definuje kategorie, kterou chcete filtrovat.
* [activityStatus](#activitystatus) -definuje stav aktivity
* [activityType](#activitytype) -definuje typ aktivity
* [aktivita](#activity) -definuje aktivitu jako řetězec  
* [objektu actor nebo název](#actorname) -definuje objektu actor tvar názvu objektu actor
* [objektu actor/objectid](#actorobjectid) -definuje objektu actor v podobě ID objektu actor   
* [objektu actor/upn](#actorupn) -definuje objektu actor v podobě název Princip objektu actor uživatele (UPN) 
* [Cílová nebo](#targetname) -definuje cílový tvar názvu objektu actor
* [cíl/objectid](#targetobjectid) -definuje cíl v podobě ID cílové složky  
* [cíl/upn](#targetupn) -definuje objektu actor v podobě název Princip objektu actor uživatele (UPN)   

- - -
### <a name="activitydate"></a>Datum
**Podporované operátory**: eq, ge, le, gt, lt

**Příklad**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Poznámky k**:

data a času musí být ve formátu UTC

- - -
### <a name="category"></a>category

**Podporované hodnoty**:

| Kategorie                         | Hodnota     |
| :--                              | ---       |
| Základní adresář                   | Adresář |
| Samoobslužná správa hesel | SSPR      |
| Samoobslužná správa skupin    | SSGM      |
| Zřizování účtů             | Sync      |
| Automatická změna hesel      | Automatická změna hesel |
| Identity Protection              | IdentityProtection |
| Pozvaní uživatelé                    | Pozvaní uživatelé |
| Služba MIM                      | Služba MIM |



**Podporované operátory**: eq

**Příklad**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Podporované hodnoty**:

| Stav aktivity | Hodnota |
| :--             | ---   |
| Úspěch         | 0     |
| Selhání         | - 1   |

**Podporované operátory**: eq

**Příklad**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Podporované operátory**: eq

**Příklad**:

    $filter=activityType eq 'User'    

**Poznámky k**:

malá a velká písmena

- - -
### <a name="activity"></a>Aktivity
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Poznámky k**:

malá a velká písmena

- - -
### <a name="actorname"></a>objektu actor nebo název
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Poznámky k**:

Velká a malá písmena

- - -
### <a name="actorobjectid"></a>objektu actor/objectId
**Podporované operátory**: eq

**Příklad**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>Cílová nebo
**Podporované operátory**: eq, obsahuje, startsWith

**Příklad**:

    $filter=targets/any(t: t/name eq 'some name')    

**Poznámky k**:

Velká a malá písmena

- - -
### <a name="targetupn"></a>cíl/upn
**Podporované operátory**: eq startsWith

**Příklad**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Poznámky k**:

* Velká a malá písmena
* Je nutné přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

- - -
### <a name="targetobjectid"></a>cíl/objectId
**Podporované operátory**: eq

**Příklad**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>objektu actor/upn
**Podporované operátory**: eq startsWith

**Příklad**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Poznámky k**:

* Velká a malá písmena 
* Je nutné přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

- - -
## <a name="next-steps"></a>Další kroky
* Chcete příklady pro filtrovaný systému aktivity? Podívejte se [ukázky auditu rozhraní API služby Azure Active Directory](active-directory-reporting-api-audit-samples.md).
* Opravdu chcete získat další informace o generování sestav rozhraní API Azure AD? V tématu [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

