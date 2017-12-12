---
title: "Co je nového? Poznámky k verzi pro Azure Active Directory | Microsoft Docs"
description: "Zjistěte, co je nového v Azure Active Directory (Azure AD) včetně nejnovější poznámky k verzi, známé problémy, opravy chyb, zastaralé funkce a nadcházející změny."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9e2186839648ffc6492cd64cb140cc767729d189
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
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
 
### <a name="retiring-acs"></a>Vyřazení služby ACS



**Typ:** Plánovaná změna  
**Kategorie služby:** služby ACS  
**Schopnosti produktu:** služby Řízení přístupu 


Pozdní 2018 vyřadí Microsoft Azure Active Directory řízení přístupu (také označované jako služby Řízení přístupu nebo služby ACS).  Další informace, včetně prováděcí plán & vysoké úrovně migrace pokyny, bude k dispozici v další trvá několik týdnů. Do té doby, nechte komentáře na této stránce s dotazy týkající se služby ACS a členem náš tým pomůže odpovědět.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Omezit přístup z prohlížeče na spravovaný prohlížeč Intune 


**Typ:** Plánovaná změna  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrany




Toto chování budou moci omezit přístup pro prohlížeč k Office 365 a jiným aplikacím cloudu Azure AD připojené jako schválené aplikace pomocí Intune Managed Browser. 

Tato změna umožňuje nakonfigurovat následující podmínka pro podmíněný přístup využívající aplikace:

**Klientské aplikace:** prohlížeče

**Co je vliv změny?**

V současné době je blokován přístup při použití této podmínky. Pokud verze preview toto chování je k dispozici, veškerý přístup bude vyžadovat použití aplikace spravovaného prohlížeče. 

Podívejte se pro tuto funkci a více v nadcházející poznámky blogy a verzi. 

Další informace najdete v tématu [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nové schválené klientské aplikace pro podmíněný přístup využívající aplikaci Azure AD

 
**Typ:** Plánovaná změna  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrany




Následující aplikace plánujeme přidat do seznamu [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)

- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Další informace naleznete v tématu:

- [Požadavek schválené klientské aplikace](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory, na základě aplikace podmíněného přístupu](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Podmínky použití podporu více jazyků



**Typ:** nová funkce    
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení nebo dodržování předpisů





Správci teď můžete vytvářet nové podmínky použití (TOU), který obsahuje více dokumentů PDF. Můžete označit tyto dokumenty PDF s příslušný jazyk. Uživatelům, které spadají v oboru se zobrazí PDF s odpovídající jazyk na základě jejich předvoleb. Pokud není nalezena žádná shoda, zobrazí se výchozí jazyk.


---
 

### <a name="realtime-password-writeback-client-status"></a>Stav klienta zpětného zápisu hesla v reálném čase



**Typ:** nová funkce  
**Kategorie služby:** SSPR  
**Schopnosti produktu:** ověření uživatele


 

Nyní můžete zkontrolovat stav vašeho klienta zpětný zápis hesla na místě. Tato možnost je dostupná v **integrace v místním** části  **[resetování hesla](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)**  stránky. 

Pokud nedochází k potížím s připojením k klientovi zpětného zápisu na místě, zobrazí se chybová zpráva, která nabízí:

- Informace o Proč se nemůžete připojit k vaší místní zpětného zápisu klienta 
- Odkaz na dokumentaci, který vám pomůže při řešení problému. 


Další informace najdete v tématu [integrace v místním](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD, na základě aplikace podmíněného přístupu 



 
**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** Identity zabezpečení a ochrany





Teď můžete omezit přístup k Office 365 a jiným aplikacím cloudu Azure AD připojené k [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) podporující zásady ochrany aplikace Intune pomocí [podmíněného přístupu na základě aplikace Azure AD](active-directory-conditional-access-mam.md). Zásady ochrany aplikací Intune se používají ke konfiguraci a ochraně dat společnosti na tyto klientské aplikace.

Tím, že zkombinujete [na základě aplikace](active-directory-conditional-access-mam.md) s [na zařízení](active-directory-conditional-access-policy-connected-applications.md) zásady podmíněného přístupu, máte možnost k ochraně dat pro osobní a firemní zařízení.

Následující podmínky a ovládací prvky jsou nyní k dispozici pro použití s podmíněného přístupu na základě aplikace:

**Podmínka podporované platformy**

- iOS
- Android

**Stav aplikace klienta**

- Mobilní aplikace a klienti vzdálené plochy

**Řízení přístupu**

- Vyžadovat schválené klientské aplikace


Další informace najdete v tématu [podmíněného přístupu na základě aplikace Azure Active Directory](active-directory-conditional-access-mam.md).

 
---

### <a name="managing-azure-ad-devices-in-the-azure-portal"></a>Správa zařízení služby Azure AD na portálu Azure



**Typ:** nová funkce  
**Kategorie služby:** registrace zařízení a správy  
**Schopnosti produktu:** Identity zabezpečení a ochrany

 



Teď můžete získat všechna vaše zařízení připojená k Azure AD a aktivity související s zařízení na jednom místě. Je nové prostředí pro správu, ke správě identit zařízení a nastavení na portálu Azure. V této verzi můžete:

- Zobrazit všechna zařízení, které jsou k dispozici pro podmíněný přístup v Azure AD

- Zařízení připojená k zobrazení vlastností, včetně vašeho hybridní Azure AD

- Najít klíče nástroje BitLocker pro zařízení připojená k AD Azure, spravovat vaše zařízení s Intune a další.

- Správa nastavení zařízení související služby Azure AD


Další informace najdete v tématu [Správa zařízení pomocí webu Azure portal](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-device-platform-for-azure-ad-conditional-access"></a>Podpora systému macOS jako platforma pro podmíněný přístup k Azure AD 



**Typ:** nová funkce    
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrany 
 

Můžete teď zahrnout (nebo vyloučit) systému macOS jako podmínku platformy zařízení v Azure AD zásady podmíněného přístupu. Přidání systému macOS na platformách podporovaných zařízení můžete:

- **Registrovat a spravovat pomocí Intune zařízení systému macOS** – podobně jako u jiných platformách, jako je iOS a Android, aplikace portálu společnosti je k dispozici pro systému macOS provést jednotné registrace. Nová aplikace portálu společnosti pro systému macOS umožňuje zapsat zařízení s Intune a zaregistrovat ji pomocí služby Azure AD.
 
- **Ujistěte se, systému macOS zařízení splňovat zásady dodržování předpisů vaší organizace v Intune definované** -v v Intune na portálu Azure, můžete nyní nastavit zásady dodržování předpisů pro zařízení v systému macOS. 
  
- **Omezení přístupu k aplikacím v Azure AD jenom na zařízení kompatibilní s systému macOS** – vytváření zásad podmíněného přístupu má systému macOS jako možnost platformy samostatné zařízení. Tato možnost umožňuje vytvářet zásady podmíněného přístupu konkrétním systému macOS pro sadu cílové aplikace v Azure.

Další informace naleznete v tématu:

- [Vytvořte zásady dodržování předpisů zařízení pro zařízení v systému macOS s Intune](https://aka.ms/macoscompliancepolicy)
- [Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="nps-extension-for-azure-mfa"></a>Server NPS rozšíření pro Azure MFA 


**Typ:** nová funkce    
**Kategorie služby:** MFA  
**Schopnosti produktu:** ověření uživatele




Rozšíření serveru NPS (Network Policy Server) pro Azure MFA přidá možnosti vícefaktorového ověřování založená na cloudu k infrastruktuře ověřování pomocí existujících serverů. Rozšíření serveru NPS můžete přidat telefonní hovor, textová zpráva nebo ověření telefonu v aplikaci k vaší stávající tok ověřování bez nutnosti instalovat, konfigurovat a spravovat nové servery. 

Toto rozšíření byla vytvořena pro organizace, které chcete chránit připojení k síti VPN bez nasazení Azure MFA serveru. Rozšíření serveru NPS slouží jako adaptér mezi RADIUS a cloudu Azure MFA pro poskytování druhý faktor ověřování pro federované nebo synchronizovaných uživatelů.


Další informace najdete v tématu [vaší stávající infrastruktury pro server NPS integrovat Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Obnovit nebo trvale odebrat odstranění uživatelé


**Typ:** nová funkce    
**Kategorie služby:** Správa uživatelů  
**Schopnosti produktu:** adresáře 



V Centru správy služby Azure AD můžete provádět následující akce:

- Obnovení odstraněného uživatele 
- Trvale odstranit uživatele 


**Chcete-li vyzkoušejte ji:**

1. V Centru pro správu Azure AD, vyberte [ **všichni uživatelé** ](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) v **spravovat** části. 

2. Z **zobrazit** seznamu, vyberte **uživatelé v poslední době odstraňovali**. 

4. Vyberte jeden nebo více odstraněných uživatelů a pak je buď obnovit nebo trvale odstranit.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nové schválené klientské aplikace pro podmíněný přístup využívající aplikaci Azure AD

 
**Typ:** změněné funkce  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrany


Následující aplikace byly přidány do seznamu [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Plánovač Microsoft

- Microsoft Azure Information Protection 


Další informace naleznete v tématu:

- [Požadavek schválené klientské aplikace](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory, na základě aplikace podmíněného přístupu](active-directory-conditional-access-mam.md)


---

### <a name="ability-to-or-between-controls-in-a-conditional-access-policy"></a>Možnost pro "nebo" mezi ovládacími prvky v zásadách podmíněného přístupu 


**Typ:** změněné funkce    
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrany

 
Možnost "Nebo" (vyžadují jeden z vybraných ovládacích prvků) byla uvolněna řízení podmíněného přístupu. Tato funkce umožňuje vytvářet zásady s **nebo** mezi řízení přístupu. Například tuto funkci můžete vytvořit zásadu, která vyžaduje uživatele k přihlášení pomocí služby Multi-Factor authentication **nebo** na vyhovující zařízení.

Další informace najdete v tématu [ovládacích prvků v Azure Active Directory podmíněného přístupu](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-realtime-risk-events"></a>Agregace rizikových událostí v reálném čase


**Typ:** změněné funkce    
**Kategorie služby:** ochranu Identity  
**Schopnosti produktu:** Identity zabezpečení a ochrany


K vylepšení prostředí pro správu, v Azure AD Identity Protection, se teď všechny události riziko v reálném čase, které byly pocházely ze stejné IP adresy v daný den agregován pro každý typ události riziko. Tato změna omezuje objem událostí riziko zobrazí bez jakékoli změny v zabezpečení uživatelů.

Základní zjišťování v reálném čase funguje pokaždé, když uživatel se přihlásí. Pokud máte instalační program přihlášení riziko zabezpečení zásad na MFA nebo blokování přístupu, je stále aktivuje během každé rizikové přihlášení.

 
---
 




## <a name="october-2017"></a>Říjen 2017


### <a name="deprecating-azure-ad-reports"></a>Místo začne sestav Azure AD


**Typ:** Plánovaná změna  
**Kategorie služby:** vytváření sestav  
**Schopnosti produktu:** Identity – Správa životního cyklu  



Portál Azure nabízí:

- Nové konzole pro správu Azure Active Directory 
- Nová rozhraní API pro sestavy aktivity a zabezpečení
 
Z důvodu tyto nové funkce, sestava rozhraní API v části **/reports** vyřadí koncový bod na 10. prosince 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detekce pole Automatické přihlášení


**Typ:** pevné   
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  



Azure Active Directory podporuje zjišťování pole automatické přihlašování pro aplikace, které vykreslení pole HTML uživatelské jméno a heslo.  Tyto kroky jsou dokumentovány v článku [automaticky zaznamenání pole přihlášení pro aplikaci](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Této funkci můžete najít tak, že přidáte *Non-Galerie* aplikace na **podnikové aplikace, které** stránku [portál Azure](http://aad.portal.azure.com). Kromě toho můžete nakonfigurovat **jednotné přihlašování** režimu do této nové aplikace **založené na heslech jednotné přihlašování**zadáním adresy URL webového a pak ukládání této stránky.
 
Z důvodu problém se službou byl tuto funkci dočasně zakázán pro určitou dobu. Tento problém byl vyřešen a detekce pole Automatické přihlášení opět k dispozici.

---

### <a name="new-mfa-features"></a>Nové funkce vícefaktorového ověřování


**Typ:** nová funkce  
**Kategorie služby:** MFA  
**Schopnosti produktu:** Identity zabezpečení a ochrany  



Vícefaktorové ověřování (MFA), je zásadní součástí ochrany vaší organizace. Chcete-li více adaptivní přihlašovací údaje a zkušenosti pohodlnější způsob jednotného, byly přidány následující funkce: 

- Integrace Multi-Factor výzvy výsledků přímo do Azure AD přihlášení sestavy, včetně programový přístup k výsledkům vícefaktorového ověřování

- Hlubší integrace MFA konfigurace do konfigurace Azure AD na základě zkušeností uživatelů na portálu Azure

Pomocí této verzi public preview správu MFA a vytváření sestav jsou integrovanou součástí prostředí základní konfigurace Azure AD. Agregování obě funkce umožňuje spravovat funkce portálu správy vícefaktorového ověřování v rámci prostředí Azure AD.

Další informace najdete v tématu [odkaz pro vytváření sestav v portálu Azure Multi-Factor authentication](active-directory-reporting-activity-sign-ins-mfa.md) 


---

### <a name="introducing-terms-of-use"></a>Představení podmínky použití



**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení  



Azure AD podmínky použití poskytují jednoduše na informace pro koncové uživatele. Tím se zajistí, že uživatelé uvidí relevantní omezení pro požadavky právních nebo dodržování předpisů.

Podmínky použití služby Azure AD můžete použít v následujících scénářích:

- Obecné podmínky použití pro všechny uživatele ve vaší organizaci. 

- Konkrétní podmínky použití na základě atributů uživatele (např. lékařů vs sester nebo mezinárodní zaměstnanci domácí vs, provádí dynamických skupin). 

- Konkrétní podmínky použití pro přístup k vysoký obchodní dopad aplikace, jako například služby Salesforce.

Další informace najdete v tématu [Azure Active Directory podmínky použití](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Vylepšení Správa privilegovaných identit


**Typ:** nová funkce  
**Kategorie služby:** PIM  
**Schopnosti produktu:** Privileged Identity Management  


S Azure Active Directory Privileged Identity Management (PIM), teď můžete spravovat, ovládat a sledovat přístup k prostředkům Azure (Preview) v rámci vaší organizace na:

- Předplatná
- Skupiny prostředků
- Virtuální počítače. 

Všechny prostředky v rámci portálu Azure, které využívají funkci Azure na základě řízení přístupu Role (RBAC) můžete využít výhod všech zabezpečení a možnosti správy životního cyklu, které má Azure AD PIM nabízet.

Další informace najdete v tématu [PIM pro prostředky Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="introducing-access-reviews"></a>Představení přístupu zkontroluje


**Typ:** nová funkce  
**Kategorie služby:** přístup recenze  
**Schopnosti produktu:** zásad správného řízení  



Organizacím efektivní Správa členství ve skupinách a přístup k podnikové aplikace, které umožňují přístup recenze (preview): 

- Můžete znovu potvrzení přístupu uživatele guest pomocí přístupu recenze jejich přístupu k aplikacím a členství ve skupinách. Statistika poskytované recenze přístupu povolit kontroloři efektivně rozhodnout, zda hosté by měl neustálý přístup.

- Pomocí kontrol přístupu je možné provést opětovnou certifikaci přístupu k aplikacím a členství ve skupinách i pro zaměstnance.

Kontroly přístupu lze shromažďovat v programech sledujících dodržování předpisů a přístup k citlivým aplikacím v rámci organizace.

Další informace najdete v tématu [zkontroluje přístup k Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hiding-third-party-applications-from-my-apps-and-the-office-365-launcher"></a>Skrytí aplikace jiných výrobců z Moje aplikace a Spouštěče Office 365



**Typ:** nová funkce  
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  



Teď můžete lépe spravovat aplikace, které se zobrazí na vaší portálech uživatele prostřednictvím novou **skrýt aplikace** vlastnost. Skrytí aplikace pomáhá s případy, kdy dlaždice aplikace se zobrazí pouze u služeb back-end nebo duplicitní dlaždice a skončili zbytečného Spouštěč oken aplikace uživatele. Přepínač je umístěn v části Vlastnosti aplikace třetích stran a je popsán **viditelný pro uživatele?** Můžete také skrýt aplikaci programově pomocí prostředí PowerShell. 

Další informace najdete v tématu [skrýt aplikace třetích stran z možnosti pro uživatele v Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Co je k dispozici?**

 V rámci přechodu na novou konzolu pro správu jsou k dispozici 2 nových rozhraní API pro načítání protokoly aktivity Azure AD. Novou sadu rozhraní API poskytuje bohatší filtrování a řazení funkce kromě bohatší auditu a přihlašovací aktivity. Dříve k dispozici prostřednictvím sestavy zabezpečení dat je nyní přístupná prostřednictvím Identity Protection rizikových událostech rozhraní API v aplikaci Microsoft Graph.


## <a name="september-2017"></a>2017 září

### <a name="hotfix-for-microsoft-identity-manager"></a>Oprava hotfix pro Microsoft Identity Manager


**Typ:** změněné funkce  
**Kategorie služby:** Microsoft Identity Manager  
**Schopnosti produktu:** Identity – Správa životního cyklu  



Balíčku kumulativní opravy hotfix (sestavení 4.4.1642.0) je k dispozici od 25 září 2017 pro Microsoft Identity Manager (MIM) 2016 2016 Service Pack 1 (SP1). Tento balíček aktualizací:

- Řeší problémy a přidá vylepšení
- Je kumulativní aktualizace, který nahrazuje všechny aktualizace MIM 2016 SP1 až 4.4.1459.0 sestavení pro Microsoft Identity Manager 2016. 
- Vyžaduje, abyste měli **4.4.1302.0 sestavení Microsoft Identity Manager 2016.** 

Další informace najdete v tématu [balíčku kumulativní opravy Hotfix (sestavení 4.4.1642.0) je k dispozici pro Microsoft Identity Manager 2016 SP1](https://support.microsoft.com/en-us/help/4021562). 

---
