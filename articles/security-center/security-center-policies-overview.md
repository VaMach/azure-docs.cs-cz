---
title: "Úvod do zásad zabezpečení Azure Security Center | Microsoft Docs"
description: "Další informace o službě Azure Security Center zásady zabezpečení a jejími klíčovými funkcemi."
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 95ef2099cb16bcfd550ce2799428f1a16031f535
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="security-policies-overview"></a>Přehled zásad zabezpečení
Tento dokument obsahuje přehled zásad zabezpečení ve službě Security Center.

## <a name="what-are-security-policies"></a>Co jsou zásady zabezpečení?
Zásady zabezpečení definují požadovanou konfiguraci úloh a pomáhají zajišťovat dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení. Ve službě Security Center můžete pro předplatná Azure definovat zásady upravené pro příslušný typ úlohy nebo citlivost dat. Například aplikace využívající regulovaná data jako identifikovatelné osobní údaje může vyžadovat vyšší úroveň zabezpečení než jiné úlohy. 

Zásady Security Center obsahují následující součásti:

- Shromažďování dat: agenta zřizování a [shromažďování dat](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) nastavení.
- Zásady zabezpečení: Určuje ovládacích prvků, které jsou monitorovány a doporučené pomocí služby Security Center (Upravit [zásady zabezpečení](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) v Security Center, nebo použijte [zásad Azure](security-center-azure-policy.md), v omezené preview pro vytváření nových definice, zadejte další zásady a přiřadit zásady skupinami pro správu).
- E-mailová oznámení: zabezpečení kontakty a [e-mailová oznámení](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) nastavení.
- Cenová úroveň: volné nebo standard [ceny výběr](https://docs.microsoft.com/azure/security-center/security-center-pricing), která určuje, že funkce Security Center, které jsou k dispozici pro prostředky v oboru (může být zadán pro odběry, skupiny prostředků a pracovní prostory). 


## <a name="who-can-edit-security-policies"></a>Kdo můžete upravit zásady zabezpečení?
Security Center používá na základě rolí řízení přístupu (RBAC), která poskytuje integrované role, které lze přiřadit uživatelům, skupinám a službám v Azure. Když uživatel otevře službu Security Center, uvidí jenom informace související s prostředky, ke kterým má přístup. To znamená, že uživateli je přiřazena role Vlastníka, Přispěvatele nebo Čtenáře předplatného nebo skupiny prostředků, do které prostředek patří. Kromě těchto rolí existují ve službě Security Center dvě specifické role:

- Čtečka zabezpečení: uživatel, který patří do této role se může zobrazit práva k Security Center, která obsahuje doporučení, výstrahy, zásady a stav, ale nebude možné provádět změny.
- Správce zabezpečení: stejná jako čtečky zabezpečení, ale můžete také aktualizovat zásady zabezpečení Zavřít doporučení a výstrahy.


## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste se dozvěděli o zásady zabezpečení v Azure Security Center. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.


