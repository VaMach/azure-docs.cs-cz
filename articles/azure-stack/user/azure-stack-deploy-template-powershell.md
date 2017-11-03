---
title: "Nasazení šablon pomocí prostředí PowerShell v zásobníku Azure | Microsoft Docs"
description: "Postup nasazení virtuálního počítače pomocí šablony Resource Manageru a prostředí PowerShell."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e4837be016b569dbd0b4bf8e071e6381b8daa85f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Nasazení šablon v zásobníku Azure pomocí prostředí PowerShell

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Pomocí prostředí PowerShell pro nasazení šablon Azure Resource Manageru do Azure zásobníku Development Kit.  Správce prostředků šablony nasazení a zřizování všechny prostředky pro svoji aplikaci v rámci jediné koordinované operace.

## <a name="run-azurerm-powershell-cmdlets"></a>Spusťte rutiny prostředí AzureRM PowerShell
V tomto příkladu můžete spustit skript pro nasazení virtuálního počítače Azure zásobníku Development Kit pomocí šablony Resource Manageru.  Než budete pokračovat, ujistěte se, máte [nakonfigurované prostředí PowerShell](azure-stack-powershell-configure-user.md)  

Virtuální pevný disk použitých v této šabloně příklad je Windows Server 2012 R2 Datacenter.

1. Přejděte na <http://aka.ms/AzureStackGitHub>, vyhledejte **101jednoduché windows-vm** šablony a uložte ho do následujícího umístění: c:\\šablony\\ azuredeploy-101jednoduché windows-vm.json.
2. V prostředí PowerShell spusťte následující skript nasazení. Nahraďte *uživatelské jméno* a *heslo* pomocí svého uživatelského jména a hesla. Na následné použití, zvýšit hodnotu *$myNum* parametr, abyste zabránili přepsání vaše nasazení.
   
   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```
3. Otevřete a klikněte na portálu Azure zásobníku **Procházet**, klikněte na tlačítko **virtuální počítače**a vyhledejte nový virtuální počítač (*myDeployment001*).


## <a name="next-steps"></a>Další kroky
[Nasazení šablon pomocí sady Visual Studio](azure-stack-deploy-template-visual-studio.md)

