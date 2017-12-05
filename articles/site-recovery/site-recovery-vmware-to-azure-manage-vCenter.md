---
title: " Správa serveru VMware vCenter server v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak přidávat a spravovat v Azure Site Recovery serveru VMware vCenter."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 12/04/2017
ms.author: anoopkv
ms.openlocfilehash: 2dcca54c9e1e9330bf7c2a37dc08f67817b6af47
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Správa serveru VMware vCenter Server v Azure Site Recovery
Tento článek popisuje různé operace obnovení lokality, které lze provést na VMware vCenter.

## <a name="prerequisites"></a>Požadavky

**Podpora serveru VMware vCenter a VMware vSphere hostitele ESX** | **Podrobnosti** |
|--- | --- |
|**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, systémem 6.5,6.0, 5.5 s nejnovějšími aktualizacemi. Servery musí nacházet ve stejné síti jako konfigurační server (nebo samostatný procesový server).<br/><br/> Doporučujeme, abyste server vCenter pro správu hostitele, kde běží 6.5,6.0 nebo 5.5 s nejnovějšími aktualizacemi. Při nasazení verze 6.0 jsou podporovány pouze funkce, které jsou k dispozici v 5.5.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Příprava účtu pro automatické zjišťování
Site Recovery potřebuje přístup k VMware pro procesový server se automaticky zjistit virtuální počítače a pro převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

* **Migrace**: Pokud chcete migrovat virtuální počítače VMware do Azure, aniž by někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. To není nezbytné pro migraci.
* **Replikovat nebo obnovit**: Pokud chcete nasadit úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) účet musí být schopen spuštění operací, jako je například vytváření a odebrání disků, zapínání virtuálního počítače.
* **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


|**Úlohy** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**|
|--- | --- | --- | ---|
|**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).|
|**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, který má podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.|
|**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (AzureSiteRecoveryRole) s požadovanými oprávněními a potom přiřadit roli VMware uživatele nebo skupinu | Objekt datového centra –> Propagate pro podřízený objekt role = AzureSiteRecoveryRole<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Zdroj -> Přiřazení virtuálního počítače do fondu zdrojů, migrovat napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).|

## <a name="create-an-account-to-connect-to-vmware-vcenter-server-vmware-vsphere-exsi-host"></a>Vytvoření účtu pro připojení k serveru VMware vCenter Server nebo VMware vSphere EXSi hostitele
1. Přihlaste se k serveru konfigurace a spuštění cspsconfigtool.exe pomocí zástupce na ploše.
2. Klikněte na tlačítko **přidat účet** na **spravovat účet** kartě.

  ![Přidat účet](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Zadejte podrobnosti o účtu a klikněte na tlačítko OK pro přidání účtu. Účet musí mít oprávnění uvedených v [Příprava účet pro automatické zjišťování](#prepare-an-account-for-automatic-discovery) části.

  >[!NOTE]
  Jak dlouho trvá asi 15 minut pro informace o účtu možné synchronizovat se službou Site Recovery.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Přidružení VMware vCenter / VMware vSphere ESX host (Přidat vCenter)
* Na portálu Azure přejděte do *YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
* Na stránce s podrobnostmi o konfiguračním serveru klikněte na vCenter tlačítko +.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-to-connect-to-the-vcenter-server-vsphere-esxi-host"></a>Upravit přihlašovací údaje používané k připojení k serveru vCenter nebo hostitelů vSphere ESXi

1. Přihlášení k serveru konfigurace a spuštění cspsconfigtool.exe
2. Klikněte na tlačítko **přidat účet** na **spravovat účet** kartě.

  ![Přidat účet](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Zadejte nové podrobnosti účtu a klikněte na tlačítko OK pro přidání účtu. Účet musí mít oprávnění uvedených v [Příprava účet pro automatické zjišťování](#prepare-an-account-for-automatic-discovery) části.
4. Na portálu Azure přejděte do *YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
5. Na stránce s podrobnostmi o konfiguračním serveru klikněte na **aktualizace serveru** tlačítko.
6. Po dokončení úlohy aktualizace serveru, vyberte vCenter Server otevřete souhrnnou stránku vCenter.
7. Vyberte nově přidaný účet v **účet hostitele server vSphere vCenter** pole a klikněte na tlačítko **Uložit** tlačítko.

  ![Upravit účet](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Odstraňte vCenter v Azure Site Recovery
1. Na portálu Azure přejděte do *YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
2. Konfigurační server podrobnosti stránce vyberte vCenter Server otevřete souhrnnou stránku vCenter.
3. Klikněte na **odstranit** tlačítko pro odstranění serveru vCenter

  ![Odstranění účtu](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Pokud potřebujete změnit IP adresu nebo FQDN Vcenter, podrobnosti o portu budete muset odstranit vCenter Server a přidejte ji zpět znovu.
