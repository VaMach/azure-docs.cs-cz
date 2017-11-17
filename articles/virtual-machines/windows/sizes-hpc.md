---
title: "Azure velikosti virtuálních počítačů Windows - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostí, které jsou k dispozici pro Windows s vysokým výkonem virtuálních počítačů v Azure. Uvádí informace o počtu Vcpu, datové disky a síťové adaptéry, jakož i úložiště propustnost a šířku pásma sítě pro velikosti této série."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: fc2cae8208baa211db2166c9d66a83153fa7b445
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="high-performance-compute-vm-sizes"></a>Velikosti virtuálních počítačů pro výpočetní vysoký výkon

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA podporovat instancí
Podmnožinu náročné instance (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA). Toto rozhraní je kromě standardní Azure síťové rozhraní, které jsou dostupné další velikosti virtuálních počítačů. 
  
Toto rozhraní umožňuje podporu rdma instance komunikovat přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů. Tyto funkce RDMA může zvýšit škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).

Toto jsou požadavky pro podporu rdma virtuálních počítačů Windows pro přístup k síti Azure RDMA: 

* **Operační systém**
  
  Windows Server 2012 R2, Windows Server 2012
  
  > [!NOTE]
  > Windows Server 2016 v současné době nepodporuje připojení RDMA v Azure.
  >

* **Skupinu dostupnosti nebo Cloudová služba** – nasazení podporující RDMA virtuální počítače ve stejné skupině dostupnosti (při použití modelu nasazení Azure Resource Manager) nebo stejné cloudové služby (při použití modelu nasazení classic). Pokud používáte Azure Batch, podporující RDMA virtuální počítače musí být ve stejném fondu.

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 nebo novější, Intel MPI knihovny 5.x

  Podporované MPI implementace rozhraní Microsoft Network Direct používají ke komunikaci mezi instancemi. 

* **Adresní prostor sítě RDMA** -The RDMA sítě v Azure si vyhrazuje 172.16.0.0/16 prostor adres. Pro spouštění aplikací MPI v instance nasazené v Azure virtuální síť, ujistěte se, že adresního prostoru virtuální sítě se nepřekrývá sítě RDMA.

* **Rozšíření virtuálního počítače HpcVmDrivers** -na podporu rdma virtuálních počítačů, je nutné přidat rozšíření HpcVmDrivers instalovat ovladače zařízení systému Windows sítě pro připojení RDMA. (V určitých nasazeních A8 a A9 instancí HpcVmDrivers rozšíření se přidá automaticky.) Chcete-li přidat rozšíření virtuálního počítače pro virtuální počítač, můžete použít [prostředí Azure PowerShell](/powershell/azure/overview) rutiny. 

  
  Tento příkaz nainstaluje nejnovější rozšíření HpcVMDrivers verze 1.1 na existující podporující RDMA virtuální počítač s názvem *Můjvp* nasazené ve skupině prostředků s názvem *myResourceGroup* v  *Západní USA* oblasti:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Další informace najdete v tématu [rozšíření virtuálního počítače a funkce](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Můžete také pracovat se rozšíření pro virtuální počítače nasazené v [modelu nasazení classic](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Pomocí sady HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností vytvořit výpočetní cluster v Azure ke spouštění aplikací MPI systému Windows a další úlohy v prostředí HPC. HPC Pack 2012 R2 a novější verze zahrnují běhového prostředí pro MS-MPI, která využívá Azure RDMA síť při nasazení na virtuálních počítačích RDMA podporovat.




## <a name="other-sizes"></a>Další velikosti
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)
- [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)
- [Optimalizované z hlediska GPU](sizes-gpu.md)

## <a name="next-steps"></a>Další kroky

- Kontrolní seznamy pro použití instance náročné s HPC Pack v systému Windows Server, najdete v části [nastavení clusteru s podporou Windows RDMA pomocí sady HPC Pack ke spouštění aplikací MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Použití náročné instancí při spuštění aplikací MPI pomocí služby Azure Batch naleznete v části [pomocí úkolů s více instancemi ke spouštění aplikací rozhraní MPI (Message Passing) ve službě Azure Batch](../../batch/batch-mpi.md).

- Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.




