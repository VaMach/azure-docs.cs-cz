---
title: "Průvodce Azure AD Privileged Identity Management zabezpečení"
description: "Při prvním použití Azure Active Directory Privileged Identity Management rozšíření, zobrazí se Průvodce zabezpečení. Tento článek popisuje kroky pro pomocí průvodce."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 260d178f3d8158411b3ad266e3b0d15edbebc722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Pomocí Průvodce zabezpečení v Azure AD Privileged Identity Management 
Pokud jste první osobou ke spuštění Azure Privileged Identity Management (PIM) pro vaši organizaci, zobrazí se průvodce. Průvodce vám pomůže pochopit bezpečnostní rizika privilegované identity a jak používat PIM k omezení těchto rizik. Nepotřebujete žádné změny existujících přiřazení role v průvodci, pokud chcete provést později.

## <a name="what-to-expect"></a>Co můžete očekávat
Předtím, než vaše organizace zahájí pomocí PIM, všechna přiřazení rolí jsou trvalé: uživatelé se vždycky nacházejí v těchto rolí i v případě, že v současné době nepotřebují svá oprávnění.  Prvním krokem v Průvodci se dozvíte, seznam rolí s vysokými oprávněními a počet uživatelů, kteří jsou aktuálně v těchto rolí. Můžete zobrazit další podrobnosti konkrétní roli Další informace o uživatelích, pokud jeden nebo více těchto nejste obeznámeni.

Druhý krok průvodce vám dává příležitost, chcete-li změnit přiřazení rolí správce.  

> [!WARNING]
> Je důležité, že máte minimálně jeden globální správce a více než jeden správce privilegovaných rolí s účtem organizace (ne účet Microsoft). Pokud je jen jeden správce privilegovaných rolí, organizace nebude možné spravovat PIM, pokud tento účet je odstraněný.
> Navíc zachovat přiřazení rolí trvalé, pokud má uživatel účet Microsoft (účet, které používají pro přihlášení ke službám Microsoft, jako je Skype a Outlook.com). Pokud budete chtít vyžadovat vícefaktorové ověřování pro aktivaci pro tuto roli, tento uživatel bude uzamčena na.
> 
> 

Po provedení změn, Průvodce zobrazí. Při příštím vy nebo jiný správce privilegovaných rolí používat PIM, se zobrazí řídicí panel PIM.  

* Pokud chcete přidat nebo odebrat uživatele z role nebo změnit přiřazení z trvalé na oprávněné, přečtěte si informace v [jak přidat nebo odebrat roli uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Pokud chcete poskytnout přístup ke správě PIM více uživatelů, přečtěte si informace v [poskytnout přístup ke správě v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

