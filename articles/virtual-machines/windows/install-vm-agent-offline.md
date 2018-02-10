---
title: "Instalace agenta virtuálního počítače Azure v Offline režimu | Microsoft Docs"
description: "Zjistěte, nainstalujte agenta virtuálního počítače Azure v režimu Offline."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: b38bfaffad3e214f3b854715e4d8030cde432bae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="install-the-vm-agent-in-offline-mode-in-an-azure-windows-vm"></a>Nainstalujte agenta virtuálního počítače v režimu offline virtuální počítač Windows Azure 

Agent virtuálního počítače poskytuje užitečné funkce jako je resetování hesla místního správce a nabízených skriptu. Tento článek ukazuje, jak nainstalovat agenta virtuálního počítače pro virtuální počítač, který je offline. 

## <a name="when-to-use-offline-mode"></a>Kdy použít offline režimu

Potřebovali byste k instalaci agenta virtuálního počítače v režimu offline v následujícím scénáři:

- Nasazení virtuálního počítače Azure, který nemá nainstalován agent virtuálního počítače nebo nepracuje agenta virtuálního počítače.
- Si nepamatujete heslo správce nebo uživatel nemá přístup virtuálních počítačů.

V tomto scénáři musíte nainstalovat agenta virtuálního počítače v režimu offline. 



## <a name="detailed-steps"></a>Podrobné kroky

**Krok 1: Připojte disk operačního systému virtuálního počítače na jiný virtuální počítač jako datový disk**

1.  Odstraňte virtuální počítač. Ujistěte se, že jste vybrali **zachovat disky** možnost, pokud to uděláte.

2.  Připojte disk operačního systému jako datový disk k jiným virtuálním Počítačem (Poradce při potížích virtuálních počítačů). Další informace najdete v tématu věnovaném [postupu připojení datového disku k virtuálnímu počítači s Windows na webu Azure Portal](attach-managed-disk-portal.md).

3.  Připojení k virtuálnímu počítači pro řešení potíží. Otevřete **Správa počítače** > **disku správu**. Ujistěte se, že disk operačního systému je online a že jeho oddíly mají přiřazené písmeno jednotky.

**Krok 2: Upravte disk operačního systému pro instalaci agenta virtuálního počítače**

1.  Zkontrolujte připojení ke vzdálené ploše k odstraňování souvisejících potíží virtuálního počítače.

2.  Na disku operačního systému, který jste připojili, přejděte na **\windows\system32\config**. Zkopírujte všechny soubory jako záložní v případě, že je vyžadován vrácení zpět.

3.  Spusťte Editor registru (regedit.exe).

4.  Klikněte na tlačítko **HKEY_LOCAL_MACHINE** klíče a potom vyberte **soubor** > **načíst podregistr** v nabídce.

    ![Zatížení hive](./media/install-vm-agent-offline/load-hive.png)

5.  Přejděte na **\windows\system32\config\SYSTEM** na operačního systému, které připojené na disku a pak zadejte BROKENSYSTEM jako název podregistr. Až to uděláte, zobrazí se hive v registru **HKEY_LOCAL_MACHINE**.

6.  Přejděte na **\windows\system32\config\SOFTWARE** na disk operačního systému jste připojili, zadejte BROKENSOFTWARE jako název podregistr.

7.  Pokud máte aktuální verzi agenta, který není funkční, proveďte zálohu aktuální konfiguraci. Pokud virtuální počítač nemá nainstalovaného agenta virtuálního počítače, přejděte k dalšímu kroku.  
      
    1. Přejmenujte složku \windowsazure \windowsazure.old

    2. Exportovat následující registry
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Použijte existující soubory pro řešení potíží virtuální počítač jako úložiště pro instalaci agenta virtuálního počítače: 

    1. Z Poradce při potížích virtuálních počítačů exportujte následující podklíče registru formát (REG): 

        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![Obrázek o export klíče registru](./media/install-vm-agent-offline/backup-reg.png)

    2. Upravit tyto tři registru soubory, změňte **systému** klíče položky do **BROKENSYSTEM**a potom uložte soubory.
        ![Bitovou kopii, změny klíče registru](./media/install-vm-agent-offline/change-reg.png)
    3. Dvojitým kliknutím na soubor registru soubory importovat.
    4. Ujistěte se, že tyto klíče jsou úspěšně importovány do podregistr BROKENSYSTEM: WindowsAzureGuestAgent, WindowsAzureTelemetryService a RdAgent.

9.  Zkopírujte složku agenta virtuálního počítače z C:\windowsazure\packages k &lt;disk operačního systému, který jste připojili&gt;: \windowsazure\packages.
    ![Obrázek o kopírovat soubory](./media/install-vm-agent-offline/copy-package.png)
      
    **Poznámka:** nemáte zkopírujte složku protokoly. Po spuštění služby, nové protokoly se budou generovat.

10.  Klikněte na tlačítko BROKENSYSTEM a vyberte **soubor** > **Uvolnit podregistr** z nabídky.

11.  Klikněte na tlačítko BROKENSOFTWARE a vyberte **soubor** > **Uvolnit podregistr** z nabídky.

12.  Odpojte disk a pak znovu vytvořte virtuální počítač pomocí disku operačního systému.

13.  Pokud přístup k virtuálnímu počítači se nyní zobrazí RDAgent systémem a protokoly získávání vytvořené.

14. Pokud je virtuální počítač vytvořené pomocí modelu nasazení classic, jste hotovi. Ale pokud je virtuální počítač vytvořený pomocí modelu nasazení Resource Manager, musí také používáte prostředí Azure PowerShell aktualizovat vlastnosti parametr ProvisionGuestAgent tak, že Azure ví, že virtuální počítač má nainstalovaného agenta. K tomu, spusťte následující příkazy v prostředí Azure PowerShell:

        $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        $vm.VM.ProvisionGuestAgent = $true
        Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>

    Pokud spustíte **Get-AzureVM** po provedení těchto kroků **GuestAgentStatus** vlastnost by měla naplněno místo prázdná stránka se zobrazí:

        Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus

## <a name="next-steps"></a>Další postup

- [Přehled služby Azure agenta virtuálního počítače](agent-user-guide.md)
- [Rozšíření virtuálního počítače a funkce pro Windows](extensions-features.md)