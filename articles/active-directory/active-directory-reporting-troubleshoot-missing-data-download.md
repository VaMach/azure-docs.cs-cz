---
title: "Řešení potíží: Chybějící data ve stažených protokolech aktivity služby Azure Active Directory – Preview | Dokumentace Microsoftu"
description: "Nabízí řešení pro chybějící data ve stažených protokolech aktivity služby Azure Active Directory – Preview."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/09/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e0d65edcb7c14114565402038b0958c3a2ffb477
ms.lasthandoff: 03/10/2017


---

# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>V stažených protokolech aktivity služby Azure Active Directory nejde najít žádná data


## <a name="symptoms"></a>Příznaky

Ve stažených protokolech aktivity (auditu nebo přihlášení) se nezobrazují žádné záznamy pro zvolený čas. Proč? 

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Příčina

Když si stáhnete protokoly aktivity na webu Azure Portal, omezujeme rozsah na 120 tisíc záznamů řazených od nejposlednějších. 

## <a name="resolution"></a>Řešení

Můžete využít [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md), abyste načetli až milion záznamů v libovolném časovém okamžiku. Náš doporučený postup je pravidelně spouštět skript, který volá rozhraní API pro vytváření sestav a načítá záznamy přírůstkově v časovém intervalu (například denně nebo týdně).

## <a name="next-steps"></a>Další kroky
Přečtěte si [nejčastější dotazy ke generování sestav v Azure Active Directory](active-directory-reporting-faq.md).


