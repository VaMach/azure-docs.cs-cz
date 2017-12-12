---
title: "Zprávu nelicencovaná využití | Microsoft Docs"
description: "Nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.openlocfilehash: 298102fedc766d362d1b3c525293212825eef5c7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="unlicensed-usage-report"></a>Zprávu nelicencovaná využití
Nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD. To umožňuje, aby lépe použijete licencí, které jste zakoupili a k identifikaci víte, že pokud potřebujete další licence. 

Tato sestava zobrazuje využití aktivní placené funkce za posledních 30 dní. 

## <a name="report-structure"></a>Struktura sestavy
| Název sloupce | Popis |
|:--- |:--- |
| Nelicencovaného uživatele |Jméno uživatele |
| Funkce |Název funkce. Příklad: podmíněného přístupu |
| Aplikace s přístupem |Název aplikace, která pracuje s funkcí. Příklad: Office 365 Sharepointu Online |

> [!NOTE]
> Pokud uživatelský účet je odstraněný sloupec 'Nelicencovaného uživatele' vyplní s ID, jako je 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Funkce podmíněného přístupu
Uživatelé bez licence budou označeny při přístupu služby, který má zásady podmíněného přístupu použito v případě, že nemají licenci Azure AD Premium. 

To se vztahuje na MFA / zásady umístění, jakož i zařízení zásady, které pomocí Intune.

## <a name="see-also"></a>Viz také
* [Použití podmíněného přístupu s Office 365 a další služby Azure Active Directory připojení aplikace](active-directory-conditional-access-azure-portal.md)
* [Začínáme s podmíněným přístupem ke službě Azure AD](active-directory-conditional-access-azure-portal-get-started.md) 

