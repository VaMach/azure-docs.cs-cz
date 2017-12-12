---
title: "Správce tenanta zvýšení přístupu – Azure AD | Microsoft Docs"
description: "Toto téma popisuje předdefinovaných do rolí pro řízení přístupu na základě role (RBAC)."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: andredm
ms.openlocfilehash: 894ccd13684a79590b75821514ef6922abb8fdaf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Zvýšení přístupu jako správce klienta s řízením přístupu na základě rolí

Řízení přístupu na základě rolí pomáhá správci klientů získat dočasný – zvýšení úrovní oprávnění přístupu tak, aby se můžete udělit oprávnění vyšší než normální. Správce klienta můžete zvýšit samu sebe roli správce přístupu uživatelů v případě potřeby. Tato role poskytuje klienta oprávnění správce k udělení samu sebe nebo ostatní role v oboru "/".

Tato funkce je důležitá, protože umožňuje Správce tenanta zobrazíte všechny odběry, které existují v organizaci. Umožňuje také pro automatizaci aplikace, jako je fakturace a auditování pro přístup k Všechna předplatná a přesné informace o stavu organizace přinášejí fakturace nebo správu prostředků.  

## <a name="use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>ElevateAccess můžete používat pro přístup klientů pomocí centra pro správu Azure AD

1. Přejděte na [centra pro správu Azure Active Directory](https://aad.portal.azure.com) a přihlaste se pomocí můžete přihlašovací údaje.

2. Zvolte **vlastnosti** z Azure AD levé nabídce.

3. V **vlastnosti** okně Najít **globálního správce můžete spravovat předplatná Azure**, zvolte **Ano**, pak **Uložit**.
    > [!IMPORTANT] 
    > Pokud vyberete **Ano**, přiřadí **správce přístupu uživatelů** role v kořenovém adresáři "/" (kořenovém oboru) pro uživatele, ke kterému jste teď přihlášení k portálu. **To umožňuje uživatelům zobrazit všechny ostatní předplatných Azure.**
    
    > [!NOTE] 
    > Pokud vyberete **ne**, odebere **správce přístupu uživatelů** role v kořenovém adresáři "/" (kořenovém oboru) pro uživatele, ke kterému jste teď přihlášení k portálu.

> [!TIP] 
> Dojem je, že toto je globální vlastnost pro Azure Active Directory, ale funguje na jednotlivé uživatele pro aktuálně přihlášeného uživatele. Pokud máte práva globálního správce ve službě Azure Active Directory, můžete použít funkci elevateAccess pro uživatele, který jste právě přihlášení do centra pro správu Azure Active Directory.

![Globaladmin centra pro správu – vlastnosti – Azure AD Spravovat předplatné Azure – snímek obrazovky](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="view-role-assignments-at-the--scope-using-powershell"></a>Přiřazení role zobrazení v oboru "/" pomocí prostředí PowerShell
Chcete-li zobrazit **správce přístupu uživatelů** přiřazení v  **/**  obor, použijte `Get-AzureRmRoleAssignment` rutiny prostředí PowerShell.
    
```
Get-AzureRmRoleAssignment* | where {$_.RoleDefinitionName -eq "User Access Administrator" -and $_SignInName -eq "<username@somedomain.com>" -and $_.Scope -eq "/"}
```

**Příklad výstupu**:

RoleAssignmentId: /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0    
Obor: /    
DisplayName: uživatelské jméno    
SignInName:username@somedomain.com    
RoleDefinitionName: Správce přístupu uživatelů    
Hodnoty vlastnosti RoleDefinitionId: 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9    
ObjectId: d65fd0e9-c185-472c-8f26-1dafa01f72cc    
ObjectType: uživatele    

## <a name="delete-the-role-assignment-at--scope-using-powershell"></a>Umožňuje odstranit přiřazení role v "/" obor pomocí prostředí Powershell:
Odstraněním přiřazení pomocí následující rutiny prostředí PowerShell:
```
Remove-AzureRmRoleAssignment -SignInName <username@somedomain.com> -RoleDefinitionName "User Access Administrator" -Scope "/" 
```

## <a name="use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>Použít elevateAccess klienta přístup pomocí rozhraní REST API

Základní proces funguje pomocí následujících kroků:

1. Pomocí REST, volání *elevateAccess*, která vám uděluje role správce přístupu uživatelů v "/" oboru.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Vytvoření [přiřazení role](/rest/api/authorization/roleassignments) přiřadit žádnou roli v jakémkoli oboru. Následující příklad ukazuje vlastnosti přiřazení role čtenáře v "/" oboru:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Při přístupu správce uživatele můžete můžete také odstranit přiřazení role v "/" oboru.

4. Vaše oprávnění správce přístupu uživatelů odvolat, dokud se znovu požadován.


## <a name="how-to-undo-the-elevateaccess-action-with-the-rest-api"></a>Vrácení zpět elevateAccess akce s rozhraní REST API

Při volání *elevateAccess* vytvořit přiřazení role pro sebe, aby tato oprávnění odvolat, budete muset odstranit přiřazení.

1.  Volání GET definice rolí kde roleName = správce přístupu uživatelů k určení názvu GUID role správce přístupu uživatelů.
    1.  ZÍSKAT *https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$ filter = roleName + eq +'+ správce přístupu uživatelů +*

        ```
        {"value":[{"properties":{
        "roleName":"User Access Administrator",
        "type":"BuiltInRole",
        "description":"Lets you manage user access to Azure resources.",
        "assignableScopes":["/"],
        "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
        "createdOn":"0001-01-01T08:00:00.0000000Z",
        "updatedOn":"2016-05-31T23:14:04.6964687Z",
        "createdBy":null,
        "updatedBy":null},
        "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
        "type":"Microsoft.Authorization/roleDefinitions",
        "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
        "nextLink":null}
        ```

        Uložte identifikátor GUID z *název* parametr v tomto případě **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Musíte také seznam přiřazení role pro správce tenanta na obor klienta. Zobrazí seznam všech přiřazení v obor klienta pro PrincipalId ze správce, který provedl přístup zvýšení volání. To se zobrazí seznam všech přiřazení v klientovi pro ObjectID. 
    1. ZÍSKAT *https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$ filter = principalId + eq + {objectid}*
    
        >[!NOTE] 
        >Správce klienta by neměl mít mnoho přiřazení, pokud dotaz výše vrátí příliš mnoho přiřazení, se můžete dotazovat i pro všechna přiřazení právě na úrovni oboru klienta a pak filtrovat výsledky: získání *https://management.azure.com/providers/ Microsoft.Authorization/roleAssignments? api-version = 2015-07-01 & $filter=atScope()*
        
    2. Výše uvedené volání vrátí seznam přiřazení rolí. Najít přiřazení role, kde je oboru "/" a končí hodnoty vlastnosti RoleDefinitionId s názvem role GUID, které jste získali v kroku 1 a PrincipalId odpovídá ObjectId správce klienta. Přiřazení role vypadá takto:

        ```
        {"value":[{"properties":{
        "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
        "principalId":"{objectID}",
        "scope":"/",
        "createdOn":"2016-08-17T19:21:16.3422480Z",
        "updatedOn":"2016-08-17T19:21:16.3422480Z",
        "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
        "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
        "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
        "type":"Microsoft.Authorization/roleAssignments",
        "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
        "nextLink":null}
        ```
        
        Znovu, uložte identifikátor GUID z *název* parametr v tomto případě **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

    3. Nakonec použijte zvýrazněné **RoleAssignment ID** odstranit přiřazení přidal zvýšení oprávnění k přístupu:

        Odstranit https://management.azure.com /providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01

## <a name="next-steps"></a>Další kroky

- Další informace o [Správa řízení přístupu na základě rolí pomocí REST](role-based-access-control-manage-access-rest.md)

- [Spravovat přístup k přiřazení](role-based-access-control-manage-assignments.md) na portálu Azure
