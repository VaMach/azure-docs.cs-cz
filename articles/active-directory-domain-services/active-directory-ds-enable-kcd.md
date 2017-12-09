---
title: "Azure Active Directory Domain Services: Povolení vynuceného delegování protokolu kerberos | Microsoft Docs"
description: "Povolení vynuceného delegování protokolu kerberos na spravované domény Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: maheshu
ms.openlocfilehash: f5ecb8b2afc0c05fb8b2cb3850adcd2510ca29d2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Nakonfigurujte omezené delegování protokolu Kerberos (použitím KCD) na spravované doméně
Mnoho aplikací potřebovat přístup k prostředkům v kontextu uživatele. Služba Active Directory podporuje mechanismus delegování protokolu Kerberos, která umožňuje tento případ použití volat. Navíc můžete omezit delegování tak, aby se dalo přistupovat pouze konkrétní prostředky v kontextu uživatele. Spravované domény služby Azure AD Domain Services se liší od tradiční domén služby Active Directory, vzhledem k tomu, že jsou zamčené bezpečněji dolů.

Tento článek ukazuje, jak nakonfigurovat omezené delegování protokolu Kerberos na spravované doméně služby Azure AD Domain Services.

## <a name="kerberos-constrained-delegation-kcd"></a>Omezené delegování protokolu Kerberos (použitím KCD)
Delegování protokolu Kerberos umožňuje účet zosobnění jiný objekt zabezpečení (například uživatel) pro přístup k prostředkům. Vezměte v úvahu webové aplikace, který přistupuje k back endové webové rozhraní API v kontextu uživatele. V tomto příkladu webové aplikace (spuštěn v kontextu účtu služby nebo účet počítače) zosobňuje uživatele při přístupu k prostředku (back endové webové rozhraní API). Delegování protokolu Kerberos je nezabezpečené, protože se neomezuje z prostředků, které vydává za jiný účet přístup v kontextu uživatele.

Omezené delegování protokolu Kerberos (použitím KCD) omezuje služby nebo prostředky, ke kterým může zadaný server fungovat jménem uživatele. Tradiční použitím KCD vyžaduje oprávnění správce domény nakonfigurovat účet domény pro službu a omezuje účet na jedinou doménu.

Tradiční použitím KCD má také několik problémů, které jsou s ním spojená. V dřívějších operačních systémech Pokud správce domény konfiguroval účet na základě použitím KCD pro službu, neměl Správce služeb nijak užitečné vědět, které front-endové služby delegované na služby prostředků vlastnil. A všechny front-endová služba, která mohla delegovat na službu prostředků reprezentována potenciální místo pro útok. Pokud serveru, který hostoval front-endové služby došlo k ohrožení, a že byl nakonfigurován pro delegoval na služby, služby prostředků může také dojít k ohrožení.

> [!NOTE]
> Na spravované doméně služby Azure AD Domain Services nemáte oprávnění správce domény. Proto **tradiční použitím KCD nelze konfigurovat ve spravované doméně**. Použijte použitím KCD založené na prostředcích, jak je popsáno v tomto článku. Tento mechanismus je také bezpečnější.
>
>

## <a name="resource-based-kcd"></a>Použitím KCD založené na prostředcích
Správci služeb z Windows serveru 2012 a vyšší získáte možnost konfigurovat omezené delegování pro jejich službu. Správce služeb back-end v tomto modelu, můžete povolit nebo odepřít konkrétní front-endové služby pomocí použitím KCD. Tento model se označuje jako **založené na prostředcích použitím KCD**.

Na základě prostředků použitím KCD je nakonfigurován pomocí prostředí PowerShell. Můžete použít `Set-ADComputer` nebo `Set-ADUser` rutin, v závislosti na tom, zda je účet zosobnění účtu počítače nebo účet služby nebo účet uživatele.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Konfigurace založené na prostředcích použitím KCD pro účet počítače na spravované doméně
Předpokládejme, máte webovou aplikaci spuštěné v počítači "contoso100-webapp.contoso100.com'. Potřebuje pro přístup k prostředku (webového rozhraní API systémem "contoso100-api.contoso100.com') v kontextu uživatele domény. Zde je, jak by nastavit založené na prostředcích použitím KCD pro tento scénář.

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Konfigurace založené na prostředcích použitím KCD pro uživatelský účet na spravované doméně
Předpokládejme, že máte webové aplikace spuštěna jako účet služby, appsvc' a musí se přístup k prostředku (web API spuštěna jako účet služby - 'backendsvc') v kontextu uživatele domény. Zde je, jak by nastavit založené na prostředcích použitím KCD pro tento scénář.

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Omezené delegování přehled protokolu Kerberos](https://technet.microsoft.com/library/jj553400.aspx)
