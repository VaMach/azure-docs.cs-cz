---
title: "Otočit tajných klíčů v zásobníku Azure | Microsoft Docs"
description: "Naučte se otočit tajných klíčů v zásobníku Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: e2e9d93af3889714ade1d0364a6f747c184e6d75
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Otočit tajné klíče v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy*

Aktualizace hesla pro součásti zásobníku Azure v pravidelných cadence.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Aktualizace hesla pro řadič pro správu základní desky (BMC)

Řadiče pro správu základní desky (BMC) monitorování fyzickému stavu vašich serverů. Specifikace a pokyny, aktualizace hesla řadiče BMC lišit v závislosti na dodavatele hardwaru, výrobce (OEM).

1. Aktualizujte BMC na fyzických serverech zásobník Azure podle pokynů vaší výrobce OEM. Heslo pro každou BMC ve vašem prostředí musí být stejné.
2. Otevřete koncový bod privilegované v relacích zásobník Azure. Pokyny naleznete v tématu [pomocí privilegované koncový bod v zásobníku Azure](azure-stack-privileged-endpoint.md).
3. Po vaše prostředí PowerShell řádku změnil na **[ERCS virtuálního počítače nebo IP adresu name]: PS >** nebo **[azs-ercs01]: PS >**, v závislosti na prostředí, spusťte `Set-BmcPassword` spuštěním `invoke-command`. Vaše proměnná privilegované koncový bod relace předejte jako parametr. Příklad:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```
    
    Můžete také použít statické verze prostředí PowerShell s hesla jako řádky kódu:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```

## <a name="next-steps"></a>Další postup

Další informace o zabezpečení a zásobník Azure najdete v tématu [postavení zabezpečení infrastruktury Azure zásobníku](azure-stack-security-foundations.md).
