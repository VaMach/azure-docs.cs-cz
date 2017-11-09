---
title: "Co je nového? Poznámky k verzi pro Azure Active Directory | Microsoft Docs"
description: "Zjistěte, co je nového v Azure Active Directory (Azure AD) včetně nejnovější poznámky k verzi, známé problémy, opravy chyb, zastaralé funkce a nadcházející změny."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 766ab10c853eb56a1203686d21d0e131c44a41b5
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Co je nového ve službě Azure Active Directory?




> Vždy aktuální s co je nového ve službě Azure Active Directory se přihlásíte k tomuto odběru [kanálu](https://docs.microsoft.com/api/search/rss?search=%22what%27s%20new%20in%20azure%20active%20directory%3F%22&locale=en-us) ve své oblíbené čtečky informačního kanálu RSS.



Budeme průběžně zlepšují Azure Active Directory. Aby vám nedovolí zůstat aktuální pomocí nejnovější Toto téma poskytuje informace o:

-   Nejnovější verze 
-   Známé problémy 
-   Opravy chyb 
-   Zastaralé funkce 
-   Plány pro změny 

Pokroku prosím tuto stránku pravidelně aktualizujeme ho měsíčně.

## <a name="november-2017"></a>2017 listopadu

**Typ:** zastaralé funkce  
**Kategorie služby:** služby ACS  
**Schopnosti produktu:** služby Řízení přístupu 

<a name="acs-retirement"></a>

Microsoft Azure Active Directory řízení přístupu (také označované jako služby Řízení přístupu nebo služby ACS) se postupně vyřazuje z provozu v pozdní 2018.  Další informace, včetně prováděcí plán & vysoké úrovně migrace pokyny, budou uvedené v další trvá několik týdnů. Do té doby, nechejte prosím komentáře na této stránce s dotazy týkající se služby ACS a bude členem náš tým oslovení pomohou odpovědí.

---


## <a name="october-2017"></a>Říjen 2017

**Typ:** Plánovaná změna  
**Kategorie služby:** vytváření sestav  
**Schopnosti produktu:** Identity – Správa životního cyklu  


**Místo začne sestav Azure AD (beta verze) rozhraní API v části `https://graph.windows.net/<tenant-name>/reports/` uzlu**

Portál Azure nabízí:

- Nové konzole pro správu Azure Active Directory 
- Nová rozhraní API pro sestavy aktivity a zabezpečení
 
Z důvodu tyto nové funkce, sestava rozhraní API v části **/reports** vyřadí koncový bod na 10. prosince 2017. 

---

**Typ:** pevné   
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  


Azure Active Directory podporuje zjišťování pole automatické přihlašování pro aplikace, které vykreslení pole HTML uživatelské jméno a heslo.  Tyto kroky jsou dokumentovány v článku [automaticky zaznamenání pole přihlášení pro aplikaci](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Této funkci můžete najít tak, že přidáte *Non-Galerie* aplikace na **podnikové aplikace, které** stránku [portál Azure](http://aad.portal.azure.com). Kromě toho můžete nakonfigurovat **jednotné přihlašování** režimu do této nové aplikace **založené na heslech jednotné přihlašování**zadáním adresy URL webového a pak ukládání této stránky.
 
Z důvodu problém se službou byl tuto funkci dočasně zakázán pro určitou dobu. Tento problém byl vyřešen a detekce pole Automatické přihlášení opět k dispozici.

---

**Typ:** nová funkce  
**Kategorie služby:** MFA  
**Schopnosti produktu:** Identity zabezpečení a ochrany  


Vícefaktorové ověřování (MFA) na světě, které jsme za provozu v je nedílnou součást vámi ochrany vaší organizace. Týmem identity v Microsoft je vyvíjející se vícefaktorové ověřování, aby plynulejší více adaptivní přihlašovací údaje a zkušenosti. Dnes souhlasím s radostí oznamujeme dva důležité kroky v této cesty: 

- Integrace Multi-Factor výzvy výsledků přímo do Azure AD přihlášení sestavy, včetně programový přístup k výsledkům vícefaktorového ověřování

- Hlubší integrace MFA konfigurace do základní konfigurace Azure AD na základě zkušeností uživatelů na portálu Azure

Pomocí této verzi public preview správu MFA a vytváření sestav jsou integrované součástí základní konfigurace prostředí Azure AD, umožňuje spravovat funkce portálu správy vícefaktorového ověřování v rámci prostředí Azure AD.

Další informace najdete v tématu [odkaz pro vytváření sestav v portálu Azure Multi-Factor authentication](active-directory-reporting-activity-sign-ins-mfa.md) 


---
**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení  


**Azure AD podmínky použití** vám poskytne jednoduchý způsob na informace pro koncové uživatele. Tím se zajistí, že uživatelé uvidí relevantní omezení pro požadavky právních nebo dodržování předpisů.

Podmínky použití služby Azure AD můžete použít v následujících scénářích:

- Obecné podmínky použití pro všechny uživatele ve vaší organizaci. 

- Konkrétní podmínky použití na základě atributů uživatele (např. lékařů vs sester nebo mezinárodní zaměstnanci domácí vs, provádí dynamických skupin). 

- Konkrétní podmínky použití pro přístup k vysoký obchodní dopad aplikace, jako například služby Salesforce.

Další informace najdete v tématu [Azure Active Directory podmínky použití](active-directory-tou.md).


---
**Typ:** nová funkce  
**Kategorie služby:** PIM  
**Schopnosti produktu:** Privileged Identity Management  


S Azure Active Directory Privileged Identity Management (PIM), teď můžete spravovat, řízení a monitorování přístupu k prostředkům Azure (Preview) v rámci vaší organizace. To zahrnuje odběry, skupiny prostředků a i virtuálních počítačů. Všechny prostředky v rámci portálu Azure, které využívají funkci Azure na základě řízení přístupu Role (RBAC) můžete využít výhod všech skvělé zabezpečení a možnosti správy životního cyklu, které má Azure AD PIM nabízet a některé skvělé nové funkce plánujeme uvede Azure AD role brzy k dispozici.

Další informace najdete v tématu [PIM pro prostředky Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Typ:** nová funkce  
**Kategorie služby:** přístup recenze  
**Schopnosti produktu:** zásad správného řízení  


Organizacím efektivní Správa členství ve skupinách a přístup k podnikové aplikace, které umožňují přístup recenze (preview): 

- Můžete znovu potvrzení přístupu uživatele guest pomocí přístupu recenze jejich přístupu k aplikacím a členství ve skupinách. Statistika poskytované recenze přístupu povolit kontroloři efektivně rozhodnout, zda hosté by měl neustálý přístup.

- Pomocí kontrol přístupu je možné provést opětovnou certifikaci přístupu k aplikacím a členství ve skupinách i pro zaměstnance.

Kontroly přístupu lze shromažďovat v programech sledujících dodržování předpisů a přístup k citlivým aplikacím v rámci organizace.

Další informace najdete v tématu [zkontroluje přístup k Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Typ:** nová funkce  
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  


**Schopnost skrýt aplikací jiných výrobců z Moje aplikace a Spouštěče Office 365**

Teď můžete lépe spravovat aplikace, které se zobrazí na vaší portálech uživatele prostřednictvím novou **skrýt aplikace** vlastnost. To pomáhá s případy, kdy dlaždice aplikace se zobrazí pouze u služeb back-end nebo duplicitní dlaždice a skončili zbytečného Spouštěč oken aplikace uživatele. Přepínač je umístěn v části Vlastnosti aplikace třetích stran a je popsán **viditelný pro uživatele?**. Můžete také skrýt aplikaci programově pomocí prostředí PowerShell. 

Další informace najdete v tématu [skrýt aplikace třetích stran z možnosti pro uživatele v Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Co je k dispozici?**

 V rámci přechodu na novou konzolu pro správu jsme provedli 2 nových rozhraní API pro načítání protokoly aktivity Azure AD, které jsou k dispozici. Novou sadu rozhraní API zadejte bohatší filtrování a řazení funkce kromě bohatší auditu a přihlašovací aktivity. Dříve k dispozici prostřednictvím sestavy zabezpečení dat je nyní přístupná prostřednictvím Identity Protection rizikových událostech rozhraní API v aplikaci Microsoft Graph.


## <a name="september-2017"></a>2017 září

**Typ:** změněné funkce  
**Kategorie služby:** Microsoft Identity Manager  
**Schopnosti produktu:** Identity – Správa životního cyklu  


Balíčku kumulativní opravy hotfix (sestavení 4.4.1642.0) je k dispozici od 25 září 2017 pro Microsoft Identity Manager (MIM) 2016 2016 Service Pack 1 (SP1). Tento balíček aktualizací:

- Řeší problémy a přidá vylepšení
- Je kumulativní aktualizace, který nahrazuje všechny aktualizace MIM 2016 SP1 až 4.4.1459.0 sestavení pro Microsoft Identity Manager 2016. 
- Vyžaduje, abyste měli **4.4.1302.0 sestavení Microsoft Identity Manager 2016.** 

Další informace najdete v tématu [balíčku kumulativní opravy Hotfix (sestavení 4.4.1642.0) je k dispozici pro Microsoft Identity Manager 2016 SP1](https://support.microsoft.com/en-us/help/4021562). 

---
