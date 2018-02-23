---
title: "Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Postup přiřazení přístup uživatele nebo skupinu odebrat z podnikové aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 084ffcbe473290a8734b1c8b8847b517dac4f6b6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory
Je snadno odebrat uživatele nebo skupiny z se přiřadí přístup k jednomu z vaší podnikové aplikace v Azure Active Directory (Azure AD). Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.

> [!NOTE]
> Pro Microsoft Applications (například aplikace Office 365) pomocí prostředí PowerShell odstranit uživatelům podnikové aplikace.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Jak odebrat uživatele nebo skupiny přiřazení firemní aplikace na portálu Azure?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.
3. Na **Azure Active Directory - *directoryname***  (to znamená, Azure AD stránce pro adresář spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Na **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí seznam aplikací, které můžete spravovat.
5. Na **podnikové aplikace – všechny aplikace** vyberte aplikaci.
6. Na ***appname*** (to znamená, že stránka s názvem vybranou aplikaci v názvu), vyberte **uživatelé a skupiny**.

    ![Výběr uživatelů nebo skupin](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Na ***appname*** **-uživatele & přiřazení skupiny** stránky, vyberte jednu z další uživatele nebo skupiny a pak vyberte **odebrat** příkaz. Zkontrolujte vaše rozhodnutí příkazového řádku.

    ![Vyberte příkaz odebrat](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Jak odebrat uživatele nebo skupiny přiřazení podnikové aplikace pomocí prostředí PowerShell?
1. Otevřete příkazový řádek se zvýšenými oprávněními prostředí Windows PowerShell.

    >[!NOTE] 
    > Potřebujete nainstalovat modul AzureAD (použijte příkaz `Install-Module -Name AzureAD`). Pokud se zobrazí výzva k instalaci modulu NuGet nebo nového modulu Azure Active Directory V2 PowerShell, zadejte Y a stiskněte klávesu ENTER.

2. Spustit `Connect-AzureAD` a přihlaste se pomocí uživatelského účtu globálního správce.
3. Pomocí následujícího skriptu přiřadit uživatele a role k aplikaci:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ``` 
## <a name="next-steps"></a>Další postup

- [Zobrazit všechny moje skupin](active-directory-groups-view-azure-portal.md)
- [Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)
- [Zakázat přihlášení uživatele pro aplikaci, enterprise](active-directory-coreapps-disable-app-azure-portal.md)
- [Změna názvu nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
