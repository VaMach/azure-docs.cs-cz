---
title: "Pomocí řízení přístupu na základě rolí pro správu Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak použít a spravovat nasazení Azure Site Recovery pomocí řízení přístupu na základě Role (RBAC)"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: manayar
ms.openlocfilehash: 9dd74014bf05234a83c7678b67b42b96cd8b8d64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-site-recovery-deployments"></a>Pomocí řízení přístupu na základě rolí pro správu nasazení Azure Site Recovery

Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete v rámci týmu oddělit odpovědnosti a udělit pouze oprávnění k přístupu konkrétním uživatelům podle potřeby k provedení určité úlohy.

Azure Site Recovery poskytuje 3 předdefinovaných rolí k řízení operace správy Site Recovery. Další informace na [předdefinované role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

* [Lokality obnovení Přispěvatel](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) – tato role má všechna oprávnění vyžadovaná ke správě operací Azure Site Recovery v trezoru služeb zotavení. Uživatel k této roli, ale nelze vytvořit nebo odstranit trezor služeb zotavení nebo přiřadit přístupová práva ostatním uživatelům. Tato role je nejvhodnější pro správce obnovení po havárii, kteří můžete povolit a spravovat zotavení po havárii aplikace nebo celé organizace, jako možné.
* [Operátor obnovení lokality](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) – tato role nemá oprávnění ke spuštění a převzetí služeb při selhání a navrácení služeb po obnovení operations manager. S touto rolí uživatele nelze povolit nebo zakázat replikaci, vytvořit nebo odstranit trezory, zaregistrujte novou infrastrukturu nebo přiřadit přístupová práva jiným uživatelům. Tato role je nejvhodnější pro operátor obnovení po havárii, kdo může převzetí služeb při selhání virtuálního počítače nebo přejít k podrobnostem aplikace při pokyn vlastníci aplikace a správci IT v situacích skutečná nebo simulované po havárii, jako je například zotavení po Havárii. Po vyřešení po havárii, operátor zotavení po Havárii můžete znovu nastavit ochranu a navrácení služeb po obnovení virtuálních počítačů.
* [Lokality obnovení čtečky](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) – tato role nemá oprávnění k zobrazení všech operací správy Site Recovery. Tato role je nejvhodnější pro monitorování vedení IT, který můžete sledovat aktuální stav ochrany a zvýšení lístky žádostí o podporu, pokud je to nutné.

Pokud hledáte, můžete definovat vlastní role pro ještě větší kontrolu, přečtěte si postup [vytvářet vlastní role](../active-directory/role-based-access-control-custom-roles.md) v Azure.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Oprávnění potřebná k povolení replikace pro nové virtuální počítače
Když nový virtuální počítač se replikují do Azure pomocí Azure Site Recovery, se ověří úrovně přístupu uživatele přidružené k zajistěte, aby uživatel měl všechna oprávnění vyžadovaná ke používat prostředky Azure Site Recovery poskytuje.

Pokud chcete povolit replikaci pro nový virtuální počítač, musí mít uživatel:
* Oprávnění k vytvoření virtuálního počítače ve vybrané skupině prostředků
* Oprávnění k vytvoření virtuálního počítače ve vybrané virtuální síti
* Oprávnění k zápisu do vybraného účtu úložiště

Uživatel potřebuje následující oprávnění k dokončení replikace nového virtuálního počítače.

> [!IMPORTANT]
>Zajistěte, aby odpovídající oprávnění jsou přidány za model nasazení (Resource Manager / Classic) používá pro nasazení prostředků.

| **Typ prostředku** | **Model nasazení** | **Oprávnění** |
| --- | --- | --- |
| Compute | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Classic | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Síť | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Classic | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Úložiště | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Classic | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Skupina prostředků | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Zvažte použití, Přispěvatel virtuálních počítačů a Přispěvatel Classic virtuálních počítačů [předdefinované role](../active-directory/role-based-access-built-in-roles.md) pro nasazení Resource Manager a klasický modelů v uvedeném pořadí.

## <a name="next-steps"></a>Další kroky
* [Řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.
* Zjistěte, jak spravovat přístup pomocí:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Na základě rolí řešení potíží s řízení přístupu](../active-directory/role-based-access-control-troubleshooting.md): umožňuje získat návrhy pro řešení běžných problémů.
