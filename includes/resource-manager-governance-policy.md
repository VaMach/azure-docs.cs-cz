---
title: zahrnout soubor
description: zahrnout soubor
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c1e490aacdc9f712c6e186e56fe35fd5872fb575
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
[Azure zásady](/azure/azure-policy/) nápovědy byste si ověřit, všechny prostředky v předplatném splňovat firemními standardy. Pomocí zásad můžete snížit náklady na omezení možnosti nasazení pouze tyto typy prostředků a skladových položek, které jsou schváleny. Můžete definovat pravidla a akce pro vaše prostředky a tato pravidla se vynucují automaticky během nasazení. Například můžete řídit typy prostředků, které jsou nasazeny. Nebo můžete omezit schválené umístění pro prostředky. Některé zásady Odepřít akce a některé zásady, nastavit auditování akce.

Zásady je doplňkem k řízení přístupu na základě role (RBAC). RBAC se zaměřuje na uživatelský přístup, a je výchozí Odepřít a explicitní povolit systému. Zásady se zaměřuje na vlastnosti prostředku, během a po nasazení. Jeho výchozí povolit a explicitní odepřít systému.

Existují dvě koncepty zásadám – definice zásady a přiřazení zásad. Definice zásad popisuje správu podmínek, které chcete vynutit. Přiřazení zásad převádí definici zásady do akce pro konkrétní obor.

![Přiřadit zásady](./media/resource-manager-governance-policy/policy-concepts.png)

Azure nabízí několik předdefinovaných zásad definice, které můžete použít bez nutnosti jakékoli úpravy. Můžete předat hodnoty parametrů můžete zadat hodnoty, které jsou povoleny ve vašem oboru. Pokud definice předdefinovaných zásad nemáte splnění požadavků, můžete [vytvořit vlastní zásady definice](../articles/azure-policy/create-manage-policy.md).

### <a name="who-can-create-and-assign-policies"></a>Kdo může vytvořit a přiřadit zásady

Pokud chcete používat zásady, musíte být ověřeni pomocí RBAC. Konkrétně váš účet potřebuje:

* Oprávnění `Microsoft.Authorization/policydefinitions/write` pro definování zásady.
* Oprávnění `Microsoft.Authorization/policyassignments/write` pro přiřazení zásady.

Tato oprávnění nejsou součástí **Přispěvatel** role.
