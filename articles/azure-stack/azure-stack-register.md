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
ms.date: 10/27/2017
ms.author: erikje
ms.openlocfilehash: 24cde66a132ae2e1ba0eb9b1564915746e5ca448
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
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
- [Registrovaný poskytovatel prostředků Azure zásobníku](#register-azure-stack-resource-provider-in-azure).

Pokud nemáte předplatné Azure, který splňuje tyto požadavky, můžete [vytvořit bezplatný účet Azure zde](https://azure.microsoft.com/en-us/free/?b=17.06). Registrace Azure zásobníku způsobuje bez nákladů na vaše předplatné Azure.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Registrace poskytovatele prostředků Azure zásobníku v Azure
> [!NOTE] 
> Tento krok stačí provést jen jednou v prostředí Azure zásobníku.
>

1. Spusťte prostředí Powershell ISE jako správce.
2. Přihlaste se k účtu Azure, který je vlastníkem předplatného Azure s parametrem - EnvironmentName nastavena na "AzureCloud".
3. Zaregistrujte zprostředkovatele prostředků Azure "Microsoft.AzureStack".

Příklad: 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```


## <a name="register-azure-stack-with-azure"></a>Zaregistrovat Azure zásobník Azure

> [!NOTE]
>Všechny tyto kroky musí dokončit v hostitelském počítači.
>

1. [Instalace prostředí PowerShell pro Azure zásobníku](azure-stack-powershell-install.md). 
2. Kopírování [RegisterWithAzure.psm1 skriptu](https://go.microsoft.com/fwlink/?linkid=842959) do složky (třeba C:\Temp).
3. Spusťte prostředí PowerShell ISE jako správce a naimportujte modul RegisterWithAzure.    
4. Z RegisterWithAzure.psm1 skriptu spusťte modul AzsRegistration přidat. Nahraďte zástupné symboly následující: 
    - *YourCloudAdminCredential* je objekt prostředí PowerShell, který obsahuje místní domény přihlašovací údaje pro domain\cloudadmin (pro development kit jde azurestack\cloudadmin).
    - *YourAzureSubscriptionID* je ID předplatného Azure, které chcete použít k registraci Azure zásobníku.
    - *YourAzureDirectoryTenantName* je název adresáře klientovi Azure přidružené k předplatnému Azure. V klientovi adresář bude vytvořen prostředek registrace. 
    - *YourPrivilegedEndpoint* je název [privilegované koncový bod](azure-stack-privileged-endpoint.md).

    ```powershell
    Add-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -AzureDirectoryTenantName $YourAzureDirectoryTenantName  -AzureSubscriptionId $YourAzureSubscriptionId -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Development 
    ```
5. V okně místní přihlášení zadejte přihlašovací údaje předplatného Azure.

## <a name="verify-the-registration"></a>Ověření registrace

1. Přihlaste se k portálu správce (https://adminportal.local.azurestack.external).
2. Klikněte na tlačítko **další služby** > **Marketplace správu** > **přidat z Azure**.
3. Pokud se zobrazí seznam položek, které jsou k dispozici prostřednictvím Azure (např. WordPress), vaše Aktivace proběhla úspěšně.

> [!NOTE]
> Po dokončení registrace se nebude zobrazovat aktivní upozornění pro není registraci.

## <a name="next-steps"></a>Další kroky

[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)

