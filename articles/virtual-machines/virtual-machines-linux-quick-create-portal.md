---
title: "Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Virtuální počítače s Linuxem si můžete vytvořit také pomocí webu Azure Portal."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 1/17/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: beff4fb41ed46b016088734054e7a7897fed1a30
ms.openlocfilehash: 7287b87b1e50e28de06a5363a1f35bd7ac34d51c
ms.lasthandoff: 02/15/2017


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Vytvoření virtuálního počítače s Linuxem v Azure pomocí Portálu
Tento článek ukazuje, jak vytvořit virtuální počítač s Linuxem pomocí [webu Azure Portal](https://portal.azure.com/).

Požadavky:

* [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Soubory veřejného a privátního klíče SSH](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sign-in"></a>Přihlášení
Přihlaste se k webu Azure Portal s vaší identitou účtu Azure. Klikněte na **+ Nový** v levém horním rohu:

![Vytvoření prostředku Azure](./media/virtual-machines-linux-quick-create-portal/create_new_resource.png)

## <a name="choose-vm"></a>Výběr virtuálního počítače
V seznamu **Marketplace** klikněte na **Compute** a potom v seznamu imagí **Vybrané aplikace** vyberte **Ubuntu Server 16.04 LTS**.  V dolní části ověřte, že je nastavený model nasazení `Resource Manager`, a potom klikněte na **Vytvořit**.

![Výběr image virtuálního počítače z Azure Marketplace](./media/virtual-machines-linux-quick-create-portal/create_new_vm.png)

## <a name="enter-vm-options"></a>Zadání možností virtuálního počítače
Na stránce **Základy** zadejte:

* Název virtuálního počítače
* typ disku virtuálního počítače (SSD ve výchozím nastavení, nebo HDD),
* uživatelské jméno správce,
* **Typ ověřování** nastavte na **Veřejný klíč SSH**,
* veřejný klíč SSH jako řetězec (z vašeho adresáře `~/.ssh/`),
* název skupiny prostředků nebo vyberte existující skupinu

a pokračujte kliknutím na **OK**. Okno by mělo vypadat jako na tomto snímku obrazovky:

![Zadání základních možností virtuálního počítače](./media/virtual-machines-linux-quick-create-portal/enter_basic_vm_details.png)

## <a name="choose-vm-size"></a>Výběr velikosti virtuálního počítače
Vyberte velikost virtuálního počítače. V následujících příkladech je zvolena velikost **DS1_V2 Standard**, která nainstaluje Ubuntu na disk typu Premium. **S** ve velikosti virtuálního počítače označuje podporu SSD. Kliknutím na **Vybrat** nakonfigurujte nastavení.

![Výběr velikosti virtuálního počítače Azure](./media/virtual-machines-linux-quick-create-portal/select_vm_size.png)

## <a name="storage-and-network"></a>Úložiště a síť
V okně **Nastavení** se můžete rozhodnout, jestli pro virtuální počítač chcete použít Azure Managed Disks. Aktuální výchozí nastavení je použití nespravovaných disků. O službu Azure Managed Disks se stará platforma Azure a služba nevyžaduje žádné přípravy ani umístění, kam ji uložit. Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../storage/storage-managed-disks-overview.md). V případě nespravovaných disků je nutné vytvořit nebo vybrat účet úložiště pro virtuální pevné disky:

![Výběr účtu úložiště pro nespravované disky](./media/virtual-machines-linux-quick-create-portal/configure_non_managed_disks.png)

Pokud se rozhodnete použít službu Azure Managed Disks, nemusíte konfigurovat žádné další nastavení účtu, jak je znázorněno v následujícím příkladu:

![Výběr možnosti Azure Managed Disks na portálu](./media/virtual-machines-linux-quick-create-portal/select_managed_disks.png)

U zbývajících nastavení sítě ponechejte výchozí hodnoty.

## <a name="confirm-vm-settings-and-launch"></a>Potvrzení nastavení virtuálního počítače a spuštění
Potvrďte nastavení svého nového virtuálního počítače s Ubuntu a klikněte na **OK**.

![Kontrola nastavení a vytvoření virtuálního počítače Azure](./media/virtual-machines-linux-quick-create-portal/review_final_vm_settings.png)

## <a name="select-the-vm-resource"></a>Výběr prostředku virtuálního počítače
Otevřete domovskou stránku portálu a z nabídky v levém horním rohu vyberte **Skupiny prostředků**. V případě potřeby seznam rozbalte kliknutím na tři pruhy, jak je znázorněno tady:

![Otevření seznamu skupin prostředků](./media/virtual-machines-linux-quick-create-portal/select_resource_group.png)

Vyberte skupinu prostředků a potom klikněte na nový virtuální počítač:

![Vyhledání nastavení síťového adaptéru virtuálního počítače Azure](./media/virtual-machines-linux-quick-create-portal/select_vm_resource.png)

## <a name="find-the-public-ip"></a>Vyhledání veřejné IP adresy
Zobrazte **Veřejnou IP adresu** přiřazenou k vašemu virtuálnímu počítači:

![Získání veřejné IP adresy virtuálního počítače Azure](./media/virtual-machines-linux-quick-create-portal/view_public_ip_address.png)

## <a name="ssh-to-the-vm"></a>Přístup k virtuálnímu počítači přes SSH
Přístup přes SSH k veřejné IP adrese získáte pomocí veřejného klíče SSH.  Z pracovní stanice se systémem Mac nebo Linux můžete získat přístup přes SSH přímo z terminálu. Na pracovní stanici s Windows musíte pro přístup k Linuxu přes SSH použít PuTTY, MobaXTerm nebo Cygwin.  Pokud potřebujete pracovní stanici s Windows připravit na přístup k Linuxu přes SSH, můžete použít tento návod.

[Jak použít klíče SSH s Windows v Azure](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```
ssh -i ~/.ssh/azure_id_rsa ops@40.112.255.214
```

## <a name="next-steps"></a>Další kroky
Rychle jste si vytvořili virtuální počítač s Linuxem, abyste si ho mohli otestovat a abychom si ukázali, jak se to dělá. Pokud budete chtít vytvořit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků.

* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablon](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


