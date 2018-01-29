---
title: "Kurz: Konfigurace Asana pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do Asana."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Kurz: Konfigurace Asana pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Asana a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Asana.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active Active directory
*   Klient služby Asana s [Enterprise](https://www.asana.com/pricing) plánu nebo lépe povoleno 
*   Uživatelský účet v Asana s oprávněními správce 

> [!NOTE] 
> Azure AD zřizování integrace spoléhá na [Asana API](https://app.asana.com/api/1.0/scim/Users) tedy k dispozici pro Asana.

## <a name="assigning-users-to-asana"></a>Přiřazení uživatelů k Asana

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu budou synchronizovány pouze uživatelé, kteří mají "přiřazený" k aplikaci ve službě Azure AD. 

Před konfigurací a povolení zřizování služby, musíte se rozhodnout, jaké uživatelů ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Asana. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Asana podle pokynů tady:

[Přiřazení uživatele k aplikaci enterprise](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Důležité tipy pro přiřazování uživatelů do Asana

*   Dále je doporučeno jednoho uživatele Azure AD pro Asana přidělí otestovat konfiguraci zřizování. Další uživatelé mohou přiřadit později.

## <a name="configuring-user-provisioning-to-asana"></a>Konfiguraci zřizování uživatelů k Asana 

Tato část vás provede připojení k Asana uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování služby vytvářet, aktualizovat a zakázat přiřadit uživatelské účty v Asana podle přiřazení uživatelů ve službě Azure AD.

> [!TIP]
> Můžete také pro Asana povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Konfigurace automatického účet zřizování uživatelů k Asana ve službě Azure AD:

1)  V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2) Pokud jste již nakonfigurovali Asana pro jednotné přihlašování, vyhledejte instanci Asana pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Asana** v galerii aplikací. Vyberte **Asana** ve výsledcích hledání a přidejte ji do seznamu aplikací.

3)  Vyberte instanci Asana a pak vyberte **zřizování** kartě.

4)  Nastavte **režimu zřizování** k **automatické**.

![Zřizování Asana](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) V části přihlašovací údaje správce, postupujte podle níže pokyny k vygenerování tokenu a zadejte ho **tajný klíč tokenu** textové pole.

* Přihlášení k [Asana](https://app.asana.com) pomocí účtu správce.
* Klikněte na tlačítko profilové fotky ze horním panelu a vyberte aktuální nastavení název organizace
* Přejděte na kartu účty služby
* Klikněte na tlačítko Přidat účet služby
* O aktualizovat název a podle potřeby profil fotografií, zkopírujte **tokenu** a klikněte na Uložit změny

6) Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Asana. Pokud se nepovede připojit, ujistěte se, váš účet Asana má oprávnění správce a zkuste to **"Test připojení"** krok opakujte.

7) Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.

8) Klikněte na **Uložit**. 

9) V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Asana**.

10) V **mapování atributů** , projděte si atributy uživatele, které budou synchronizované z Azure AD Asana. Všimněte si, že atributy vybrán jako **párování** vlastnosti se použije tak, aby odpovídaly uživatelské účty v Asana pro operace aktualizace. Vyberte **Uložit** tlačítko potvrzení změny. V tématu [přizpůsobení atribut mapování uživatelů zřizování](active-directory-saas-customizing-attribute-mappings.md) další podrobnosti

11) Povolit zřizování služby pro Asana Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části

12) Klikněte na **Uložit**. 

Tím se spustí počáteční synchronizaci všech uživatelů přidružených k Asana v části uživatelé. Počáteční trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Asana.

Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Další prostředky

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-asana-tutorial.md)
