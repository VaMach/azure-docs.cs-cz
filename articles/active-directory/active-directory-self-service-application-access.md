---
title: "Přístup k aplikaci Samoobslužné služby a delegované správy pomocí služby Azure Active Directory | Microsoft Docs"
description: "Tento článek popisuje, jak umožnit přístup k aplikaci Samoobslužné služby a delegované správy pomocí služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 39c62461c9659b0cb4422de88686283ba462c53b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Přístup k aplikaci Samoobslužné služby a delegované správy pomocí služby Azure Active Directory
Povolení schopnosti samoobslužné služby pro koncové uživatele je běžný scénář pro podnikové IT. Velký počet uživatelů, velký počet aplikací a osobě, která je best-informed rozhodnutí pro udělení přístupu nemusí být správce adresáře. Často je nejlepší osoba rozhodnout, kdo má přístup k aplikaci vedoucí týmu nebo jiných delegovaný správce. Ale je uživatel, který používá aplikaci, a uživatel ví, co potřebují, abyste mohli dělat svou práci.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. 

Přístup k aplikaci Samoobslužné služby je funkce [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 a P2 licencí, které umožňují správcům adresáře:

* Povolit uživatelům žádat o přístup k aplikacím pomocí "Získat další aplikace" v dlaždici [přístupový Panel Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Sadu uživatelů, kteří aplikace můžete požádat o přístup k
* Nastavte, zda schválení je potřeba pro uživatele, abyste mohli samoobslužné přiřadit přístup k aplikaci
* Sada, kdo by měl schválení žádosti a správa přístupu pro každou aplikaci

Dnes tato funkce je podporována pro všechny předem integrované a přihlašování vlastní aplikace, které podporují federované nebo založené na heslech jedním [galerii aplikací Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/), včetně aplikace, jako je Google Apps služby Salesforce, Dropbox a další.
Tento článek popisuje, jak:

* Konfigurace přístupu k aplikaci Samoobslužné služby pro koncové uživatele, včetně konfigurace pracovního postupu volitelné schválení 
* Delegovat správu přístupu pro konkrétní aplikace nejvhodnější lidem ve vaší organizaci a povolit, aby uživatelé používali přístupový panel Azure AD pro schválení žádosti o přístup přímo přiřadit přístup pro vybrané uživatele nebo (volitelně) nastavit přihlašovací údaje pro přístup k aplikaci při konfiguraci založené na heslech jednotné přihlašování

## <a name="configuring-self-service-application-access"></a>Konfigurace přístupu k aplikaci Samoobslužné služby
Chcete-li povolit přístup k aplikaci Samoobslužné služby a nakonfigurovat aplikace, které mohou být přidány nebo požadoval koncovým uživatelům, postupujte podle následujících pokynů.

1. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/).

2.   V části **služby Active Directory** část, vyberte adresář a pak vyberte **aplikace** kartě. 

3. Vyberte **přidat** tlačítko a použijete možnost galerie a vyberte Přidat aplikaci.

4. Po přidání aplikace získáte stránku rychlý Start aplikace. Klikněte na tlačítko **nakonfigurovat jednotné přihlašování**, vyberte požadované jeden přihlašování režim a uložte konfiguraci. 

5. Potom vyberte **konfigurace** kartě. Chcete-li povolit uživatelům žádat o přístup k této aplikaci z přístupový panel Azure AD, nastavte **povolit přístup k aplikaci Samoobslužné služby** k **Ano**.
  
  ![][1]

6. Chcete-li volitelně konfigurovat pracovní postup schválení žádosti o přístup, nastavte **vyžadovat schválení před udělením přístupu** k **Ano**. Pak lze vybrat jeden nebo více schvalovatelů pomocí **schvalovatelů** tlačítko.

  Approver může být každý uživatel v organizaci pomocí účtu Azure AD a může být zodpovědná za účet zřizování, Správa licencí, nebo jiné obchodní proces vaše organizace vyžaduje před udělením přístupu k aplikaci. Schvalovatel může být i vlastník skupiny jednoho nebo více sdílených účet skupiny a můžete přiřadit uživatele do jedné z těchto skupin a umožnit jim přístup prostřednictvím sdíleného účtu. 

  Pokud je vyžadováno žádné schválení, uživatelé okamžitě získat aplikaci přidat do jejich přístupový panel Azure AD. Pokud aplikace má byly nastaveny pro [zřizování automatické uživatelů](active-directory-saas-app-provisioning.md), nebo byl nastavený [režimu SSO "spravovaná uživatelem" heslo](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), uživatel byste již měli mít uživatel účet a znát heslo.

7. Pokud aplikace je nakonfigurovaná pro použití založené na heslech jednotné přihlašování, pak možnost pro povolení schvalovatel nastavit jednotné přihlašování pověření jménem každého uživatele, je také k dispozici. Další informace najdete v části na [Delegovaná správa přístupu](#delegated-application-access-management).

8. Nakonec **skupiny uživatelů Self-Assigned** se zobrazuje název skupiny, která se používá k ukládání uživatele, kterým byla udělena nebo přístupem k aplikaci. Schvalovatel přístup stává vlastníkem této skupiny. Pokud je název skupiny, zobrazí neexistuje, vytvoří se automaticky. Název skupiny Volitelně lze nastavit na název existující skupiny.

9. Chcete-li uložit konfiguraci, klikněte na tlačítko **Uložit** v dolní části obrazovky. Uživatelé teď můžou k žádosti o přístup k této aplikaci na přístupovém panelu.

10. Pokud chcete vyzkoušet činnost koncového uživatele, přihlaste se k přístupový panel Azure AD vaší organizace na https://myapps.microsoft.com, pokud možno pomocí jiného účtu, který není approver aplikace. 

11. V části **aplikace** , klikněte na **získat další aplikace** dlaždici. Tuto dlaždici zobrazí Galerie všechny aplikace, které jsou zapnuty pro přístup k aplikaci Samoobslužné služby v adresáři, s možností vyhledávání a filtrování podle kategorie aplikace na levé straně. 

12. Kliknutím na aplikaci zahájí proces žádosti. Pokud není třeba žádný proces schválení, pak aplikace bude okamžitě přidají pod **aplikace** karta po krátké potvrzení. Pokud je vyžadováno schválení, pak se zobrazí dialogové okno oznamující to a e-mail je odeslán schvalovatelů. Můžete musí být podepsané do přístupového panelu jako jiný schvalovatel zobrazíte tento proces žádosti.

13. E-mailu přesměruje schvalovatel schválit požadavek a přihlášení do přístupového panelu Azure AD. Jakmile se žádost schválí (a žádné speciální procesy, které definujete prováděly schvalujícím,), uživateli se zobrazí aplikace se zobrazí pod jejich **aplikace** karty, které můžete přihlásit do ní.

## <a name="delegated-application-access-management"></a>Správa přístupu k delegované aplikací
Schvalovatel přístup aplikaci lze všechny uživatele ve vaší organizaci, který je nejvhodnější osoba schválí nebo zamítne přístup k dané žádosti. Tento uživatel může být zodpovědná za zřizování účtu licencování nebo jiné obchodní proces vaše organizace vyžaduje před udělením přístupu k aplikaci.

Při konfiguraci přístup k aplikaci Samoobslužné služby, které jsou popsané výše, všechny přiřazené aplikace schvalovatelů najdete v části Další **spravovat aplikace** dlaždice na přístupový panel Azure AD, která ukazuje, které aplikace, které jsou přístup správce. Kliknutím na tlačítko aplikace zobrazuje obrazovka s několik možností.

![][2]

### <a name="approve-requests"></a>Schválení žádostí
**Schválení žádosti o** dlaždice umožňuje schvalovatelů zobrazíte všechny čeká na schválení, které jsou specifické pro tuto aplikaci a přesměruje na kartě schválení, kde žádosti je možné potvrdit nebo odepřít. Schvalovatel dále přijímá automatizovaných e-mailů, vždy, když je vytvořen požadavek, který jim vydává pokyn co dělat.

### <a name="add-users"></a>Přidání uživatelů
**Přidat uživatele** dlaždice umožňuje schvalovatelů přímo vybrané uživatelům udělit přístup k aplikaci. Po kliknutí na tuto dlaždici, uvidí schvalovatel, že zobrazí se dialogové okno jim umožňuje zobrazení a hledání pro uživatele v jejich adresáře. Přidání uživatele výsledků v aplikaci se zobrazí v těchto uživatele Azure AD přístup panelů nebo Office 365. Pokud jakékoli ruční uživatele procesu zřizování je potřeba na aplikaci, než bude moci přihlásit uživatele, pak schvalovatel proveďte před udělením přístupu tohoto procesu.  

### <a name="manage-users"></a>Správa uživatelů
**Spravovat uživatele** dlaždice umožňuje schvalovatelů přímo aktualizovat nebo odebrat uživatele, kteří mají přístup k aplikaci. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Konfigurovat přihlašovací údaje pro jednotné přihlašování k heslo (pokud existuje)
**Konfigurace** dlaždici se zobrazují, pouze pokud aplikace byla nakonfigurována správcem IT používat založené na heslech jednotné přihlašování a správce udělena schvalovatel možnost nastavit přihlašovací údaje heslo jednotné přihlašování, jak je popsáno výše. Při výběru schvalovatel nabízí několik možností, jak přihlašovací údaje rozšířeny přiřadit uživatele:

![][3]

* **Uživatelé přihlásit pomocí hesla** – v tomto režimu přiřazené uživatelům vědět, co uživatelských jmen a hesel jsou pro aplikaci a výzva k zadání jejich při jejich prvním přihlášení k aplikaci. Tento scénář odpovídá heslo jednotné přihlašování – případ kde [uživatelé spravují přihlašovací údaje](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Uživatelé se automaticky přihlásíte pomocí samostatné účty, které spravují** – v tomto režimu přiřazené uživatelé nemusí zadávat ani vědět své přihlašovací údaje specifické pro aplikace, pokud se přihlášení k aplikaci. Místo toho schvalovatel Nastaví pověření pro každého uživatele po přiřazení přístupu pomocí **přidat uživatele** dlaždici. Když uživatel klikne na aplikaci v jejich přístupového panelu nebo Office 365, jsou automaticky přihlášeni pomocí pověření, která nastavuje schvalovatel. Tento scénář odpovídá heslo jednotné přihlašování – případ kde [správci spravují přihlašovací údaje](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Uživatelé se automaticky přihlásíte pomocí jeden účet, který lze spravovat** -ve speciálním případě tomto případě je vhodné použít při všechny přiřazené uživatelé musí mít udělen přístup pomocí jednoho sdíleného účtu. Nejběžnější případ použití pro tuto funkci je s aplikací sociálních médií, kde organizace má jeden "společnost" účet a více uživatelů třeba, aby aktualizace k tomuto účtu. Tento scénář také odpovídá heslo jednotné přihlašování – případ kde [správci spravují přihlašovací údaje](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Ale po výběru této možnosti, schvalovatel vyzváni k zadání uživatelského jména a hesla pro jednu sdíleného účtu. Po dokončení všech uživatelů přiřazených přihlášení pomocí tohoto účtu, když kliknete na aplikaci v jejich panelů přístup k Azure AD nebo Office 365.

## <a name="additional-resources"></a>Další zdroje
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
