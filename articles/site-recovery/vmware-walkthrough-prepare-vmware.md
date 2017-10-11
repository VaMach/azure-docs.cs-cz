---
title: "Příprava místních prostředků VMware pro replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které potřebujete pro replikaci úlohy běžící na virtuálních počítačích VMware do úložiště Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 3e1c589030210c2eae1ad9c02811775d9d6365d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-to-azure"></a>Krok 6: Příprava místní replikace VMware do Azure

Postupujte podle pokynů v tomto článku připravit místní servery VMware pro interakci s Azure Site Recovery a připravit virtuální počítače VMWare pro instalaci služby Mobility. Agent služby Mobility, musí být nainstalován na všech virtuálních počítačích na místě, které chcete replikovat do Azure.

## <a name="prepare-for-automatic-discovery"></a>Příprava pro automatické zjišťování

Site Recovery automaticky vyhledá virtuální počítače spuštěné v hostitelích ESXi vSphere (s nebo bez vCenter server). Site Recovery pro automatické zjišťování, potřebuje účet pro přístup hostitelů a serverů:

1. Pokud chcete použít vyhrazený účet, vytvoření role (na úrovni vCenter s oprávnění popsaná v následující tabulce. Pojmenujte ji jako **Azure_Site_Recovery**.
2. Potom vytvořte uživatele na serveru hostitele/vCenter vSphere a přiřadit role uživatele. Tento uživatelský účet je zadat během nasazování Site Recovery.


### <a name="vmware-account-permissions"></a>Oprávnění pro uživatelský účet VMware

Site Recovery potřebuje přístup k VMware u procesového serveru na automatické zjišťování virtuálních počítačů a převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

- **Migrace**: Pokud chcete migrovat virtuální počítače VMware do Azure, aniž by někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. Tato akce není nezbytné pro migraci.
- **Replikovat nebo obnovit**: Pokud chcete nasadit úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) účet musí být schopen spuštění operací, jako je například vytváření a odebrání disků, zapínání virtuální počítače atd.
- **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


**Úkol** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**
--- | --- | --- | ---
**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).
**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, který má podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (Azure_Site_Recovery) s požadovanými oprávněními a potom přiřadit roli VMware uživatele nebo skupinu | Objekt datového centra –> Propagate pro podřízený objekt role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Zdroj -> Přiřazení virtuálního počítače do fondu zdrojů, migrovat napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).


## <a name="prepare-for-push-installation-of-the-mobility-service"></a>Příprava pro nabízenou instalaci služby Mobility

Služba Mobility musí být nainstalovaná na všech virtuálních počítačích, které chcete replikovat. Existuje několik způsobů, jak nainstalovat službu, včetně ruční instalaci, nabízená instalace z procesového serveru Site Recovery a instalace pomocí metod, jako je například System Center Configuration Manager.

Pokud chcete použít nabízenou instalaci, je nutné připravit účtu, který Site Recovery můžete použít pro přístup k virtuálnímu počítači.

- Můžete použít domény nebo místní účet
- Pro systém Windows Pokud nepoužíváte účet domény, je nutné zakázat řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li to provést, v registru podle **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.
- Pokud chcete přidat položku registru pro Windows z rozhraní příkazového řádku, zadejte:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Pro systémy Linux musí být účet root na zdrojovém serveru Linux.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 7: vytvoření trezoru](vmware-walkthrough-create-vault.md)
