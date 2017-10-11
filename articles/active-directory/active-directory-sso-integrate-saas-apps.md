---
title: "Integrovat Azure Active Directory jednotné přihlašování s aplikacemi SaaS | Microsoft Docs"
description: "Povolte jednotné přihlašování, ověřování a zřizování centralizovanou správu přístupu aplikace SaaS ve službě Azure Active Directory uživatelů. Přehled o tom, jak integrovat Azure Active Directory k aplikacím SaaS."
services: active-directory
keywords: Integrace s aplikacemi SaaS Azure AD
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fc0d297598c334ca8f6f8a2bd3ae948c87956342
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrovat Azure Active Directory jednotné přihlašování s aplikacemi SaaS
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Portál Azure Classic](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Abyste mohli začít, nastavení jednotného přihlašování pro aplikaci, která se převedou do vaší organizace, budete používat existující adresář v Azure Active Directory (Azure AD). Můžete vytvořit adresář služby Azure AD, který získáte prostřednictvím Microsoft Azure, Office 365 nebo služby Windows Intune. Pokud máte dva nebo více z těchto, najdete v části [Správa adresáře služby Azure AD](active-directory-administer.md) k určení, která chcete použít.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Pro přiřazení rolí správce v Centru správy služby Azure AD najdete v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Authentication
Pro aplikace, které podporují WS-Federation, SAML 2.0 nebo OpenID Connect protokoly služby Azure Active Directory používá podpisové certifikáty pro navazování vztahů důvěryhodnosti. Další informace o tom najdete v tématu [Správa certifikátů pro federované jednotné přihlašování](active-directory-sso-certs.md).

Pro aplikace, které podporují pouze HTML založené na formulářích přihlášení, Azure Active Directory používá 'heslo překlenutí vyrovnávací paměti' Chcete-li vytvářet vztahy důvěryhodnosti. Díky tomu mohou uživatelé ve vaší organizaci být automaticky přihlášeni k aplikaci SaaS Azure AD pomocí informací o účtu uživatele z aplikace SaaS. Azure AD shromažďuje a bezpečně ukládá informace o uživatelském účtu a související heslo. Další informace najdete v tématu [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorizace
Zřízené účet umožňuje uživateli autorizovat použití aplikace po mít ověření přes jednotné přihlašování. Zřizování uživatelů lze provést ručně, nebo v některých případech můžete přidávat a odebírat uživatelské informace z aplikace SaaS na základě změn provedených ve službě Azure Active Directory. Další informace o použití existujících konektorů Azure AD pro automatizované zajišťování najdete v tématu [automatické zřizování uživatelů a jeho rušení pro aplikace SaaS](active-directory-saas-app-provisioning.md).

Jinak můžete ručně přidat informace o uživateli do aplikace, nebo použití jiných zřizování řešení, které jsou k dispozici na webu Marketplace.

## <a name="access"></a>Access
Azure AD poskytuje několik způsobů přizpůsobitelné k nasazení aplikací pro koncové uživatele ve vaší organizaci. Abyste neměli zablokován do žádné konkrétní nasazení nebo řešení přístupu. Můžete použít [řešení, které nejlépe vyhovuje vašim potřebám](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Další informace pro aplikace již používá
Nastavení jednotného přihlašování na pro aplikaci, která už vaše organizace používá je jiný proces, který z procesu vytváření nových účtů pro novou aplikaci. Existují dvě předběžné kroků, včetně: mapování identit uživatelů v aplikaci do Azure AD identity a pochopení, jak uživatelé budou mít po integraci přihlášení k aplikaci.

> [!NOTE]
> Chcete-li nastavit jednotné přihlašování pro existující aplikace, musíte mít práva globálního správce v obou Azure AD a aplikace SaaS.
>
>

### <a name="mapping-user-accounts"></a>Mapování uživatelských účtů
Identita uživatele obvykle má jedinečný identifikátor, který může být e-mailovou adresu, nebo hlavní název uživatele (UPN). Budete muset odkaz (map) identity aplikace jednotlivých uživatelů k jejich odpovídajících identit Azure AD. Existuje několik způsobů, jak to provést v závislosti na tom, jak požadavek na ověření vaší aplikace.

Další informace o mapování identit aplikací s Azure AD identity najdete v tématu [přizpůsobení deklarace identity vystavené v tokenu SAML](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) a [přizpůsobení mapování atributů pro zřizování](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Principy přihlašování uživatele
Pokud integrujete jednotného přihlašování pro aplikace, která se už používá, je důležité si uvědomit, že bude mít vliv činnost koncového uživatele. Pro všechny aplikace budou uživatelé začít se přihlásit pomocí svých přihlašovacích údajů Azure AD. Také může být, že uživatel musí použít jiný portál přístup k aplikacím.

Jednotné přihlašování pro některé aplikace lze provést na přihlašovací aplikace rozhraní, ale pro jiné aplikace, bude uživatel muset projít centrální portál, jako ([Moje aplikace](http://myapps.microsoft.com) nebo [Office 365](http://portal.office.com/myapps)) pro přihlášení. Další informace o různých typech jednotné přihlašování a jejich činnosti uživatelů [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

Je jiný prostředek cenné *potlačení souhlas uživatele* v [směrných vývojáři](active-directory-applications-guiding-developers-for-lob-applications.md) článku.

## <a name="next-steps"></a>Další kroky
Pro aplikace SaaS, které můžete najít v galerii aplikací, Azure AD poskytuje řadu [kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md).

Pokud aplikace není v galerii aplikací, můžete [ho přidat do galerii aplikací Azure AD jako vlastní aplikaci](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Je mnohem víc podrobností na všechny tyto problémy v knihovně Azure.com počínaje [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

Plus, nezapomeňte si [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md).
