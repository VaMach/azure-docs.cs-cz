---
title: "Jak konfigurovat výstrahy zabezpečení | Microsoft Docs"
description: "Zjistěte, jak konfigurovat výstrahy zabezpečení pro rozšíření Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: e057120e31eeebc3da274536c09d6d9972854338
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Jak konfigurovat výstrahy zabezpečení v Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Výstrahy zabezpečení
Azure Správa privilegovaných identit (PIM) generuje výstrahy, když je aktivita podezřelého nebo unsafe ve vašem prostředí. Výstrahy, zobrazí na řídicím panelu PIM. Vyberte výstrahu, pokud chcete zobrazit sestavu obsahující seznam uživatelů nebo rolí, které aktivoval výstrahu.

![Výstrahy zabezpečení PIM řídicí panel – snímek obrazovky][1]

| Výstrahy | Aktivační události | Doporučení |
| --- | --- | --- |
| **Role se přiřazují mimo PIM** |Správce byl trvale přiřazen k roli mimo PIM rozhraní. |Zkontrolujte nové přiřazení role. Vzhledem k tomu, že jiné služby lze přiřadit pouze trvalých správců, k oprávněné přiřazení podle potřeby změňte. |
| **Role jsou příliš často aktivován** |V rámci časový limit v nastavení nebyly příliš mnoho opětovných aktivací stejné role. |Kontaktujte uživatele, který chcete zobrazit, proč se aktivaci role mnoho časy. Možná je příliš krátká pro je k dokončení úkolů, nebo možná používáte skripty mohl automaticky aktivovat roli, časový limit. |
| **Role nevyžadují službu Multi-Factor authentication pro aktivaci** |Existuje rolí bez vícefaktorového ověřování povoleno v nastavení. |Jsme vícefaktorové ověřování vyžadovat pro nejvíce vysoce privilegované role, ale důrazně vyzývá povolit MFA pro aktivaci všech rolí. |
| **Nepoužívají správci svoje privilegované role** |Existují oprávněné správce, kteří nedávno neaktivovali jejich rolí. |Spusťte kontrola přístupu k určení uživatelů, kteří již nepotřebují přístup. |
| **Existuje příliš mnoho globální správci** |Existuje více globálních správců, než se nedoporučuje. |Pokud máte velký počet globálních správců, je pravděpodobné, že uživatelé jsou stále větší oprávnění než potřebují. Přesunout uživatelé méně privilegované role, nebo vytvořit některé z nich vhodné pro roli místo trvale přiřazená. |

## <a name="configure-security-alert-settings"></a>Konfigurace nastavení výstrah zabezpečení
Můžete přizpůsobit některé z výstrah zabezpečení v PIM pro práci s prostředím a cíle zabezpečení. Postupujte podle těchto kroků k dosažení okně nastavení:

1. Přihlaste se k [portál Azure](https://portal.azure.com/) a vyberte **Azure AD Privileged Identity Management** dlaždici na řídicím panelu.
2. Vyberte **spravovat privilegované role** > **nastavení** > **výstrahy nastavení**.
   
    ![Přejděte do nastavení výstrah zabezpečení][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Výstraha "Role jsou aktivované příliš často"
Tato výstraha aktivuje, pokud uživatel aktivuje stejné privilegované role několikrát v zadaném období. Můžete nakonfigurovat časové období a počet aktivací.

* **Obnovení aktivace časový rámec**: Zadejte počet dnů, hodin, minut a druhé časové období, které chcete použít ke sledování podezřelých obnovení.
* **Počet obnovení aktivace**: Zadejte počet aktivací od 2 do 100, které považujete za worthy výstrahy v časovém rámci jste zvolili. Je toto nastavení můžete změnit přesunutím posuvníku nebo zadáním čísla do textového pole.

### <a name="there-are-too-many-global-administrators-alert"></a>Výstraha "Existuje příliš mnoho globální správci"
PIM aktivuje tuto výstrahu, pokud jsou splněny dva různé kritéria a nakonfigurujete oba dva. Nejprve budete muset dosáhne určité prahové hodnoty globálních správců. Procento vaše přiřazení role celkový druhý, musí být globální správce. Pokud splňujete pouze jeden z těchto měření, výstraha se nezobrazí.  

* **Minimální počet globálních správců**: Zadejte počet globálních správců, od 2 do 100, vezměte v úvahu dobu unsafe.
* **Procento globálních správců**: Zadejte procento správci, kteří jsou globální správci, od 0 % na 100 %, která nebezpečné ve vašem prostředí.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Výstraha "Správci nejsou pomocí jejich privilegované role"
Tato výstraha aktivuje, pokud uživatel přejde určitou dobu bez aktivace role.

* **Počet dnů**: Zadejte počet dnů od 0 do 100, který uživatel může přejít bez aktivace role.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
