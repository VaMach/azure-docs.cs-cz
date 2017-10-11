---
title: "Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** výstrahy stavu řešení Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center
Azure Security Center doporučí, vyřešení výstrahy stavu zjištěné endpoint protection.  Security Center vám umožní zobrazit virtuálních počítačů (VM) mají selhání endpoint protection a kolik selhání.

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení. Není to podrobný průvodce.
> 
> 

## <a name="implement-the-recommendation"></a>Implementace doporučení
1. V **doporučení okno**, vyberte **výstrahy stavu řešení Endpoint Protection**.
   ![Vyřešení výstrah stavu služby Endpoint Protection][1]
2. Otevře se okno pro **Endpoint Protection selhání** které seznamy virtuálních počítačů s chybami a počet selhání pro každý virtuální počítač. Vyberte virtuální počítač ze seznamu.
   ![Selhání ochrany koncového bodu][2]
3. A **selhání seznamu** okno otevře pro vybraný virtuální počítač, zobrazení seznamu chyb. Vyberte ze seznamu Další selhání. Otevře se okno s informacemi o vybrané selhání.
   ![Seznam selhání][3]
   ![událostí selhání][4]

## <a name="see-also"></a>Viz také
Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
