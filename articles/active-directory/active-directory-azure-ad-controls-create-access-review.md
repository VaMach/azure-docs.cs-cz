---
title: "Vytvoření kontrola přístupu pro členy skupiny nebo uživatelé s přístupem k aplikaci s Azure AD | Microsoft Docs"
description: "Naučte se vytvářet kontrola přístupu pro členy skupiny nebo uživatelé s přístupem k aplikaci."
services: active-directory
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b2f8985f12e17ac69543cfb3a33725f796eedde8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Vytvoření kontrola přístupu členů skupiny nebo přístup k aplikaci s Azure AD

Přiřazení přístupu stát "zastaralé", když uživatelé mají přístup, které nepotřebují víc. Abyste snížili riziko spojené s přiřazením zastaralé přístup, mohou správci Azure Active Directory (Azure AD) k vytvoření kontrola přístupu pro členy skupiny nebo uživatelé přiřazení k aplikaci. Další informace o těchto scénářích najdete v tématu [spravovat přístup uživatelů](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) a [spravovat přístup hosta](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Vytvoření kontrola přístupu

1. Jako globální správce, přejděte na [přístupu zkontroluje stránky](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)a vyberte **programy**.

2. Vyberte program, který obsahuje zkontrolujte řízení přístupu, kterou chcete vytvořit. **Výchozí Program** je vždy k dispozici, nebo můžete vytvořit v jiné aplikaci. Například můžete mít jeden program pro každou initiative dodržování předpisů nebo obchodní cíle.

3. V rámci programu, vyberte **ovládací prvky**a potom vyberte **přidat** přidání ovládacího prvku.

4. Název každé kontrola přístupu. Každý zkontrolujte volitelně zadejte popis. Název se zobrazí kontroloři.

5. Nastaví počáteční a koncové datum. Ve výchozím nastavení zkontrolujte přístup spustí stejný den, který je vytvořen a končí na jeden měsíc. Spuštění, můžete změnit a koncovém datu měli přístup zkontrolujte spuštění v budoucnu a poslední ale chcete počet dní.

6. Recenze přístup může být pro členy skupiny nebo pro uživatele, kteří byly přiřazené k aplikaci. Můžete vytvořit další obor přístupu zkontrolujte kontrola pouze uživatele typu Host kdo jsou členy (nebo přiřazené k aplikaci), namísto kontroly všech uživatelů, kteří jsou členy, nebo kteří mají přístup k aplikaci.

7. Vyberte jeden nebo více osob, zobrazíte všechny uživatele v oboru. Nebo můžete vybrat tak, aby měl členy Zkontrolujte své vlastní přístup. Pokud prostředek je skupina, můžete požádat vlastníků skupiny ke kontrole. Také můžete vyžadovat, aby kontroloři při schválí přístup zadat příslušný důvod.

8. Nakonec vyberte **spustit**.


## <a name="manage-the-access-review"></a>Spravovat kontrola přístupu

Ve výchozím nastavení Azure AD odešle e-mail recenzentům krátce po spustí kontrola. Pokud se rozhodnete, že není mít službu Azure AD odesílat e-mailu, nezapomeňte informovat o tom, kontrolorů, které kontrola přístupu čeká na jejich dokončení. Je možné zobrazit pokyny, jak [kontrolujte přístup](active-directory-azure-ad-controls-perform-access-review.md). Pokud zkontrolovali pro hosty zkontrolovat svůj vlastní přístup, zobrazit jejich pokyny, jak [zkontrolujte přístup k vlastní](active-directory-azure-ad-controls-perform-access-review.md).

Je-li některé z kontroloři jsou hosté, hosté upozornění e-mailem pouze v případě, že jste již přijetí jejich pozvánku.


Průběh můžete sledovat jejich recenze v řídicím panelu Azure AD v dokončení kontroloři **zkontroluje přístup** části. Žádné oprávnění došlo ke změně v adresáři, dokud [byla dokončena kontrola](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Další kroky

Po spuštění kontrola přístupu Azure AD automaticky odesílá kontroloři e-mailu, který zobrazí výzvu k kontrolujte přístup. Pokud uživatel nedostali e-mailu, můžete jim poslat pokyny, jak [kontrolujte přístup](active-directory-azure-ad-controls-perform-access-review.md). 

Po dobu zkontrolujte přístup je nad nebo správce zastaví kontrola přístupu, postupujte podle kroků v [dokončit kontrola přístupu](active-directory-azure-ad-controls-complete-access-review.md) zobrazit a použít výsledky.


