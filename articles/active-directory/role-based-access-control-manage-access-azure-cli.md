---
title: "Správa řízení přístupu na základě rolí (RBAC) pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat na základě rolí řízení přístupu (RBAC) pomocí rozhraní příkazového řádku Azure tak, že uvedete role a role akce a přiřazení rolí k oborům předplatné a aplikace."
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 5c099a7fd8848c2934603ec9b2db8947885226f9
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Správa řízení přístupu na základě rolí pomocí rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


S na základě rolí řízení přístupu (RBAC), můžete definovat přístupu pro uživatele, skupiny a objekty služby podle přiřazení rolí v určitém rozsahu. Tento článek popisuje, jak můžete spravovat přístup pomocí rozhraní příkazového řádku Azure (CLI).

## <a name="prerequisites"></a>Požadavky

Chcete-li spravovat RBAC pomocí rozhraní příkazového řádku Azure, musíte mít následující požadavky:

* [Azure CLI 2.0](/cli/azure/overview). Můžete ho používat v prohlížeči přes [Azure Cloud Shell](../cloud-shell/overview.md) nebo si ho můžete [nainstalovat](/cli/azure/install-azure-cli) v systémech macOS, Linux nebo Windows a spouštět z příkazového řádku.

## <a name="list-roles"></a>Seznam rolí

### <a name="list-role-definitions"></a>Definice rolí seznamu

K zobrazení seznamu všech definic rolí k dispozici, použijte [seznamu definice role az](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list
```

Následující příklad uvádí název a popis všech definic rolí k dispozici:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Následující příklad vypíše všechny definice předdefinovaná role:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description, "type":.properties.type}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "type": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role-definition"></a>Seznam akcí definici role

K zobrazení seznamu akce definici role, použijte [seznamu definice role az](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list --name <role_name>
```

Následující příklad zobrazí *Přispěvatel* definice role:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "properties": {
      "additionalProperties": {
        "createdBy": null,
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedBy": null,
        "updatedOn": "2016-12-14T02:04:45.1393855Z"
      },
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything except access to resources.",
      "permissions": [
        {
          "actions": [
            "*"
          ],
          "notActions": [
            "Microsoft.Authorization/*/Delete",
            "Microsoft.Authorization/*/Write",
            "Microsoft.Authorization/elevateAccess/Action"
          ]
        }
      ],
      "roleName": "Contributor",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Následující příklad zobrazí *akce* a *notActions* z *Přispěvatel* role:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.properties.permissions[0].actions, "notActions":.properties.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Následující příklad uvádí akce *Přispěvatel virtuálních počítačů* role:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .properties.permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Přístup k seznamu

### <a name="list-role-assignments-for-a-user"></a>Seznam přiřazení role pro uživatele

K zobrazení seznamu přiřazení rolí pro konkrétního uživatele, použijte [seznamu přiřazení role az](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --assignee <assignee>
```

Ve výchozím nastavení se zobrazí pouze přiřazení obor k předplatnému. Chcete-li zobrazit přiřazení vymezeny prostředek nebo skupina, použijte `--all`.

Následující příklad vypíše přiřazení rolí, které jsou přiřazeny přímo na  *patlong@contoso.com*  uživatele:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.properties.principalName, "roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Seznam přiřazení rolí pro skupinu prostředků.

K zobrazení seznamu přiřazení rolí, které pro skupinu prostředků, použijte [seznamu přiřazení role az](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Následující příklad vypíše přiřazení rolí pro *pharma. prodej projectforecast* skupiny prostředků:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="assign-access"></a>Přiřadit přístup pro

### <a name="assign-a-role-to-a-user"></a>Přiřazení role uživatele

Chcete-li přiřadit role pro uživatele v oboru skupiny prostředků, použijte [vytvořit přiřazení role az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Následující příklad přiřadí *Přispěvatel virtuálních počítačů* role  *patlong@contoso.com*  uživatel na *pharma. prodej projectforecast* oboru skupiny prostředků:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="assign-a-role-to-a-group"></a>Přiřazení role do skupiny

Chcete-li přiřadit roli do skupiny, použijte [vytvořit přiřazení role az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Následující příklad přiřadí *čtečky* role *Ann Mack Team* skupiny s ID 22222222-2222-2222-2222-222222222222 na obor předplatného. Chcete-li získat ID skupiny, můžete použít [seznam skupiny ad az](/cli/azure/ad/group#az_ad_group_list) nebo [zobrazit skupiny ad az](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Následující příklad přiřadí *Přispěvatel virtuálních počítačů* role *Ann Mack Team* skupiny s ID 22222222-2222-2222-2222-222222222222 v oboru prostředků pro virtuální síť s názvem *pharma prodej projektu sítí*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="assign-a-role-to-an-application"></a>Přiřazení role k aplikaci

K přiřazení rolí k aplikaci, použijte [vytvořit přiřazení role az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Následující příklad přiřadí *Přispěvatel virtuálních počítačů* role aplikace s 44444444-4444-4444-4444-444444444444 ID objektu na *pharma. prodej projectforecast* skupiny prostředků rozsah. Chcete-li získat ID objektu aplikace, můžete použít [seznamu aplikací ad az](/cli/azure/ad/app#az_ad_app_list) nebo [zobrazení aplikací ad az](/cli/azure/ad/app#az_ad_app_show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Odebrání přístupu

### <a name="remove-a-role-assignment"></a>Odebrat přiřazení role

Pokud chcete odstranit přiřazení role, použijte [odstranit přiřazení role az](/cli/azure/role/assignment#az_role_assignment_delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Následující příklad odebere *Přispěvatel virtuálních počítačů* přiřazení role z  *patlong@contoso.com*  uživatele na *pharma. prodej projectforecast* prostředků Skupina:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Následující příklad odebere *čtečky* roli z *Ann Mack Team* skupiny s ID 22222222-2222-2222-2222-222222222222 na obor předplatného. Chcete-li získat ID skupiny, můžete použít [seznam skupiny ad az](/cli/azure/ad/group#az_ad_group_list) nebo [zobrazit skupiny ad az](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="custom-roles"></a>Vlastní role

### <a name="list-custom-roles"></a>Vlastní role seznamu

K zobrazení seznamu rolí, které jsou k dispozici pro přiřazení v oboru, použijte [seznamu definice role az](/cli/azure/role/definition#az_role_definition_list).

Obě následující příklady seznamu všechny vlastní role v aktuálním předplatném:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.properties.roleName, "type":.properties.type}'
```

```azurecli
az role definition list --output json | jq '.[] | if .properties.type == "CustomRole" then {"roleName":.properties.roleName, "type":.properties.type} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Operator Role",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}

...
```

### <a name="create-a-custom-role"></a>Vytvořit vlastní roli

Chcete-li vytvořit vlastní roli, použijte [vytvoření definice role az](/cli/azure/role/definition#az_role_definition_create). Definice role může být popis JSON nebo cestu k souboru, který obsahuje popis JSON.

```azurecli
az role definition create --role-definition <role_definition>
```

Následující příklad vytvoří vlastní roli s názvem *virtuální počítač operátor*. Tuto vlastní roli přiřadí přístup ke všem operacím čtení z *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* přístup k prostředkům zprostředkovatelé a přiřadí Pokud chcete spustit, restartujte a monitorování virtuálních počítačů. Tuto vlastní roli můžete použít ve dvou předplatných. Tento příklad používá soubor JSON jako vstup.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

### <a name="update-a-custom-role"></a>Aktualizovat vlastní role

Chcete-li aktualizovat vlastní role, nejprve použijte [seznamu definice role az](/cli/azure/role/definition#az_role_definition_list) načíst definici role. Za druhé proveďte požadované změny k definici role. Nakonec použijte [aktualizaci definice role az](/cli/azure/role/definition#az_role_definition_update) se uložit definici aktualizované role.

```azurecli
az role definition update --role-definition <role_definition>
```

Následující příklad přidá *Microsoft.Insights/diagnosticSettings/* operace *akce* z *virtuální počítač operátor* vlastní role.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

### <a name="delete-a-custom-role"></a>Odstranit vlastní roli

Chcete-li odstranit vlastní roli, použijte [odstranit definici role az](/cli/azure/role/definition#az_role_definition_delete). Chcete-li zadat role, kterou chcete odstranit, použijte název role nebo ID role. Chcete-li zjistit role ID, použijte [seznamu definice role az](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Následující příklad odstraní *virtuální počítač operátor* vlastní role:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Další postup

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

