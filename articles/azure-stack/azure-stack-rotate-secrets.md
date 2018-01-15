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
ms.openlocfilehash: 0a4118a8927e4261fafa307af5b9c29623ce5c3f
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/13/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Otočit tajné klíče v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy*

Aktualizace hesla pro součásti zásobníku Azure v pravidelných cadence.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Aktualizace hesla pro řadič pro správu základní desky (BMC)

Řadiče pro správu základní desky (BMC) monitorování fyzickému stavu vašich serverů. Specifikace a pokyny, aktualizace hesla řadiče BMC lišit v závislosti na dodavatele hardwaru, výrobce (OEM).

1. Aktualizujte BMC na fyzických serverech zásobník Azure podle pokynů vaší výrobce OEM. Heslo pro každou BMC ve vašem prostředí musí být stejné.
2. Otevřete koncový bod privilegované v relacích zásobník Azure. Pokyny naleznete v tématu [pomocí privilegované koncový bod v zásobníku Azure](azure-stack-privileged-endpoint.md).
3. Po vaše prostředí PowerShell řádku změnil na **[ERCS virtuálního počítače nebo IP adresu name]: PS >** nebo **[azs-ercs01]: PS >**, v závislosti na prostředí, spusťte `Set-BmcPassword` spuštěním `invoke-command`. Vaše proměnná privilegované koncový bod relace předejte jako parametr.  
Příklad:
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Další postup

Další informace o zabezpečení a zásobník Azure najdete v tématu [postavení zabezpečení infrastruktury Azure zásobníku](azure-stack-security-foundations.md).