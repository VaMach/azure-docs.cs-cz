---
title: "Zaregistrovat Azure zásobníku | Microsoft Docs"
description: "Zaregistrujte Azure zásobníku ve vašem předplatném Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: erikje
ms.openlocfilehash: 6ce8f86592ece59e338578be86c2cb673c35dbc1
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Registrace Azure zásobníku u vašeho předplatného Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Můžete zaregistrovat [zásobník Azure](azure-stack-poc.md) s Azure můžete stáhnout z Azure marketplace položky a nastavit obchodování při generování sestav dat zpět do společnosti Microsoft. 

> [!NOTE]
>Registrace je doporučená, protože umožňuje otestovat důležité zásobník Azure funkcí, jako je syndikace marketplace a vytváření sestav využití. Po registraci Azure zásobníku využití oznamovány obchodu Spojených států v Azure. Zobrazí se v rámci předplatného, které jste použili pro registraci. Azure uživatelé Development Kit zásobníku nejsou účtovat žádné využití, které patří.
>


## <a name="get-azure-subscription"></a>Získání předplatného Azure

Před registrací zásobník Azure s Azure, musíte mít:

- ID předplatného pro předplatné Azure. Chcete-li získat ID, přihlaste se k Azure, klikněte na **další služby** > **odběry**, klikněte na předplatné, které chcete použít, a v části **Essentials** můžete najít **ID předplatného**. Čína, Německu a US government Cloudová předplatná nejsou aktuálně podporovány.
- Uživatelské jméno a heslo pro účet, který je vlastníkem předplatného (podporovány jsou účty MSA nebo 2FA).
- Azure Active Directory pro předplatné Azure. Tento adresář můžete najít v Azure podržením ukazatele nad miniatury v pravém horním rohu portálu Azure. 

Pokud nemáte předplatné Azure, který splňuje tyto požadavky, můžete [vytvořit bezplatný účet Azure zde](https://azure.microsoft.com/en-us/free/?b=17.06). Registrace Azure zásobníku způsobuje bez nákladů na vaše předplatné Azure.


## <a name="register-azure-stack-with-azure"></a>Zaregistrovat Azure zásobník Azure

> [!NOTE]
> Všechny tyto kroky musí spustit z počítače, který má přístup k privilegované koncový bod. Pro Azure zásobníku Development Kit je hostitelský počítač. Pokud používáte integrovaný systém, obraťte se na vaše operátor zásobník Azure.
>

1. Otevřete konzolu prostředí PowerShell jako správce a [nainstalujte prostředí PowerShell pro Azure zásobníku](azure-stack-powershell-install.md).  

2. Přidáte účet Azure, který použijete k registraci Azure zásobníku. Chcete-li to provést, spusťte `Add-AzureRmAccount` rutiny s parametrem EnvironmentName nastavena na "AzureCloud". Zobrazí se výzva k zadání přihlašovacích údajů účtu Azure a možná budete muset použít 2 ověřování na základě konfigurace vašeho účtu. 

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Pokud máte více předplatných, spusťte následující příkaz a vyberte ten, že který chcete použít:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Zaregistrujte zprostředkovatele prostředků AzureStack ve vašem předplatném Azure. Chcete-li to provést, spusťte následující příkaz:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Odstranit všechny existující verze moduly Powershellu, které odpovídají registrace a [stáhněte nejnovější verzi z webu GitHub](azure-stack-powershell-download.md).  

6. Z adresáře "AzureStack-nástroje master", který je vytvořen v předchozím kroku přejděte do složky "Registrace" a naimportujte modul ".\RegisterWithAzure.psm1":  

   ```powershell 
   Import-Module .\RegisterWithAzure.psm1 
   ```

7. Ve stejné relaci prostředí PowerShell spusťte následující skript. Po zobrazení výzvy k zadání pověření zadejte `azurestack\cloudadmin` jako uživatel a heslo je stejné jako to, co můžete používat pro místní správce během nasazení.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Add-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -AzureSubscriptionId $AzureContext.Subscription.SubscriptionId `
       -AzureDirectoryTenantName $AzureContext.Tenant.TenantId `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development 
   ```

   | Parametr | Popis |
   | -------- | ------------- |
   | CloudAdminCredential | Přihlašovací údaje domény cloudu, které slouží jako [přístup privilegované koncový bod](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Uživatelské jméno je ve formátu "\<domény Azure zásobníku\>\cloudadmin". Pro development kit uživatelské jméno je nastavena na "azurestack\cloudadmin". Pokud používáte integrovaný systém, obraťte se na vaše zásobník Azure operátor získat tuto hodnotu.|
   | AzureSubscriptionId | Předplatné Azure, který použijete k registraci Azure zásobníku.|
   | AzureDirectoryTenantName | Název adresáře klientovi Azure, která souvisí s předplatným Azure. V klientovi adresář bude vytvořen prostředek registrace. |
   | PrivilegedEndpoint | Předem nakonfigurovaná vzdáleného prostředí PowerShell konzoly, která poskytuje funkce jako je shromažďování protokolů a jiné post úlohy nasazení. Pro development kit privilegované koncový bod je hostován na virtuálním počítači "AzS-ERCS01". Pokud používáte integrovaný systém, obraťte se na vaše zásobník Azure operátor získat tuto hodnotu. Další informace naleznete [pomocí privilegované koncový bod](azure-stack-privileged-endpoint.md) tématu.|
   | BillingModel | Fakturační model, který používá vaše předplatné. Povolené hodnoty pro tento parametr jsou – "Kapacity", "PayAsYouUse" a "Vývoj". Pro development kit tato hodnota nastavena na "Vývoj". Pokud používáte integrovaný systém, obraťte se na vaše zásobník Azure operátor získat tuto hodnotu. |

8. Po dokončení skriptu, zobrazí se zpráva "aktivace Azure zásobníku (Tento krok může trvat až 10 minut)." 

## <a name="verify-the-registration"></a>Ověření registrace

1. Přihlaste se k portálu správce (https://adminportal.local.azurestack.external).
2. Klikněte na tlačítko **další služby** > **Marketplace správu** > **přidat z Azure**.
3. Pokud se zobrazí seznam položek, které jsou k dispozici prostřednictvím Azure (např. WordPress), vaše Aktivace proběhla úspěšně.

> [!NOTE]
> Po dokončení registrace se nebude zobrazovat aktivní upozornění pro není registraci.

## <a name="next-steps"></a>Další kroky

[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)

