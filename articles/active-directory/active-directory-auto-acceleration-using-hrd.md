---
title: "Konfigurace přihlášení automaticky-akcelerace pro aplikaci pomocí Domů zásad zjišťování sféry | Microsoft Docs"
description: "Vysvětluje, co je to klient služby Azure AD a jak spravovat službu Azure pomocí služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-using-home-realm-discovery-hrd-policy"></a>Konfigurace přihlášení automaticky zrychlení pro aplikace pomocí zásad zjišťování domovské sféry

## <a name="introduction-to-home-realm-discovery-and-auto-acceleration"></a>Úvod do zjišťování domovské sféry a automaticky akcelerace
Následující dokument obsahuje úvodní informace o zjišťování domovské sféry a automaticky akcelerace.

### <a name="home-realm-discovery"></a>Zjišťování domovské sféry
Zjišťování domovské sféry je proces, který umožňuje Azure Active Directory k určení během přihlášení, kde uživatel potřebuje k ověření.  Při přihlašování k klient služby Azure AD pro přístup k prostředku nebo Azure AD běžné přihlašovací stránky, uživatel zadá uživatelské jméno (UPN).  Azure AD, využívá k vyhledávání, kde uživatel potřebuje k přihlášení.   Uživatel může třeba přesměrováni na jednu z následujících ověření:

- Domácí klient uživatele (může být stejném tenantovi jako prostředek uživatel se pokouší o přístup k). 
- Účet Microsoft.   Uživatel je hostovaný v klientovi prostředků
- Jiného poskytovatele Identity Federovaná pomocí klienta Azure AD.  Zprostředkovatele identity místní například AD FS pro instanci.

### <a name="auto-acceleration"></a>Akcelerace automaticky 
Některé organizace nakonfigurovat jejich klienta Azure Active Directory vytvořit federaci s jinou IdP, například služby AD FS pro ověření uživatele.  V těchto případech po přihlášení do aplikace zobrazí se nejprve uživateli s přihlašovací stránku služby Azure AD a jakmile se jejich UPN, pak jsou přesměrováni na přihlašovací stránce IdP zadali.  V případech, kde má smysl správci chtít mít směrován přímo na stránku přihlášení po přihlášení na konkrétní aplikace, uživatelé přeskočení úvodní stránky Azure Active Directory. To se označuje jako "přihlášení automaticky zrychlení".

V případech, kde je federovaného klienta k jiné poskytovatelů identity pro přihlášení povolení automatického akcelerace díky uživatele přihlásit více zjednodušený v případech, pokud víte, že všichni přihlášení může být ověřen pomocí tohoto IdP.  Můžete nakonfigurovat automatické akcelerace pro jednotlivé aplikace.

>[!NOTE]
>Pokud nakonfigurujete aplikaci pro automatické akcelerace, uživatele typu Host nelze se přihlásit. Pořízení uživatele přímo k federované poskytovatelů identity pro ověřování je jednosměrný ulici, protože neexistuje žádný způsob, jak se vrátit na stránku pro přihlášení k Azure Active Directory.  Host uživatele, kteří mohou potřebovat přesměrováni na jiných klientů nebo externí IdP jako účet Microsoft k ověření, nebudou moct přihlásit k dané aplikaci, jako je přeskočen krok zjišťování domovské sféry.  

Existují dva způsoby, jak řídit automaticky akcelerace k federované IdP.  Buď:   

- Použijte pomocný parametr domény na žádosti o ověření pro aplikaci 
- Nakonfigurujte zásady HomeRealmDiscovery k povolení automaticky akcelerace

## <a name="domain-hints"></a>Pomocné parametry domény 
Pomocné parametry domény jsou direktivy obsažené v žádosti o ověření z aplikace.  Slouží k urychlení uživatele na jejich federované IdP přihlašovací stránku nebo můžete používají víceklientské aplikace pro urychlení uživatele rovnou na partnerské Azure AD přihlašovací stránka pro svého tenanta.  Například largeapp.com aplikace může povolit zákazníkům pro přístup k aplikaci na vlastní adresu URL contoso.largeapp.com a může zahrnovat nápovědu domény Contoso.com v žádosti o ověření. Pomocný parametr syntaxi domény se liší v závislosti na protokol používá a se obvykle konfigurují v aplikaci.

**WS-Federation**: whr=contoso.com v řetězci dotazu

**SAML**: buď SAML ověřování žádost obsahující nápovědu domény nebo whr=contoso.com řetězec dotazu

**Otevřete připojení ID**: domain_hint=contoso.com řetězec dotazu 

Pokud nápovědu domény je zahrnuta v žádosti o ověření z aplikace a klienta je Federovaná pomocí dané domény, pokusí se Azure AD přihlášení přesměrovat na rozšíření IdP nakonfigurován pro tuto doménu.  Pokud pomocný parametr domény neodkazuje na ověřené federované domény, je ignorováno a zjišťování domovské sféry normální volána.

Toto [příspěvku na blogu](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/) Další informace o automatické zrychlení pomocí Rady k doméně podporované službou Azure Active Directory.

>[!NOTE]
>Pokud nápovědu domény je součástí požadavku na ověření jeho přítomnosti přepíše jakékoli HRD zásady, které jsou nastaveny pro aplikaci.

## <a name="home-realm-discovery-hrd-policy"></a>Zásady zjišťování domovské sféry
Některé aplikace neposkytují způsob, jak nakonfigurovat, které budou emitování žádost o ověření a v těchto případech není možné použít k řízení automaticky akcelerace pomocné parametry domény.   Auto akcelerace je možné nakonfigurovat přes zásadu, abyste dosáhli stejné chování.  

### <a name="setting-hrd-policy"></a>Nastavení zásad HRD
Existují tři kroky nastavení přihlášení automaticky akcelerace na aplikace


1. Vytváření zásad HRD pro automatické akcelerace
2. Vyhledání Princip služby, ke kterému chcete připojit zásady
3. Zásady se připojuje k Princip služby.  Zásady pravděpodobně byla vytvořena v klientovi, ale nemají nijak neprojeví, dokud jsou připojené k entitě.  Zásadu HRD může být připojen k objektu služby a v jednom okamžiku může být pouze jedna zásada HRD aktivní na danou entitu.  

Microsoft Azure Active Directory Graph API přímo nebo rutin prostředí PowerShell Azure Active Directory můžete použít k nastavení akcelerace automaticky pomocí zásad HRD

Rozhraní Graph API, které pracují se zásady je popsáno [zde](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations).

Tady je příklad HRD definice zásady:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

Typ zásad se "HomeRealmDiscoveryPolicy".

Pokud **AccelerateToFederatedDomain** je hodnota false, pak zásada nemá žádný vliv **PreferredDomain** by měl být uveden urychlit do domény a můžete tento parametr vynechán, v případě, že klient má jenom jeden federované domény.  Pokud je vynechán a je více než jedna ověřená, federovanou doménu, zásada nemá žádný vliv.

Pokud **PreferredDomain** je zadán, musí se shodovat ověřené, federované domény pro klienta, a všichni uživatelé v aplikace musí být schopen Přihlaste se na tuto doménu.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioritu a vyhodnocení zásad HRD
HRD zásady můžete vytvořit a posléze přiřazeny k určité organizace a objekty služby. To znamená, že je možné pro víc zásad, které chcete použít pro konkrétní aplikaci. Zásady HRD, projeví se řídí následujícími pravidly:


- Pokud je k dispozici v žádosti o ověření domény nápovědu, je ignorováno jakékoli zásady připojení a chování zadaném v pomocném parametru domény se používá.
- Jinak Pokud zásady je explicitně přiřazen k objektu služby, vynuceno. 
- Pokud je pomocný parametr žádné domény a objekt služby je explicitně přiřazeny žádné zásady, je vyžadována zásadu explicitně přiřazená k organizaci nadřazeného objektu služby. 
- Pokud neexistuje žádné pomocný parametr domény a byla přiřazena žádná zásada instanční objekt nebo organizace, použije se výchozí chování HRD.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-using-hrd-policy-with-azure-active-directory-powershell-cmdlets"></a>Kurz pro nastavení přihlášení automaticky akcelerace aplikace pomocí zásad HRD pomocí rutin Powershellu pro Azure Active Directory
Projdeme několik scénářů, včetně:


- Nastavení akcelerace automaticky pro aplikaci pro klienta s jednou doménou federované
- Nastavení akcelerace automaticky pro aplikaci na jednu z několika domén, které je ověřeno pro vašeho klienta
- Výpis aplikace, pro které je nakonfigurované zásady

### <a name="prerequisites"></a>Požadavky
V následujících příkladech vytvářet, aktualizovat, propojení a odstranit zásady na objekty služby aplikace ve službě Azure AD.

1.  Pokud chcete začít, stáhněte si nejnovější verze Preview rutiny Azure AD PowerShell. 
2.  Jakmile máte rutiny Azure AD PowerShell, spusťte příkaz Connect pro přihlášení do Azure AD pomocí vašeho účtu správce.

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Spusťte následující příkaz zobrazíte všechny zásady ve vaší organizaci.

    ``` powershell
    Get-AzureADPolicy
    ```

Když se nic nevrátí, nemáte žádné zásady, vytvořené v klientovi

### <a name="example-setting-auto-acceleration-for-an-application"></a>Příklad: Nastavení automatického akcelerace pro aplikaci 
V tomto příkladu vytvoříte zásadu, která automaticky zrychluje uživatele na přihlašovací obrazovku AD FS při přihlášení k aplikaci.  To se provádí bez nich museli nejprve zadejte uživatelské jméno na přihlašovací stránce služby Azure AD. 

#### <a name="step-1-create-an-hrd-policy"></a>Krok 1: Vytvoření zásady HRD
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Pokud máte jeden federované domény, který ověřuje uživatele pro aplikace, stačí vytvořit HRD zásad.  
Pokud chcete zobrazit nové zásady a získat jeho ObjectID, spusťte následující příkaz.

``` powershell
Get-AzureADPolicy
```


Až budete mít zásadu HRD, chcete-li povolit automatické zrychlení, můžete ho přiřadit více zásadami služby aplikací.

#### <a name="step-2-locate-the-service-principal-to-assign-the-policy-to"></a>Krok 2: Vyhledejte hlavní název služby přiřadit zásady.  
Je nutné ObjectId objekty služby, které chcete přiřadit zásady. Existuje několik způsobů, jak zjistit ID objektu objekty služby.    

Můžete použít na portálu, můžete zadat dotaz [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) nebo přejděte na našich [nástroji Průzkumník grafu](https://graphexplorer.cloudapp.net/) a přihlásit ke svému účtu Azure AD zobrazíte všechna firemní objekty služby, nebo protože používáte prostředí PowerShell, můžete použít rutinu get-AzureADServicePrincipal seznam zásad služby a jejich ID.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Krok 3: Přiřaďte zásady instančního objektu  
Jakmile máte ObjectId instanční objekt, který chcete provést konfiguraci automatického akcelerace pro aplikace, spusťte následující příkaz k přidružení zásady HRD, že kterou jste vytvořili v kroku 1 s instanční objekt, který jste vyhledali v kroku 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Tento příkaz můžete opakovat pro každý objekt služby chcete přidat zásadu.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Krok 4: Zkontrolujte, jaké objekty služby aplikace vaší automaticky akcelerace zásady přiřazeny ke
Ke kontrole aplikací, které mají automaticky akcelerace zásady nakonfigurované použijte rutinu Get-AzureADPolicyAppliedObject a předejte jí objectId zásad, které chcete zkontrolovat.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Krok 5: Hotovo!
Zkuste aplikace a zjistit, zda je funkční nové zásady.

### <a name="example-listing-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Příklad: Výpis aplikace, pro které je nakonfigurované zásady protokolu auto akcelerace

#### <a name="step-1-list-all-policies-created-in-your-organization"></a>Krok 1: Seznamu všechny zásady vytvořené ve vaší organizaci 

``` powershell
Get-AzureADPolicy
```

Poznámka: **ID objektu** chcete seznamu přiřazení pro zásady.

#### <a name="step-2-list-the-service-principals-the-policy-is-assigned-to"></a>Krok 2: Seznam objekty služby, přiřazené zásady.  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-removing-an-auto-acceleration-policy-for-an-application"></a>Příklad: Odebrání zásad protokolu auto akcelerace pro aplikace
#### <a name="step-1-use-the-previous-example-to-get-the-objectid-of-the-policy-and-that-of-the-application-service-principal-you-wish-to-remove-it-from"></a>Krok 1: Použití předchozí příklad ObjectId zásad, a u objektu aplikace služby, které chcete odebrat z
#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Krok 2: Odebrání přiřazení zásady instančního aplikace.  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-the-policy-is-assigned-to"></a>Krok 3: Kontrola odebrání tak, že uvedete objekty služby zásady přiřazeny ke 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Další kroky
- Další informace o tom, jak funguje ověřování ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD](develop/active-directory-authentication-scenarios.md).
- Další informace o uživatele jednotné přihlašování najdete v části [přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-enterprise-apps-manage-sso.md)
- Přejděte [Příručka pro vývojáře Active Directory](develop/active-directory-developers-guide.md) přehled veškerý obsah pro vývojáře.
