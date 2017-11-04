---
title: "Zásady uchování sestav Azure Active Directory | Microsoft Docs"
description: "Zásady uchovávání informací na data sestavy ve vašem Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 933d251903f4ca74902e984e7a1581a75345ee7f
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Zásady uchování sestav Azure Active Directory


Toto téma poskytuje odpovědi na nejčastější dotazy ve spojení s uchovávání dat pro sestavy jinou aktivitu v Azure Active Directory. 

**Otázka: jak můžete získat shromažďování dat aktivity spustit?**

**ODPOVĚĎ:**

| Edice Azure AD | Počáteční kolekce |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Při registraci pro předplatné |
| Azure AD Free | Při prvním spuštění [okno Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) nebo použít [reporting rozhraní API](https://aka.ms/aadreports)  |

---
**Otázka: Pokud je vaše data aktivity k dispozici na portálu Azure?**

**ODPOVĚĎ:**

- **Okamžitě** – Pokud již pracujete s sestav na portálu Azure classic
- **V rámci 2 hodiny** – Pokud jste nezapnuli generování sestav na portálu Azure classic

---
**Otázka: jak můžete získat kolekce signálů zabezpečení spustit?**  

**Odpověď:** signálů zabezpečení, se proces shromažďování spustí, když jste přihlásit k centru pro ochranu Identity použít. 


---
**Otázka: jak dlouho shromážděná data ukládají?**

**ODPOVĚĎ:**

**Sestavy aktivit**    

| Sestava                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Audit adresáře        | 7 dní        | 30 dní             | 30 dní             |
| Přihlašovací aktivita       | Není k dispozici           | 30 dní             | 30 dní             |
| Použití Azure MFA        | 90 dnů       | 90 dnů             | 90 dnů             |

**Signály zabezpečení**

| Sestava         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Ohrožení uživatelé  | 7 dní        | 30 dní             | 90 dnů             |
| Rizikové přihlášení | 7 dní        | 30 dní             | 90 dnů             |

---
