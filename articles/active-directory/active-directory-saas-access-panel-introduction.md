---
title: "Co je na přístupovém panelu v Azure Active Directory? | Dokumentace Microsoftu"
description: "Další informace o použití variace přístupový panel (webový prohlížeč, aplikace pro Android, aplikace iPhone a iPad) pro přístup k aplikacím SaaS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a63e2d583e1203708e49c0fcef99876d2055343a
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="what-is-the-access-panel"></a>Co je na přístupovém panelu?

Přístupový panel je webový portál. Umožňuje, aby uživatele, který má pracovní nebo školní účet v Azure Active Directory k zobrazení a spuštění aplikace založené na cloudu správce Azure AD udělil je přístup k. Můžete vytvořit také skupinu samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu.

Přístupový panel je nezávislý na portálu Azure a nemá můžete mít předplatné Azure.

![Přístupový panel][1]

Přístupový panel umožňuje upravit některé z nastavení profilu, včetně možnosti:

- Změňte heslo přidružené k pracovní nebo školní účet

- Upravit nastavení resetování hesla

- Upravit nastavení kontaktů a předvoleb týkající se vícefaktorového ověřování (pro účty, které byly vyžaduje použití ho správce)

- Zobrazit účet, který podrobnosti, jako je například ID uživatele, alternativní e-mailu a mobile a office telefonní čísla a zařízení

- Zobrazit a spustit cloudové aplikace, které je správce Azure AD udělil přístup. Další informace o panel přístupu z hlediska uživatelů najdete v tématu pomocí přístupového panelu. 

- Samoobslužná správa skupin. Přesněji řečeno správce můžete vytvořit a spravovat skupiny zabezpečení a žádosti o členství ve skupinách zabezpečení ve službě Azure AD. Další informace najdete v tématu [Samoobslužná správa skupin uživatelů ve službě Azure AD](active-directory-accessmanagement-self-service-group-management.md) a [spravovat vaše skupiny](active-directory-manage-groups.md).




## <a name="accessing-the-access-panel"></a>Přístup k přístupovému panelu

Když přejdete následující adresu URL ve webovém prohlížeči se můžete dostat na přístupovém panelu:`http://myapps.microsoft.com`

Pokud máte vlastní branding nakonfigurovaný pro přihlašovací stránku, můžete načíst branding připojením domény vaší organizace na konec adresy URL:`http://myapps.microsoft.com/<your domain>.com`

V takovém případě můžete použít libovolný název aktivní nebo ověřené domény, který byl nakonfigurován na portálu Azure.

![Název domény Wingtip Toys][2]  

Budete muset distribuovat adresu URL pro všechny uživatele, kteří se přihlašují k aplikacím, které jsou integrované s Azure AD.

## <a name="authentication"></a>Authentication

Pokud chcete dostat na přístupovém panelu, musíte být ověřeni přes pracovní nebo školní účet ve službě Azure AD. Vám mohou být ověřeni pro Azure AD přímo. Případně pokud organizace nakonfiguroval federace pomocí služby Active Directory Federation Services (AD FS) nebo jiných technologií, vám může být ověřen pomocí Windows Server Active Directory.

Pokud máte předplatné Azure nebo Office 365 a používáte portál Azure nebo aplikace Office 365, zobrazí se seznam aplikací bez přihlášení znovu. Pokud jste nejsou ověřené. zobrazí se výzva k přihlášení pomocí uživatelského jména a hesla pro váš účet ve službě Azure AD. Pokud vaše organizace má nakonfigurovali federaci, zadejte uživatelské jméno je dostačující.

Pokud jste se ověřili, můžete pracovat s aplikacemi, které správce má integrované s adresářem. Informace o integraci aplikací s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Požadavky na webový prohlížeč

Minimálně na přístupovém panelu vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. Pro uživatele k přihlášení do aplikace prostřednictvím založené na heslech jednotné přihlašování (SSO) musí být nainstalován rozšíření přístup k panelu v prohlížeči. Rozšíření se stáhne automaticky, když vyberete aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Rozšíření přístup k panelu je aktuálně dostupné pro Internet Explorer 8 a novější, okraj a Chrome, Firefox prohlížeče.

## <a name="mobile-app-support"></a>Podpora mobilních aplikací

Tým služby Azure Active Directory publikuje mé mobilní aplikace aplikace. Při instalaci aplikace se můžete přihlásit pomocí hesla aplikace jednotného přihlašování na zařízení iOS a Android.

> [!NOTE]
> Můžete přihlásit do aplikace, které podporují federace se službou Azure AD (včetně Salesforce, Google Apps, Dropbox, pole, Concur, Workday, Office 365 a více než 70 dalších) prakticky libovolný webový prohlížeč na libovolném zařízení, bez nutnosti modul plug-in nebo mobilní aplikace. Všechny ostatní [přístup k panelu prostředí](https://myapps.microsoft.com/) také nevyžadují mé aplikace mobilní aplikace, který se má použít na mobilním zařízení.
>
>

### <a name="my-apps-for-android"></a>Moje aplikace pro Android

Moje aplikace pro Android je podporována v jakékoli zařízení s Androidem se systémem Android verze 4.1 nebo novější.  
Je k dispozici v [obchodu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Moje aplikace pro Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Moje aplikace pro zařízení iPhone a iPad

Moje aplikace pro iOS je podporována na jakékoli zařízení iPhone nebo iPad, který používá verzi iOS 7 nebo novější.  
Je k dispozici v [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Moje aplikace pro iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Spravovaný prohlížeč pro Moje aplikace

Moje aplikace je taky integrovaná v Intune Managed Browser. Intune Managed Browser pro iOS a zařízení se systémem Android hraje důležitou roli v zajistíte, že jejího zabezpečení dat na mobilních zařízeních. Umožňuje bezpečně a přejděte na webové stránky, které může obsahovat informace o společnosti a poskytuje zabezpečené procházení webu prostředí.  
Pro vás rychlý přístup k mé aplikace na domovské stránce Managed Browser a ve své záložky budete méně kliknutí k dosažení všech aplikací, které chcete získat přístup.

Je k dispozici v [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) a [Google Play Storu](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Mananged prohlížeče pro Moje aplikace][5]    





## <a name="tips-for-testing-the-user-experience"></a>Tipy pro testování činnost koncového uživatele

Pokud jste správcem Azure a jste přihlášení k portálu Azure pomocí účtu v adresáři, budete automaticky přihlášení k přístupovému panelu jako aktuálního účtu. V takovém případě se zobrazí všechny aplikace, které vám byly přiřazeny.

**Testování jako *různých* uživatelský účet:**

1. Klikněte na nabídku uživatele v pravém horním rohu portálu Azure nebo na přístupovém panelu a potom vyberte **Odhlásit**. 
2. Přejděte na [přístupový panel](http://myapps.microsoft.com).
3. Na stránce přihlášení zadejte uživatelské jméno a heslo pro účet ve vašem adresáři, kterou chcete otestovat.


## <a name="starting-applications"></a>Spuštění aplikace

Několik typů aplikací, se může zobrazit na panel přístupu.

### <a name="office-365-applications"></a>Aplikace Office 365

Pokud vaše organizace používá Office 365 aplikace a máte licenci pro ně, zobrazí se aplikace Office 365 na přístupový panel.

Když kliknete dlaždici aplikace pro aplikaci Office 365, je přesměrován do aplikace a automaticky přihlášení.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation

Správce můžete přidat aplikace v oddílu služby Active Directory na portálu Azure, s režimem jednotné přihlašování, nastavte na **Azure AD jednotné přihlašování**. Tyto aplikace lze zobrazit pouze pokud vám správce udělil explicitně přístup k aplikacím.

Když kliknete na dlaždici pro jednu z těchto aplikací, jsou přesměrování a automaticky přihlášení k aplikaci.

### <a name="password-based-sso-without-identity-provisioning"></a>Založené na heslech jednotné přihlašování bez zřizování identity

Správce můžete přidat aplikace v oddílu služby Active Directory na portálu Azure, s režimem jednotné přihlašování, nastavte na **založené na heslech jednotné přihlašování**. Všechny aplikace, které jsou nakonfigurované v tomto režimu mohou prohlížet všichni uživatelé v adresáři.

První kliknutí na tlačítko dlaždice pro jednu z těchto aplikací, zobrazí se výzva k instalaci modulu plug-in heslo jednotné přihlašování pro aplikace Internet Explorer nebo Chrome. Instalace může vyžadovat restartování webového prohlížeče. Po návratu do přístupového panelu a znovu klikněte na dlaždici aplikace, zobrazí se výzva k zadání uživatelského jména a hesla pro aplikaci. Když zadáte uživatelské jméno a heslo, tyto přihlašovací údaje jsou bezpečně uložené a propojený s vaším účtem ve službě Azure AD.

Při příštím kliknutí na dlaždici aplikace budete automaticky přihlášení k aplikaci.  
Nemusíte znovu zadejte své přihlašovací údaje a nebo instalace modulu plug-in jednotné přihlašování heslo.

Pokud vaše přihlašovací údaje se změnila v cílové aplikace třetích stran, je nutné také aktualizovat svoje přihlašovací údaje, které jsou uložené ve službě Azure AD. 

**Chcete-li aktualizovat přihlašovací údaje:**

1. Vyberte ikonu na dlaždici aplikace.
2. Vyberte **aktualizovat přihlašovací údaje** opětovné zadání uživatelského jména a hesla pro aplikaci.


### <a name="password-based-sso-with-identity-provisioning"></a>Založené na heslech jednotného přihlašování k při zřizování identity

Správce můžete přidat aplikace v **služby Active Directory** části portálu Azure s režimem jednotné přihlašování, nastavte na **založené na heslech jednotné přihlašování**, společně s zřizování identity.

Poprvé, klikněte dlaždici aplikace pro jednu z těchto aplikací, zobrazí se výzva k instalaci **heslo jednotného přihlašování k modulu plug-in pro aplikace Internet Explorer nebo Chrome**. Instalace může vyžadovat restartování webového prohlížeče.  
Po návratu do přístupového panelu a znovu klikněte na dlaždici aplikace, budete automaticky přihlášení k aplikaci.

Některé aplikace může vyžadovat změny hesla na první přihlášení. Pokud vaše přihlašovací údaje se změnila v cílové aplikace třetích stran, je nutné také aktualizovat přihlašovací údaje, které jsou uložené ve službě Azure AD. 

**Chcete-li aktualizovat přihlašovací údaje:**

1. Vyberte ikonu na dlaždici aplikace.
2. Vyberte **aktualizovat přihlašovací údaje** opětovné zadání uživatelského jména a hesla pro aplikaci.


### <a name="application-with-existing-sso-solutions"></a>Aplikace s stávajících řešení pro jednotné přihlašování

Konfigurace jednotného přihlašování pro aplikace, portál Azure poskytuje třetí možnost jako **existující jednotné přihlašování**. Tato možnost umožňuje správci vytvořit odkaz na aplikaci a umístěte ji na panel přístupu pro vybraného uživatele.

Například pokud aplikace je nakonfigurovaný k ověřování uživatelů pomocí služby AD FS 2.0, správce můžete použít **existující jednotné přihlašování** možnost pro vytvoření odkazu na ni na panel přístupu. Při přístupu k propojení, se ověřují pomocí služby AD FS 2.0 nebo jakoukoli existující jednotného přihlašování k řešení, které poskytuje aplikace.


## <a name="next-steps"></a>Další kroky

- Pokud chcete zobrazit seznam všech témat, které se vztahují ke správě aplikací, najdete v článku [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md).
 
- Informace o tom, jak integrovat SaaS aplikace do Azure AD, najdete v článku [seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md).
 
- Další informace o správě aplikací s Azure AD, najdete v článku [Úvod k přístupu k jedné přihlašování a správu aplikace pomocí Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Další informace o zřizování uživatelů najdete v tématu [automatizace zřizování uživatelů a rušení zajištění pro aplikace SaaS](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
