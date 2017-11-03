---
title: "Oprávnění v Azure Security Center | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure Security Center používá řízení přístupu na základě rolí k přiřazení oprávnění pro uživatele a identifikuje povolené akce pro každou roli."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 0aaa99dda44d2020afd3e841e84020eb4ff87a85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="permissions-in-azure-security-center"></a>Oprávnění v Azure Security Center

Azure Security Center používá [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md). To poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md), které se dají v Azure přiřadit uživatelům, skupinám a službám.

Security Center vyhodnocuje konfigurace vaše prostředky a identifikují problémy se zabezpečením a ohrožení zabezpečení. V Centru zabezpečení zobrazí jenom informace týkající se prostředek v případě jsou přiřazenou roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo skupinu prostředků, které daný prostředek patří.

Kromě těchto rolí existují ve službě Security Center dvě specifické role:

* **Zabezpečení čtečky**: uživatel, který patří do této role má zobrazení práva k Security Center. Uživatel může zobrazit doporučení, výstrahy, zásady zabezpečení a stavy zabezpečení, ale nelze provádět změny.
* **Správce zabezpečení**: uživatel, který patří do této role má stejné oprávnění jako čtečky zabezpečení a zda můžete taky aktualizovat zásady zabezpečení a zavření výstrah a doporučení.

> [!NOTE]
> Role zabezpečení, čtečky zabezpečení a správce zabezpečení, mají přístup pouze ve službě Security Center. Role zabezpečení nemají přístup do jiných oblastí služby Azure, jako je například úložiště, webové a mobilní telefon nebo Internet věcí.
>
>

## <a name="roles-and-allowed-actions"></a>Role a povolených akcí

Následující tabulka zobrazuje role a povolené akce v Centru zabezpečení. X označuje, že je povolené akce pro tuto roli.

| Role | Upravit zásady zabezpečení | Použít doporučení zabezpečení pro prostředek | Zavření výstrahy a doporučení | Zobrazit výstrahy a doporučení |
|:--- |:---:|:---:|:---:|:---:|
| Vlastník předplatného | X | X | X | X |
| Předplatné přispěvatele | X | X | X | X |
| Vlastník skupiny prostředků | -- | X | -- | X |
| Skupina prostředků přispěvatele | -- | X | -- | X |
| Čtenář | -- | -- | -- | X |
| Správce zabezpečení. | X | -- | X | X |
| Čtečka zabezpečení | -- | -- | -- | X |

> [!NOTE]
> Doporučujeme přiřadit uživatelům tu nejvíc omezenou roli, kterou ke své práci potřebují. Například přiřadíte role Čtenář uživatelům, kteří potřebují jenom zobrazit informace o stavu zabezpečení prostředku, ale nemusí provádět žádné akce, třeba uplatňovat doporučení nebo upravovat zásady.
>
>

## <a name="next-steps"></a>Další kroky
Tento článek vysvětlení, jak Security Center používá RBAC přiřadit oprávnění pro uživatele a identifikovat povolené akce pro každou roli. Teď, když jste obeznámeni s přiřazení rolí, které jsou potřeba k monitorování stavu zabezpečení vašeho předplatného, upravit zásady zabezpečení a použijete doporučení, zjistěte, jak:

- [Nastavení zásad zabezpečení v Centru zabezpečení](security-center-policies.md)
- [Správa doporučení zabezpečení v Centru zabezpečení](security-center-recommendations.md)
- [Sledování stavu zabezpečení prostředků Azure](security-center-monitoring.md)
- [Správa a reakce na výstrahy zabezpečení ve službě Security Center](security-center-managing-and-responding-alerts.md)
- [Sledování partnerských řešení zabezpečení](security-center-partner-solutions.md)
