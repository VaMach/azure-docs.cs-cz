---
title: "Řešení potíží: Chybějící data v protokolech aktivity služby Azure Active Directory | Dokumentace Microsoftu"
description: "Obsahuje seznam různých dostupných sestav pro Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
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
ms.openlocfilehash: 18af51e95a283a5cd33688484a0d7477eb4b957d
ms.contentlocale: cs-cz
ms.lasthandoff: 05/08/2017

---

# Nemůžu v protokolu aktivity Azure Active Directory najít některé provedené akce
<a id="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log" class="xliff"></a>


## Příznaky
<a id="symptoms" class="xliff"></a>

Provedl jsem nějaké akce na webu Azure Portal a očekával jsem pro tyto akce zobrazení protokolu auditu v okně `Activity logs > Audit Logs`, ale nemůžu je najít.

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## Příčina
<a id="cause" class="xliff"></a>

Akce se v protokolu auditu aktivity nezobrazí okamžitě. Zobrazení operací v protokolech auditu na portálu může od jejich provedení trvat 15 minut až jednu hodinu.

## Řešení
<a id="resolution" class="xliff"></a>

Počkejte 15 minut až hodinu a pak se podívejte, jestli se akce v protokolu zobrazily. Pokud je stále nevidíte, vytvořte prosím lístek podpory a my se na to podíváme.


## Další kroky
<a id="next-steps" class="xliff"></a>
Přečtěte si [nejčastější dotazy ke generování sestav v Azure Active Directory](active-directory-reporting-faq.md).


