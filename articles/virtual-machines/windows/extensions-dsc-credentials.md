---
title: "Předání přihlašovacích údajů do Azure pomocí DSC | Microsoft Docs"
description: "Přehled o bezpečně předávání přihlašovací údaje pro virtuální počítače Azure pomocí konfigurace požadovaného stavu prostředí PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Předání přihlašovacích údajů k obslužné rutině rozšíření Azure DSC
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Tento článek se týká konfigurace požadovaného stavu rozšíření pro Azure. Přehled obslužná rutina rozšíření DSC naleznete na adrese [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Předávání přihlašovací údaje
Jako součást procesu konfigurace můžete potřebovat nastavit uživatelské účty, získat přístup ke službám, nebo instalaci programu v kontextu uživatele. Provádět tyto kroky, budete muset zadat přihlašovací údaje. 

DSC umožňuje parametrizované konfigurace, ve kterých jsou přihlašovací údaje předané do konfigurace a bezpečně uloženo na soubory MOF. Obslužná rutina rozšíření Azure zjednodušuje správu přihlašovacích údajů prostřednictvím automatické správy certifikátů. 

Vezměte v úvahu následující skript konfigurace DSC, který vytvoří místní uživatelský účet pomocí zadaného hesla:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Je důležité zahrnout *uzlu localhost* jako součást konfigurace. Pokud tento příkaz chybí, následující kroky nefungují jako obslužná rutina rozšíření konkrétně hledá příkaz localhost uzlu. Je také důležité zahrnout typecast *[PsCredential]*podle konkrétního typu aktivuje rozšíření pro šifrování přihlašovacích údajů. 

Tento skript publikujte do úložiště objektů blob:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Nastavit rozšíření Azure DSC a zadejte přihlašovací údaje:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Jak jsou zabezpečené přihlašovací údaje
Spuštění tohoto kódu vyzve k zadání pověření. Jakmile je poskytována, uloží se do paměti stručně. Když je publikována s `Set-AzureVmDscExtension` rutiny ho se přenesou pomocí protokolu HTTPS k virtuálnímu počítači, kde Azure ukládá zašifrovaná na disku, certifikátem místní počítač. Pak je stručně dešifrovat v paměti a znovu zašifrována mají být předány DSC.

Toto chování se liší od [pomocí zabezpečené konfigurace bez obslužná rutina rozšíření](https://msdn.microsoft.com/powershell/dsc/securemof). Prostředí Azure poskytuje způsob, jak přenášet data konfigurace bezpečně prostřednictvím certifikátů. Pokud používáte obslužná rutina rozšíření DSC, není nutné poskytnout $CertificatePath nebo $CertificateID nebo $Thumbprint položku v ConfigurationData.

## <a name="next-steps"></a>Další kroky
Další informace o obslužná rutina rozšíření Azure DSC najdete v tématu [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Najít další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.

