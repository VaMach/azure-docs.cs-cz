---
title: "Účet zřizování oznámení | Microsoft Docs"
description: "Zjistěte, jak zajistit, že budete upozorněni problémy související s zřizování uživatelů, které vyžadují vaši pozornost povolením účet zřizování oznámení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: b99037fc28eca1a3ebffefb9e99991e74f52c9a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="account-provisioning-notifications"></a>Oznámení o zřizování účtů
Zřizování uživatelů, můžete automatizovat proces správy uživatele v aplikacích třetích stran SaaS. <br>
Přestože se automatizovaného procesu, je čas od času interakce se tento proces vyžaduje. <br>
To je, například v případě, pokud heslo účtu, že jste nakonfigurovali k výměně dat s třetí strany SaaS aplikace vypršela. 

Povolením účet zřizování oznámení můžete zajistit, že budete upozorněni problémy související s zřizování uživatelů, které vyžadují vaši pozornost.

Aktivace nebo deaktivace účet zřizování oznámení jako součást konfigurace pro třetí strany aplikace SaaS zřizování uživatelů.

![Zřizování uživatelů][1] 

Pokud chcete aktivovat účet zřizování oznámení, zaškrtněte políčko související na **potvrzení** dialogové okno stránky a pak zadejte e-mailový alias příjemce.

![Oznámení o zřizování účtů][2]

Distribučního seznamu můžete zadat jako recipient; je ale důležité si uvědomit, že e-mailové oznámení obsahuje odkazy na sestavy, které jsou přístupné správci Azure AD.

Pokud máte účet zřizování oznámení povolena, obdržíte e-mailů o zásadních problémů, které se vztahují k zřizování uživatelů. Ale abyste se vyhnuli přetížení e-mailu, jenom obdržíte jeden oznámení e-mailu na každý den pro každou aplikaci SaaS oznámení e-mailu je povolený pro.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizovat uživatele zřízení nebo zrušení zřízení k aplikacím SaaS](active-directory-saas-app-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md)
* [Seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
