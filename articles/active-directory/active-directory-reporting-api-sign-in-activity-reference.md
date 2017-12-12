---
title: "Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API | Microsoft Docs"
description: "Referenční dokumentace rozhraní API sestavy přihlašovací aktivita služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: e213e6fcf10e98cb8e4344692475eb8d41d1afb5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API
Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům sestavy přihlašovací aktivita pomocí kódu nebo související nástroje.
Obor tohoto tématu je k poskytování referenční informace o **API sestavy aktivity přihlášení**.

Přejděte na téma:

* [Přihlašovací aktivity](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace
* [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.


## <a name="who-can-access-the-api-data"></a>Kdo má přístup k rozhraní API data?
* Uživatelé a objekty služby v roli správce zabezpečení nebo čtečky zabezpečení
* Globální správci
* Jakékoli aplikaci, která má oprávnění pro přístup k rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)

Pokud chcete konfigurovat přístup k aplikaci pro přístup k zabezpečení rozhraní API, jako jsou například události přihlášení, použijte tuto Powershellovou přidat aplikace objektu služby do role Čtenář zabezpečení

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Požadavky
Přístup k této sestavě prostřednictvím rozhraní API pro vytváření sestav, musíte mít:

* [Edici Azure Active Directory Premium P1 a P2](active-directory-editions.md)
* Byla dokončena [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Přístup k rozhraní API
Můžete buď přístup toto rozhraní API prostřednictvím [Explorer grafu](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell. Aby PowerShell správně interpretovat syntaxe filtru OData, který se používá při voláních REST grafu AAD, je nutné použít backtick (neboli: čárka) znak "řídicí" znak $. Backtick znak, který slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell provést literálu výklad znak $, a zamezit tak složitá jako název proměnné prostředí PowerShell (ie: $filter).

Graf Explorer je aktivní v tomto tématu. V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>Koncový bod rozhraní API
Toto rozhraní API pomocí následující základní identifikátor URI se můžete dostat:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Z důvodu objem dat toto rozhraní API může mít jeden milión vrácené záznamy. 

Toto volání se vrátí data v dávkách. Má každé dávky nesmí být delší než 1 000 záznamů.  
Chcete-li získat další dávky záznamů, použijte odkaz na další. Získat [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informace z první sady vrácené záznamy. Token přeskočit bude na konci výsledek nastaveno.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Podporované filtry
Počet záznamů, které se vrátí pomocí rozhraní API můžete zúžit volání v podobě filtru.  
Pro přihlášení rozhraní API související data, jsou podporovány následující filtry:

* **$top =\<počet vrácených\>**  – Pokud chcete omezit počet vrácených záznamů. Toto je náročná operace. Tento filtr byste neměli používat, pokud chcete vrátit tisíce objektů.  
* **$filter =\<údajů filtru\>**  – Pokud chcete zadat typ záznamy, na kterých vám nejvíc záleží na základě pole podporovaný filtr

## <a name="supported-filter-fields-and-operators"></a>Pole podporovaný filtr a operátory
Pokud chcete zadat typ záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci následující pole filtru:

* [signinDateTime](#signindatetime) -definuje datum nebo rozsah dat
* [ID uživatele](#userid) -definuje konkrétní uživatele na základě ID uživatele.
* [userPrincipalName](#userprincipalname) -definuje konkrétní uživatele na základě uživatele hlavní název uživatele (UPN)
* [appId](#appid) -definuje konkrétní aplikace na základě ID aplikace
* [appDisplayName](#appdisplayname) -definuje konkrétní aplikace na základě aplikace zobrazovaný název
* [loginStatus](#loginStatus) -definuje stav přihlášení (úspěch nebo chyba)

> [!NOTE]
> Při použití Průzkumníku grafu, je potřeba mít správnou velikost pro každý písmeno je vaše pole filtru.
> 
> 

Chcete-li zúžit rozsah vrácená data, můžete vytvořit kombinace podporované filtry a pole filtru. Například následující příkaz vrátí top 10 záznamy mezi 1. července 2016 a července 2016 6.:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Podporované operátory**: eq, ge, le, gt, lt

**Příklad**:

Pomocí konkrétní datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Použití rozsahu dat.    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Poznámky k**:

Parametr data a času musí být ve formátu UTC 

- - -
### <a name="userid"></a>ID uživatele
**Podporované operátory**: eq

**Příklad**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Poznámky k**:

Hodnota ID uživatele je řetězcová hodnota

- - -
### <a name="userprincipalname"></a>UserPrincipalName
**Podporované operátory**: eq

**Příklad**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Poznámky k**:

Hodnota userPrincipalName je řetězcová hodnota

- - -
### <a name="appid"></a>appId
**Podporované operátory**: eq

**Příklad**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Poznámky k**:

Hodnota appId je řetězcová hodnota

- - -
### <a name="appdisplayname"></a>appDisplayName
**Podporované operátory**: eq

**Příklad**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Poznámky k**:

Hodnota appDisplayName je řetězcová hodnota

- - -
### <a name="loginstatus"></a>loginStatus
**Podporované operátory**: eq

**Příklad**:

    $filter=loginStatus+eq+'1'  


**Poznámky k**:

Existují dvě možnosti pro loginStatus: 0 - Úspěch, 1 – Chyba

- - -
## <a name="next-steps"></a>Další kroky
* Chcete příklady pro filtrovaný přihlašovací aktivity? Podívejte se [ukázky sestavy rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-samples.md).
* Opravdu chcete získat další informace o generování sestav rozhraní API Azure AD? V tématu [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

