---
title: "Správce tenanta zvýšení přístupu – Azure AD | Microsoft Docs"
description: "Toto téma popisuje předdefinovaných do rolí pro řízení přístupu na základě role (RBAC)."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 22b62be1773c5042ecf6ee078e68a4ffdf791d53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Zvýšení přístupu jako správce klienta s řízením přístupu na základě rolí

Řízení přístupu na základě rolí pomáhá správci klientů získat dočasný – zvýšení úrovní oprávnění přístupu tak, aby se můžete udělit oprávnění vyšší než normální. Správce klienta můžete zvýšit samu sebe roli správce přístupu uživatelů v případě potřeby. Tato role poskytuje klienta oprávnění správce k udělení samu sebe nebo ostatní role v oboru "/".

Tato funkce je důležitá, protože umožňuje Správce tenanta zobrazíte všechny odběry, které existují v organizaci. Také to umožňuje pro aplikace, automatizace (jako je fakturace a auditování) pro přístup k Všechna předplatná a poskytují přesné informace o stavu organizace pro správu fakturace nebo asset.  

## <a name="how-to-use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>Postup použití elevateAccess ke klientovi přístup pomocí centra pro správu Azure AD

V [centra pro správu Azure Active Directory](https://aad.portal.azure.com) můžete vyvolat tuto funkci z **vlastnosti**.
Tato funkce je volána **globálního správce můžete spravovat předplatná Azure**. Dojem je, že toto je globální vlastnost pro Azure Active Directory, ale funguje na jednotlivé uživatele pro aktuálně přihlášeného uživatele. Pokud máte práva globálního správce ve službě Azure Active Directory, můžete použít funkci elevateAccess pro uživatele, který jste právě přihlášení do centra pro správu Azure Active Directory.

Výběr **Ano** a potom **Uložit**: to **přiřadí** **správce přístupu uživatelů** role v kořenovém adresáři "/" (kořenovém oboru) pro uživatele s který jste právě přihlášení k portálu.

Výběr **ne** a potom **Uložit**: to **odebere** **správce přístupu uživatelů** role v kořenovém adresáři "/" (kořenovém oboru) pro uživatele s který jste právě přihlášení k portálu.

![Globaladmin centra pro správu – vlastnosti – Azure AD Spravovat předplatné Azure – snímek obrazovky](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="how-to-use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>Jak používat elevateAccess klienta přístup pomocí rozhraní REST API

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

1.  Volání [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) kde roleName = správce přístupu uživatelů k určení názvu GUID role správce přístupu uživatelů. Odpověď by měla vypadat takto:

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

2. Volání [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) kde principalId = vlastní ObjectId. Rutina Vypíše seznam všech přiřazení v klientovi. Vyhledejte ten, kde je oboru "/" a končí hodnoty vlastnosti RoleDefinitionId s názvem role GUID, které jste získali v kroku 1. Přiřazení role by měl vypadat takto:

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

3. Nakonec volání [roleAssignments odstranění](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) kde roleAssignmentId = název GUID, které jste získali v kroku 2.

## <a name="next-steps"></a>Další kroky

- Další informace o [Správa řízení přístupu na základě rolí pomocí REST](role-based-access-control-manage-access-rest.md)

- [Spravovat přístup k přiřazení](role-based-access-control-manage-assignments.md) na portálu Azure
