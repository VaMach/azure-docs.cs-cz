---
title: "Přiřadit uživatele nebo skupinu enterprise aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak vybrat aplikaci enterprise přiřadit uživatele nebo skupinu k němu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.openlocfilehash: 8e61044f261033a473241e2de152026bf49c4c70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Přiřadit uživatele nebo skupinu enterprise aplikace v Azure Active Directory
Je snadné přiřadit uživatele nebo skupinu pro vaše podnikové aplikace v Azure Active Directory (Azure AD). Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Přiřazení přístupu uživatelů k aplikaci enterprise
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte do textového pole Azure Active Directory a potom vyberte **Enter**.
3. Na **Azure Active Directory – *directoryname***  okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Na **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí seznam aplikací, které můžete spravovat.
5. Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **uživatelé a skupiny**.

    ![Výběr příkaz všechny aplikace](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Na ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte **přidat** příkaz.
8. Na **přidat přiřazení** vyberte **uživatelů a skupin**.

    ![Přiřazení uživatele nebo skupiny do aplikace](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Na **uživatelů a skupin** okně vyberte jeden nebo více uživatelů nebo skupin ze seznamu a pak vyberte **vyberte** tlačítko v dolní části okna.
10. Na **přidat přiřazení** vyberte **Role**. Potom na **vybrat roli** okně, vyberte roli, které chcete použít na vybrané uživatele nebo skupiny a pak vyberte **OK** tlačítko v dolní části okna.
11. Na **přidat přiřazení** okně, vyberte **přiřadit** tlačítko v dolní části okna. Přiřazené uživatele nebo skupiny, bude mít oprávnění definovaná podle vybrané role pro tuto aplikaci enterprise.

## <a name="next-steps"></a>Další kroky
* [Zobrazit všechny moje skupin](active-directory-groups-view-azure-portal.md)
* [Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Zakázat přihlášení uživatele pro aplikaci, enterprise](active-directory-coreapps-disable-app-azure-portal.md)
* [Změna názvu nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
