---
title: "Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Virtuální počítače s Linuxem si můžete vytvořit také pomocí webu Azure Portal."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/28/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b1446cd8892e14988ff428eaa03233f8e9aefb8a


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Vytvoření virtuálního počítače s Linuxem v Azure pomocí Portálu
Tento článek ukazuje, jak vytvořit virtuální počítač s Linuxem pomocí [webu Azure Portal](https://portal.azure.com/).

Požadavky:

* [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Soubory veřejného a privátního klíče SSH](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="sign-in"></a>Přihlášení
Po přihlášení k webu Azure Portal s vaší identitou účtu Azure klikněte v levém horním rohu na **+ Nový**:

![obrazovka1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

## <a name="choose-vm"></a>Výběr virtuálního počítače
V seznamu **Marketplace** klikněte na **Virtuální počítače** a potom v seznamu imagí **Vybrané aplikace** vyberte **Ubuntu Server 14.04 LTS**.  V dolní části ověřte, že je nastavený model nasazení `Resource Manager`, a potom klikněte na **Vytvořit**.

![obrazovka2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

## <a name="enter-vm-options"></a>Zadání možností virtuálního počítače
Na stránce **Základy** zadejte:

* Název virtuálního počítače
* Uživatelské jméno pro účet Admin
* Typ ověřování nastavený na **Veřejný klíč SSH**
* Veřejný klíč SSH jako řetězec (z vašeho adresáře `~/.ssh/`)
* Název skupiny prostředků nebo vyberte existující skupinu.

Pak pokračujte kliknutím na **OK** a zvolte velikost virtuálního počítače. Obrazovka by měla vypadat jako tento snímek obrazovky:

![obrazovka3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

## <a name="choose-vm-size"></a>Výběr velikosti virtuálního počítače
Zvolte velikost **DS1**, která nainstaluje Ubuntu na disk SSD typu Premium. Pak klikněte na **Vybrat** a nakonfigurujte nastavení.

![obrazovka4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

## <a name="storage-and-network"></a>Úložiště a síť
V okně **Nastavení** nechte nastavené výchozí hodnoty pro možnosti Úložiště a Síť a pak kliknutím na **OK** zobrazte souhrn.  Všimněte si, že zvolením možnosti DS1 se typ disku nastavil na Premium SSD: **S** v názvu označuje SSD.

![obrazovka5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

## <a name="confirm-vm-settings-and-launch"></a>Potvrzení nastavení virtuálního počítače a spuštění
Potvrďte nastavení svého nového virtuálního počítače s Ubuntu a klikněte na **OK**.

![obrazovka6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

## <a name="find-the-vm-nic"></a>Vyhledání síťové karty virtuálního počítače
Otevřete řídicí panel Portálu a v části **Síťová rozhraní** zvolte svoji síťovou kartu.

![obrazovka7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

## <a name="find-the-public-ip"></a>Vyhledání veřejné IP adresy
Otevřete nabídku veřejných IP adres v nastaveních síťové karty.

![obrazovka8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

## <a name="ssh-to-the-vm"></a>Přístup k virtuálnímu počítači přes SSH
Přístup přes SSH k veřejné IP adrese získáte pomocí veřejného klíče SSH.  Z pracovní stanice se systémem Mac nebo Linux můžete získat přístup přes SSH přímo z terminálu. Na pracovní stanici s Windows musíte pro přístup k Linuxu přes SSH použít PuTTY, MobaXTerm nebo Cygwin.  Pokud potřebujete pracovní stanici s Windows připravit na přístup k Linuxu přes SSH, můžete použít tento návod.

[Jak použít klíče SSH s Windows v Azure](virtual-machines-linux-ssh-from-windows.md)

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Další kroky
Rychle jste si vytvořili virtuální počítač s Linuxem, abyste si ho mohli otestovat a abychom si ukázali, jak se to dělá. Pokud budete chtít vytvořit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků.

* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablon](virtual-machines-linux-cli-deploy-templates.md)
* [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md)




<!--HONumber=Nov16_HO2-->


