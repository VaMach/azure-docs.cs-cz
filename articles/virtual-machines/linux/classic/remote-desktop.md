---
title: "Vzdálená plocha pro virtuální počítač s Linuxem | Microsoft Docs"
description: "Postup instalace a konfigurace vzdálené plochy pro připojení k virtuálnímu počítači Microsoft Azure Linux pro model nasazení Classic"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: 68031d548bdbeda9a83d1bceaaea7c5bbcab3188
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Použití Vzdálené plochy pro připojení k virtuálnímu počítači Microsoft Azure s Linuxem
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Aktualizovaná verze Resource Manager v tomto článku, najdete v části [zde](../use-remote-desktop.md).

## <a name="overview"></a>Přehled
Protokol RDP (Remote Desktop Protocol) je vlastní protokol, pro systém Windows. Jak jsme pomocí protokolu RDP pro vzdálené připojení k virtuální počítač s Linuxem (virtuálním počítači)?

Tyto pokyny vám poskytne odpověď! Pomůže k instalaci a konfiguraci xrdp na vašem Microsoft Azure Linux virtuálním počítači, který vám umožní připojit se k němu pomocí vzdálené plochy z počítače s Windows. Linux virtuálního počítače s Ubuntu nebo OpenSUSE jako v příkladu v tomto návodu budeme používat.

Nástroj xrdp je serveru RDP s otevřeným zdrojem, která umožňuje připojit Linux server pomocí vzdálené plochy z počítače s Windows. RDP má lepší výkon než VNC (Computing virtuální sítě). VNC vykreslí pomocí grafiky JPEG kvality a může být pomalé, zatímco RDP je rychlý a jasný.

> [!NOTE]
> Již musí mít virtuální počítač Microsoft Azure s Linuxem. Vytvoření a nastavení virtuálního počítače s Linuxem najdete v tématu [kurzu virtuální počítač Azure s Linuxem](createportal.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Vytvořit koncový bod pro vzdálené plochy
Budeme používat výchozí koncový bod 3389 pro vzdálené plochy v této dokumentace. Nastavit 3389 koncový bod jako `Remote Desktop` k virtuálním počítačům s Linuxem jako níže:

![Bitové kopie](./media/remote-desktop/endpoint-for-linux-server.png)

Pokud nevíte jak nastavit koncový bod pro virtuální počítač, najdete v části [v tomto návodu](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Nainstalujte Gnome plochy
Připojení k virtuálním počítačům s Linuxem pomocí `putty`a nainstalujte `Gnome Desktop`.

Ubuntu použijte:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


Pro OpenSUSE použijte:

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a>Nainstalujte xrdp
Ubuntu použijte:

    #sudo apt-get install xrdp

Pro OpenSUSE použijte:

> [!NOTE]
> Aktualizujte OpenSUSE verze na verzi, kterou používáte v následujícím příkazu. Následující příklad je pro `OpenSUSE 13.2`.
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Spusťte xrdp a nastavte xdrp při spuštění
Pro OpenSUSE použijte:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Pro Ubuntu, bude spuštěn xrdp a eanbled na spouštěcí až po instalaci automaticky.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Pokud používáte verzi Ubuntu později než Ubuntu 12.04LTS pomocí xfce
Vzhledem k tomu, že aktuální verze xrdp nepodporuje Gnome Desktop pro verze Ubuntu později než Ubuntu 12.04LTS, budeme používat `xfce` plochy místo.

Chcete-li nainstalovat `xfce`, použijte tento příkaz:

    #sudo apt-get install xubuntu-desktop

Potom povolte `xfce` použití tohoto příkazu:

    #echo xfce4-session >~/.xsession

Upravte konfigurační soubor `/etc/xrdp/startwm.sh`:

    #sudo vi /etc/xrdp/startwm.sh   

Přidejte řádek `xfce4-session` před řádek `/etc/X11/Xsession`.

Chcete-li restartovat službu xrdp, použijte toto:

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Připojení virtuálním počítačům s Linuxem z počítače s Windows
V počítači s Windows spusťte klienta vzdálené plochy a zadejte název DNS virtuálních počítačů Linux. Přejděte na řídicí panel virtuálního počítače na portálu Azure a klikněte na tlačítko `Connect` připojení virtuálním počítačům s Linuxem. V takovém případě se zobrazí okno přihlášení:

![Bitové kopie](./media/remote-desktop/no2.png)

Přihlaste se pomocí uživatelské jméno a heslo k virtuálním počítačům s Linuxem.

## <a name="next-steps"></a>Další kroky
Další informace o používání xrdp najdete v tématu [http://www.xrdp.org/](http://www.xrdp.org/).
