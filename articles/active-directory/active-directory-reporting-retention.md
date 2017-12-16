---
title: "Zásady uchování sestav Azure Active Directory | Microsoft Docs"
description: "Zásady uchovávání informací na data sestavy ve vašem Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 61d3e8fbe26ab24ba0b551e52be0769228f09a11
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
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

- **Okamžitě** – Pokud již pracujete s sestav na portálu Azure
- **V rámci 2 hodiny** – Pokud jste nezapnuli generování sestav na portálu Azure

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
| Použití Azure MFA        | 30 dní       | 30 dní             | 30 dní             |

**Signály zabezpečení**

| Sestava         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Ohrožených uživatelích  | 7 dní        | 30 dní             | 90 dnů             |
| Rizikové přihlášení | 7 dní        | 30 dní             | 90 dnů             |

---
