---
title: "Azure Active Directory Identity Protection oznámení | Microsoft Docs"
description: "Zjistěte, jak oznámení podporují vaše aktivity šetření."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: abc0f3926905295a9cf239146cce7fc57da7eb29
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure oznámení ochrany identit služby Active Directory
Azure AD Identity Protection odešle dva typy automatizované oznámení e-mailů vám pomůžou spravovat riziko pro uživatele a rizikových událostí:

* Uživatel, dojde k ohrožení výstrahy e-mailu
* Týdenní digest e-mailu

## <a name="user-compromised-alert-email"></a>Uživatel, dojde k ohrožení výstrahy e-mailu
Ohrožené e-mailových upozornění na uživatele se vygeneruje, když Azure AD Identity Protection identifikuje účet jako ohrožení zabezpečení. E-mailu obsahuje odkaz na uživatelé označení příznakem pro sestavu riziko v řídicím panelu ochrany identit. Doporučujeme prozkoumat okamžitě oznámení o ohrožených účty.

## <a name="weekly-digest-email"></a>Týdenní digest e-mailu
Týdenní digest e-mailu obsahuje souhrn nových událostí riziko.<br>
Obsahuje:

* Ohrožení uživatelé
* Podezřelé aktivity
* Zjištěné ohrožení zabezpečení
* Odkazy na související sestavy v Identity Protection

<br>
![Náprava](./media/active-directory-identityprotection-notifications/400.png "nápravy")
<br>

Můžete přepnout odesílání týdenní ověřování algoritmem digest, e-mailu.
<br><br>
![Uživatel rizika](./media/active-directory-identityprotection-notifications/62.png "rizika uživatele")
<br>

**Tím otevřete dialogové okno Konfigurace související**:

1. Na **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.
   <br><br>
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/401.png "riziko zásady uživatele")
   <br>
2. V **Obecné** klikněte na tlačítko **oznámení**.
   <br><br>
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/405.png "riziko zásady uživatele")
   <br>

## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)
