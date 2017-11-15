---
title: "Matici podpory pro replikaci do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje podporované operační systémy a součásti služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: da120d8e325867eaf9eb8b9be1ae8d9152db54c4
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>Podporu pro replikaci do sekundární lokality s Azure Site Recovery

Tento článek shrnuje, co je podporováno při použití [Azure Site Recovery](site-recovery-overview.md) služby replikace do sekundární místní lokalitu.

## <a name="supported-scenarios"></a>Podporované scénáře

**Nasazení** | **Podrobnosti** 
--- | ---
**Z VMware do VMware** | Zotavení po havárii virtuálních počítačů VMware na místě sekundární lokality VMware.<br/><br/> Stažení [InMage Scout uživatelská příručka](https://aka.ms/asr-scout-user-guide)
**Z Hyper-V do Hyper-V** | Zotavení po havárii v místě virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární cloudu VMM.<br></br> Bez VMM není podporována.



  

## <a name="host-servers"></a>Hostitelské servery

**Nasazení** | **Podpora**
--- | ---
**Virtuální počítač VMware nebo fyzický server** | vCenter 5.5, 6.0 a verze 6.5 (podpora pro verzi 5.5 funkce)
**Technologie Hyper-V s nástrojem VMM** | Windows Server 2016 a Windows Server 2012 R2 s nejnovější aktualizace.<br/><br/> Hostitelé systému Windows Server 2016 se mají spravovat nástrojem VMM 2016.<br/><br/> Cloudy VMM 2016 s směs systému Windows Server 2016 a 2012 R2 hostitelů nejsou aktuálně podporovány.<br/><br/> Nasazení, které zahrnují upgrade existující VMM 2012 R2 na System Center 2016 nejsou aktuálně podporovány.


## <a name="support-for-replicated-machine-os-versions"></a>Podpora pro verze operačního systému replikovaného počítače

Následující tabulka shrnuje podporu operačního systému pro počítače replikovat pomocí Site Recovery. Libovolnou úlohu lze spustit na podporovaný operační systém.

**VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | ---
64bitová verze systému Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 s v minimálně SP1<br/><br/> Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos verze 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8 systémem Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4  | Všechny hostované operační systém [podporovaná technologií Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)

## <a name="linux-machine-storage"></a>Úložiště počítače Linux

Je možné replikovat jenom počítače se systémem Linux s následující úložiště:

- Systém (EXT3, ETX4, ReiserFS, XFS) souborů.
- Vícenásobný software zařízení Mapper.
- Správce svazků (LVM2).
- Fyzické servery s HP CCISS řadič úložiště nejsou podporovány.
- Systém souborů ReiserFS je podporována pouze na SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration"></a>Konfigurace sítě

### <a name="hosts"></a>Hostitelé

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Seskupování síťových adaptérů | Ano | Ano
SÍTĚ VLAN | Ano | Ano
IPv4 | Ano | Ano
IPv6 | Ne | Ne

### <a name="guest-vms"></a>Hostované virtuální počítače

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Seskupování síťových adaptérů | Ne | Ne
IPv4 | Ano | Ano
IPv6 | Ne | Ne
Statická IP adresa (Windows) | Ano | Ano
Statická IP adresa (Linux) | Ano | Ano
Více síťovými Kartami | Ano | Ano


## <a name="storage"></a>Úložiště

### <a name="host-storage"></a>Hostování úložiště

**Úložiště (hostitel)** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
SYSTÉM SOUBORŮ NFS | Ano | Není k dispozici
SMB 3.0 | Není k dispozici | Ano
SÍŤ SAN (ISCSI) | Ano | Ano
S více cestami (MPIO) | Ano | Ano

### <a name="guest-or-physical-server-storage"></a>Host nebo fyzický server úložiště

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
VMDK | Ano | Není k dispozici
VHD/VHDX | Není k dispozici | Ano (až 16 disků)
Fin 2 virtuálních počítačů | Není k dispozici | Ano
Sdílený disk clusteru | Ano  | Ne
Šifrované disku | Ne | Ne
ROZHRANÍ UEFI| Ano | Není k dispozici
SYSTÉM SOUBORŮ NFS | Ne | Ne
SMB 3.0 | Ne | Ne
RDM | Ano | Není k dispozici
Disk > 1 TB | Ano | Ano
Svazek s prokládané disku > 1 TB<br/><br/> LVM | Ano | Ano
Prostory úložiště | Ne | Ano
Přidat nebo odebrat aktivní disku | Ano | Ne
Vyloučení disku | Ano | Ano
S více cestami (MPIO) | Není k dispozici | Ano

## <a name="vaults"></a>trezory

**Akce** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Přesunutí trezorů v rámci skupiny prostředků (v rámci nebo předplatných) | Ne | Ne
Přesunout úložiště, sítě, virtuální počítače Azure mezi skupinami prostředků (v rámci nebo předplatných) | Ne | Ne

## <a name="provider-and-agent"></a>Zprostředkovatel a agent

**Název** | **Popis** | **Nejnovější verzi** | **Podrobnosti**
--- | --- | --- | --- | ---
**Zprostředkovatele Azure Site Recovery** | Koordinuje komunikaci mezi místními servery a Azure <br/><br/> Pokud VMM server není nainstalována na místní servery VMM nebo na servery Hyper-V | 5.1.19 ([dostupná z portálu](http://aka.ms/downloaddra)) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Služba mobility** | Koordinuje replikaci mezi místními servery VMware nebo fyzických serverů a sekundární lokality<br/><br/> Nainstalovat na virtuální počítač VMware nebo fyzických serverů, které chcete replikovat  | Není k dispozici (je k dispozici z portálu) | Není k dispozici


## <a name="next-steps"></a>Další kroky

- [Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokality](tutorial-vmm-to-vmm.md)
- [Replikace fyzických serverů a virtuálních počítačů VMware do sekundární lokality](tutorial-vmware-to-vmware.md)
