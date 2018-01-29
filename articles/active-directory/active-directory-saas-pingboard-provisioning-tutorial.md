---
title: "Kurz: Konfigurace Pingboard pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do Pingboard."
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
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: f74e890cf716cfd4bc7b41fcc4aa244969cafde5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Kurz: Konfigurace Pingboard pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést, chcete-li povolit automatické zřizování a jeho rušení uživatelských účtů ze služby Azure Active Directory k Pingboard.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klient služby Azure Active Directory
*   Klient Pingboard [Pro účet](https://pingboard.com/pricing) 
*   Uživatelský účet v Pingboard s oprávněními správce 

> [!NOTE] 
> Zřizování integrace Azure Active Directory spoléhá na rozhraní API Pingboard (`https://your_domain.pingboard.com/scim/v2`) která je k dispozici ke svému účtu.

## <a name="assigning-users-to-pingboard"></a>Přiřazení uživatelů k Pingboard

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé, kteří mají "přiřazený" k aplikaci v Azure Active Directory. 

Před konfigurací a povolení zřizování služby, je nutné rozhodnout, kterými se uživatelé v Azure Active Directory představují uživatele, kteří potřebují přístup k vaší aplikaci Pingboard. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Pingboard podle pokynů tady:

[Přiřazení uživatele k aplikaci enterprise](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Důležité tipy pro přiřazování uživatelů do Pingboard

*   Doporučujeme přiřadit jenom jednoho konkrétního uživatele Azure Active Directory Pingboard otestovat konfiguraci zřizování. Další uživatelé mohou přiřadit později.

## <a name="configuring-user-provisioning-to-pingboard"></a>Konfiguraci zřizování uživatelů k Pingboard 

Tato část vás provede připojení k Pingboard uživatelský účet zřizování rozhraní API služby Azure Active Directory a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v Pingboard podle přiřazení uživatelů v Azure Chcete-li služby Active Directory.

> [!TIP]
> Můžete také pro Pingboard povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>Konfigurace automatického uživatele zřizování účtu k Pingboard v Azure Active Directory:

1)  V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2) Pokud jste již nakonfigurovali Pingboard pro jednotné přihlašování, vyhledejte instanci Pingboard pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Pingboard** v galerii aplikací. Vyberte **Pingboard** ve výsledcích hledání a přidejte ji do seznamu aplikací.

3)  Vyberte instanci Pingboard a pak vyberte **zřizování** kartě.

4)  Nastavte **režimu zřizování** k **automatické**.

![Zřizování Pingboard](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) V části přihlašovací údaje správce proveďte následující kroky:

* V **URL klienta** textovému poli, zadejte `https://your_domain.pingboard.com/scim/v2` a nahraďte Vaše_Doména skutečné domény
* Přihlaste se k [Pingboard](https://pingboard.com/) pomocí účtu správce.
* Klikněte na doplňky > integrace > Azure Active Directory.
* Klikněte na kartu Konfigurace a vyberte **povolit zřizování uživatelů z Azure**.
* Kopírování **tokenu nosiče OAuth** pole a zadejte do **tajný klíč tokenu** textové pole.

6) Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure Active Directory se může připojit k aplikaci Pingboard. Pokud se nepovede připojit, ujistěte se, váš účet Pingboard má oprávnění správce a zkuste to **"Test připojení"** krok opakujte.

7) Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka následující.

8) Klikněte na **Uložit**. 

9) V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Pingboard**.

10) V **mapování atributů** , projděte si uživatelské atributy, které mají být synchronizovány ze služby Azure Active Directory k Pingboard. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Pingboard pro operace aktualizace. Vyberte **Uložit** tlačítko potvrzení změny. Další informace najdete v tématu [přizpůsobení atribut mapování uživatelů zřizování](active-directory-saas-customizing-attribute-mappings.md).

11) Povolit službě Azure Active Directory zřizování služby pro Pingboard, změňte **Stav zřizování** k **na** v **nastavení** části

12) Klikněte na tlačítko **Uložit** se spustit počáteční synchronizaci uživatelů se přiřadila Pingboard.

Počáteční trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Pingboard.

Další informace o tom, jak číst Azure Active Directory zřizování protokoly najdete v tématu [zprávy o zřizování účtu automatické uživatele](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Další prostředky

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-pingboard-tutorial.md)
