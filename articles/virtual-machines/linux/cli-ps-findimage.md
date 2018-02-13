---
title: "Vyberte bitové kopie virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku Azure k určení vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79eb69b83e4ffc0a4ad7c2631ce4d1306a1e335c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Postup nalezení bitové kopie virtuálního počítače s Linuxem v Azure Marketplace pomocí Azure CLI
Toto téma popisuje, jak pomocí Azure CLI 2.0 najít Image virtuálních počítačů v Azure Marketplace. Tyto informace slouží k určení image pořízenou prostřednictvím Marketplace, při vytváření virtuálního počítače s Linuxem.

Ujistěte se, že jste nainstalovali nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a jsou přihlášení k účtu Azure (`az login`).

## <a name="terminology"></a>Terminologie

Obrázky Marketplace jsou určené v rozhraní příkazového řádku a dalších nástrojů Azure podle hierarchie:

* **Vydavatel** -organizace, který vytvořil bitovou kopii. Příklad: kanonickém tvaru
* **Nabízejí** -skupina související bitové kopie vytvořené vydavatelem. Příklad: Ubuntu Server
* **Skladová položka** – instanci nabídku, jako je například hlavní verze distribuce. Příklad: 16.04-LTS
* **Verze** -číslo verze SKU bitové kopie. Při zadávání bitovou kopii, můžete nahradit číslo verze se "nejnovější", který vybere nejnovější verzi rozdělení.

Pokud chcete zadat image pořízenou prostřednictvím Marketplace, obvykle použít image *URN*. Název URN kombinuje tyto hodnoty oddělené dvojtečkou (:): *vydavatele*:*nabízejí*:*Sku*:*verze*. 


## <a name="list-popular-images"></a>Seznam oblíbených obrázků

Spustit [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#az_vm_image_list) příkazu, aniž by `--all` možnost, podívejte se do seznamu oblíbených Image virtuálních počítačů v Azure Marketplace. Například spusťte následující příkaz, který zobrazí seznam oblíbených bitové kopie v mezipaměti ve formátu tabulky:

```azurecli
az vm image list --output table
```

Výstup obsahuje název URN (hodnota v *Urn* sloupec), který se používá k určení bitovou kopii. Při vytváření virtuálního počítače s jedním z těchto oblíbených bitových kopií Marketplace, můžete alternativně zadat název URN alias, například *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Vyhledání konkrétních imagí

Chcete-li vyhledat konkrétní image virtuálního počítače na Marketplace, použijte `az vm image list` s `--all` možnost. Tato verze příkazu trvá delší dobu a lze vrátit zdlouhavé výstup, takže obvykle filtrovat seznam podle `--publisher` nebo jiný parametr. 

Například následující příkaz zobrazí všechny Debian nabídky (Nezapomeňte, že bez `--all` přepnout, prochází pouze místní mezipaměti běžné bitových kopií):

```azurecli
az vm image list --offer Debian --all --output table 

```

Částečné výstup: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

Použití filtrů podobné s `--location`, `--publisher`, a `--sku` možnosti. Termínem shodují jen částečně můžete provést i na filtrem, třeba hledání `--offer Deb` najít všechny Debian Image.

Pokud nezadáte konkrétního umístění s `--location` možnost hodnoty `westus` jsou vráceny ve výchozím nastavení. (Spuštěním nastavte jiné výchozí umístění `az configure --defaults location=<location>`.)

Například následující příkaz zobrazí všechny Debian SKU 8 v `westeurope`:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Částečné výstup:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-the-images"></a>Přejděte bitové kopie 
Jiný způsob, jak najít bitovou kopii v umístění je spuštění [bitové kopie virtuálních počítačů az seznamu vydavatelů](/cli/azure/vm/image#az_vm_image_list_publishers), [bitové kopie virtuálních počítačů az seznamu nabízí](/cli/azure/vm/image#az_vm_image_list_offers), a [bitové kopie virtuálních počítačů az seznamu SKU](/cli/azure/vm/image#az_vm_image_list_skus) příkazy v pořadí. Pomocí těchto příkazů určit tyto hodnoty:

1. Vypsat vydavatele imagí.
2. Pro daného vydavatele vypsat jeho nabídky.
3. Pro danou nabídku vypsat její skladovou jednotku (SKU).


Například následující příkaz seznam vydavatelů bitové kopie v umístění západní USA:

```azurecli
az vm image list-publishers --location westus --output table
```

Částečné výstup:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Tyto informace použijte k vyhledání nabídky od konkrétní vydavatele. Například pokud Canonical vydavatele bitové kopie v umístění západní USA, najít jejich nabídky spuštěním `azure vm image list-offers`. Předejte umístění a vydavatele jako v následujícím příkladu:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Výstup:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Uvidíte, že v oblasti západní USA Canonical publikuje **UbuntuServer** nabídku Azure. Ale co skladové jednotky (SKU)? Chcete-li získat tyto hodnoty, spusťte `azure vm image list-skus` a nastavte umístění, vydavatele a nabídky, které byly zjištěny:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Výstup:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Nakonec použijte `az vm image list` příkazu najděte na konkrétní verzi verze SKU chcete například **16.04 LTS**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Výstup:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```
## <a name="next-steps"></a>Další postup
Nyní můžete přesněji bitovou kopii, kterou chcete použít provedením Poznámka URN hodnoty. Předat tuto hodnotu s `--image` parametr při vytváření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#az_vm_create) příkaz. Mějte na paměti, že můžete volitelně nahradit číslo verze v název URN "poslední zálohy". Tato verze je vždy nejnovější verzi rozdělení. Pokud chcete rychle vytvořit virtuální počítač pomocí informací o URN, přečtěte si téma [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure](tutorial-manage-vm.md).
