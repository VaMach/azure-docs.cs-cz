---
title: "Správa oprávnění a přístupová práva pomocí RBAC - Azure RBAC | Microsoft Docs"
description: "Začínáme se správou přístupu pomocí řízení přístupu na základě role Azure na portálu Azure. Používejte přiřazení rolí k přiřazení oprávnění v adresáři."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 7df1ebcc65998a2078f5f215277eef351e48ca1b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-role-based-access-control-in-the-azure-portal"></a>Začínáme s řízením přístupu na základě rolí na portálu Azure
Zaměřené na zabezpečení společnosti by měla soustředit na poskytnutí zaměstnanci přesný oprávnění, které potřebují. Účet, který se útočníci můžou zpřístupnit příliš mnoho oprávnění. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. Azure na základě rolí řízení přístupu (RBAC) pomáhá vyřešit tento problém tak, že nabídka vyladění správy přístupu pro Azure.

Pomocí RBAC, můžete v rámci týmu oddělit povinností a poskytnout pouze takovou úroveň přístupu pro uživatele, kteří potřebují k provádění svých úloh. Namísto udělení každý uživatel neomezený oprávnění v vašeho předplatného Azure nebo prostředky, můžete povolit jenom určité akce. Například použijte funkci RBAC umožníte jednoho zaměstnance spravovat virtuální počítače v předplatném, zatímco jiné můžete spravovat databází SQL v rámci stejného předplatného.

## <a name="basics-of-access-management-in-azure"></a>Základy správy přístupu v Azure
Každé předplatné Azure je přidružen jeden adresář Azure Active Directory (AD). Uživatelé, skupiny a aplikací z adresáře, můžete spravovat prostředky v rámci předplatného Azure. Přiřaďte tyto přístupová práva pomocí portálu Azure, nástroje příkazového řádku Azure a rozhraní API pro správu Azure.

Udělit přístup přiřazením příslušné role RBAC uživatelům, skupinám a aplikace v určité oboru. Předplatné, skupinu prostředků nebo jediný zdroj, může být oboru přiřazení role. Role přiřazené v nadřazeném oboru podřízené objekty jsou v něm obsažena také uděluje přístup. Například uživatel s přístupem do skupiny prostředků můžete spravovat všechny prostředky, které obsahuje, jako jsou weby, virtuální počítače a podsítě.

![Vztah mezi elementy Azure Active Directory – diagram](./media/role-based-access-control-what-is/rbac_aad.png)

Role RBAC, který přiřadíte Určuje, jaké prostředky uživatele, skupiny nebo aplikace můžete spravovat v rámci tohoto oboru.

## <a name="built-in-roles"></a>Vestavěné role
Azure RBAC má tři základní role, které platí pro všechny typy prostředků:

* **Vlastník** má úplný přístup ke všem prostředkům, včetně právo delegovat přístup k ostatním.
* **Přispěvatel** můžete vytvářet a spravovat všechny typy prostředků Azure, ale nelze udělit přístup ostatním uživatelům.
* **Čtečka** můžete zobrazit stávající prostředky Azure.

Zbytek role RBAC v Azure povolit správu konkrétních prostředků Azure. Například role Přispěvatel virtuálních počítačů umožňuje uživatelům vytvářet a spravovat virtuální počítače. Nedává je přístup k virtuální síť nebo podsíť, které se virtuální počítač připojí k. 

[Předdefinované role RBAC](role-based-access-built-in-roles.md) uvádí role, které jsou k dispozici v Azure. Určuje operace a rozsah, který každé předdefinované role uděluje uživatelům. Pokud zjišťujete, kam můžete definovat vlastní role pro ještě větší kontrolu, najdete v části jak sestavit [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Dědění prostředků hierarchie a přístupu
* Každý **předplatné** v Azure patří k pouze jednomu adresáři. (Ale každý adresář může mít více než jedno předplatné).
* Každý **skupiny prostředků** patří do jenom jedno předplatné.
* Každý **prostředků** patří do skupiny jen jeden prostředek.

V podřízené obory dědí přístup, který udělíte na nadřazené rozsahy. Například:

* Role čtenáře přiřadíte skupiny v oboru předplatné služby Azure AD. Členové této skupiny můžete zobrazit všechny skupiny prostředků a prostředků v předplatném.
* Role Přispěvatel přiřadíte k aplikaci v oboru skupiny prostředků. Umožňuje spravovat prostředky všechny typy v příslušné skupině prostředků, ale ne další skupiny zdrojů v rámci předplatného.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC oproti správci classic předplatného
Classic předplatné správci a pomocní Správci mají úplný přístup k předplatnému Azure. Můžou spravovat prostředky pomocí [portál Azure](https://portal.azure.com) pomocí rozhraní API Správce Azure Resource Manager, nebo [portál Azure classic](https://manage.windowsazure.com) a modelu nasazení Azure classic. V modelu RBAC klasických správců přiřazené roli vlastníka na obor předplatného.

Azure RBAC podporují pouze na portálu Azure a nových rozhraní API Správce Azure Resource Manager. Uživatelé a aplikace, které jsou přiřazené role RBAC nelze pomocí portálu pro správu classic a modelu nasazení Azure classic.

## <a name="authorization-for-management-vs-data-operations"></a>Oprávnění pro správu oproti operace dat
Azure RBAC podporuje pouze operace správy prostředků Azure v portálu Azure a rozhraní API Správce Azure Resource Manager. Všechny operace úrovně dat pro prostředky Azure se nejde autorizovat. Například může autorizovat někdo chcete spravovat účty pro úložiště, ale ne na objekty BLOB nebo tabulky v rámci účtu úložiště. Podobně databáze SQL je možné spravovat, ale nikoli tabulky v něm.

## <a name="next-steps"></a>Další kroky
* Začínáme s [řízení přístupu na základě rolí na portálu Azure](role-based-access-control-configure.md).
* Další informace najdete v článku [Vestavěné role řízení přístupu na základě role v Azure](role-based-access-built-in-roles.md).
* Definujte své [Vlastní role funkce řízení přístupu na základě role v Azure](role-based-access-control-custom-roles.md).
