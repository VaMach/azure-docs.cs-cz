---
title: "Přizpůsobení deklarace identity vystavené v tokenu SAML pro podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Naučte se přizpůsobovat deklarace identity vystavené v tokenu SAML pro podnikové aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: jeevansd
manager: mtillman
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 7394857f55493b072e6ea549c8eeec54a808f5e5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="customizing-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Přizpůsobení deklarace identity vystavené v tokenu SAML pro podnikové aplikace v Azure Active Directory
Dnes Azure Active Directory podporuje jednotného přihlašování na s většinou podnikových aplikací, včetně obě aplikace předem integrovaných v galerii aplikací Azure AD a také vlastních aplikací. Když se uživatel přihlásí k aplikaci prostřednictvím služby Azure AD pomocí protokolu SAML 2.0, Azure AD odešle token do aplikace (přes HTTP POST). A pak aplikaci ověří a použije token k přihlásit uživatele místo výzvy k zadání uživatelského jména a hesla. Tyto tokeny SAML obsahují informace o uživateli známé jako "deklarace identity".

Ve vztahu identity Předčítání, "deklarace identity" je informace, které stavy zprostředkovatele identity o uživateli uvnitř tokenu vydaného pro tohoto uživatele. V [tokenu SAML](http://en.wikipedia.org/wiki/SAML_2.0), tato data je obvykle součástí příkaz atribut SAML. Jedinečné ID uživatele je obvykle reprezentována v předmětu SAML také nazývané jako název identifikátor.

Ve výchozím nastavení Azure Active Directory vystaví SAML token do vaší aplikace, která obsahuje deklaraci NameIdentifier s hodnotou uživatelské jméno (hlavní název uživatele NEBOLI) ve službě Azure AD. Tato hodnota může jednoznačné identifikaci uživatele. SAML token také obsahuje další deklarace obsahující uživatele e-mailová adresa, jméno a příjmení.

Chcete-li zobrazit nebo upravit deklarace identity vystavené v tokenu SAML k aplikaci, otevřete aplikaci na portálu Azure. Vyberte **zobrazení a upravovat všechny ostatní atributy uživatele** zaškrtnout políčko **uživatelské atributy** část aplikace.

![Uživatelské atributy oddílu][1]

Existují dvě možné důvody, proč je potřeba upravit deklarace identity vystavené v tokenu SAML:
* Aplikace byla zapsána vyžadují jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity.
* Aplikace byly nasazeny způsobem, který vyžaduje NameIdentifier deklarace identity na něco jiného než uživatelské jméno (hlavní název uživatele NEBOLI) uložené ve službě Azure Active Directory.

Můžete upravit všechny výchozí hodnoty deklarace identity. Vyberte řádek pro deklarace identity v tabulce atributy tokenu SAML. Tím se otevře **Upravit atribut** části a pak můžete upravit název deklarace identity, hodnotu a přidružený k deklaraci oboru názvů.

![Upravit atribut uživatele][2]

Rovněž můžete odebrat deklarace (jiné než NameIdentifier) pomocí místní nabídky, otevře se kliknutím na **...**  ikonu.  Můžete také přidat nové deklarace, pomocí **přidat atribut** tlačítko.

![Upravit atribut uživatele][3]

## <a name="editing-the-nameidentifier-claim"></a>Úpravy NameIdentifier deklarace identity
K vyřešení problému, kde byla aplikace nasazená pomocí jiné uživatelské jméno, klikněte na **uživatelský identifikátor** rozevírací nabídky **uživatelské atributy** části. Tato akce poskytuje dialogové okno s několik možností:

![Upravit atribut uživatele][4]

V rozevíracím seznamu vyberte **user.mail** nastavit NameIdentifier deklarace identity jako e-mailovou adresu uživatele v adresáři. Nebo vyberte **user.onpremisessamaccountname** nastavení pro uživatele je název účtu SAM, které se synchronizovaly z místní služby Azure AD.

Můžete také použít speciální **ExtractMailPrefix()** funkce odebrat příponu domény z e-mailovou adresu, název účtu SAM nebo hlavní název uživatele. To vyextrahuje pouze první část předávány prostřednictvím uživatelské jméno (například "joe_smith" místo joe_smith@contoso.com).

![Upravit atribut uživatele][5]

Jsme nyní jste také přidali **join()** funkce pro připojení k doméně ověřené s hodnotou identifikátoru uživatele. Když vyberete funkci join() v **uživatelský identifikátor** nejprve vybrat identifikátor uživatele jako jako e-mailovou adresu nebo uživatele hlavní název a potom v druhé rozevíracího seznamu vyberte ověřené domény. Pokud vyberete e-mailovou adresu s ověřenou doménu, pak Azure AD extrahuje uživatelské jméno z první hodnota joe_smith z joe_smith@contoso.com a jeho připojení s contoso.onmicrosoft.com. Podívejte se na následující příklad:

![Upravit atribut uživatele][6]

## <a name="adding-claims"></a>Přidání deklarace identity
Při přidání deklarace identity, můžete zadat název atributu (která nepotřebuje striktně dodržovat vzor URI podle specifikace SAML). Nastavte hodnotu na jakýkoli atribut uživatele, který je uložen v adresáři.

![Přidat atribut uživatele][7]

Například budete muset poslat oddělení, které uživatel patří do jejich organizace jako deklarace identity (například prodej). Zadejte název deklarace podle očekávání aplikací a potom vyberte **user.department** jako hodnotu.

> [!NOTE]
> Pokud pro daného uživatele neexistuje žádná hodnota uložené pro vybraný atribut, není právě této deklarace identity vystavené v tokenu.

> [!TIP]
> **User.onpremisesecurityidentifier** a **user.onpremisesamaccountname** podporují jenom při synchronizaci dat uživatele z místní služby Active Directory pomocí [Azure AD Připojení nástroje](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Deklarace identity s omezeným přístupem

Existují některé s omezeným přístupem deklarace identity v SAML. Pokud přidáte tyto deklarace, Azure AD, nebude odesílat tyto deklarace identity. Následují sadě deklarací identity SAML omezený:

    | Typ deklarace identity (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired |
    | http://schemas.microsoft.com/identity/Claims/accesstoken |
    | http://schemas.microsoft.com/identity/Claims/openid2_id |
    | http://schemas.microsoft.com/identity/Claims/identityprovider |
    | http://schemas.microsoft.com/identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/identity/Claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/NameIdentifier [MR1] |
    | http://schemas.microsoft.com/identity/Claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationInstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/groups.Link |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/Claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SID |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/UPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/Claims/scope |

## <a name="next-steps"></a>Další kroky
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikací Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
