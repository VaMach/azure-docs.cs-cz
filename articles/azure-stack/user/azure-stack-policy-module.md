---
title: "Použití modulu zásad Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak omezit předplatné služby Azure se bude chovat, jako je předplatné Azure zásobníku"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: sngun
ms.openlocfilehash: c9c96d4a536949e0406af53d218949b57517ba87
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Správa Azure zásad pomocí modulu zásad Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Modul zásad zásobník Azure umožňuje nakonfigurovat předplatné služby Azure pomocí stejné verze a dostupnost služby jako zásobník Azure.  Modul používá **New-AzureRMPolicyAssignment** vytvořte zásadu Azure, což omezí typy prostředků a služeb dostupných v předplatném.  Po dokončení můžete vyvíjet aplikace, které jsou cílem zásobník Azure vašeho předplatného Azure.  

## <a name="install-the-module"></a>Instalace modulu
1. Nainstalujte požadovaná verze modulu AzureRM PowerShell, jak je popsáno v krok 1 z [instalaci prostředí PowerShell pro Azure zásobníku](azure-stack-powershell-install.md).   
2. [Stažení nástroje Azure zásobníku z Githubu](azure-stack-powershell-download.md)  
3. [Konfigurace PowerShellu pro použití s Azure Stackem](azure-stack-powershell-configure-user.md)

4. Naimportujte modul AzureStack.Policy.psm1:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-subscription"></a>Uplatňovat zásady na předplatné
Tento příkaz lze použít výchozí zásady Azure zásobníku u vašeho předplatného Azure. Dřív, než spustíte, nahraďte *název předplatného Azure* s předplatným Azure.

```PowerShell
Login-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Použití zásad pro skupinu prostředků
Můžete použít zásady v podrobnější metoda.  Například můžete mít další prostředky spuštěná ve stejném předplatném.  Použití zásad pro určité skupiny zdrojů, což umožňuje testování aplikace pro Azure zásobníku pomocí prostředků Azure, můžete určit obor. Dřív, než spustíte, nahraďte *název předplatného Azure* názvem svého předplatného Azure.

```PowerShell
Login-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Zásady v akci
Jakmile nasadíte zásady Azure, obdržíte chybu při pokusu o nasazení na prostředek, který zakazuje zásadami.  

![Výsledek prostředků nasazení se nezdařilo z důvodu omezení zásad](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Další kroky
[Nasazení šablon pomocí PowerShellu](azure-stack-deploy-template-powershell.md)

[Nasazení šablon pomocí rozhraní příkazového řádku Azure](azure-stack-deploy-template-command-line.md)

[Nasazení šablon pomocí sady Visual Studio](azure-stack-deploy-template-visual-studio.md)
