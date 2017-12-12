---
title: "Řešení potíží s dynamické členství ve skupinách | Microsoft Docs"
description: "Tipy k řešení potíží pro dynamické členství ve skupinách v Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 0bb4c294cc6a4e1c9c2f1ad405c539854b6bcf5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Řešení potíží s dynamickým členstvím ve skupinách
**I nakonfigurovali pravidlo na skupinu, ale aktualizovat žádné členství ve skupině**<br/>Ověřte hodnoty atributů uživatele v pravidle: existují uživatelé, kteří splňovat pravidla? Pokud se vše spokojeni, počkejte prosím chvíli skupiny k naplnění. V závislosti na velikosti tenanta může první naplnění skupiny nebo její naplnění po změně pravidel trvat až 24 hodin.

**I nakonfigurovali pravidlo, ale teď se odebrat členy existující pravidla**<br/>Toto je očekávané chování. Stávající členy skupiny se odeberou, když je pravidlem povolené nebo změnit. Uživatelé vrácená z vyhodnocení pravidla jsou přidány jako členy do skupiny.     

**Není zobrazen, že změní okamžitě při přidání nebo změna pravidla, proč ne?**<br/>Vyhodnocování členství vyhrazené se provádí pravidelně v procesu asynchronní pozadí. Jak dlouho trvá proces je určen podle počet uživatelů ve vašem adresáři a velikost skupiny vytvořeny v důsledku pravidlo. Obvykle adresáře s malý počet uživatelů se zobrazí změn členství ve skupinách za méně než několik minut. Adresáře s velký počet uživatelů, může trvat 30 minut nebo déle, než se naplnění.

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
