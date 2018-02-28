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
ms.date: 02/22/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6e6f9ca3b314ee2f58d8007e7ddc93ddd213e361
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Přidání bitové kopie systému Linux do Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Linuxové virtuální počítače (VM) můžete nasadit v Azure zásobníku přidáním bitové kopie založené na systému Linux do Azure Marketplace zásobníku. Nejjednodušší způsob, jak přidat bitovou kopii systému Linux do protokolů Azure je přes správu Marketplace. Tyto bitové kopie byly připravené a testovány z hlediska kompatibility s Azure zásobníku.

## <a name="marketplace-management"></a>Správa webu Marketplace

Chcete-li Linux bitové kopie z Azure Marketplace, použijte postupy v následujícím článku. Vyberte Image Linux, které můžete chtít nabízet uživatelům v zásobníku vaší Azure.

[Stažení položky marketplace z Azure do Azure zásobníku](azure-stack-download-azure-marketplace-item.md).

## <a name="prepare-your-own-image"></a>Příprava vlastní image

Můžete připravit Linux image pomocí jedné z následujících pokynů:

   * [Na základě centOS distribuce](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Stáhněte a nainstalujte [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/).

    Azure Linux Agent verze 2.2.2 nebo novější je nutný k poskytování virtuálním počítačům s Linuxem v Azure zásobníku a některé verze nefungují (2.2.12 a 2.2.13 příklady). Řadu distribucí uvedené dříve již obsahují verzi agenta (obvykle nazývá `WALinuxAgent` nebo `walinuxagent`). Pokud vytvoříte vlastní image, musíte nainstalovat agenta ručně. Nainstalovaná verze se dá určit název balíčku, nebo spuštěním `/usr/sbin/waagent -version` ve virtuálním počítači.

    Podle pokynů níže – ruční instalace agenta Azure Linux.

   a. Nejprve stáhnout nejnovější verzi agenta Azure Linux z [Githubu](https://github.com/Azure/WALinuxAgent/releases), například:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. Rozbalte Azure agenta:

            # tar -vzxf v2.2.21.tar.gz
   c. Nainstalujte python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d. Nainstalujte agenta Azure:

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     Systémy s Pythonem 2.x a Python 3.x nainstalován-souběžného možná nutné spustit následující příkaz:

         sudo python3 setup.py install --register-service
     Další informace najdete v tématu Azure Linux Agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Přidání bitové kopie na Marketplace s cílem](azure-stack-add-vm-image.md). Ujistěte se, že `OSType` parametr je nastaven na `Linux`.
3. Po přidání bitovou kopii na Marketplace, je-li vytvořit položku Marketplace. a mohou uživatelé nasadit virtuální počítač s Linuxem.

## <a name="next-steps"></a>Další postup
[Přehled nabídky služeb v Azure zásobníku](azure-stack-offer-services-overview.md)
