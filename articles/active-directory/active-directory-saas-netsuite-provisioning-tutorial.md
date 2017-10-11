---
title: 'Kurz: Azure Active Directory integrace s Netsuite | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 277c393536615fc8bfe8af0bc6d487115f04776c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Kurz: Konfigurace Netsuite pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Netsuite a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Netsuite.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Netsuite jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Netsuite s oprávněními správce týmu.

## <a name="assigning-users-to-netsuite"></a>Přiřazení uživatelů k Netsuite

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Netsuite. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Netsuite podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a>Důležité tipy pro přiřazování uživatelů do Netsuite

*   Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Netsuite a otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování Netsuite uživatele, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojení k Netsuite na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Netsuite podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP] 
> Můžete také pro Netsuite povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-user-account-provisioning"></a>Ke konfiguraci zřizování účtu uživatele:

Cílem této části se popisují postup povolení zřizování uživatelů z uživatelských účtů služby Active Directory pro Netsuite.

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Netsuite pro jednotné přihlašování, vyhledejte instanci Netsuite pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Netsuite** v galerii aplikací. Vyberte Netsuite ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci Netsuite a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**. 

    ![Zřizování](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:
   
    a. V **uživatelské jméno správce** textovému poli, zadejte název, který má účtu Netsuite **správce systému** profil v Netsuite.com přiřazen.
   
    b. V **heslo správce** textovému poli, zadejte heslo pro tento účet.
      
6. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Netsuite.

7. V **e-mailové oznámení** pole, zadejte e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko.

8. Klikněte na tlačítko **uložit.**

9. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Netsuite.**

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Netsuite. Všimněte si, že atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Netsuite pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

11. Povolit zřizování služby pro Netsuite Azure AD, změňte **Stav zřizování** k **na** v části Nastavení

12. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Netsuite v části Uživatelé a skupiny. Všimněte si, že počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Netsuite.

Nyní můžete vytvořit testovací účet. Chcete-li ověřit, že účet byly synchronizovány Netsuite Počkejte až 20 minut.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-netsuite-tutorial.md)