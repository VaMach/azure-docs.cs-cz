---
title: "Přidání bitové kopie systému Linux do Azure zásobníku"
description: "Zjistěte, jak přidat Linux bitové kopie do protokolů Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: anajod
ms.openlocfilehash: 29e5443de4dc43efe6d536b0f8b9cfc6ad37a669
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Přidání bitové kopie systému Linux do Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit* 

Přidání bitové kopie založené na systému Linux do Azure Marketplace zásobníku můžete nasadit virtuální počítače s Linuxem v Azure zásobníku. Nejjednodušší způsob, jak přidat bitovou kopii systému Linux do protokolů Azure je přes správu Marketplace.

## <a name="marketplace-management"></a>Správa webu Marketplace

Chcete-li Linux bitové kopie z Azure Marketplace, použijte postupy v následujícím článku. Vyberte Image Linux, které můžete chtít nabízet uživatelům v zásobníku vaší Azure.

[Stažení položky marketplace z Azure do Azure zásobníku](azure-stack-download-azure-marketplace-item.md).

## <a name="download-an-image"></a>Stáhnout obrázek

Můžete stáhnout a extrahovat Image Azure zásobníku kompatibilní Linux pomocí následujících odkazů:


   * [Bitnami](https://bitnami.com/azure-stack)
   * [CentOS](http://olstacks.cloudapp.net/latest/)
   * [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
   * [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
   * [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

1. Extrahování bitovou kopii virtuálního pevného disku, v případě potřeby a [lze přidat image do Marketplace](azure-stack-add-vm-image.md). Ujistěte se, že `OSType` parametr je nastaven na `Linux`.
2. Po přidání bitovou kopii na Marketplace, je-li vytvořit položku Marketplace. a mohou uživatelé nasadit virtuální počítač s Linuxem.

## <a name="prepare-your-own-image"></a>Příprava vlastní image

Můžete připravit Linux image pomocí jedné z následujících pokynů:
   
   * [Na základě centOS distribuce](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Oracle Linux](../virtual-machines/linux/oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](../virtual-machines/linux/redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Stáhněte a nainstalujte [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/).
   
    Azure Linux Agent verze 2.1.3 nebo vyšší se vyžaduje ke zřízení virtuálním počítačům s Linuxem v Azure zásobníku. Řadu distribucí uvedené dříve již zahrnout tato verze agenta nebo vyšší jako balíček jejich úložiště (obvykle nazývá `WALinuxAgent` nebo `walinuxagent`). Ale pokud verze balíčku Azure agent je menší než 2.1.3 (například 2.0.18 nebo nižší), musíte ručně nainstalovat agenta. Nainstalovaná verze se dá určit buď z název balíčku, nebo spuštěním `/usr/sbin/waagent -version` ve virtuálním počítači.
   
    Podle pokynů níže – ruční instalace agenta Azure Linux.
   
   a. Nejprve stáhnout nejnovější verzi agenta Azure Linux z [Githubu](https://github.com/Azure/WALinuxAgent/releases), například:
     
            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.16.tar.gz
   b. Rozbalte Azure agenta:
     
            # tar -vzxf v2.2.16.tar.gz
   c. Nainstalujte python setuptools
     
        **Debian / Ubuntu**
     
            # sudo apt-get update
            # sudo apt-get install python-setuptools
     
        **Ubuntu 16.04+**
     
            # sudo apt-get install python3-setuptools
     
        **RHEL / CentOS / Oracle Linux**
     
            # sudo yum install python-setuptools
   d. Nainstalujte agenta Azure:
     
            # cd WALinuxAgent-2.2.16
            # sudo python3 setup.py install --register-service
     
     Systémy s Pythonem 2.x a Python 3.x nainstalován-souběžného možná nutné spustit následující příkaz:
     
         sudo python3 setup.py install --register-service
     Další informace najdete v tématu Azure Linux Agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Přidání bitové kopie na Marketplace s cílem](azure-stack-add-vm-image.md). Ujistěte se, že `OSType` parametr je nastaven na `Linux`.
3. Po přidání bitovou kopii na Marketplace, je-li vytvořit položku Marketplace. a mohou uživatelé nasadit virtuální počítač s Linuxem.

## <a name="next-steps"></a>Další postup
[Přehled nabídky služeb v Azure zásobníku](azure-stack-offer-services-overview.md)

