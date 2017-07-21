---
title: "Řešení potíží: Chybějící data ve stažených protokolech aktivity služby Azure Active Directory | Dokumentace Microsoftu"
description: "Nabízí řešení pro chybějící data ve stažených protokolech aktivity služby Azure Active Directory."
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
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9109c698e4e8b43eeb7534c338adc99476012a3f
ms.contentlocale: cs-cz
ms.lasthandoff: 05/08/2017

---

# V stažených protokolech aktivity služby Azure Active Directory nejde najít žádná data
<a id="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded" class="xliff"></a>


## Příznaky
<a id="symptoms" class="xliff"></a>

Ve stažených protokolech aktivity (auditu nebo přihlášení) se nezobrazují žádné záznamy pro zvolený čas. Proč? 

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## Příčina
<a id="cause" class="xliff"></a>

Když si stáhnete protokoly aktivity na webu Azure Portal, omezujeme rozsah na 120 tisíc záznamů řazených od nejposlednějších. 

## Řešení
<a id="resolution" class="xliff"></a>

Můžete využít [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md), abyste načetli až milion záznamů v libovolném časovém okamžiku. Náš doporučený postup je pravidelně spouštět skript, který volá rozhraní API pro vytváření sestav a načítá záznamy přírůstkově v časovém intervalu (například denně nebo týdně).

## Další kroky
<a id="next-steps" class="xliff"></a>
Přečtěte si [nejčastější dotazy ke generování sestav v Azure Active Directory](active-directory-reporting-faq.md).


