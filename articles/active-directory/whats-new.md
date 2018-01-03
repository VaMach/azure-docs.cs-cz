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
ms.date: 12/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 408e1680e7aee13663bb6511e65fd5685eb7d518
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Co je nového ve službě Azure Active Directory?




> Vždy aktuální s co je nového ve službě Azure Active Directory se přihlásíte k odběru naší ![RSS](./media/whats-new/feed-icon-16x16.png) [kanálu](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Budeme průběžně zlepšují Azure Active Directory. Aby vám nedovolí zůstat aktuální pomocí nejnovější Toto téma poskytuje informace o:

-   Nejnovější verze 
-   Známé problémy 
-   Opravy chyb 
-   Zastaralé funkce 
-   Plány pro změny 

Pokroku prosím tuto stránku pravidelně aktualizujeme ho měsíčně.


## <a name="december-2017"></a>2017 prosinec
 

### <a name="terms-of-use-in-the-access-panel-for-end-users"></a>Podmínky použití na přístupovém panelu pro koncové uživatele

**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení nebo dodržování předpisů
 
Koncoví uživatelé nyní mají možnost Přejít na panel přístupu a zobrazit podmínky použití, který jste dříve přijali.

Uživatelé si můžou zobrazit a přečíst podmínky použití, které přijali. To lze provést následujícím způsobem:

1. Přejděte a přihlaste se do [MyApps portál](https://myapps.microsoft.com).

2. V pravém horním rohu klikněte na své jméno a z rozevírací nabídky vyberte **Profil**. 

3. Na svém profilu klikněte na **Přečíst podmínky použití**. 

4. Tady si můžete přečíst podmínky použití, které jste přijali. 

Další informace najdete v tématu [funkce Azure Active Directory podmínky použití (Preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou)
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Nové přihlašovací prostředí Azure AD

**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** ověření uživatele
 
Jako součást cesty a umožňuje konvergovat Azure AD a identity systémů účtu Microsoft jsme mít přepracovali uživatelského rozhraní na obou systémů, aby měli konzistentní vzhled a chování. Kromě toho jsme mít čísla stránek vložena na přihlašovací stránku služby Azure AD, aby shromažďujeme uživatelské jméno nejprve následuje přihlašovací údaje na druhý obrazovky.

Další informace najdete v tématu [nové prostředí Azure AD přihlášení je nyní ve verzi Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/)
 
---
 

### <a name="fewer-login-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-login"></a>Menší počet výzev k přihlášení: V nové "zůstat přihlášeni" rozhraní pro přihlášení k Azure AD

**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** ověření uživatele
 
Jsme nahradit **zůstat přihlášeni** zaškrtávací políčko je na přihlašovací stránku služby Azure AD s nového řádku, který se zobrazí poté, co uživatel úspěšně ověří. 

Pokud uživatel odpoví **Ano** na tuto výzvu služby jim poskytne trvalé obnovovací token. Toto je stejné chování jako kontroluje, když uživatel **zůstat přihlášeni** políčko ve starší verzi uživatelského rozhraní. U federovaných klientů tato výzva se zobrazí po úspěšně ověří uživatele federované služby.

Další informace najdete v tématu [méně výzev k přihlášení: je nové prostředí "Zůstat přihlášeni" pro Azure AD ve verzi preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) 

---
 

### <a name="add-configuration-to-require-the-tou-to-be-expanded-prior-to-accepting"></a>Přidáte konfiguraci tak, aby vyžadovala podmínky, které mají být rozbaleny před přijetím.

**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení
 
Jsme přidali nyní možnost pro správce tak, aby vyžadovala své koncové uživatele rozšíření podmínky použití před vyjádření souhlasu s podmínkami.

Vyberte buď možnost zapnout nebo vypnout pro vyžadovat, aby podmínky použití rozbalte položku uživatelé. Pokud je tato volba zapnutá, než uživatelé přijmou podmínky použití, budou je muset zobrazit.

Další informace najdete v tématu [funkce Azure Active Directory podmínky použití (Preview)](active-directory-tou.md)
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Vymezená aktivace pro přiřazení oprávněné rolí

**Typ:** nová funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management
 
Vymezená aktivace umožňuje aktivovat přiřazení rolí oprávněné prostředků Azure s menší nezávislé než původní přiřazení výchozí hodnoty. Vlastník předplatného jsou například přiřazeny ve vašem klientovi. Vymezená aktivace, můžete aktivovat vlastníka pro až pět prostředky obsažené v rámci předplatného (vezměte v úvahu skupin prostředků, virtuální počítače, např...). Obor aktivací může omezit možnost provádění nežádoucí změny k důležitým prostředkům Azure.

Další informace najdete v tématu [co je Azure AD Privileged Identity Management?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-azure-ad-app-gallery"></a>Nové federované aplikace v galerii aplikací Azure AD

**Typ:** nová funkce  
**Kategorie služby:** podnikové aplikace  
**Schopnosti produktu:** 3. stran integrace
 
V prosinec 2017 jsme přidali následující nové aplikace v naší Galerie aplikace s federací podporu:

|Název|Typ integrace|Popis|
|:-- |----------------|:----------|
|Digitální výkladní skříň EFI|SAML 2.0|[Webové 2 tiskových aplikace](https://go.microsoft.com/fwlink/?linkid=861685)|
|Vodeclic|SAML 2.0|[Pomocí Azure AD můžete spravovat přístup uživatelů a povolit jednotné přihlašování s Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522).  Vyžaduje existující účet Vodeclic.|
|Accredible|SAML 2.0|[Vytvoření, správě a poskytování certifikáty, odznaky a blockchain pověření](https://go.microsoft.com/fwlink/?linkid=863523)|
|FactSet|SAML 2.0|[Jednotné přihlašování k aplikaci na FactSet FDSWeb](https://go.microsoft.com/fwlink/?linkid=863525)|
|Integrace se službou AD MobileIron Azure|SAML 2.0|[MobileIron](https://go.microsoft.com/fwlink/?linkid=858027) zvláště je umožnit moderní podniky k zabezpečení a spravovat informace o při jejich přesunu do mobile a cloudu, při zachování ochrany osobních údajů koncového uživatele a vztah důvěryhodnosti.|
|OBRÁZEK FUNGUJE|SAML 2.0|Pomocí Azure AD můžete spravovat přístup uživatelů, zřízení uživatelských účtů a povolit jednotné přihlašování s [IMAGE FUNGUJE](https://go.microsoft.com/fwlink/?linkid=863517). Vyžaduje předplatné existující FUNGUJE bitové kopie.|
|Jednotné přihlašování SAML pro Bitbucket podle řešení GmbH|SAML 2.0|[Jednotné přihlašování Bitbucket](https://go.microsoft.com/fwlink/?linkid=863519) delegáti ověřování do služby Azure AD, uživatelé již přihlášeni k Azure AD přímý přístup Bitbucket. Uživatelé mohou být vytvořeny a aktualizovat na průběžně s daty z SAML atributy.|
|Jednotné přihlašování SAML pro bambus podle řešení GmbH|SAML 2.0|[Jednotné přihlašování bambus](https://go.microsoft.com/fwlink/?linkid=863520) delegáti ověřování do služby Azure AD, uživatelé již přihlášeni k Azure AD můžete bambus přímý přístup.|
|Communifire|SAML 2.0|[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) je vaše moderní, plně funkční sociálních intranetu software, který podporuje vaši zaměstnanci a vaší firmy.|
|MOBI|SAML 2.0|[Centralizovat, pochopit a ovládat vaše zařízení celý ekosystém](https://go.microsoft.com/fwlink/?linkid=863521).|
|Reflektive|SAML 2.0|[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518) je moderní platforma pro správu výkonu, zpětné vazby v reálném čase a nastavení cílů. Jsme umožnit zaměstnancům jednotky vlastní vývoj, může být více strategické.|
|CybSafe|OpenID Connect & OAuth|CybSafe je platforma pro certifikaci GCHQ internetový sledování. Pokročilé technologie a analýzy dat používá ke snížení prokazatelně lidského aspektů zabezpečení internetový a riziko ochrany dat.|
|WebHR|OpenID Connect & OAuth|Všech uživatelů oblíbených sociálních HR vše v jednom Software. Důvěryhodná přes 20 000 společnosti 197 zemí|
 |Integrace se službou AD Zenegy Azure|OpenID Connect & OAuth|S touto aplikací slouží k přihlášení do Zenegy přihlašovacích údajů Azure Active Directory vaší společnosti.|
|Adobe činnost správce|SAML 2.0|Adobe prostředí správce (AEM) je řešení komplexní správu obsahu platforma pro vytváření webů, mobilní aplikace a formulářů – což usnadňuje správu marketing obsah a prostředky.|

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Pracovní postupy schválení pro role adresář Azure AD

**Typ:** změněné funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management
 
Pracovní postup schválení pro role adresář Azure AD je všeobecně dostupná.

S pracovní postup schválení privilegované role správců může vyžadovat, že členy role oprávněné žádosti o aktivaci role předtím, než můžete používat privilegované role.
Několik uživatelů a skupin může být delegované schválení odpovědnosti členy role vhodné dostávat oznámení po dokončení a jejich role je aktivní schválení

---
 

### <a name="pass-through-authentication---skype-for-business-support"></a>Předávací ověřování – Skype pro firmy podpory

**Typ:** změněné funkce  
**Kategorie služby:** ověřování (přihlášení)  
**Schopnosti produktu:** ověření uživatele


Předávací ověřování nyní podporuje uživatelská přihlášení ke Skypu pro firmy klientských aplikací, které podporují moderní ověřování, včetně Online a hybridní topologie. 

Další informace najdete v tématu [Skype pro firmy topologie podporována s moderním ověřováním](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-active-directory-privileged-identity-management-pim-for-azure-rbac-preview"></a>Aktualizace k Azure Active Directory Privileged Identity Management (PIM) pro RBAC Azure (preview)

**Typ:** změněné funkce  
**Kategorie služby:** PIM  
**Schopnosti produktu:** Privileged Identity Management
 
S naše veřejné Preview aktualizovat z Azure Active Directory Privileged Identity Management (PIM) pro Azure RBAC, můžete provádět následující akce:

Dostatečně Správa právě vyžadovat schválení použijte k aktivaci role prostředků naplánovat budoucí aktivace role, který vyžaduje schválení pro obě AAD a role RBAC Azure

 
Další informace najdete v tématu [PIM pro prostředky Azure (Preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)

 
---
 
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

Další informace najdete v tématu [balíčku kumulativní opravy Hotfix (sestavení 4.4.1642.0) je k dispozici pro Microsoft Identity Manager 2016 SP1](https://support.microsoft.com/help/4021562). 

---
