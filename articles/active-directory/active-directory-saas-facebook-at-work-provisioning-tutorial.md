---
title: "Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Naučte se automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD k firemní síti pomocí sítě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 2fc2b07189994206c8465ed24374489eefd45966
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování automatické uživatelů

Tento kurz popisuje kroky nutné automaticky zřizovat a deaktivace zřízení uživatelských účtů z Azure Active Directory (Azure AD) k síti na pracovišti ve službě Facebook.

>[!NOTE]
>Tento kurz popisuje spojnice postavená na službu zřizování uživatele Azure AD. Důležité podrobnosti o jaké této služby, jak to funguje a nejčastější dotazy, najdete v tématu [automatizace zřizování uživatelů a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Požadavky

Při konfiguraci integrace Azure AD se na pracovišti ve službě Facebook, budete potřebovat následující:

- Předplatné služby Azure AD
- Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)

Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [nabídka zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-workplace-by-facebook"></a>Přiřazení uživatelů k síti na pracovišti ve službě Facebook.

Azure AD používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly přiřazeny k aplikaci ve službě Azure AD.

Než nakonfigurujete a povolíte službu zřizování, rozhodněte, jaké uživatelů a skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k pracovní ploše aplikace Facebook. Lze potom přiřadit těmto uživatelům k pracovní ploše pomocí aplikace Facebook podle pokynů v [přiřadit uživatele nebo skupinu enterprise aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Otestujte konfiguraci zřizování přiřazením jednoho uživatele Azure AD k firemní síti pomocí sítě Facebook. Přiřaďte dalších uživatelů a skupin později.
>*   Když přiřadíte uživatele k firemní síti pomocí sítě Facebook, musíte vybrat platné uživatelské role. Roli výchozí přístup nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit zřizování automatizované uživatelů

Tato část vás provede připojení služby Azure AD s uživatelským účtem zřizování API pracovního místa ve službě Facebook. Také zjistíte, jak nakonfigurovat službu zřizování vytvářet, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook. To je založené na uživatele a přiřazení skupiny ve službě Azure AD.

>[!Tip]
>Můžete také povolit na základě SAML jednotného přihlašování pro pracoviště ve službě Facebook, pomocí postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Konfigurace uživatelského účtu ve službě Azure AD zřizování k síti na pracovišti ve službě Facebook.

Azure AD podporuje možnost automaticky synchronizovat Podrobnosti účtu přiřazené uživatelů k firemní síti pomocí sítě Facebook. Toto automatické synchronizace umožňuje síti na pracovišti ve službě Facebook se získat data, je nutné autorizovat uživatele pro přístup, než je pokus o přihlášení za poprvé. Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.

1. V [portál Azure](https://portal.azure.com), vyberte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.

2. Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte pomocí pole hledání instanci síti na pracovišti ve službě Facebook. Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací. Vyberte **síti na pracovišti ve službě Facebook** ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte **zřizování** kartě.

4. Nastavit **režimu zřizování** k **automatické**. 

    ![Snímek obrazovky pracovního místa ve službě Facebook možnosti zřizování](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. V části **přihlašovací údaje správce** zadejte **tajný klíč tokenu** a **URL klienta** ze svého pracoviště správcem sítě Facebook.

6. Na portálu Azure vyberte **Test připojení** zajistit Azure AD může připojit k pracovní ploše aplikace Facebook. Pokud se nepovede připojit, zajistěte, aby vaše pracoviště pomocí účtu sítě Facebook oprávnění správce týmu.

7. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

8. Vyberte **Uložit**.

9. V části mapování vyberte **synchronizaci Azure Active Directory Users k firemní síti pomocí sítě Facebook**.

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizovány z Azure AD k firemní síti pomocí sítě Facebook. Atributy vybrán jako **párování** vlastnosti jsou používány tak, aby odpovídaly uživatelské účty v síti na pracovišti Facebook pro operace aktualizace. Všechny změny potvrdit, vyberte **Uložit**.

11. Chcete-li povolit zřizování služby pro pracoviště ve službě Facebook, v Azure AD **nastavení** změňte **Stav zřizování** k **na**.

12. Vyberte **Uložit**.

Další informace o tom, jak nakonfigurovat automatické zřizování najdete v tématu [dokumentace Facebook](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Nyní můžete vytvořit testovací účet. Chcete-li ověřit, že účet byl synchronizován k síti na pracovišti ve službě Facebook Počkejte až 20 minut.

## <a name="additional-resources"></a>Další zdroje informací:

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-facebook-at-work-tutorial.md)

