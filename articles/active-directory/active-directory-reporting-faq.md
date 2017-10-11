---
title: "Azure Active Directory, vytváření sestav – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy týkající se vytváření sestav Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: accf292f70bf0eafdefc00c3feeaf8e346605401
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-reporting-faq"></a>Nejčastější dotazy týkající se vytváření sestav Azure Active Directory.

Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) o vytváření sestav Azure Active Directory.  
Další podrobnosti najdete v tématu [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md). 

**Otázka: co je uchovávání dat protokoly aktivity (auditu a přihlášení) na portálu Azure?** 

**Odpověď:** poskytujeme 7 dnů dat pro naše zákazníky volné a přepnutím do Azure AD Premium 1 nebo Premium 2 licenci, můžete přístup k datům po dobu 30 dnů. Další informace o uchovávání dat v [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md)

--- 

**Otázka: jak dlouho trvá až po dokončení Moje úloh data aktivit uvidí?**

**Odpověď:** protokoly auditu aktivity, jejichž latence od 15 minut až hodinu. Protokoly přihlašovací aktivity, jejichž latence od 15 minut pro většinu záznamů a až 2 hodiny pro několik záznamů.

---

**Otázka: nutné být globálním správcem zobrazíte aktivity protokoly na portálu Azure a získat data prostřednictvím rozhraní API?**

**Odpověď:** Ne. Může být buď **zabezpečení čtečky**, **správce zabezpečení** nebo **globálního správce** chcete zobrazit data na portálu Azure nebo k ní přistupují přes rozhraní API pro vytváření sestav.

---

**Otázka: je možné získat informace o protokolu činnosti Office 365 prostřednictvím portálu Azure?**

**Odpověď:** i když aktivita Office 365 a Azure AD aktivity protokoly sdílení velké množství prostředků adresáře, pokud chcete, aby ucelený pohled na protokoly aktivity Office 365, by měl přejdete do do centra pro správu Office 365 se získat informace o protokolu Office 365 aktivity.

---


**Otázka: které rozhraní API se používá k načtení informací o protokoly aktivity Office 365?**

**Odpověď:** používat pro přístup k rozhraní API pro správu Office 365 [Office 365 aktivity protokoly prostřednictvím rozhraní API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**Otázka: počet záznamů, můžete stáhnout z portálu Azure?**

**Odpověď:** až 120 kB záznamy si můžete stáhnout z portálu Azure. Záznamy jsou seřazené podle *nejnovější* a ve výchozím nastavení, můžete získat nejnovější záznamy 120 kB. 

---

**Otázka: počet záznamů, můžete dotazovat pomocí aktivity rozhraní API?**

**Odpověď:** můžete dát dotaz na záznamy až 1 milion (Pokud nechcete použít operátor top, která seřadí záznam většina poslední). Pokud používáte operátor "top", se můžete dotazovat až 500 kB záznamy. Můžete najít ukázkové dotazy týkající se používání rozhraní API zde [zde](active-directory-reporting-api-getting-started.md).

---

**Otázka: jak lze získat licenci premium?**

**Odpověď:** najdete v části [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md) pro odpověď na tuto otázku.

---

**Otázka: jak brzy by měl zobrazit data aktivity po získání licence premium?**

**Odpověď:** Pokud již máte data aktivity, jako volné licenci, pak můžete zobrazit stejná data. Pokud nemáte k dispozici žádná data, pak bude trvat jeden nebo dva dny.

---

**Otázka: Po získání licenci Azure AD premium zobrazit data poslední měsíc?**

**Odpověď:** Pokud Přepnuli jste nedávno na verzi Premium (včetně zkušební verzi), zobrazí se data až do 7 dnů původně. Když se data hromadí, zobrazí se až 30 dnů.

---

**Otázka: je riziko událostí v ochrany identit, ale nejsou zobrazeny odpovídající přihlášení v všechny přihlášení. Je to očekávané?**

**Odpověď:** Ano, Identity Protection vyhodnotí riziko pro všechny toky ověřování jestli-li být interaktivní nebo neinteraktivní. Ale všechny přihlášení pouze sestava zobrazí jenom interaktivní přihlášení.

---

**Otázka: jak můžete stáhnout "Uživatelé označení příznakem rizik" sestav na portálu Azure?**

**Odpověď:** možnost stažení *uživatelé označení příznakem rizik* sestavy přidá brzy.

---

**Otázka: Jak poznám, proč přihlášení, nebo pro uživatele byla příznakem rizikové na portálu Azure?**

**Odpověď:** Premium edition zákazníkům další informace o základní rizikových událostech kliknutím na uživatele v "Uživatelé označení příznakem rizik" nebo kliknutím na "rizikové přihlášení". Zákazníci volné a Základní edice získat zobrazíte rizikové uživatele a přihlášení bez základní informace o události riziko.

---

**Otázka: jak jsou vypočítávány IP adresy v sestavě rizikové přihlášení a přihlášení??**

**Odpověď:** IP adresy vydávají tak, že není spolehlivý připojení mezi IP adresu a kde se fyzicky nacházejí počítači s touto adresou. To ztěžuje faktorech například mobilní poskytovatelů a vydání z Centrální fondy IP adres velmi často daleko od skutečně použití klientské zařízení sítě VPN. Z výše uvedených převodu IP adresu na fyzické umístění je nejlepší úsilí na základě trasování, data registru, zpětné vyhledání a další informace. 

---
