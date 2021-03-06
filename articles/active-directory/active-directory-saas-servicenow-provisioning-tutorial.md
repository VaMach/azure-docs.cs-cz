---
title: "Kurz: Konfigurace ServiceNow pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Naučte se automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do ServiceNow."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 50d5ecd0542d236d4d68656af7808c329728aa39
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Kurz: Konfigurace ServiceNow pro zřizování automatické uživatelů s Azure Active Directory

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v ServiceNow a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do ServiceNow.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Musí mít platný klienta pro ServiceNow práci nebo ServiceNow pro vzdělávací organizace. Bezplatný zkušební účet můžete použít buď služby.
*   Uživatelský účet v ServiceNow s oprávněními správce týmu.

## <a name="assigning-users-to-servicenow"></a>Přiřazení uživatelů k ServiceNow

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci ServiceNow. Jakmile se rozhodli, můžete přiřadit tyto uživatele do aplikace ServiceNow podle pokynů tady: [přiřadit uživatele nebo skupinu enterprise aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke ServiceNow a otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.
>*   Při přiřazování ServiceNow uživatele, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit zřizování automatizované uživatelů

Tato část vás provede připojení k ServiceNow na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v ServiceNow podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
>Můžete také pro ServiceNow povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali ServiceNow pro jednotné přihlašování, vyhledejte vaše instance ServiceNow pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **ServiceNow** v galerii aplikací. Vyberte ServiceNow ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci ServiceNow a pak vyberte **zřizování** kartě.

4. Nastavte **zřizování** režim **automatické**. 

    ![zřizování](./media/active-directory-saas-servicenow-provisioning-tutorial/provisioning.png)

5. V části přihlašovací údaje správce proveďte následující kroky:
   
    a. V **název Instance ServiceNow** textovému poli, zadejte název instance ServiceNow.

    b. V **uživatelské jméno správce ServiceNow** textovému poli, zadejte uživatelské jméno správce.

    c. V **heslo správce ServiceNow** textovému poli, heslo správce.

6. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci ServiceNow. Pokud připojení nezdaří, zkontrolujte účtu ServiceNow má oprávnění správce týmu a zkuste to **"Přihlašovací údaje správce"** krok opakujte.

7. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

8. Klikněte na tlačítko **uložit.**

9. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům ServiceNow.**

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD ServiceNow. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v ServiceNow pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

11. Povolit zřizování služby pro ServiceNow Azure AD, změňte **Stav zřizování** k **na** v části Nastavení

12. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k ServiceNow v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 40 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** oddílu monitorovat průběh a odkazech na protokoly aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci ServiceNow zřizování.

Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Další zdroje informací:

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-servicenow-tutorial.md)


