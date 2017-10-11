---
title: "Přiřazení uživatele k rolí správce ve službě Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak změnit správce informace o uživateli v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: bfadf133154488f9827cfbeaa98ddb0eb84b52f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Přiřazení uživatele k rolí správce ve službě Azure Active Directory
Tento článek vysvětluje, jak přiřadit roli správce pro uživatele v Azure Active Directory (Azure AD). Informace o přidávání nových uživatelů v organizaci najdete v tématu [přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md). Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.

## <a name="assign-a-role-to-a-user"></a>Přiřazení role uživatele
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. Na **uživatelů a skupin** vyberte **všichni uživatelé**.

   ![Otevření okna všechny uživatele](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. Na **uživatelé a skupiny - všichni uživatelé** okně vyberte uživatele ze seznamu.
5. V okně pro vybraného uživatele, vyberte **Directory role**a pak mu přiřaďte uživatele k roli z **Directory role** seznamu. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).

      ![Přiřazení uživatele k roli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
* [Přidat uživatele](active-directory-users-create-azure-portal.md)
* [Resetovat heslo uživatele v nový portál Azure](active-directory-users-reset-password-azure-portal.md)
* [Změnit informace o práci uživatele](active-directory-users-work-info-azure-portal.md)
* [Správa uživatelských profilů](active-directory-users-profile-azure-portal.md)
* [Odstranění uživatele ve službě Azure AD](active-directory-users-delete-user-azure-portal.md)
