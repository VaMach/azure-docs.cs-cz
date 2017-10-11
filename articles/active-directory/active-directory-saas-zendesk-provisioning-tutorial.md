---
title: "Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do této služby."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů


Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Zendesku a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do služby ZenDesk. 

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory
*   Klienta služby ZenDesk s [plánu podnikového](https://www.zendesk.com/product/pricing/) nebo lépe povolena. 
*   Uživatelský účet v Zendesku se oprávnění správce 

> [!NOTE]
> Azure AD zřizování integrace spoléhá na [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), který je k dispozici pro týmy ZenDesk základní plán nebo lepší.

## <a name="assigning-users-to-zendesk"></a>Přiřazování uživatelů do této služby

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD. 

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci ZenDesk. Jakmile se rozhodli, můžete přiřadit tito uživatelé do vaší aplikace ZenDesk podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Důležité tipy pro přiřazování uživatelů do této služby

*   Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Zendesku a otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazení uživatele k této služby, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení. **Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.

> [!NOTE]
> Jako přidané funkce zřizování služba načte všechny vlastní role, které jsou definované v Zendesku a naimportuje je do Azure AD, kde mohou být vybrány v dialogovém okně vybrat roli. Tyto role se nebude zobrazovat na portálu Azure, po povolení zřizování služby a jeden synchronizační cyklus byla dokončena.

## <a name="configuring-user-provisioning-to-zendesk"></a>Konfiguraci zřizování uživatelů do této služby 

Tato část vás provede připojení k této služby je uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Zendesku podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP] 
> Také můžete povolit na základě SAML jednotné přihlašování pro ZenDesk, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Konfigurace automatického účet zřizování uživatelů do této služby ve službě Azure AD


1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali ZenDesk pro jednotné přihlašování, vyhledávání pro instanci služby ZenDesk pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **ZenDesk** v galerii aplikací. Vyberte ZenDesk ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci služby ZenDesk a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**.

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. V části **přihlašovací údaje správce** části, zadejte **uživatelské jméno správce & tokenkey & domény** generované účtu vaší ZenDesk (token můžete najít v rámci vašeho účtu: **správce** > **rozhraní API** > **nastavení**). 

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci služby ZenDesk. Pokud se nepovede připojit, ujistěte se, že vašeho účtu ZenDesk má oprávnění správce a opakujte krok 5.

7. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."

8. Klikněte na **Uložit**. 

9. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům ZenDesk**.

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD ZenDesk. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Zendesku pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

11. Povolit Azure AD zřizování služby pro služby ZenDesk, změňte **Stav zřizování** k **na** v **nastavení** části

12. Klikněte na **Uložit**. 

Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k této služby v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.

Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly](active-directory-saas-provisioning-reporting.md)
