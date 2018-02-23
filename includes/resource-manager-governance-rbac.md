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
ms.openlocfilehash: 24863e239620f84a57c631b3ecf5b08997de31c5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
Chcete Ujistěte se, že uživatelé ve vaší organizaci mají správnou úroveň přístupu na tyto prostředky. Nechcete, aby uživatelům udělit neomezený přístup, ale musíte také zkontrolujte, zda že mohou provádět práci. Řízení přístupu na základě role (RBAC) umožňuje spravovat uživatele, kteří mají oprávnění k dokončení určitých akcí u oboru. Role definuje sadu povolených akcí. Obor přiřazení role a zadat uživatele, kteří patří do této role pro obor.

Při plánování strategie řízení přístupu, udělte uživatelům nejnižší oprávnění k práci. Následující obrázek znázorňuje navrhované vzor přiřazení RBAC.

![Rozsah](./media/resource-manager-governance-rbac/role-examples.png)

Existují tři role, které platí pro všechny prostředky - vlastník, Přispěvatel a čtečky. Všechny účty přiřazené k roli vlastníka by měl být důkladné kontrole a je používána zřídka. Uživatelům, kteří potřebují jenom sledovat stav řešení, je třeba poskytnout role Čtenář.

Většina uživatelů jsou udělena [rolím pro konkrétní prostředky](../articles/active-directory/role-based-access-built-in-roles.md) nebo [vlastní role](../articles/active-directory/role-based-access-control-custom-roles.md) na úrovni skupiny předplatné nebo prostředek. Tyto role úzce definovat povolené akce. Přiřadit uživatele k tyto role, udělíte požadovaný přístup pro uživatele bez umožňující příliš mnoho ovládacího prvku. Účet můžete přiřadit více než jedné role, a tento uživatel získá kombinované oprávnění rolí. Udělení přístupu na úrovni prostředků je často příliš omezující pro uživatele, ale může fungovat pro automatizovaný proces určené pro konkrétní úlohu.

### <a name="who-can-assign-roles"></a>Přiřazování rolí

Vytvoření a odeberte přiřazení rolí, uživatelé musí mít `Microsoft.Authorization/roleAssignments/*` přístup. Tento přístup je poskytována prostřednictvím role vlastníka nebo správce uživatelského přístupu.