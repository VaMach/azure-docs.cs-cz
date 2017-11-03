---
title: "Správa řízení přístupu na základě rolí (RBAC) pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat na základě rolí řízení přístupu (RBAC) pomocí rozhraní příkazového řádku Azure tak, že uvedete role a role akce a přiřazení rolí k oborům předplatné a aplikace."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 77315171754304c965f296670fbba3a4751a3656
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Správa řízení přístupu na základě rolí pomocí rozhraní příkazového řádku Azure
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Řízení přístupu na základě Role (RBAC) na portálu Azure a rozhraní API služby Azure Resource Manager můžete použít ke správě přístupu k vaše předplatné a prostředky na velice přesné úrovni. Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role je v určitém rozsahu.

Ke správě RBAC mohli používat rozhraní příkazového řádku Azure (CLI), musíte mít následující požadavky:

* Azure CLI verze 0.8.8 nebo novější. Nainstalujte nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
* Azure Resource Manager v rozhraní příkazového řádku Azure. Přejděte na [pomocí rozhraní příkazového řádku Azure s Resource Managerem](../xplat-cli-azure-resource-manager.md) další podrobnosti.

## <a name="list-roles"></a>Seznam rolí
### <a name="list-all-available-roles"></a>Zobrazí seznam všech dostupných rolí
K zobrazení seznamu všech dostupných rolí, použijte:

        azure role list

Následující příklad ukazuje seznam *všechny dostupné role*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Seznam akcí role
K zobrazení seznamu akce role, použijte:

    azure role show "<role name>"

Následující příklad ukazuje akce *Přispěvatel* a *Přispěvatel virtuálních počítačů* role.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure příkazového řádku - azure role zobrazit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Přístup k seznamu
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Přiřazení rolí seznamu efektivní ve skupině prostředků.
K zobrazení seznamu přiřazení rolí, které existují ve skupině prostředků, použijte:

    azure role assignment list --resource-group <resource group name>

Následující příklad ukazuje přiřazení rolí v *pharma. prodej projecforcast* skupiny.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu podle skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Seznam přiřazení role pro uživatele
K zobrazení seznamu přiřazení rolí pro konkrétního uživatele a přiřazení, které jsou přiřazeny k skupin uživatelů, použijte:

    azure role assignment list --signInName <user email>

Můžete také zjistit přiřazení rolí, které se dědí z skupin změnou příkaz:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Následující příklad ukazuje, kterým je uděleno oprávnění k přiřazení rolí  *sameert@aaddemo.com*  uživatele. To zahrnuje role, které jsou přiřazeny přímo na uživatele a role, které se dědí z skupin.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu uživatelem – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Udělení přístupu
Po zjištění roli, kterou chcete přiřadit udělit přístup, použijte:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Přiřazení role do skupiny v oboru předplatného
K přiřazení role do skupiny v oboru předplatné, použijte:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Následující příklad přiřadí *čtečky* role *Jana Koch Team* na *předplatné* oboru.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Přiřazení role k aplikaci v oboru předplatného
K přiřazení role k aplikaci v oboru předplatné, použijte:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Následující příklad povolí *Přispěvatel* role *Azure AD* aplikace na vybraném předplatném.

 ![Příkaz Azure RBAC - přiřazení role v azure vytvořit aplikací](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Přiřazení role uživatele v oboru skupiny prostředků
Pokud chcete přiřadit role pro uživatele v oboru skupiny prostředků, použijte:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Následující příklad povolí *Přispěvatel virtuálních počítačů* role  *samert@aaddemo.com*  uživatel na *Pharma. prodej ProjectForcast* oboru skupiny prostředků.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Přiřazení role do skupiny v oboru prostředků
K přiřazení role do skupiny v oboru prostředků, použijte:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Následující příklad povolí *Přispěvatel virtuálních počítačů* role *Azure AD* na *podsítě*.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Odebrání přístupu
Chcete-li odebrat přiřazení role, použijte:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Následující příklad odebere *Přispěvatel virtuálních počítačů* přiřazení role z  *sammert@aaddemo.com*  uživatele na *Pharma. prodej ProjectForcast* prostředků Skupina.
Přiřazení role v příkladu pak odebere ze skupiny v odběru.

![RBAC Azure příkazového řádku - azure role přiřazení odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
Chcete-li vytvořit vlastní roli, použijte:

    azure role definition create --role-definition <file path>

Následující příklad vytvoří vlastní role s názvem *virtuální počítač operátor*. Tato vlastní role uděluje přístup ke všem operacím čtení z *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* zprostředkovatelé prostředků a uděluje přístup k spuštění, restartování a monitorování virtuálních počítačů. Tuto vlastní roli můžete použít ve dvou předplatných. Tento příklad používá soubor JSON jako vstup.

![JSON – definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure příkazového řádku - azure role vytvořit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Upravit vlastní roli
Chcete-li upravit vlastní roli, nejprve pomocí `azure role definition list` příkaz načíst definici role. Za druhé proveďte požadované změny do souboru definice role. Nakonec použijte `azure role definition update` se uložit definici upravené role.

    azure role definition update --role-definition <file path>

Následující příklad přidá *Microsoft.Insights/diagnosticSettings/* operace **akce**a předplatné Azure k **AssignableScopes** z Vlastní role operátora virtuálního počítače.

![JSON - upravit definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snímek obrazovky příkazového řádku - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Odstranit vlastní roli
Chcete-li odstranit vlastní roli, nejprve pomocí `azure role definition list` příkaz, abyste zjistili **ID** role. Potom použít `azure role definition delete` příkaz k odstranění role zadáním **ID**.

Následující příklad odebere *virtuální počítač operátor* vlastní role.

![RBAC Azure příkazového řádku - azure role odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Vlastní role seznamu
K zobrazení seznamu rolí, které jsou k dispozici pro přiřazení v oboru, použijte `azure role list` příkaz.

Následující příkaz vypíše všechny role, které jsou k dispozici pro přiřazení ve vybraném předplatném.

```
azure role definition list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

V následujícím příkladu *operátor virtuálního počítače* vlastní role není k dispozici v *Production4* předplatné vzhledem k tomu, že toto předplatné není v **AssignableScopes** role.

```
azure role definition list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure příkazového řádku - azure role seznam pro vlastní role – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Další kroky
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

