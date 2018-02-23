---
title: "Správa oprávnění k prostředkům na uživatele v Azure zásobníku | Microsoft Docs"
description: "Jako správce služeb nebo klienta zjistěte, jak ke správě oprávnění RBAC."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: dfec5648ff383fd98965c1918cdab6472bb3c17f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="manage-role-based-access-control"></a>Správa řízení přístupu na základě rolí

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Uživatel v zásobníku Azure může být čtečky, vlastníka nebo přispěvatele pro každou instanci předplatné, skupinu prostředků nebo služby. Například uživatel A může mít čtečky oprávnění k předplatnému 1, ale oprávnění vlastníka 7 virtuálního počítače.

* Čtečka: Zobrazit vše, co uživatel, ale nelze provést žádné změny.
* Přispěvatel: Uživatel může spravovat všechno kromě přístupu k prostředkům.
* Vlastník: Uživatel může spravovat všechno včetně přístupu k prostředkům.

## <a name="set-access-permissions-for-a-user"></a>Nastavte přístupová oprávnění pro uživatele
1. Přihlaste se pomocí účtu, který má oprávnění vlastníka prostředku, který chcete spravovat.
2. V okně pro prostředek, klikněte **přístup** ikonu ![](media/azure-stack-manage-permissions/image1.png).
3. V **uživatelé** okně klikněte na tlačítko **role**.
4. V **role** okně klikněte na tlačítko **přidat** se přidat oprávnění pro uživatele.

## <a name="next-steps"></a>Další postup


