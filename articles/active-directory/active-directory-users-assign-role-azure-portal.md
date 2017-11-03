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
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: 7574fdf8787e799e4d73fe32df9c1ad5be93abf1
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Přiřazení uživatele k rolí správce ve službě Azure Active Directory
Tento článek vysvětluje, jak přiřadit roli správce pro uživatele v Azure Active Directory (Azure AD). Informace o přidávání nových uživatelů v organizaci najdete v tématu [přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md). Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.

## <a name="assign-a-role-to-a-user"></a>Přiřazení role uživatele
1. Přihlaste se k [centru pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který má k adresáři oprávnění globálního správce.
2. Vyberte **uživatelů a skupin**.

   ![Správa uživatelů otevírání](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. Vyberte **všichni uživatelé**.

   ![Otevírání všechny skupiny uživatelů](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
1. Vyberte uživatele ze seznamu.
2. Pro vybraného uživatele, vyberte **Directory role** a pak mu přiřaďte uživatele k roli z **Directory role** seznamu. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).

      ![Přiřazení uživatele k roli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
* [Přidat uživatele](active-directory-users-create-azure-portal.md)
* [Resetovat heslo uživatele v nový portál Azure](active-directory-users-reset-password-azure-portal.md)
* [Změnit informace o práci uživatele](active-directory-users-work-info-azure-portal.md)
* [Správa uživatelských profilů](active-directory-users-profile-azure-portal.md)
* [Odstranění uživatele ve službě Azure AD](active-directory-users-delete-user-azure-portal.md)
