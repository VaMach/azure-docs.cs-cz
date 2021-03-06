---
title: "Virtuální počítač Azure s Linuxem velikostí - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostí, které jsou k dispozici pro Linux s vysokým výkonem virtuálních počítačů v Azure. Uvádí informace o počtu Vcpu, datové disky a síťové adaptéry, jakož i úložiště propustnost a šířku pásma sítě pro velikosti této série."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2017
ms.author: jonbeck
ms.openlocfilehash: cdfd09d90be9696dacc151e138920944c8bbd2c9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Vysoký výkon výpočetní velikostí virtuálních počítačů

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA podporovat instancí
Podmnožinu náročné instance (H16r, H16mr, NC24r, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA). Toto rozhraní je kromě standardní Azure síťové rozhraní, které jsou dostupné další velikosti virtuálních počítačů. 
  
Toto rozhraní umožňuje podporu rdma instance komunikovat přes síť InfiniBand, provoz se FDR sazby za H16r, H16mr a NC24r virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů. Tyto funkce RDMA může zvýšit škálovatelnost a výkon aplikací rozhraní MPI (Message Passing) spuštěna pod Intel MPI pouze 5.x. Novější verze (2017, 2018) knihovny modulu runtime Intel MPI nejsou kompatibilní s ovladači Azure RDMA.

Nasazení podporující RDMA virtuální počítače ve stejné skupině dostupnosti (při použití modelu nasazení Azure Resource Manager) nebo stejné cloudové služby (při použití modelu nasazení classic). Následují další požadavky na podporu rdma virtuální počítače s Linuxem pro přístup k síti Azure RDMA.

### <a name="distributions"></a>Distribuce
 
Nasazení náročné na výkon virtuálního počítače z jedné bitové kopie v Azure Marketplace, která podporuje připojení RDMA:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. Konfigurace ovladače RDMA na virtuálním počítači a zaregistrovat Intel ke stažení Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 pro prostředí HPC, SLES 12 SP3 pro HPC (Premium), SLES 12 SP1 pro prostředí HPC, SLES 12 SP1 pro HPC (Premium). Instalace ovladačů RDMA a Intel MPI balíčků distribuováno do virtuálního počítače. Instalace MPI spuštěním následujícího příkazu:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Na základě centOS HPC** – na základě CentOS 7.3 HPC, na základě CentOS 7.1 HPC, na základě CentOS 6.8 HPC nebo na základě CentOS verze 6.5 HPC (pro H-series, je doporučeno verze 7.1 nebo novější). RDMA ovladače a Intel MPI 5.1 jsou nainstalovány ve virtuálním počítači.  
 
  > [!NOTE]
  > Na základě CentOS HPC Image, jsou zakázané jádra aktualizace v **yum** konfigurační soubor. Je to proto, že jsou ovladače Linux RDMA distribuován jako balíček RPM a aktualizací ovladače nemusí fungovat, pokud se aktualizuje jádra.
  > 
 
### <a name="cluster-configuration"></a>Konfigurace clusteru 
    
Konfigurace dalších systému je potřeba k spouštění úloh MPI na clusterových virtuálních počítačích. Například v clusteru virtuálních počítačů, musíte vytvořit vztah důvěryhodnosti mezi výpočetní uzly. Typické nastavení, najdete v části [nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Aspekty topologie sítě
* Na podporou RDMA virtuální počítače s Linuxem v Azure je Eth1 vyhrazený pro RDMA síťový provoz. Neměňte nastavení Eth1 nebo jakékoli informace v souboru konfigurace odkazující na tuto síť. Eth0 je vyhrazený pro regulární Azure síťový provoz.

* V Azure není podporována IP přes InfiniBand (IB). Je podporován pouze RDMA přes IB.

## <a name="using-hpc-pack"></a>Pomocí sady HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností budete moci použít náročné instancí operačního systému Linux. Nejnovější verze sady HPC Pack podporu několik distribucí Linux ke spuštění na výpočetních uzlech nasazené ve virtuálních počítačích Azure, spravuje hlavního uzlu systému Windows Server. S podporou RDMA Linux výpočetní uzly systémem Intel MPI HPC Pack můžete naplánovat a spustit Linux MPI aplikace, které přístup k síti RDMA. V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Další velikosti
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](sizes-memory.md)
- [Optimalizované z hlediska úložiště](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Další postup

- Abyste mohli začít, nasazení a používání náročné velikosti s RDMA v systému Linux, najdete v části [nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.




