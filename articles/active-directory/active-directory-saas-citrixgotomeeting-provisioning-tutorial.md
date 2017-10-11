---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Kurz: Konfigurace systému Citrix GoToMeeting pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Citrix GoToMeeting a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Citrix GoToMeeting.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Citrix GoToMeeting jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Citrix GoToMeeting s oprávněními správce týmu.

## <a name="assigning-users-to-citrix-gotomeeting"></a>Přiřazení uživatelů k Citrix GoToMeeting

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Citrix GoToMeeting. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Citrix GoToMeeting podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a>Důležité tipy pro přiřazování uživatelů do Citrix GoToMeeting

*   Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Citrix GoToMeeting a otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatele Citrix GoToMeeting, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojení k systému Citrix GoToMeeting uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v systému Citrix GoToMeeting na základě uživatele a skupiny přiřazení ve službě Azure AD.

> [!TIP]
> Také můžete povolit na základě SAML jednotné přihlašování pro Citrix GoToMeeting, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu:

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Citrix GoToMeeting pro jednotné přihlašování, vyhledejte instanci Citrix GoToMeeting pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Citrix GoToMeeting** v galerii aplikací. Vyberte Citrix GoToMeeting ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci Citrix GoToMeeting a pak vyberte **zřizování** kartě.

4. Nastavte **zřizování** režim **automatické**. 

    ![Zřizování](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. V části přihlašovací údaje správce proveďte následující kroky:
   
    a. V **uživatelské jméno správce GoToMeeting Citrix** textovému poli, zadejte uživatelské jméno správce.

    b. V **heslo správce systému Citrix GoToMeeting** textovému poli, heslo správce.

6. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Citrix GoToMeeting. Pokud připojení nezdaří, zkontrolujte účtu Citrix GoToMeeting má oprávnění správce týmu a zkuste to **"Přihlašovací údaje správce"** krok opakujte.

7. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

8. Klikněte na tlačítko **uložit.**

9. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Citrix GoToMeeting.**

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Citrix GoToMeeting. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v systému Citrix GoToMeeting pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

11. Povolit Azure AD zřizování služby pro Citrix GoToMeeting, změňte **Stav zřizování** k **na** v části Nastavení

12. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Citrix GoToMeeting v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Citrix GoToMeeting.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-citrix-gotomeeting-tutorial.md)


