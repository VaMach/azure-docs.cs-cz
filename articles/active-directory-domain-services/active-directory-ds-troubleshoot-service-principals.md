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
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Azure AD Domain Services – řešení potíží s instanční objekt konfigurace

Služby, spravovat a aktualizovat vaše doména, společnost Microsoft používá různé [objekty služby](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) ke komunikaci s adresářem. Pokud jeden je špatně nakonfigurovaný. nebo odstraníte, může to způsobit narušení služeb ve službě.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Nebyl nalezen objekt služby

**Zpráva s výstrahou:**

*Objekt služby vyžaduje pro správné fungování Azure AD Domain Services se odstranil z adresáře služby Azure AD. Tato konfigurace ovlivní schopnost společnosti Microsoft monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.*

Objekty služby jsou aplikace, které společnost Microsoft používá ke správě, aktualizovat a zachování vaší spravované domény. Pokud jsou odstraněny, dělí schopnost společnosti Microsoft služby vaší domény. Následující postup použijte k určení služby, která objekty je nutné znovu vytvořit.

1. Přejděte na [podnikové aplikace – všechny aplikace](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) na portálu Azure.
2. Pomocí rozevíracího seznamu zobrazit, vyberte **všechny aplikace** a klikněte na tlačítko **použít**.
3. Pomocí následující tabulky, vyhledejte ID každé aplikace vkládání ID do vyhledávacího pole a stisknutím klávesy zadejte. Pokud výsledky hledání jsou prázdná, je nutné znovu vytvořit objekt služby podle pokynů ve sloupci "řešení".

| ID aplikace | Řešení |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Nové vytvoření chybí hlavní název služby pomocí prostředí PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Znovu zaregistrovat do oboru názvů Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Znovu zaregistrovat do oboru názvů Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Objekty služby, které vlastní opravit](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>Znovu vytvořte chybějící hlavní název služby pomocí prostředí PowerShell

*Postupujte podle těchto kroků pro chybí ID: 2565bd9d-da50-47d4-8b85-4c97f669dc36*

**Náprava:**

Je třeba Azure AD PowerShell k provedení těchto kroků. Informace o instalaci Azure AD PowerShell najdete v tématu [v tomto článku](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Chcete-li tento problém vyřešit, zadejte následující příkazy v okně prostředí PowerShell:
1. Instalace modulu AzureAD
2. Import-Module AzureAD
3. Zkontrolujte, zda je objekt služby pro Azure AD Domain Services vyžaduje chybí ve vašem adresáři spuštěním následujícího příkazu Powershellu:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. Vytvořte objekt služby zadáním následujícího příkazu Powershellu:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Po vytvoření objektu služby chybí, dvě hodiny počkejte a zkontrolujte stav vaší doméně.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Znovu zaregistrovat do oboru názvů Microsoft AAD na portálu Azure

*Postupujte podle těchto kroků pro chybí ID: 443155a6-77f3-45e3-882b-22b3a8d431fb a abba844e-bc0e-44b0-947a-dc74e5d09022*

**Náprava:**

Pomocí následujících kroků k obnovení Domain Services na adresáře:

1. Přejděte na [odběry](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.
2. Zvolte předplatné z tabulky, která souvisí s vaší spravované domény
3. Pomocí levé navigaci, zvolte **zprostředkovatelé prostředků**
4. Vyhledejte "Microsoft.AAD" v tabulce a klikněte na **znovu zaregistrovat.**
5. Pokud chcete zkontrolovat a ujistěte se, že vaše výstraha vyřeší, zobrazte stránku výstrahy stavu dvou hodin.


### <a name="service-principals-that-self-correct"></a>Objekty služby, který vlastní opravte

*Postupujte podle těchto kroků pro chybí ID: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Náprava:**

Microsoft můžete určit konkrétní objekty služby chybí, chybně nakonfigurovaná nebo odstranit. Chcete-li opravit službu rychle, Microsoft znovu vytvoří objekty služby, sám sebe. Zkontrolujte stav vaší domény po dvou hodin k zajištění, že objekt byl vytvořen znovu.

## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na produktový tým Azure Active Directory Domain Services na [sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).
