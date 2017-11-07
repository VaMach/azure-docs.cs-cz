---
title: "Řešení potíží Azure Cloud prostředí (Preview) | Microsoft Docs"
description: "Řešení potíží s prostředí cloudu Azure"
services: azure
documentationcenter: 
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: damaerte
ms.openlocfilehash: 1bf0acdfad04a55d5b85aabee150526836062002
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="troubleshooting-azure-cloud-shell"></a>Řešení potíží s prostředí cloudu Azure

Řešení známých problémů v prostředí cloudu Azure patří:

## <a name="general-resolutions"></a>Obecné řešení

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Dialogové okno úložiště - Chyba: 403 RequestDisallowedByPolicy
- **Podrobnosti o**: při vytváření účtu úložiště prostřednictvím cloudové prostředí, se nezdařilo z důvodu zásady služby Azure umístit váš správce. Chybová zpráva bude obsahovat:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Řešení**: Obraťte se na správce odstranit nebo aktualizovat Azure zásady odepření vytvoření úložiště Azure.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Dialogové okno úložiště - Chyba: 400 DisallowedOperation
 - **Podrobnosti o**: při použití předplatného služby Azure Active Directory, nelze vytvořit úložiště.
 - **Řešení**: používat předplatné Azure podporuje vytváření prostředků úložiště. Předplatná Azure AD, nebudou se moct vytváření prostředků Azure.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminálové výstup - Chyba: připojení terminál se nezdařilo: Nelze vytvořit protokolu websocket. Stiskněte klávesu `Enter` se znovu připojit.
 - **Podrobnosti o**: cloudové prostředí vyžaduje schopnost připojení protokolu websocket prostředí cloudové infrastruktury.
 - **Řešení**: Zkontrolujte jste nakonfigurovali nastavení sítě umožnit odesílání požadavky https a požadavky protokolu websocket do domén v *. console.azure.com.

## <a name="powershell-resolutions"></a>Řešení prostředí PowerShell

### <a name="no-home-directory-persistence"></a>Trvalost No $Home adresáře

- **Podrobnosti o**: všechna data aplikace (například: git, vim a dalších) zapisuje do `$Home` není zachován po relací prostředí PowerShell.
- **Řešení**: ve vašem profilu prostředí PowerShell vytvořit symbolický odkaz na aplikaci v konkrétní složce `clouddrive` na $Home.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>CTRL + C není ukončit rutiny řádku

- **Podrobnosti o**: Při pokusu o ukončení výzvou rutiny `Ctrl+C` neexistuje řádku.
- **Řešení**: ukončíte řádku stiskněte `Ctrl+C` pak `Enter`.

### <a name="gui-applications-are-not-supported"></a>Grafické uživatelské rozhraní aplikací není podporované.

- **Podrobnosti o**: Pokud uživatel spustí aplikaci pomocí grafického uživatelského rozhraní, nevrátí řádku. Například když uživatel provede klonování privátní úložiště GitHub, který je dvoufaktorové ověřování povoleno, se zobrazí dialogové okno pro dokončení dvoufaktorové ověřování.
- **Řešení**: `Ctrl+C` ukončíte příkaz.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - online nelze otevřít stránku nápovědy

- **Podrobnosti o**: Pokud typy uživatelů `Get-Help Find-Module -online`, například jeden zobrazí chybovou zprávu:`Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Řešení**: Zkopírujte adresu url a ručně ho otevřete v prohlížeči.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Řešení potíží s vzdálenou správu virtuálních počítačů Azure

- **Podrobnosti o**: z důvodu výchozí nastavení brány Windows Firewall pro WinRM může uživatel zobrazit následující chyba:`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Řešení**: Zkontrolujte, že je spuštěný virtuální počítač. Můžete spustit `Get-AzureRmVM -Status` chcete zjistit stav virtuálního počítače.  V dalším kroku přidejte nové pravidlo brány firewall na vzdálené virtuální počítač povolit vzdálená připojení z žádné podsítě, například

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 Můžete použít [rozšíření vlastních skriptů Azure](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) předejdete přihlášení k virtuálnímu počítači vzdáleného pro přidání nové pravidlo brány firewall.
 Předchozí skript můžete uložit do souboru, například `addfirerule.ps1`a odešlete ji do vašeho kontejneru úložiště Azure.
 Opakujte následující příkaz:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir`ukládá do mezipaměti výsledek v Azure jednotky

- **Podrobnosti o**: výsledek `dir` se uloží do mezipaměti v Azure jednotce.
- **Řešení**: Po vytvoření nebo odeberete prostředek v Azure jednotky zobrazení, spusťte `dir -force` aktualizovat.
