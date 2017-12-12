---
title: "Dokončení kontrola přístupu členů skupiny či přístup uživatelů k aplikaci s Azure AD | Microsoft Docs"
description: "Zjistěte, jak dokončit kontrola přístupu pro členy skupiny nebo uživatelé s přístupem k aplikaci v Azure Active Directory."
services: active-directory
documentationcenter: 
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
ms.openlocfilehash: de853d633aa65c9f08f5e28088d5240c2e4d7fa6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Dokončení kontrola přístupu členů skupiny či přístup uživatelů k aplikaci ve službě Azure AD

Správci mohou pomocí Azure Active Directory (Azure AD) [vytvořit kontrola přístupu](active-directory-azure-ad-controls-create-access-review.md) pro členy skupiny nebo uživatelé přiřazení k aplikaci. Azure AD automaticky odesílá kontroloři e-mailu, který zobrazí výzvu k kontrolujte přístup. Pokud uživatel nedostali e-mailu, můžete odeslat je podle pokynů [zkontrolujte přístup k](active-directory-azure-ad-controls-perform-access-review.md). Po dokončení zkontrolujte období přístup nebo pokud správce zastaví kontrola přístupu, postupujte podle kroků v tomto článku najdete v části a použít výsledky.

## <a name="view-an-access-review-in-the-azure-portal"></a>Zobrazit kontrola přístupu na portálu Azure

1. Přejděte na [přístupu zkontroluje stránky](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), vyberte **programy**a vyberte program, který obsahuje zkontrolujte řízení přístupu.

2. Vyberte **spravovat**a vyberte kontrolní řízení přístupu. Pokud existují mnoho ovládacích prvků v programu, můžete filtrovat pro ovládací prvky konkrétního typu a řazení podle jejich stavu. Také můžete hledat podle názvu zkontrolujte řízení přístupu a zobrazované jméno vlastníka, který ho vytvořil. 

## <a name="stop-a-review-that-hasnt-finished"></a>Zastavit kontrolu, která se nedokončila

Pokud kontrola nedosáhla naplánované koncové datum, můžete vybrat správce **Zastavit** na konec kontrola již v rané fázi. Poté, co zastavíte kontrola, uživatelé již nebude možné zkontrolovat. Kontrolu nelze restartovat, jakmile je zastavena.

## <a name="apply-the-changes"></a>Použít změny 

Po dokončení kontrola přístupu, protože bylo dosaženo koncové datum nebo správce zastavena ji ručně, vyberte buď **použít**. Výsledek kontrola je implementováno modulem aktualizace skupinu nebo aplikaci. Pokud v zkontrolujte, byl odepřen přístup uživatele, když správce vybere tuto možnost, Azure AD se odeberou jejich přiřazení členství nebo aplikace. 

Výběr **použít** nemá vliv na skupinu, která pochází z místního adresáře nebo dynamická skupina. Pokud chcete změnit skupinu, která pochází místní, stáhněte si výsledky a tyto změny budou použity k reprezentaci skupiny v tomto adresáři.

## <a name="download-the-results-of-the-review"></a>Stáhněte si výsledky kontrola

Pokud chcete načíst výsledky kontrola, vyberte **schválení** a pak vyberte **Stáhnout**. Výsledný soubor CSV lze zobrazit v aplikaci Excel nebo programy, které soubory sdíleného svazku clusteru.

## <a name="optional-delete-a-review"></a>Volitelné: Odstranění kontrolu
Pokud vás zajímá už kontrola, můžete ji odstranit. Vyberte **odstranit** odebrat kontrola z Azure AD.

> [!IMPORTANT]
> Neexistuje žádná upozornění předtím, než dojde k odstranění, proto se ujistěte, že chcete odstranit kontrola.
> 
> 

## <a name="next-steps"></a>Další kroky

- [Správa přístupu uživatelů pomocí kontrol přístupu Azure AD](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Správa přístupu uživatelů typu host pomocí kontrol přístupu Azure AD](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Správa programů a ovládacích prvků pro kontroly přístupu Azure AD](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Vytváření kontroly přístupu pro členy skupiny nebo přístupu k aplikaci](active-directory-azure-ad-controls-create-access-review.md)
- [Vytváření kontroly přístupu uživatelů v roli správce Azure AD](active-directory-privileged-identity-management-how-to-start-security-review.md)
