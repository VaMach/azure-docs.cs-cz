---
title: "Samoobslužná služba nebo virální registrace v Azure Active Directory | Microsoft Docs"
description: "Použít samoobslužné registrace v klienta služby Azure Active Directory (Azure AD)"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/03/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 2b41bb1b72cc773c29d464228c3177fbd1d9f5e0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Co je samoobslužné registrace pro Azure Active Directory?
Tento článek vysvětluje samoobslužné registrace a jak ho podporují v Azure Active Directory (Azure AD). Pokud chcete převzít kontrolu nad název domény klienta z nespravovaných Azure AD najdete v tématu [převzít kontrolu nad adresář nespravované jako správce](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Proč používat samoobslužné registrace?
* Získejte zákazníků ke službám, které mají být rychlejší
* Vytvoření e mailové nabídky pro službu
* Vytvoření e mailové registrace toky, které rychle povolit uživatelům vytvářet identit pomocí jejich aliasy snadno zapamatovat pracovní e-mailu
* Samoobslužných-služeb vytvořené adresář Azure AD mohou být změněny na spravované adresáře, který lze použít pro jiné služby

## <a name="terms-and-definitions"></a>Termíny a definice
* **Samoobslužné registrace**: Toto je metoda, podle kterého uživatel zaregistruje cloudovou službu a má u nich automaticky vytvoří ve službě Azure AD identity založené na jejich e-mailovou doménu.
* **Nespravované adresář Azure AD**: Toto je adresář, kde se má vytvořit danou identitu. Nespravované adresáře se adresář, který má žádný globální správce.
* **Ověřit e-mailu uživatel**: Jedná se o typ uživatelského účtu ve službě Azure AD. Uživatel, který má identity vytvoří automaticky po registraci na nabídku samoobslužné služby se označuje jako uživatel s ověřenou e-mailu. Ověření e-mailu uživatel je členem regulární adresáře označené creationmethod = EmailVerified.

## <a name="how-do-i-control-self-service-settings"></a>Jak řídit nastavení samoobslužné služby?
Správci mají dvou ovládacích prvků samoobslužné služby ještě dnes. Kontroly nad zda:

* Uživatelé mohou připojit adresáři e-mailem.
* Uživatelé mohou sami licence pro aplikace a služby.

### <a name="how-can-i-control-these-capabilities"></a>Jak můžete řídit tyto funkce?
Správce můžete nakonfigurovat tyto možnosti, pomocí následujících parametrů rutiny Set-MsolCompanySettings Azure AD:

* **AllowEmailVerifiedUsers** Určuje, jestli uživatel může vytvořit nebo připojit k nespravované adresáři. Pokud tento parametr nastavte na $false, žádní uživatelé ověřit e-mailu se můžete zapojit do adresáře.
* **AllowAdHocSubscriptions** řídí schopnost uživatelům provádět samoobslužné registrace. Pokud tento parametr nastavte na $false, žádní uživatelé provést samoobslužné registrace.

### <a name="how-do-the-controls-work-together"></a>Jak ovládací prvky spolupracují?
Tyto dva parametry můžete použít ve spojení definovat přesnější kontrolu nad samoobslužné registrace. Například následující příkaz umožní uživatelům provádět samoobslužné registrace, ale pouze pokud tito uživatelé již máte účet ve službě Azure AD (jinými slovy, uživatele, kteří se potřebují účet ověřit e-mailu, který chcete vytvořit nejprve nelze provést samoobslužné registrace):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Následující vývojový diagram vysvětluje různé kombinace pro tyto parametry a výsledný podmínky k adresáři a samoobslužné registrace.

![][1]

Další informace a příklady použití těchto parametrů najdete v tématu [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Další kroky
* [Přidání vlastního názvu domény do Azure AD](add-custom-domain.md)
* [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referenční informace k rutinám Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
