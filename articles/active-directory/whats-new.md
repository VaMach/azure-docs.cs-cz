---
title: "Co je nového? Poznámky k verzi pro Azure Active Directory | Microsoft Docs"
description: "Zjistěte, co je nového v Azure Active Directory (Azure AD), jako je například nejnovější poznámky k verzi, známé problémy, opravy chyb, zastaralé funkce a nadcházející změny."
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
ms.date: 01/23/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f32fc2d2df73f4a01c300d565595e2c00b2baf4b
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="whats-new-in-azure-active-directory"></a>Co je nového ve službě Azure Active Directory?




> Nejnovější informace s co je nového ve službě Azure Active Directory (Azure AD) se přihlásíte k odběru [ ![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us) [kanálu](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Azure AD obdrží vylepšení průběžně. Zůstane aktuální pomocí nejnovější, tento článek poskytuje informace o:

-   Nejnovější verze
-   Známé problémy
-   Opravy chyb
-   Zastaralé funkce
-   Plány pro změny

Tato stránka se aktualizuje každý měsíc, takže pokroku pravidelně.


## <a name="january-2018"></a>2018 leden
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nové federované aplikace k dispozici v galerii aplikací Azure AD 

**Typ:** nová funkce  
**Kategorie služby:** podnikové aplikace  
**Schopnosti produktu:** 3. stran integrace
 

V lednu 2018 byly přidány následující nové aplikace s podporou federace v galerii aplikací:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [Software pro správu ochrany osobních údajů OneTrust](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federovaný Directory](https://go.microsoft.com/fwlink/?linkid=864699) a [přesnost NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Úplný přehled všechny kurzy k dispozici, najdete v části [integraci aplikací SaaS s Azure Active Directory](https://aka.ms/appstutorial).
 

---
 


### <a name="sign-in-with-additional-risk-detected"></a>Zjistilo se přihlášení s dalšími riziky.

**Typ:** nová funkce  
**Kategorie služby:** Identity Protection  
**Schopnosti produktu:** Identity zabezpečení a ochrany
 

Přehledy, které máte pro událost zjištěnou riziko je vázaný na vaše předplatné Azure AD. S Azure AD Premium P2 edition získáte nejvíce podrobné informace o všech základní detekce.

S edice Azure AD Premium P1 detekce, které nejsou předmětem licence zobrazí jako riziko událost přihlášení s další riziko zjištěna.

Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-reporting-risk-events.md).
 

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Skrýt aplikací Office 365 z panelů přístup koncového uživatele

**Typ:** nová funkce  
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování
 

Teď můžete lépe spravovat jak aplikace Office 365 zobrazovat ve panelů přístup uživatelů prostřednictvím nové nastavení uživatele. Tato možnost je užitečná pro snižuje množství aplikací v panelů přístupu uživatele, pokud chcete zobrazit pouze aplikace Office na portálu Office. Toto nastavení se nachází v **uživatelská nastavení** a je popsán **uživatelé vidí pouze aplikace Office 365 na portálu Office 365**.
 

Další informace najdete v tématu [skrýt aplikace z možnosti pro uživatele v Azure Active Directory](active-directory-coreapps-hide-third-party-app.md).

---
 


### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Bezproblémové přihlašování v aplikacích, které jsou povolené pro jednotné přihlašování heslo přímo z adresy URL aplikace 

**Typ:** nová funkce  
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování
 

Rozšíření prohlížeče Moje aplikace je nyní k dispozici prostřednictvím pohodlný nástroj, který poskytuje jednotné přihlašování v Moje aplikace na funkce jako zástupce v prohlížeči. Po instalaci uživatele se zobrazí ikona waffle v prohlížeči, který poskytuje je rychlý přístup k aplikacím. Uživatelé nyní mohou využít výhod:

- Umožňuje přímo se přihlásit k aplikacím na základě hesla-jednotného přihlašování z přihlašovací stránky aplikace
- Spuštění všech aplikací pomocí funkce rychlého vyhledávání
- Zástupce nedávno použité aplikací z rozšíření
- Rozšíření není k dispozici pro okraj a Chrome, Firefox.
 
Další informace najdete v tématu [Moje aplikace zabezpečené přihlašování rozšíření](active-directory-saas-access-panel-introduction.md#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Správa služby Azure AD byl vyřazen prostředí na portálu Azure classic

**Typ:** zastaralé   
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** adresáře
 

Od verze 8 leden 2018, Správa Azure AD byl vyřazen prostředí na portálu Azure classic. To došlo ve spojení s vyřazení z portálu Azure classic, sám sebe. Do budoucna, měli byste použít [centra pro správu Azure AD](https://aad.portal.azure.com) pro všechny vaše založené na portálu správy Azure AD.
 
---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Správa služby Azure AD byl vyřazen prostředí na portálu Azure classic

**Typ:** zastaralé  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** adresáře
 

Od verze 8 leden 2018 byl vyřazen PhoneFactor webový portál. Tento portál byl použit pro správu MFA serveru, ale tyto funkce byly přesunuty do portálu Azure na stránce portal.azure.com. 

Konfigurace vícefaktorového ověřování se nachází v: **Azure Active Directory \> MFA serveru**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Přestat používat sestav Azure AD


**Typ:** zastaralé  
**Kategorie služby:** vytváření sestav  
**Schopnosti produktu:** Identity – Správa životního cyklu  


S obecné dostupnosti novou konzolu pro správu služby Azure Active Directory a nová rozhraní API pro aktivity a zabezpečení sestavy, sestava rozhraní API je teď dostupná v části "/ sestavy" koncový bod byl vyřazen od konce 31. prosince 2017.


**Co je k dispozici?**

V rámci přechodu na novou konzolu pro správu byla provedena 2 nových rozhraní API k dispozici pro načtení protokoly aktivity Azure AD. Novou sadu rozhraní API zadejte bohatší filtrování a řazení funkce kromě bohatší auditu a přihlašovací aktivity. Dříve k dispozici prostřednictvím sestavy zabezpečení dat je nyní přístupná prostřednictvím Identity Protection rizikových událostech rozhraní API v aplikaci Microsoft Graph.

Další informace naleznete v tématu:

- [Začínáme s Azure Active Directory, vytváření sestav rozhraní API](active-directory-reporting-api-getting-started-azure-portal.md)

- [Začínáme s Azure Active Directory Identity Protection a Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


---


## <a name="december-2017"></a>2017 prosinec
 

### <a name="terms-of-use-in-the-access-panel"></a>Podmínky použití na přístupovém panelu

**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení nebo dodržování předpisů
 
Nyní můžete přejít na Panel přístupu a zobrazit podmínky použití, který jste dříve přijali.

Postupujte následovně:

1. Přejděte na [MyApps portál](https://myapps.microsoft.com)a přihlaste se.

2. V pravém horním rohu vyberte své jméno a pak vyberte **profil** ze seznamu. 

3. Na vaše **profil**, vyberte **přečtěte si podmínky použití**. 

4. Teď můžete zkontrolovat podmínky použití, který jste přijali. 

Další informace najdete v tématu [Azure AD podmínky použití funkce (preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Nové přihlašovací prostředí Azure AD

**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** ověření uživatele
 
Azure AD a identity systémem účtů Microsoft uživatelská byla znovu navržena tak, aby mají konzistentní vzhled a chování. Kromě toho na přihlašovací stránku služby Azure AD shromažďuje uživatelské jméno nejprve následuje přihlašovací údaje na druhý obrazovky.

Další informace najdete v tématu [nové přihlašovací prostředí Azure AD je nyní ve verzi public preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 

### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Menšího přihlášení: V nové "zůstat přihlášeni" rozhraní pro přihlášení k Azure AD

**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** ověření uživatele
 
**Zůstat přihlášeni** políčko na přihlašovací stránce služby Azure AD byl nahrazený nového řádku, který se zobrazí po úspěšně ověřit. 

Pokud je odpověď **Ano** na tuto výzvu služby vám dává trvalé obnovovací token. Toto chování je stejné jako když jste vybrali **zůstat přihlášeni** políčko ve starší verzi uživatelského rozhraní. U federovaných klientů se zobrazí tuto výzvu po úspěšné ověření u služby federované.

Další informace najdete v tématu [menšího přihlášení: je nové prostředí "zůstat přihlášeni" pro Azure AD ve verzi preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---
 

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Přidat konfiguraci tak, aby vyžadovala podmínky použití, které mají být rozbaleny před přijetím

**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení
 
Možnost pro správce vyžaduje, aby jejich uživatelé rozbalte podmínky použití před vyjádření souhlasu s podmínkami.

Vyberte buď **na** nebo **vypnout** budou muset uživatelé rozbalte podmínky použití. **Na** nastavení vyžaduje, aby uživatelé zobrazit podmínky použití před přijetím je.

Další informace najdete v tématu [Azure AD podmínky použití funkce (preview)](active-directory-tou.md).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Vymezená aktivace pro přiřazení oprávněné rolí

**Typ:** nová funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management
 
Vymezená aktivace můžete použít k aktivaci přiřazení rolí oprávněné prostředků Azure s menší nezávislé než původní přiřazení výchozí hodnoty. Příkladem je-li, můžete jako vlastník předplatného přiřazena ve vašem klientovi. Při použití aktivace oboru můžete aktivovat roli vlastníka pro až pět prostředky obsažené v rámci předplatného (například skupiny prostředků a virtuálních počítačů). Obor aktivací může omezit možnost provádění nežádoucí změny k důležitým prostředkům Azure.

Další informace najdete v tématu [co je Azure AD Privileged Identity Management?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Nové federované aplikace v galerii aplikací Azure AD

**Typ:** nová funkce  
**Kategorie služby:** podnikové aplikace  
**Schopnosti produktu:** integrace třetích stran
 
V prosinec 2017 byly přidány následující nové aplikace s podporou federace v galerii aplikací:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), správci prostředí Adobe [digitální výkladní skříň EFI](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE FUNGUJE](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD integrace](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [jednotné přihlašování SAML pro bambus podle řešení GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [Jednotné přihlašování SAML pro Bitbucket podle řešení GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, integrace Zenegy Azure AD.

Úplný přehled všechny kurzy k dispozici, najdete v části [integraci aplikací SaaS s Azure Active Directory](https://aka.ms/appstutorial).

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Pracovní postupy schválení pro role adresář Azure AD

**Typ:** změněné funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management
 
Pracovní postup schválení pro role adresář Azure AD je všeobecně dostupná.

S pracovní postup schválení může správce privilegovaných rolí vyžadovat členům role způsobilých pro žádost o aktivaci role předtím, než můžete používat privilegované role. Několik uživatelů a skupin může být delegované schválení odpovědnosti. Členy role vhodné přijímat upozornění, když je dokončen schválení a jejich role je aktivní.

---
 

### <a name="pass-through-authentication-skype-for-business-support"></a>Předávací ověřování: Skype pro firmy podpory

**Typ:** změněné funkce  
**Kategorie služby:** ověřování (přihlášení)  
**Schopnosti produktu:** ověření uživatele


Předávací ověřování nyní podporuje uživatelská přihlášení ke Skypu pro firmy klientských aplikací, které podporují moderní ověřování, která zahrnuje online a hybridní topologie. 

Další informace najdete v tématu [Skype pro firmy topologie podporována s moderním ověřováním](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Aktualizace k Azure AD Privileged Identity Management for RBAC Azure (preview)

**Typ:** změněné funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management
 
S aktualizací ve verzi public preview služby Azure AD Privileged Identity Management (PIM) pro řízení řízení přístupu (RBAC) můžete provádět následující akce:

* Použití akorát správy.
* Vyžadovat schválení k aktivaci role prostředků.
* Naplánujte budoucí aktivace roli, která vyžaduje schválení pro obě Azure AD a Azure RBAC rolí.

 
Další informace najdete v tématu [Privileged Identity Management pro prostředky Azure (preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

 
---
 
## <a name="november-2017"></a>2017 listopadu
 
### <a name="access-control-service-retirement"></a>Vyřazení služby Řízení přístupu



**Typ:** Plánovaná změna  
**Kategorie služby:** služby Řízení přístupu  
**Schopnosti produktu:** služby Řízení přístupu 


 Pozdní 2018 vyřadí Azure Active Directory řízení přístupu (také označované jako služba Řízení přístupu). Další informace, které zahrnují prováděcí plán a pokyny vysoké úrovně migrace bude k dispozici v další trvá několik týdnů. Na této stránce můžete nechat komentáře jakékoli dotazy týkající se služby Řízení přístupu a seskupení odpoví.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Omezit přístup z prohlížeče na Intune Managed Browser 


**Typ:** Plánovaná změna  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrana




K Office 365 a jiným aplikacím cloudu Azure AD připojení můžete omezit přístup z prohlížeče pomocí Intune Managed Browser jako schválené aplikace. 

Teď můžete nakonfigurovat následující podmínka pro podmíněný přístup využívající aplikace:

**Klientské aplikace:** prohlížeče

**Co je vliv změny?**

V současné době je blokován přístup při použití této podmínky. Pokud je k dispozici ve verzi preview, veškerý přístup bude vyžadovat použití aplikace spravovaného prohlížeče. 

Vyhledejte této schopnosti a další informace v poznámkách k nadcházející blogy a verzi. 

Další informace najdete v tématu [podmíněný přístup, ve službě Azure AD](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nové schválené klientské aplikace pro podmíněný přístup využívající aplikaci Azure AD

 
**Typ:** Plánovaná změna  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrana




Následující aplikace plánujeme přidat do seznamu [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Další informace naleznete v tématu:

- [Požadavek schválené klientské aplikace](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD, na základě aplikace podmíněného přístupu](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Podmínky použití podpora více jazyků



**Typ:** nová funkce    
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení nebo dodržování předpisů





Správci teď můžete vytvářet nové podmínky použití, které obsahují více dokumentů PDF. Můžete označit tyto dokumenty PDF s příslušný jazyk. Uživatelům se zobrazí PDF s odpovídající jazyk na základě jejich předvoleb. Pokud není nalezena žádná shoda, zobrazí se výchozí jazyk.


---
 

### <a name="real-time-password-writeback-client-status"></a>Stav klienta zpětného zápisu hesla v reálném čase



**Typ:** nová funkce  
**Kategorie služby:** samoobslužného resetování hesel  
**Schopnosti produktu:** ověření uživatele


 

Nyní můžete zkontrolovat stav vašeho klienta zpětný zápis hesla na místě. Tato možnost je dostupná v **integrace v místním** části [resetování hesla](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) stránky. 

Pokud jsou problémy s připojením k klientovi zpětného zápisu na místě, zobrazí chybovou zprávu, která nabízí:

- Informace o Proč se nemůžete připojit k vaší lokálního klienta zpětný zápis.
- Odkaz na dokumentaci, který vám pomůže při řešení problému. 


Další informace najdete v tématu [integrace v místním](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD, na základě aplikace podmíněného přístupu 



 
**Typ:** nová funkce  
**Kategorie služby:** Azure AD  
**Schopnosti produktu:** Identity zabezpečení a ochrana





Teď můžete omezit přístup k Office 365 a jiným aplikacím cloudu Azure AD připojené k [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) podporující zásady ochrany aplikací služby Intune s použitím [podmíněného přístupu na základě aplikace Azure AD](active-directory-conditional-access-mam.md). Zásady ochrany aplikací Intune se používají ke konfiguraci a ochraně dat společnosti na tyto klientské aplikace.

Tím, že zkombinujete [na základě aplikace](active-directory-conditional-access-mam.md) s [na zařízení](active-directory-conditional-access-policy-connected-applications.md) zásady podmíněného přístupu, máte možnost k ochraně dat pro osobní a firemní zařízení.

Následující podmínky a ovládací prvky jsou nyní k dispozici pro použití s podmíněného přístupu na základě aplikace:

**Podmínka podporované platformy**

- iOS
- Android

**Stav aplikace klienta**

- Mobilní aplikace a desktopoví klienti

**Řízení přístupu**

- Vyžaduje se klientem schválená aplikace.


Další informace najdete v tématu [podmíněného přístupu na základě aplikace Azure AD](active-directory-conditional-access-mam.md).

 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Správa zařízení služby Azure AD na portálu Azure



**Typ:** nová funkce  
**Kategorie služby:** registrace zařízení a správy  
**Schopnosti produktu:** Identity zabezpečení a ochrana

 



Nyní můžete najít všechna vaše zařízení připojená k Azure AD a aktivity související s zařízení na jednom místě. Je nové prostředí pro správu, ke správě identit zařízení a nastavení na portálu Azure. V této verzi můžete:

- Zobrazte všechna zařízení, které jsou k dispozici pro podmíněný přístup v Azure AD.
- Zobrazit vlastnosti, které zahrnují vaše hybridní Azure zařízení připojená k AD.
- Najít klíče nástroje BitLocker pro zařízení připojená k AD Azure, spravovat vaše zařízení s Intune a další.
- Spravovat nastavení související s zařízení služby Azure AD.

Další informace najdete v tématu [spravovat zařízení pomocí portálu Azure](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Podpora systému macOS jako platforma pro podmíněný přístup k Azure AD 



**Typ:** nová funkce    
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrana 
 

Nyní můžete zahrnout (nebo vyloučit) systému macOS jako podmínka platformy zařízení zásady podmíněného přístupu Azure AD. Přidání systému macOS na platformách podporovaných zařízení můžete:

- **Registrovat a spravovat zařízení v systému macOS pomocí Intune.** Podobně jako u jiných platformách, jako je iOS a Android, aplikace portálu společnosti je k dispozici pro systému macOS provést jednotné registrace. Nová aplikace portálu společnosti pro systému macOS můžete použít k registraci zařízení v Intune a zaregistrovat ji pomocí služby Azure AD.
- **Zkontrolujte, zda systému macOS zařízení splňovat zásady dodržování předpisů vaší organizace definované v Intune.** V Intune na portálu Azure můžete nyní můžete nastavit zásady dodržování předpisů pro zařízení v systému macOS. 
- **Omezte přístup k aplikacím ve službě Azure AD pouze systému macOS kompatibilních zařízení.** Vytváření zásad podmíněného přístupu má systému macOS jako možnost platformy samostatné zařízení. Teď můžete vytvářet zásady podmíněného přístupu v systému macOS specifické pro cílové aplikace nastavit v Azure.

Další informace naleznete v tématu:

- [Vytvořte zásady dodržování předpisů zařízení pro zařízení v systému macOS s Intune](https://aka.ms/macoscompliancepolicy)
- [Podmíněný přístup v Azure AD](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Rozšíření serveru zásad sítě pro Azure Multi-Factor Authentication 


**Typ:** nová funkce    
**Kategorie služby:** vícefaktorového ověřování  
**Schopnosti produktu:** ověření uživatele




Cloudové služby Multi-Factor authentication možnosti k infrastruktuře ověřování přidá rozšíření Network Policy Server pro Azure Multi-Factor Authentication pomocí existujících serverů. S příponou Network Policy Server přidáte telefonní hovor, textová zpráva nebo ověření telefonu v aplikaci do vaší stávající tok ověřování. Nemusíte instalovat, konfigurovat a spravovat nové servery. 

Toto rozšíření byla vytvořena pro organizace, které chcete chránit připojení virtuální privátní sítě bez nasazení serveru Azure Multi-Factor Authentication. Network Policy Server, který rozšíření funguje jako adaptér mezi RADIUS a cloudové ověřování Azure Multi-Factor Authentication pro poskytování druhý faktor ověřování pro federované nebo synchronizovaných uživatelů.


Další informace najdete v tématu [integrovat stávající infrastruktury Network Policy Server Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Obnovit nebo trvale odebrat odstranění uživatelé


**Typ:** nová funkce    
**Kategorie služby:** Správa uživatelů  
**Schopnosti produktu:** adresáře 



V Centru správy služby Azure AD můžete provádět následující akce:

- Obnovte odstraněný uživatel. 
- Trvale odstraní uživatele.


**Chcete-li vyzkoušejte ji:**

1. V Centru pro správu Azure AD, vyberte [všichni uživatelé](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) v **spravovat** části. 

2. Z **zobrazit** seznamu, vyberte **uživatelé v poslední době odstraňovali**. 

3. Vyberte jeden nebo více odstraněných uživatelů a pak je buď obnovit nebo trvale odstranit.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nové schválené klientské aplikace pro podmíněný přístup využívající aplikaci Azure AD

 
**Typ:** změněné funkce  
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrana


Následující aplikace byly přidány do seznamu [schválení klientských aplikací](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Další informace naleznete v tématu:

- [Požadavek schválené klientské aplikace](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD, na základě aplikace podmíněného přístupu](active-directory-conditional-access-mam.md)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Použití "Nebo" mezi ovládacími prvky v zásadách podmíněného přístupu 


**Typ:** změněné funkce    
**Kategorie služby:** podmíněného přístupu  
**Schopnosti produktu:** Identity zabezpečení a ochrana

 
Teď můžete použít "nebo" (vyžadují jeden z vybraných ovládacích prvků) pro řízení podmíněného přístupu. Tato funkce slouží k vytvoření zásady s "nebo" mezi řízení přístupu. Například můžete tuto funkci pro vytvoření zásady, které vyžadují uživatele k přihlášení pomocí služby Multi-Factor authentication "nebo" na vyhovující zařízení.

Další informace najdete v tématu [ovládacích prvků v Azure AD podmíněného přístupu](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Agregace v reálném čase rizikových událostí


**Typ:** změněné funkce    
**Kategorie služby:** ochranu Identity  
**Schopnosti produktu:** Identity zabezpečení a ochrana


V Azure AD Identity Protection jsou teď pro každý typ události riziko agregován všechny události v reálném čase rizik, které pocházely ze stejné IP adresy v daný den. Tato změna omezuje objem událostí riziko uvedených bez jakékoli změny v zabezpečení uživatelů.

Základní v reálném čase zjišťování funguje při každém přihlášení uživatele. Pokud máte zásadu přihlášení riziko zabezpečení nastavit tak, aby aplikace Multi-Factor authentication nebo blokování přístupu, je stále aktivuje během každé rizikové přihlášení.

 
---
 




## <a name="october-2017"></a>Říjen 2017


### <a name="deprecate-azure-ad-reports"></a>Přestat používat sestav Azure AD


**Typ:** Plánovaná změna  
**Kategorie služby:** vytváření sestav  
**Schopnosti produktu:** Identity – Správa životního cyklu  



Portál Azure nabízí:

- Novou konzolu pro správu Azure AD.
- Nová rozhraní API pro sestavy aktivity a zabezpečení.
 
Z důvodu tyto nové funkce byly sestavy rozhraní API v části endpoint/Reports vyřazeno na 10. prosince 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detekce pole Automatické přihlášení


**Typ:** pevné   
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  



Azure AD podporuje zjišťování pole automatické přihlašování pro aplikace, které vykreslení uživatelské jméno a heslo pole HTML. Tyto kroky jsou dokumentovány v článku [automaticky zaznamenání pole přihlášení pro aplikaci](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Této funkci můžete najít tak, že přidáte *Non-Galerie* aplikace na **podnikové aplikace, které** stránku [portál Azure](http://aad.portal.azure.com). Kromě toho můžete nakonfigurovat **jednotné přihlašování** režimu do této nové aplikace **založené na heslech jednotné přihlašování**, zadejte adresu URL webu a potom uložte stránky.
 
Z důvodu problém se službou byl dočasně zakázán tuto funkci. Tento problém byl vyřešen a detekce pole Automatické přihlášení opět k dispozici.

---

### <a name="new-multi-factor-authentication-features"></a>Nové funkce služby Multi-Factor authentication


**Typ:** nová funkce  
**Kategorie služby:** vícefaktorového ověřování  
**Schopnosti produktu:** Identity zabezpečení a ochrana  



Vícefaktorové ověřování (MFA), je zásadní součástí ochrany vaší organizace. Chcete-li více adaptivní přihlašovací údaje a zkušenosti pohodlnější způsob jednotného, byly přidány následující funkce: 

- Výzvy Multi-Factor výsledky jsou integrované přímo do Azure AD přihlášení zprávu, která zahrnuje programový přístup k MFA výsledky.
- Konfigurace vícefaktorového ověřování se více úzce integruje do konfigurace Azure AD prostředí na portálu Azure.

Pomocí této verzi public preview správu MFA a vytváření sestav jsou integrovanou součástí prostředí základní konfigurace Azure AD. Teď můžete spravovat funkce portálu správy vícefaktorového ověřování v rámci prostředí Azure AD.

Další informace najdete v tématu [referenční informace pro vytváření sestav v portálu Azure MFA](active-directory-reporting-activity-sign-ins-mfa.md). 


---

### <a name="terms-of-use"></a>Podmínky použití



**Typ:** nová funkce  
**Kategorie služby:** podmínky použití  
**Schopnosti produktu:** zásad správného řízení  



Můžete použít Azure AD podmínky použití na informace, jako je důležité omezení pro požadavky právních nebo dodržování předpisů pro uživatele.

Podmínky použití služby Azure AD můžete použít v následujících scénářích:

- Obecné podmínky použití pro všechny uživatele ve vaší organizaci
- Konkrétní podmínky použití na základě atributů uživatele (například lékařů oproti sester) nebo domácí oproti mezinárodní zaměstnanci, provádí dynamických skupin
- Konkrétní podmínky použití pro přístup k velkým dopadem obchodních aplikací, jako je Salesforce

Další informace najdete v tématu [Azure AD podmínky použití](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Vylepšení Správa privilegovaných identit


**Typ:** nová funkce  
**Kategorie služby:** Privileged Identity Management  
**Schopnosti produktu:** Privileged Identity Management  


S Azure AD Privileged Identity Management můžete spravovat, řízení a monitorování v rámci vaší organizace na přístup k prostředkům Azure (preview):

- Předplatná
- Skupiny prostředků
- Virtuální počítače 

Všechny prostředky v rámci portálu Azure, které používají funkci Azure RBAC můžete využít výhod všech zabezpečení a možnosti správy životního cyklu, které má Azure AD Privileged Identity Management na nabídku.

Další informace najdete v tématu [Privileged Identity Management pro prostředky Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="access-reviews"></a>Kontroly přístupu


**Typ:** nová funkce  
**Kategorie služby:** přístup recenze  
**Schopnosti produktu:** zásad správného řízení  



Organizace můžou efektivně spravovat přístup k podnikovým aplikacím a členství ve skupinách pomocí přístupu recenze (preview): 

- Můžete znovu certifikovat přístup uživatelů typu host pomocí kontrol jejich přístupu k aplikacím a jejich členství ve skupinách. Kontroloři můžete efektivně rozhodnout, zda povolit hosté dál přístup k přehledům poskytované recenze přístup podle.
- Pomocí kontrol přístupu je možné provést opětovnou certifikaci přístupu k aplikacím a členství ve skupinách i pro zaměstnance.

Kontroly přístupu lze shromažďovat v programech sledujících dodržování předpisů a přístup k citlivým aplikacím v rámci organizace.

Další informace najdete v tématu [zkontroluje přístup k Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Skrýt aplikace jiných výrobců z Moje aplikace a Spouštěč aplikace Office 365



**Typ:** nová funkce  
**Kategorie služby:** Moje aplikace  
**Schopnosti produktu:** jednotného přihlašování  



Nyní můžete lépe spravovat aplikace, které se zobrazí na vaši uživatelé portálech prostřednictvím novou **skrýt aplikace** vlastnost. Můžete skrýt, aplikacemi a pomáhá v případech, kde dlaždice aplikace zobrazí pro back endové služby nebo duplicitní dlaždice a Spouštěč oken aplikace zbytečné soubory uživatelů. Přepínač se **vlastnosti** část aplikace třetích stran a je popsán **viditelný pro uživatele?** Také můžete skrýt aplikaci programově pomocí prostředí PowerShell. 

Další informace najdete v tématu [skrýt aplikace jiných výrobců od koncových uživatelů ve službě Azure AD](active-directory-coreapps-hide-third-party-app.md). 


**Co je k dispozici?**

 V rámci přechodu na novou konzolu pro správu, dvou nových rozhraní API pro načítání aktivita služby Azure AD protokoly jsou k dispozici. Novou sadu rozhraní API poskytuje bohatší filtrování a řazení funkce kromě bohatší auditu a přihlašovací aktivity. Dříve k dispozici prostřednictvím teď sestavy zabezpečení dat je přístupný prostřednictvím API Identity ochrany riziko událostí v aplikaci Microsoft Graph.


## <a name="september-2017"></a>2017 září

### <a name="hotfix-for-identity-manager"></a>Oprava hotfix pro Identity Manager


**Typ:** změněné funkce  
**Kategorie služby:** Identity Manager  
**Schopnosti produktu:** Identity – Správa životního cyklu  



Balíček aktualizací oprav hotfix (sestavení 4.4.1642.0) je k dispozici od 25 září 2017 pro Identity Manager 2016 Service Pack 1. Tento balíček aktualizací:

- Řeší problémy a přidá vylepšení.
- Je kumulativní aktualizace, který nahrazuje všechny aktualizace Identity Manager 2016 Service Pack 1 až sestavení 4.4.1459.0 pro Identity Manager 2016. 
- Vyžaduje, abyste měli sestavení 4.4.1302.0 Identity Manager 2016. 

Další informace najdete v tématu [balíčku kumulativní opravy Hotfix (sestavení 4.4.1642.0) je k dispozici pro Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---
