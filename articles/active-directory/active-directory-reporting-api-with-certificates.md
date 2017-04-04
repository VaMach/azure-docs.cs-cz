---
title: "Získání dat pomocí rozhraní API pro generování sestav Azure AD s certifikáty | Dokumentace Microsoftu"
description: "Vysvětluje, jak používat rozhraní API pro generování sestav Azure AD s přihlašovacími údaji ve formě certifikátů k získání dat z adresářů bez zásahu uživatele."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: c1345dcda6e52267a8037ffd7207e6bc3b0d3b31
ms.lasthandoff: 03/28/2017


---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a>Získání dat pomocí rozhraní API pro generování sestav Azure AD s certifikáty
Tento článek probírá, jak používat rozhraní API pro generování sestav Azure AD s přihlašovacími údaji ve formě certifikátů k získání dat z adresářů bez zásahu uživatele. 

## <a name="use-the-azure-ad-reporting-api"></a>Použití rozhraní API pro vytváření sestav Azure AD 
Rozhraní API pro vytváření sestav Azure AD vyžaduje, abyste provedli následující kroky:
 *    Požadavky na instalaci
 *    Nastavení certifikátu v aplikaci
 *    Získání přístupového tokenu
 *    Použití přístupového tokenu pro volání rozhraní Graph API

Informace o zdrojovém kódu najdete v tématu popisujícím [využití modulu rozhraní API pro sestavy](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils). 

### <a name="install-prerequisites"></a>Požadavky na instalaci
Musíte mít nainstalované Azure AD PowerShell verze 2 a modul AzureADUtils.

1. Stáhněte a nainstalujte Azure AD PowerShell verze 2 podle pokynů pro [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).
2. Stáhněte modul Azure AD Utils z umístění [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Tento modul poskytuje několik rutin nástrojů, jako jsou:
   * Nejnovější verze ADAL pomocí Nugetu
   * Přístupové tokeny od uživatele, klíče aplikace a certifikáty pomocí ADAL
   * Rozhraní Graph API zpracovávající stránkové výsledky

**Instalace modulu Azure AD Utils:**

1. Vytvořte adresář pro uložení modulu nástrojů (například c:\azureAD) a stáhněte modul z GitHubu.
2. Otevřete relaci PowerShellu a přejděte do adresáře, který jste právě vytvořili. 
3. Naimportujte modul a nainstalujte ho v cestě modulů PowerShellu pomocí rutiny Install-AzureADUtilsModule. 

Relace by měla vypadat podobně jako tato obrazovka:

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a>Nastavení certifikátu v aplikaci
1. Pokud již máte aplikaci, získejte její ID objektu na webu Azure Portal. 

  ![portál Azure](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Otevřete relaci PowerShellu a připojte se ke službě Azure AD pomocí rutiny Connect-AzureAD.

  ![portál Azure](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Pomocí rutiny New-AzureADApplicationCertificateCredential z AzureADUtils do ni přidejte přihlašovací údaje ve formě certifikátu. 

>[!Note]
>Je třeba zadat ID objektu aplikace, který jste si poznamenali dříve, a také objekt certifikátu (ten získáte pomocí jednotky Cert:).
>


  ![portál Azure](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Získání přístupového tokenu

Chcete-li získat přístupový token, použijte rutinu Get-AzureADGraphAPIAccessTokenFromCert z AzureADUtils. 

>[!NOTE]
>Budete muset použít ID aplikace, místo ID objektu použitého v poslední části.
>

 ![portál Azure](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a>Použití přístupového tokenu pro volání rozhraní Graph API

Nyní můžete vytvořit skript. Dále je uvedený příklad používající rutinu Invoke-AzureADGraphAPIQuery z AzureADUtils. Tato rutina zpracuje vícestránkové výsledky a pak tyto výsledky odešle do kanálu PowerShellu. 

 ![portál Azure](./media/active-directory-report-api-with-certificates/script-completed.png)

Nyní jste připraveni k exportu do souboru CSV a jeho uložení do systému SIEM. Můžete také zabalit váš skript do naplánované úlohy, abyste získávali data Azure AD z vašeho klienta pravidelně bez nutnosti ukládat klíče aplikace ve zdrojovém kódu. 

## <a name="next-steps"></a>Další kroky
[Základy správy identit Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>




