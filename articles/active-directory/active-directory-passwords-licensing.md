---
title: "Správa licencí: Azure AD SSPR | Microsoft Docs"
description: "Azure AD samoobslužného resetování hesel licenční požadavky"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: db849cd1e9da634064f79fbc041098542580ad02
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD

V pořadí pro resetování hesel služby Azure AD pro funkce které **musí mít alespoň jednu licenci přiřazené ve vaší organizaci**. Nevynucovat jsme licencování na vlastní uživatelské prostředí resetování hesla na uživatele. Chcete-li udržovat kompatibilitu s licenční smlouvou Microsoft, přiřadit licence pro všechny uživatele, které používají prémiových funkcí.

* **Uživatelé cloudové** -Office 365 (O365) žádné placené SKU nebo Azure AD Basic
* **Cloud** nebo **místních uživatelů** – Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo zabezpečení produktivní Enterprise (ZAD)

## <a name="licenses-required-for-password-writeback"></a>Licence potřebné pro zpětný zápis hesla

Pokud chcete používat zpětný zápis hesla, musí mít jednu z následujících licencí přiřazených ve vašem klientovi.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!WARNING]
> Samostatné Office 365 licenční plány **nepodporují zpětný zápis hesla** a vyžaduje jedna z předchozí plány pro tato funkce fungovat.

Další informace o licencování včetně náklady najdete na následujících stránkách

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory funkce a možnosti](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Povolit skupinu nebo uživatele na základě licencí

Azure AD nyní podporuje na základě skupiny licencování povolení správci přiřadit hromadných licencí pro skupinu uživatelů, nikoli po jednom přiřazení. [Přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Předtím, než je možné přiřadit licence pro uživatele, Správce musí určovat vlastnost "Umístění využití" na uživatele. Přiřazení licencí, lze provést v rámci uživatele > Profil > v oddílu nastavení na portálu Azure. **Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez využití umístění zadané umístění adresáře.**

## <a name="next-steps"></a>Další kroky

* [Jak dokončení úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetovat nebo změnit heslo](active-directory-passwords-update-your-own-password.md).
* [Registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).
* [Jaká data používá SSPR a jaká data by měla můžete naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Jaké metody ověřování jsou k dispozici pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad s SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč je starat o něm?](active-directory-passwords-writeback.md)
* [Jak sestavy na aktivitu v SSPR](active-directory-passwords-reporting.md)
* [Co jsou všechny možnosti v SSPR a co znamená, že?](active-directory-passwords-how-it-works.md)
* [Myslím, že něco je poškozená. Jak odstranit SSPR?](active-directory-passwords-troubleshoot.md)
* [Je nutné zadat otázku, která nebyla někde zahrnutých, jinak](active-directory-passwords-faq.md)
