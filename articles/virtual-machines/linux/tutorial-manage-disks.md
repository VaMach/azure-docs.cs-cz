---
title: "Spravovat disky systému Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – spravovat disky systému Azure pomocí Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 41b122cdb4dcb836b431004fc162ebe06d0c8b17
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="manage-azure-disks-with-the-azure-cli"></a>Spravovat disky systému Azure pomocí Azure CLI

Virtuální počítače Azure pomocí disků ukládat virtuální počítače operačního systému, aplikace a data. Při vytváření virtuálního počítače je důležité, abyste zvolili velikost disku a konfigurace, které jsou vhodné pro očekávané úlohy. Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů. Informace o:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků
> * Změna velikosti disků
> * Snímky disku


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Výchozí disky systému Azure

Vytvoření virtuálního počítače Azure má dva disky jsou automaticky připojena k virtuálnímu počítači. 

**Disk s operačním systémem** -provoz systémových disků může mít velikost až 1 terabajt a hostuje virtuální počítače operačního systému. Disk operačního systému je označeno */dev/sda* ve výchozím nastavení. Disk ukládání do mezipaměti konfigurace disku operačního systému je optimalizovaná pro výkon operačního systému. Z důvodu tuto konfiguraci, disk operačního systému **neměli** hostitelem aplikace nebo data. Pro aplikace a data použijte datových disků, které jsou podrobně popsané dál v tomto článku. 

**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na stejném hostiteli Azure jako virtuální počítač. Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování. Pokud se virtuální počítač přesune do nového hostitele, je odebrat všechna data uložená na dočasném disku. Velikost dočasné disku je určen podle velikosti virtuálního počítače. Dočasné disky jsou označeny */dev/sdb* a mít přípojný bod systému */mnt*.

### <a name="temporary-disk-sizes"></a>Velikosti dočasné disků

| Typ | Velikost virtuálního počítače | Maximální velikost dočasného disk (GB) |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 800 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 800 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 6144 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 5630 |
| [GPU](sizes-gpu.md) | N řady | 1440 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 2000 |

## <a name="azure-data-disks"></a>Azure datových disků

Pro instalaci aplikací a ukládání dat přidáním dalších datových disků. Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště. Každý datový disk má maximální kapacita 1 terabajt. Velikost virtuálního počítače určuje, kolik datových disků lze připojit k virtuálnímu počítači. Pro každý virtuální procesory virtuálních počítačů lze připojit dvěma datovými disky. 

### <a name="max-data-disks-per-vm"></a>Maximální počet datových disků na virtuální počítač

| Typ | Velikost virtuálního počítače | Maximální počet datových disků na virtuální počítač |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 32 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 32 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 64 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 64 |
| [GPU](sizes-gpu.md) | N řady | 48 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 32 |

## <a name="vm-disk-types"></a>Typy disků virtuálních počítačů

Azure nabízí dva typy disku.

### <a name="standard-disk"></a>Disků na úrovni Standard

Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu. Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.

### <a name="premium-disk"></a>Premium disku

Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku. Ideální pro virtuální počítače se systémem produkční zatížení. Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series. Pro prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost disku určuje typ disku. Když vyberete, je velikost disku hodnota zaokrouhlený nahoru na další typ. Například pokud je velikost disku je menší než 128 GB, typ disku je P10. Pokud je velikost disku je 129 GB až 512 GB, velikost je P20. Nic více než 512 GB, velikost je P30.

### <a name="premium-disk-performance"></a>Výkon disku Premium

|Typ disku úložiště Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Velikost disku (round nahoru) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Maximum vstupně-výstupních operací za sekundu (IOPS) na disk | 500 | 2,300 | 5,000 |
Propustnost / disk | 100 MB/s | 150 MB/s | 200 MB/s |

Při výše uvedené tabulce jsou uvedeny maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků. Pro instanci virtuálního počítače Standard_GS5 můžete dosáhnout maximálně 80 000 IOPS. Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](sizes.md).

## <a name="create-and-attach-disks"></a>Vytvořte a připojte disky

Datové disky můžete vytvořit a v okamžiku vytvoření virtuálního počítače nebo na existující virtuální počítač připojen.

### <a name="attach-disk-at-vm-creation"></a>Připojit disk při vytváření virtuálního počítače

Vytvořte skupinu prostředků pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Vytvoření virtuálního počítače pomocí [vytvořit virtuální počítač az]( /cli/azure/vm#create) příkaz. `--datadisk-sizes-gb` Argument slouží k určení, že další disk vytvořit a připojit k virtuálnímu počítači. Pokud chcete vytvořit a připojit více než jeden disk, použijte mezerami oddělený seznam hodnot velikosti disku. V následujícím příkladu je virtuální počítač vytvořený s dvěma datovými disky, oba 128 GB. Vzhledem k velikosti disků jsou 128 GB, jsou obě tyto disky nakonfigurované jako P10s, které poskytují maximální 500 IOPS na disku.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Připojit disk k existující virtuální počítač

Chcete-li vytvořit a připojit nový disk k existující virtuální počítač, použijte [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) příkaz. Následující příklad vytvoří disk premium, 128 GB, velikost a připojí k virtuální počítač vytvořený v předchozím kroku.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Příprava datových disků

Jakmile byla přiřazena disk k virtuálnímu počítači, musí se nakonfigurovat na použití disku operačního systému. Následující příklad ukazuje, jak ručně nakonfigurovat disk. Tento proces je také možné automatizovat pomocí inicializací cloudu, kterému se věnujeme v [novější kurzu](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Ruční konfigurace

Vytvořte připojení SSH k virtuálnímu počítači. Nahraďte IP adresu příklad veřejné IP adresy virtuálního počítače.

```azurecli-interactive 
ssh 52.174.34.95
```

Rozdělit disk s `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Zapsat pomocí systému souborů k oddílu `mkfs` příkaz.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Připojte nový disk, aby byl přístupný v operačním systému.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Disk je teď přístupná prostřednictvím *datadrive* přípojný bod, který lze ověřit spuštěním `df -h` příkaz. 

```bash
df -h
```

Výstup ukazuje nový disk připojit na */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Aby se zajistilo, že jednotka je znovu připojeny po restartování systému, je nutné přidat do */etc/fstab* souboru. Uděláte to tak získat identifikátor UUID disku spolu s `blkid` nástroj.

```bash
sudo -i blkid
```

Výstup zobrazuje identifikátor UUID jednotky `/dev/sdc1` v tomto případě.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Přidá řádek podobný následujícímu k */etc/fstab* souboru. Také Upozorňujeme, že zápis překážek můžete zakázat pomocí *bariéry = 0*, tato konfigurace může zlepšit výkon disku. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Teď, když byl nakonfigurován na disku, zavřete relace SSH.

```bash
exit
```

## <a name="resize-vm-disk"></a>Změna velikosti disku virtuálního počítače

Jakmile nasazen virtuální počítač, disk operačního systému nebo jakýchkoli připojených datových disků je možné zvýšit velikost. Zvětšení velikosti disku je v případě nutnosti další úložný prostor nebo vyšší úroveň výkonu (P10, P20, P30). Všimněte si, že se disky není možné snížit velikost.

Před zvýšením velikosti disku, je potřeba Id nebo název disku. Použití [seznam disků az](/cli/azure/disk#az_disk_list) příkaz vrátí všechny disky ve skupině prostředků. Poznamenejte si název disku, který chcete přizpůsobit.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

Virtuální počítač musí být také navrácena. Použití [az OM deallocate]( /cli/azure/vm#deallocate) příkaz k zastavení a zrušit přidělení virtuálního počítače.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Použití [aktualizace disku az](/cli/azure/vm/disk#update) příkaz ke změně velikosti disku. Tento příklad změní velikost disku s názvem *myDataDisk* na 1 terabajt.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Po dokončení operace změny velikosti, spusťte virtuální počítač.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Pokud jste ke změně velikosti disku operačního systému, je automaticky rozšířena oddílu. Pokud jste změnili datový disk, je potřeba rozšířit v operačním systému virtuální počítače žádné aktuální oddíly.

## <a name="snapshot-azure-disks"></a>Snímek disky systému Azure

Pořízení snímku disku vytvoří čtení pouze, v okamžiku kopie disku. Azure snímky virtuálních počítačů jsou užitečné pro rychle uložení stavu virtuálního počítače, před prováděním změn konfigurace. V případě, že změny konfigurace ukázat jako nežádoucí, může být stav virtuálního počítače obnovena pomocí snímku. Virtuální počítač má více než jeden disk, je převzat snímek každého disku nezávisle na ostatních. Za vyjádření zálohování konzistentní s aplikací, vezměte v úvahu před přepnutím snímky disku zastavení virtuálního počítače. Můžete taky použít [služby Azure Backup](/azure/backup/), což umožňuje provádět automatizované zálohování spuštěného virtuálního počítače.

### <a name="create-snapshot"></a>Vytvořit snímek

Před vytvořením snímku disku virtuálního počítače, je potřeba Id nebo název disku. Použití [az virtuálních počítačů zobrazit](https://docs.microsoft.com/cli/azure/vm#az_vm_show) příkaz vrátí id disku. V tomto příkladu je id disku uložené v proměnné, aby se může použít později.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Teď, když máte id disku virtuálního počítače, následující příkaz vytvoří snímek disku.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Vytvoření disku ze snímku

Tento snímek pak může být převedena na disk, který můžete použít k opětovnému vytvoření virtuálního počítače.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Virtuální počítač obnovit ze snímku

K předvedení obnovení virtuálního počítače, odstraňte existující virtuální počítač. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Vytvoření nového virtuálního počítače z disku snímku.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Připojte datový disk

Musí být znovu připojit k virtuálnímu počítači všechny disky data.

Nejprve najít název disku dat pomocí [seznam disků az](https://docs.microsoft.com/cli/azure/disk#az_disk_list) příkaz. Tento příklad umístí název disku do proměnné s názvem *datadisk*, která je použita v dalším kroku.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Použití [připojit disk virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) příkazu připojit disk.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků
> * Změna velikosti disků
> * Snímky disku

Přechodu na další informace o automatizaci konfigurace virtuálního počítače v dalším kurzu.

> [!div class="nextstepaction"]
> [Automatizace konfigurace virtuálních počítačů](./tutorial-automate-vm-deployment.md)
