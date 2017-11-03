---
title: "Vytvářet a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvářet a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI"
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
ms.openlocfilehash: bef7f6ef13f6d31c16d40deb46f168ae52a9e61b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a>Vytvářet a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI

Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí. Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit a připojit k virtuálnímu počítači
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v *eastus* oblast. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

Skupina prostředků je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) příkaz. 

Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností. V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* systémem Ubuntu Server. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

To může trvat několik minut pro vytvoření virtuálního počítače. Po vytvoření virtuálního počítače Azure CLI výstupy informace o virtuálním počítači. Poznamenejte si `publicIpAddress`, tuto adresu můžete použít pro přístup k virtuálnímu počítači... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a>Připojit k virtuálnímu počítači

Nyní můžete připojit k virtuálnímu počítači pomocí protokolu SSH v prostředí cloudu Azure nebo z místního počítače. Nahraďte na příkladu IP adresu `publicIpAddress` si poznamenali v předchozím kroku.

```bash
ssh 52.174.34.95
```

Po přihlášení k virtuálnímu počítači, můžete nainstalovat a nakonfigurovat aplikace. Po dokončení zavřete relace SSH jako za normálních okolností:

```bash
exit
```

## <a name="understand-vm-images"></a>Pochopení Image virtuálních počítačů

Azure marketplace obsahuje celou řadu imagí, které lze použít k vytvoření virtuálních počítačů. V předchozích krocích byl vytvořen virtuální počítač pomocí Ubuntu obrázku. V tomto kroku rozhraní příkazového řádku Azure se používá pro vyhledávání na webu marketplace CentOS bitové kopie, které se pak použije k nasazení druhého virtuálního počítače.  

Pokud chcete zobrazit seznam nejvíc běžně používané bitové kopie, použijte [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkaz.

```azurecli-interactive 
az vm image list --output table
```

Výstup příkazu vrátí nejoblíbenější Image virtuálních počítačů v Azure.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Úplný seznam můžete zobrazit tak, že přidáte `--all` argument. Seznam obrázků lze také filtrovat podle `--publisher` nebo `–-offer`. V tomto příkladu seznam je filtrovaný pro všechny Image s nabídku odpovídající *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Částečné výstup:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

K nasazení virtuálního počítače pomocí konkrétní image, poznamenejte si hodnotu v *Urn* sloupce. Při zadávání bitovou kopii, číslo verze bitové kopie lze nahradit "nejnovější", který vybere nejnovější verzi rozdělení. V tomto příkladu `--image` argument slouží k určení nejnovější verze CentOS 6.5 image.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>Pochopení velikosti virtuálních počítačů

Velikost virtuálního počítače určuje množství výpočetní prostředky, jako je například procesoru, grafického procesoru a paměti, které jsou k dispozici pro virtuální počítač. Virtuální počítače musí být dimenzovány správně očekávané pracovní zátěže. Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.

### <a name="vm-sizes"></a>Velikosti virtuálních počítačů

V následující tabulce velikostí rozděluje do případy použití.  

| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Obecné účely](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7| Vyrovnáváním procesoru do paměti. Ideální pro vývoj / testování a malé a střední řešení aplikacím a datům.  |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)   | Služby FS, F             | Vysoké využití procesoru do paměti. Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.        |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Vysoká paměti na core. Výborně hodí pro relačních databází, středních a velkých mezipaměti a analýzy v paměti.                 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)      | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| [GPU](sizes-gpu.md)          | VS, NC            | Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.       |
| [Vysoký výkon](sizes-hpc.md) | H, A8 11          | Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA). 


### <a name="find-available-vm-sizes"></a>Najít dostupných velikostí virtuálních počítačů

Pokud chcete zobrazit seznam velikostí virtuálních počítačů v určité oblasti, použijte [seznam velikosti virtuálních počítačů az](/cli/azure/vm#list-sizes) příkaz. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Částečné výstup:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Vytvoření virtuálního počítače s určitou velikost

V předchozím příkladu vytvoření virtuálního počítače s velikostí nebylo zadáno, výsledkem je výchozí velikost. Velikost virtuálního počítače lze vybrat pomocí čas vytvoření [vytvořit virtuální počítač az](/cli/azure/vm#create) a `--size` argument. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>Změna velikosti virtuálního počítače

Po nasazení virtuálního počítače, můžete změnit velikost můžete zvýšit nebo snížit přidělení prostředků. Zobrazí aktuální velikost virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):

```azurecli-interactive
az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize
```

Před změnou velikosti virtuálního počítače, zkontrolujte, jestli požadovaná velikost je dostupná v aktuálním Azure clusteru. [Az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options) příkaz vrátí seznam velikostí. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
Pokud požadovaná velikost je k dispozici, virtuální počítač velikost lze změnit ze stavu na zapnuté, ale po restartu během operace. Použití [změnit velikost virtuálního počítače az]( /cli/azure/vm#resize) příkaz k provedení změna.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

Pokud požadovaná velikost není v aktuální clusteru, musí být navrácena, než dojde k operace změny velikosti virtuálního počítače. Použití [az OM deallocate]( /cli/azure/vm#deallocate) příkaz k zastavení a zrušit přidělení virtuálního počítače. Poznámka: Pokud virtuální počítač je zapnutý zpět, může odeberou všechna data na dočasné disku. Veřejná IP adresa se také změní, pokud se používá statickou IP adresu. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Jakmile navrácena, může dojít, změnu velikosti. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Po změny velikosti můžete spustit virtuální počítač.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>Stavy spotřeby virtuálních počítačů

Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie. Tento stav představuje aktuální stav virtuálního počítače z hlediska hypervisoru. 

### <a name="power-states"></a>Stavy spotřeby.

| Stav napájení | Popis
|----|----|
| Spouštění | Označuje, že virtuální počítač se spouští. |
| Běžící (Spuštěno) | Určuje, zda je virtuální počítač spuštěn. |
| Zastavování | Označuje, že je zastavení virtuálního počítače. | 
| Zastaveno | Označuje, že virtuální počítač je zastavená. Virtuální počítače v zastaveném stavu stále toho vám být účtovány poplatky za výpočty.  |
| Rušení přidělování | Označuje, že virtuální počítač je navrácena. |
| Zrušeno | Označuje, že virtuální počítač je odebrán z hypervisor, ale stále k dispozici v rovině řízení. Virtuální počítače ve stavu Deallocated nevznikají poplatky za výpočty. |
| - | Označuje, že stav napájení virtuálního počítače neznámý. |

### <a name="find-power-state"></a>Najít stav napájení

Chcete-li načíst stav konkrétní virtuální počítač, použijte [az virtuální počítač získat zobrazení instance](/cli/azure/vm#get-instance-view) příkaz. Je nutné zadat platný název pro virtuální počítač a skupinu prostředků. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Výstup:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Úlohy správy

Během životního cyklu virtuálního počítače můžete spustit úlohy správy, jako je například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete vytvořit skripty pro automatizaci úloh opakovaných nebo komplexní. Použití Azure CLI, mnoho běžné úlohy správy lze spustit z příkazového řádku nebo ve skriptech. 

### <a name="get-ip-address"></a>Získat IP adresu

Tento příkaz vrátí privátní a veřejné IP adresy virtuálního počítače.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Zastavit virtuální počítač

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Spustit virtuální počítač

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Odstranění skupiny prostředků

Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci, například virtuálních počítačů, virtuální sítě a disku. `--no-wait` Parametr vrátí ovládacího prvku do příkazového řádku bez čekání na dokončení operace. `--yes` Parametr potvrdí, že chcete odstranit prostředkům bez další dotazy Uděláte to tak.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:

> [!div class="checklist"]
> * Vytvořit a připojit k virtuálnímu počítači
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače

Přechodu na v dalším kurzu se dozvíte o disky virtuálních počítačů.  

> [!div class="nextstepaction"]
> [Vytvoření a správa virtuálních počítačů disky](./tutorial-manage-disks.md)
