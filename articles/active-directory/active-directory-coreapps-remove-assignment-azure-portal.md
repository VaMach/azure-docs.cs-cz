---
title: "Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Postup přiřazení přístup uživatele nebo skupinu odebrat z podnikové aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 99cbc54b4daa988dbf741275ce92d1c863af6720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory
Je snadno odebrat uživatele nebo skupiny z se přiřadí přístup k jednomu z vaší podnikové aplikace v Azure Active Directory (Azure AD). Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Jak odebrat uživatele nebo přiřazení skupiny?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.
3. Na **Azure Active Directory – *directoryname***  okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Na **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí seznam aplikací, které můžete spravovat.
5. Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **uživatelé a skupiny**.

    ![Výběr uživatelů nebo skupin](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Na ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte jednu z další uživatele nebo skupiny a pak vyberte **odebrat** příkaz. Zkontrolujte vaše rozhodnutí příkazového řádku.

    ![Vyberte příkaz odebrat](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Další kroky
* [Zobrazit všechny moje skupin](active-directory-groups-view-azure-portal.md)
* [Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)
* [Zakázat přihlášení uživatele pro aplikaci, enterprise](active-directory-coreapps-disable-app-azure-portal.md)
* [Změna názvu nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
