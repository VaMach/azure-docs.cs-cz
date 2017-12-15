---
title: "Správa účtů pro správu nouzový přístup ve službě Azure AD | Microsoft Docs"
description: "Popisuje, jak použít účty pro nouzový přístup umožňující organizacím omezit privilegovaný přístup v rámci stávajícího prostředí Azure Active Directory."
services: active-directory
keywords: "Nemáte přidat nebo upravit klíčová slova bez konzultace ohledně specialistou na SEO."
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 75ea8e445c890e7af5ab14f4fc4c53cfb1af4568
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="managing-emergency-access-administrative-accounts-in-azure-ad"></a>Správa účtů pro správu nouzový přístup ve službě Azure AD 

Pro většinu každodenní aktivit nejsou potřeba oprávnění role Globální správce.  Uživatelé by neměl být trvale přiřazeny k roli, uživatel může nechtěně provádět úlohy s vyšší oprávnění, než je třeba. Když uživatel potřebuje tak, aby fungoval jako globální správce, se musí aktivovat přiřazení role pomocí Azure AD PIM, buď na svůj vlastní účet nebo alternativního účtu správce.

Kromě uživatelů s ohledem na přístupová práva pro správu sami muset připravit zajistit, že není získat do situace, kdy se může nechtěně uzamčení správu jejich klienta Azure AD z důvodu jejich nemohou přihlásit zákazníků v nebo aktivovat účet existující konkrétního uživatele jako správce.  Zmírnit jejich dopad náhodných nedostatečná přístup pro správu prostřednictvím ukládání v jejich klienta dvou nebo více *účty pro nouzový přístup*.

Účty pro nouzový přístup pomáhají organizacím omezit privilegovaný přístup v rámci stávajícího prostředí Azure Active Directory. Tyto účty jsou vysoce privilegované a nebyly přiřazeny do konkrétní osoby. Účty pro nouzový přístup je omezen na nouzový 'pohotovostní' scénáře, kdy nejde použít normální účty pro správu.  Organizace musí zajistit za účelem řízení a snižuje využití nouzový účet pouze tento čas, pro které je nezbytné.

Scénáře, ve kterých může být třeba organizace, aby pomocí účtu, nouzový přístup:

 - Uživatelské účty jsou federovaný a federation není k dispozici z důvodu přerušení sítě nebo výpadek zprostředkovatele identity.  Například pokud hostitel poskytovatele identity v prostředí zákazníka je mimo provoz, pak uživatel nebude moci přihlásit, pokud Azure AD přesměruje na jejich zprostředkovatele identity. 
 - Správce se registrovaný prostřednictvím metody vícefaktorového ověřování a všechny jejich jednotlivých zařízení jsou k dispozici.  Je možné, že jsou uživatelé nebudou moct dokončení vícefaktorového ověřování pro aktivaci role, pokud pro instanci výpadku sítě buňky může zabránit uživatelům nebudou moct odpoví na telefonní hovory nebo přijímat textové zprávy a pouze mechanismus, který byl tento jejich registraci jejich zařízení. 
 - Poslední osobě, která měla přístup pro globální správu opustili společnost.  Zabrání se odstranit poslední účet globálního správce Azure AD, ale nezabrání účet před odstraněním nebo zablokuje v místním, vedoucí k situaci, kde se nepodařilo obnovit tento účet organizace.

## <a name="initial-configuration"></a>Počáteční konfigurace

Vytvořte dva nebo více účtů nouzový přístup.  Tady by měly být jen cloudové účty pomocí *. onmicrosoft.com doméně, zda nejsou federovaný nebo synchronizováni z místního prostředí.  

Jejich by neměl být přidružen k jednotlivé uživatele v organizaci.  Organizace muset Ujistěte se, že přihlašovací údaje pro tyto účty jsou uchovány zabezpečení a známé pouze pro jednotlivce, kteří jsou autorizovaní k jejich použití. 

Poznámka: Obvykle heslo pro účet nouzový přístup k účtu jsou rozdělené na dvě nebo tři části a pak zapisují na samostatné části dokumentu a uložená v zabezpečené, chráněné trezory, které jsou v zabezpečené samostatná místa. Ujistěte se, v případě, že jednotlivé zaměstnanec, nedostupný v době jistotu, že vaše účty pro nouzový přístup nejsou připojen k žádné zadané zaměstnanec mobilní telefony, že cesta se jednotlivé zaměstnanci nebo jiné přihlašovací údaje specifické pro zaměstnance tokeny hardwaru je potřeba přihlašovací údaje. 

### <a name="initial-configuration-with-permanent-assignments"></a>Počáteční konfigurace s trvalé přiřazení

Jednou z možností je přiřadit uživatele jako členy rolí globálního správce.  To může být vhodné pro organizace, které nemají Azure AD Premium P2 odběry.

Azure AD doporučuje, že můžete vyžadovat, že vícefaktorové ověřování (MFA) pro všechny jednotlivé uživatele, včetně správců a všichni ostatní uživatelé, kteří by měla mělo výraznější vliv, pokud jejich účet byl ohrožení zabezpečení (například finanční osoby). Tím se snižuje riziko útoků z důvodu ohroženými heslo. Ale pokud vaše organizace nemá sdílená zařízení, potom MFA nemusí být možné pro tyto účty pro nouzový přístup.  Pokud konfigurujete zásady podmíněného přístupu tak, aby vyžadovala [registrace MFA pro každý správce](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) pro Azure AD a další spojené aplikace SaaS, bude pravděpodobně nutné ke konfiguraci zásad vyloučení vyloučit účty pro nouzový přístup z tohoto požadavek.

### <a name="initial-configuration-with-approvals"></a>Počáteční konfigurace s schválení

Další možností je konfigurovat uživatele jako oprávněné a schvalovatelů aktivovat roli globálního správce.  Bude vyžadovat organizaci předplatných Azure AD Premium P2, jakož i MFA možnost, který je vhodný pro použití sdílené mezi více osob a síťové prostředí.  To je proto aktivace roli globálního správce vyžaduje, aby uživatel k dříve provedli vícefaktorového ověřování.  Další informace na [jak vyžadovat vícefaktorové ověřování v Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Použití vícefaktorového ověřování přidružený osobní zařízení není vhodné účty pro nouzový přístup, protože v skutečné nouzový jedna osoba, která musí mít přístup k MFA zaregistrovat zařízení nemusí být k tomu, který není podržíte osobní zařízení.  Pokud z důvodu nepředpokládaného okolností mobilního telefonu nebo jiné sítě nejsou k dispozici průběhu přírodní katastrofě nouzový také zvážit povahu hrozeb.  Proto je důležité zajistit, že jsou všechny registrované zařízení nacházet v známé a bezpečné umístění, které má více prostředky pro komunikaci se službou Azure AD.

## <a name="ongoing-monitoring"></a>Průběžné monitorování

Monitorování [Azure AD přihlášení a protokoly auditu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-sign-ins) pro všechny přihlášení a audit aktivity z účty pro nouzový přístup.  Tyto účty za normálních okolností by neměl být přihlášení a nesmí být provádění změn, tak z nich je pravděpodobně být neobvyklé a vyžadují zabezpečení šetření.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Kontrola ověření účtu se musí vyskytovat v pravidelných intervalech

Alespoň proveďte následující kroky:
 - Každých 90 dnech
 - Pokud došlo ke změnám v IT oddělení – úlohu změnit, odeslání nebo nové přijetím
 - Pokud se změnily předplatná Azure AD v organizaci

K zajištění, že zaměstnanci probíhá Trénink v tom, jak použít účty pro nouzový přístup:

1.  Zajistěte, aby pracovníci monitorování zabezpečení by se měl dozvědět, probíhá kontrola aktivity účtu.
2.  Ověření, cloudu uživatelské účty můžete přihlásit a aktivace jejich rolí a že uživatelé potřebují k provedení těchto kroků nouzová probíhá trénink na proces.
3.  Ujistěte se, že nebyly zaregistrovány pro MFA nebo SSPR všechny jednotlivé uživatele zařízení nebo osobní údaje.  
4. Pokud účty je zaregistrovaný pro MFA zařízení, pro použití při aktivaci role, zkontrolujte, zda zařízení přístupné všem správcům, kteří možná muset použít nouzová.  Také ověřte, že je zařízení zaregistrované prostřednictvím alespoň dva mechanismů, které nesdílí běžné režimu selhání (např. zařízení může komunikovat k Internetu prostřednictvím bezdrátové sítě zařízení i přes síť zprostředkovatele buněk).
5.  Aktualizujte přihlašovací údaje.

## <a name="next-steps"></a>Další kroky
- [Přidání cloudových uživatelů](add-users-azure-active-directory.md) a [přiřadit roli globálního správce nového uživatele](active-directory-users-assign-role-azure-portal.md)
- [Registrace pro Azure Active Directory Premium](active-directory-get-started-premium.md), pokud jste to ještě neudělali
- [Vyžadovala Azure MFA pro jednotlivé uživatele přiřazené jako správci](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states)
- [Konfiguraci lepší ochranu globálních správců v Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), pokud používáte Office 365
- [Provést kontrolu přístupu globální správci](active-directory-privileged-identity-management-how-to-start-security-review.md) a [přechod existující globální správci pro více konkrétní role správce](active-directory-assign-admin-roles-azure-portal.md)

