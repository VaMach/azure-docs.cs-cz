---
title: "Migrovat virtuální počítače z AWS do Azure | Microsoft Docs"
description: "Tento článek popisuje, jak migrovat virtuální počítače běžící v Amazon Web Services (AWS) do Azure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: b3c0727a279649f4f7dae30d41027129ce5b04ee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrovat virtuální počítače v Amazon Web Services (AWS) do Azure s Azure Site Recovery

Tento článek popisuje, jak migrovat instance AWS Windows na Azure virtuální počítače s [Azure Site Recovery](site-recovery-overview.md) služby.

Migrace je efektivně převzetí služeb při selhání AWS do Azure. Nelze navrácení služeb po obnovení počítače do AWS a neexistuje žádný probíhající replikace. Tento článek popisuje kroky pro migraci na portálu Azure a jsou založené na pokyny [replikace fyzický počítač do Azure](site-recovery-vmware-to-azure.md).

POST jakékoli dotazy nebo připomínky na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Podporované operační systémy

Obnovení lokality lze použít k migraci EC2 instancí s některým z následujících operačních systémů:

- Windows (pouze 64bitová verze)
    - Windows Server 2008 R2 SP1 + (Citrix PV ovladače nebo pouze ovladače AWS PV. **Nejsou podporovány instance systémem RedHat PV ovladače**) Windows serveru 2012 systému Windows Server 2012 R2
- Linux (pouze 64bitová verze)
    - Red Hat Enterprise Linux 6.7 (pouze HVM virtualizované instance)

## <a name="prerequisites"></a>Požadavky

Zde je nutné pro toto nasazení:

* **Konfigurační server**: virtuální počítač Amazon EC2, systém Windows Server 2012 R2 je nasazen jako konfigurační server. Ve výchozím nastavení jsou nainstalované ostatní Azure Site Recovery součásti (procesový server a hlavní cílový server) při nasazování konfigurační server. Tento článek popisuje kroky pro migraci na portálu Azure a jsou založené na pokyny [Další informace](site-recovery-components.md)

* **Instance EC2**: The Amazon EC2 instancí virtuálních počítačů, které chcete migrovat.

## <a name="deployment-steps"></a>Kroky nasazení

1. Vytvořte trezor služby Recovery Services.
2. Skupina zabezpečení vašich EC2 instancí by měla obsahovat pravidla nakonfigurovaný tak, aby umožňovala komunikaci mezi EC2 instanci, která chcete migrovat a instance, na kterém plánujete nasadit konfigurační Server.

3. Na stejném Amazon virtuální privátní Cloud jako vaše EC2 instance nasaďte server konfigurace automatické obnovení systému. Odkazovat VMware / z fyzických na Azure požadavky pro nasazení požadavky na konfiguraci serveru.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Jakmile konfigurační server je nasazena v AWS a zaregistrována trezoru služeb zotavení, byste měli vidět konfigurační server a procesový server v rámci infrastruktury Site Recovery, jak je uvedeno níže:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Poté, co nasadíte konfigurační server (může trvat až 15 minustes pro zobrazí), ověření, že může komunikovat s virtuálními počítači, které chcete migrovat.

6. [Nakonfigurování nastavení replikace](site-recovery-setup-replication-settings-vmware.md).

7. Povolení replikace: povolení replikace pro virtuální počítače, které chcete migrovat. Můžete zjistit pomocí privátních IP adres, které můžete získat z konzole EC2 EC2 instance.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Jakmile chráněné instance EC2 a replikaci do Azure se dokončí, [spustit testovací převzetí služeb](site-recovery-test-failover-to-azure.md) ověření výkon aplikace v Azure.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Spuštění převzetí služeb při selhání z AWS do Azure pro každý virtuální počítač. Volitelně můžete vytvořit plán obnovení a spusťte převzetí služeb při selhání, migrovat několik virtuálních počítačů z AWS do Azure. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

10. U migrace není nutné provádět převzetí služeb při selhání. Místo toho vybrat možnost provedení migrace pro každý počítač, který chcete migrovat. Dokončení migrace akci dokončí se proces migrace, odebere replikaci pro počítač a zastaví fakturace Site Recovery pro tento počítač.

    ![Migrace](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Další kroky

- [Připravte migrované počítače na povolení replikace](site-recovery-azure-to-azure-after-migration.md) do jiné oblasti pro případ, že by bylo nutné je zotavit po havárii.
- Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).
