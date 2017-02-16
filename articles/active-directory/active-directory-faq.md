---
title: "Nejčastější dotazy ke službě Azure Active Directory | Dokumentace Microsoftu"
description: "V nejčastějších dotazech ke službě Azure Active Directory získáte odpovědi na otázky související s přístupem do služby Azure a Azure Active Directory, správou hesel a přístupem k aplikacím."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3f550639f0e48f34200ad480f94524df2cc0b2b6


---
# <a name="azure-active-directory-faq"></a>Nejčastější dotazy ke službě Azure Active Directory
Azure Active Directory je komplexní řešení Identity jako služby (IDaaS), které pokrývá všechny prvky identity, řízení přístupu a zabezpečení.

Další podrobnosti naleznete v tématu [Co je Azure Active Directory?](active-directory-whatis.md).

## <a name="accessing-azure-and-azure-active-directory"></a>Přístup ke službě Azure a Azure Active Directory
**Otázka: Proč se při pokusu o přístup ke službě Azure AD na portálu Azure Classic zobrazí zpráva „Žádné předplatné nenalezeno“ (https://manage.windowsazure.com)?**

**Odpověď:** Přístup na portál Azure Classic vyžaduje, aby měl každý uživatel oprávnění na základě předplatného služby Azure. Pokud máte placené služby Office 365 nebo Azure AD, přejděte na adresu [http://aka.ms/accessAAD](http://aka.ms/accessAAD), kde můžete provést jednorázovou aktivaci, v opačném případě je třeba si aktivovat plnou [zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/) nebo placené předplatné. 

Další podrobnosti najdete v tématu:

* [Jak je předplatné Azure propojeno se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Správa adresáře pro předplatné služeb Office 365 ve službě Azure](active-directory-manage-o365-subscription.md)

- - -
**Otázka: Jaký je vztah mezi službami Azure AD, Office 365 a Azure?**

**Odpověď:** Azure Active Directory poskytuje běžné funkce identity a přístupu ke všem online službám společnosti Microsoft. Ať používáte službu Office 365, Microsoft Azure, Intune nebo další, již používáte službu Azure AD, kterou se aktivuje správa přihlašování a přístupu pro všechny tyto služby. 

Všichni uživatelé aktivovaní pro online služby společnosti Microsoft jsou v jedné nebo více instancí služby Azure AD definováni jako uživatelské účty. Těmto účtům můžete povolit přístup k bezplatným funkcím služby Azure AD, například ke cloudovým aplikacím.

Kromě toho placené služby AD Azure (např. Azure AD Basic, Premium, EMS atd.) doplňují ostatní online služby, např. Office 365 či Microsoft Azure o komplexní řešení správy a zabezpečení celého podniku.

- - -
## <a name="getting-started-with-hybrid-azure-ad"></a>Začínáme s hybridní službou Azure AD
**Otázka: Jak můžu připojit místní adresář ke službě Azure AD?**

**Odpověď:** Místní adresář můžete připojit ke službě Azure AD pomocí služby **Azure AD Connect**. 

Podrobnější informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

- - -
**Otázka: Jak mám nastavit jednotné přihlašování mezi místním adresářem a cloudovými aplikacemi?**

**Odpověď:** Jednotné přihlašování je třeba nastavit pouze mezi místním adresářem a službou Azure AD. Pokud cloudové aplikace používáte prostřednictvím služby Azure AD, služba automaticky povede uživatele, aby provedli správné ověření pomocí místních přihlašovacích údajů.

Implementaci jednotného přihlašování z místního prostředí lze snadno nastavit pomocí federačních řešení, například ADFS, nebo konfigurací synchronizace hodnot hash hesel.  Obě možnosti můžete snadno nasadit pomocí průvodce konfigurací služby Azure AD Connect.

Podrobnější informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

- - -
**Otázka: Poskytuje Azure Active Directory samoobslužný portál pro uživatele v naší organizaci?**

**Odpověď:** Ano, Azure Active Directory poskytuje pro uživatelskou samoobsluhu a přístup k aplikacím [Přístupový panel Azure AD](http://myapps.microsoft.com). Pokud jste zákazníkem služby Office 365, najdete mnoho stejných funkcí na portálu služeb Office 365. 

Další informace naleznete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md). 

- - -
**Otázka: Pomůže mi Azure AD spravovat místní infrastrukturu?**

**Odpověď:** Ano, pomůže. Edice Azure AD Premium nabízí **Connect Health**. Služba Azure AD Connect Health pomáhá monitorovat místní infrastrukturu identity a synchronizační služby a lépe proniknout do jejich funkce.  

Další informace naleznete v tématu [Sledování infrastruktury místních identit a synchronizačních služeb v cloudu](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Správa hesel
**Otázka: Je možné použít zpětný zápis hesla služby Azure AD bez synchronizace hesla? (NEBOLI, chci používat Azure AD SSPR se zpětným zápisem hesla, ale nechci svá hesla ukládat v cloudu.)**

**Odpověď:** Pokud chcete aktivovat zpětný zápis, není třeba synchronizovat hesla služby AD do služby Azure AD. Ve federovaném prostředí se jednotné přihlašování služby Azure AD spoléhá při ověřování uživatele na místní adresář. Tento scénář nevyžaduje, aby se ve službě Azure AD sledovalo místní heslo.

- - -
**Otázka: Jak dlouho trvá zpětný zápis hesla do místního prostředí AD?**

**Odpověď:** zpětný zápis hesla funguje v reálném čase. 

Další informace najdete v tématu [Začínáme se správou hesel](active-directory-passwords-getting-started.md). 

- - -
**Otázka: Je možné použít zpětný zápis hesel, která spravuje správce?**

**Odpověď:** Ano, pokud máte aktivní zpětný zápis hesel, zapíšou se operace s heslem provedené správcem zpět do místního prostředí.  

Další odpovědi na dotazy související s hesly naleznete v části [Správa hesel – nejčastější dotazy](active-directory-passwords-faq.md).

- - -
## <a name="application-access"></a>Přístup k aplikaci
**Otázka: Kde najdu seznam aplikací, které jsou předem integrovány se službou Azure AD a jejími funkcemi?**

**Odpověď:** Azure AD má více než 2 600 předem integrovaných aplikací od společnosti Microsoft, poskytovatelů služeb aplikací nebo partnerů. Všechny předem integrované aplikace podporují jednotné přihlašování. Jednotné přihlašování umožňuje používat k přístupu do aplikací firemní přihlašovací údaje. Některé aplikace také podporují automatické zřizování a jeho rušení

Úplný seznam předem integrovaných aplikací najdete v části [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**Otázka: Co mám dělat, když se požadovaná aplikace nenachází na webu Azure AD Marketplace?**

**Odpověď:** Se službou Azure AD Premium můžete přidávat a konfigurovat libovolné aplikace. V závislosti na funkcích aplikace a předvolbách můžete nakonfigurovat jednotné přihlašování a automatické zřizování.  

Další podrobnosti najdete v tématu:

* [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikací Azure Active Directory](active-directory-saas-custom-apps.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md) 

- - -
**Otázka: Jak se uživatelé můžou přihlašovat do aplikací pomocí služby Azure Active Directory?**

**Odpověď:** Azure Active Directory uživatelům nabízí několik způsobů zobrazení a přístupu k aplikacím, jako například:

* Přístupový panel služby Azure AD
* Spouštěč aplikace Office 365
* Přímé přihlášení k federovaným aplikacím
* Přímé odkazy na federované nebo existující aplikace či aplikace založené na hesle

Další informace naleznete v tématu [Nasazení integrovaných aplikací Azure AD pro uživatele](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**Otázka: Jakými způsoby Azure Active Directory umožňuje ověřování a jednotné přihlašování k aplikacím?**

**Odpověď:** Azure Active Directory podporuje mnoho standardizovaných protokolů pro ověřování a autorizaci, například SAML 2.0, OpenID Connect, OAuth 2.0 a WS-Federation. Azure AD podporuje funkce ukládání hesel do trezoru a automatického přihlašování pro aplikace, které podporují pouze ověření na základě formuláře.   

Další informace naleznete v tématu:

* [Scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)
* [Protokoly ověřování pro Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Jak funguje jednotné přihlašování pomocí služby Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**Otázka: Je možné přidat místní aplikace?**

**Odpověď:** Proxy soubory aplikace služby Azure AD poskytují snadný a bezpečný přístup k místním webovým aplikacím podle vašeho výběru. Aplikace můžete používat stejným způsobem jako aplikace SaaS ve službě Azure Active Directory. Není nutná síť VPN nebo změna infrastruktury sítě.  

Další podrobnosti naleznete v tématu [Jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).

- - -
**Otázka: Jak můžu vyžadovat vícefaktorové ověřování pro uživatele, kteří používají určitou aplikaci?**

**Odpověď:** Díky podmíněnému přístupu ke službě Azure AD můžete ke každé aplikaci přiřadit jedinečné zásady přístupu. V zásadách můžete požadovat MFA po celou dobu, nebo když uživatelé nejsou připojeni k místní síti.  

Další podrobnosti najdete v tématu [Zabezpečení přístupu k Office 365 a jiným aplikacím připojeným ke službě Azure Active Directory](active-directory-conditional-access.md).

- - -
**Otázka: Co je automatické zřizování uživatelů pro aplikace SaaS?**

**Odpověď:** Azure Active Directory umožňuje automatizovat vytváření, údržbu a odebírání uživatelských identit v mnoha oblíbených cloudových aplikacích (SaaS). 

Další informace najdete v tématu [Automatizace zřizování uživatelů a jeho rušení pro aplikace SaaS ve službě Azure Active Directory](active-directory-saas-app-provisioning.md)

- - -



<!--HONumber=Dec16_HO1-->


