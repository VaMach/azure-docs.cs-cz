---
title: "Instalace nástroje Azure ovladač N-series pro Windows | Microsoft Docs"
description: "Jak nastavit NVIDIA GPU ovladače pro N-series virtuální počítače se systémem Windows v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b480d10df777a2757c073ff77e1845d33d63163a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Nastavení GPU ovladače pro N-series virtuální počítače se systémem Windows Server
Abyste mohli využívat možnosti GPU Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, nainstalujte podporované NVIDIA grafické ovladače. Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series. Informace o instalaci ovladačů je také k dispozici pro [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Základní specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Windows GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Instalace ovladače

1. Připojte pomocí vzdálené plochy pro každý virtuální počítač N-series.

2. Stáhněte, extrahovat a nainstalujte ovladač podporované pro operační systém Windows.

Na virtuálních počítačích Azure vs je nutné provést restart po instalaci ovladačů. Na virtuálních počítačích NC není nutné restartovat počítač.

## <a name="verify-driver-installation"></a>Ověření instalace ovladačů

Můžete ověřit, instalace ovladačů ve Správci zařízení. Následující příklad ukazuje úspěšná konfigurace karty tesla – měrná K80 ve virtuálním počítači Azure názvového kontextu.

![Vlastnosti ovladač GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

K dotazování na stav zařízení GPU, spusťte [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) pomocí ovladače nainstalovaný nástroj příkazového řádku.

1. Otevřete příkazový řádek a změňte **C:\Program Files\NVIDIA Corporation\NVSMI** adresáře.

2. Spustit **nvidia smi**. Pokud je nainstalován ovladač zobrazí se výstup podobný níže. Všimněte si, že **GPU Util** ukazuje **0 %** Pokud aktuálně používáte zatížení grafického procesoru na virtuálním počítači.

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA sítě pro virtuální počítače NC24r

Připojení k síti RDMA, můžete povolit pro NC24r virtuální počítače nasazené ve stejné sadě dostupnosti. Rozšíření HpcVmDrivers musí být přidaný do nainstalovat ovladače zařízení sítě systému Windows, které umožňují připojení RDMA. Chcete-li přidat rozšíření virtuálního počítače na virtuální počítač NC24r, použijte [prostředí Azure PowerShell](/powershell/azure/overview) rutiny pro Azure Resource Manager.

> [!NOTE]
> V současné době pouze Windows Server 2012 R2 podporuje RDMA síť na virtuálních počítačích NC24r.
> 

Chcete-li nainstalovat nejnovější verze 1.1 HpcVMDrivers rozšíření na existující virtuální počítač podporuje RDMA s názvem Můjvp v oblasti západní USA:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Další informace najdete v tématu [rozšíření virtuálního počítače a funkce ve Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Síť RDMA podporuje rozhraní MPI (Message Passing) provozu pro aplikace spuštěné s [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) nebo Intel MPI 5.x. 


## <a name="next-steps"></a>Další kroky

* Další informace o grafickými procesory NVIDIA na virtuálních počítačích N-series najdete v tématu:
    * [Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)
    * [Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)

* Můžete také stáhnout a nainstalovat 8 Toolkit CUDA pro vývojářům tvorbu GPU accelerated aplikací pro grafickými procesory tesla – měrná NVIDIA [systému Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) nebo [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). Další informace najdete v tématu [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


