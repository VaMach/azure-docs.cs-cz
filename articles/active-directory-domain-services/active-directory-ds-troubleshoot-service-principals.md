---
title: "Azure Active Directory Domain Services: Řešení potíží s konfigurací instanční objekt | Microsoft Docs"
description: "Řešení potíží s konfigurací objekt služby pro Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2018
ms.author: ergreenl
ms.openlocfilehash: 7388bb291f665f195355a01d19a82cba9ed453eb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Řešení potíží s neplatná instanční objekt konfigurace pro spravované doméně

Tento článek vám odstraňovat potíže a řešit chyby související objekt konfigurace služby, které mít za následek následující zpráva s výstrahou pomůže:

## <a name="alert-aadds102-service-principal-not-found"></a>Výstrahy AADDS102: Instanční objekt nebyl nalezen
**Upozornění:** *požadované pro Azure AD Domain Services fungovat správně A instanční objekt se odstranil z adresáře služby Azure AD. Tato konfigurace ovlivní schopnost společnosti Microsoft monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.*

[Služba objektů](../active-directory/develop/active-directory-application-objects.md) jsou aplikace, které společnost Microsoft používá ke správě, aktualizovat a zachování vaší spravované domény. Pokud jsou odstraněny, dělí schopnost společnosti Microsoft služby vaší domény. 


## <a name="check-for-missing-service-principals"></a>Zkontrolujte chybějící objekty služby
K určení služby, která objekty je nutné znovu vytvořit pomocí následujících kroků:

1. Přejděte na [podnikové aplikace – všechny aplikace](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) na portálu Azure.
2. V **zobrazit** rozevíracího seznamu vyberte **všechny aplikace** a klikněte na tlačítko **použít**.
3. Pomocí následující tabulky, vyhledejte ID každé aplikace vkládání ID do vyhledávacího pole a stisknutím klávesy zadejte. Pokud výsledky hledání jsou prázdná, je nutné znovu vytvořit objekt služby podle pokynů ve sloupci "řešení".

| ID aplikace | Řešení |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Znovu vytvořte chybějící hlavní název služby pomocí prostředí PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Znovu zaregistrovat do oboru názvů Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Znovu zaregistrovat do oboru názvů Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Objekty služby, které vlastní opravit](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Znovu vytvořte chybějící hlavní název služby pomocí prostředí PowerShell
Postupujte podle těchto kroků, pokud objekt služby s ID ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` chybí z adresáře služby Azure AD.

**Náprava:** potřebujete Azure AD PowerShell k provedení těchto kroků. Informace o instalaci Azure AD PowerShell najdete v tématu [v tomto článku](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Chcete-li tento problém vyřešit, zadejte následující příkazy v okně prostředí PowerShell:
1. Instalace modulu Azure AD PowerShell a naimportujte ho.
    
    ```powershell 
    Install-Module AzureAD
    Import-Module AzureAD
    ```
    
2. Zkontrolujte, zda je objekt služby pro Azure AD Domain Services vyžaduje chybí ve vašem adresáři spuštěním následujícího příkazu Powershellu:
    
    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```
    
3. Vytvořte objekt služby zadáním následujícího příkazu Powershellu:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```
    
4. Po vytvoření na chybějící službu objektu zabezpečení, dvě hodiny počkejte a zkontrolujte stav vaší spravované domény.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Znovu zaregistrovat do oboru názvů Microsoft AAD na portálu Azure
Postupujte podle těchto kroků, pokud objekt služby s ID ```443155a6-77f3-45e3-882b-22b3a8d431fb``` nebo ```abba844e-bc0e-44b0-947a-dc74e5d09022``` chybí z adresáře služby Azure AD.

**Náprava:** pomocí následujících kroků k obnovení Domain Services na adresáře:

1. Přejděte na [odběry](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.
2. Zvolte předplatné z tabulky, která souvisí s vaší spravované domény
3. Pomocí levé navigaci, zvolte **zprostředkovatelé prostředků**
4. Vyhledejte "Microsoft.AAD" v tabulce a klikněte na **znovu zaregistrovat.**
5. K zajištění, že je vyřešený výstrahy, zobrazte stránku stavu vaší spravované domény dvou hodin.


## <a name="service-principals-that-self-correct"></a>Objekty služby, který vlastní opravte
Postupujte podle těchto kroků, pokud objekt služby s ID ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` chybí z adresáře služby Azure AD.

**Náprava:** Azure AD Domain Services může rozpoznat, kdy se tato konkrétní instanční objekt nebyl nalezen, chybně nakonfigurovaná nebo odstranit. Službu znovu vytvoří tuto instanční objekt. Zkontrolujte stav vaší spravované domény po dvou hodin k zajištění, že objekt služby byl vytvořen znovu.


## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na produktový tým Azure Active Directory Domain Services na [sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).
