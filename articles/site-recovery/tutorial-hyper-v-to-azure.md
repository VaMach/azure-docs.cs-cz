---
title: "Nastavit zotavení po havárii pro místní technologie Hyper-V virtuální počítače (bez VMM) do Azure s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii v místě virtuálních počítačů technologie Hyper-V (bez VMM) do Azure se službou Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure. Tento kurz je relevantní pro virtuální počítače Hyper-V, které nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vyberte zdroj replikace a cíle.
> * Nastavení prostředí zdroje replikace, včetně místní součásti Site Recovery a cílovém prostředí replikace.
> * Vytvoření zásady replikace.
> * Povolení replikace pro virtuální počítač.

Toto je třetí kurz v řadě. V tomto kurzu předpokládá, že jste už dokončili úkoly v předchozí kurzy:

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava místní technologie Hyper-V](tutorial-prepare-on-premises-hyper-v.md)

Než začnete, je vhodné [zkontrolujte architekturu](concepts-hyper-v-to-azure-architecture.md) pro tento scénář obnovení po havárii.

## <a name="select-a-replication-goal"></a>Vyberte cíle replikace


1. V **všechny služby** > **trezory služeb zotavení**, klikněte na název trezoru jsme připravené v předchozím kurzu **ContosoVMVault**.
2. V **Začínáme**, klikněte na tlačítko **Site Recovery**. Pak klikněte na tlačítko **Příprava infrastruktury**
3. V **cíl ochrany** > **kam se vaše počítače umístěné**, vyberte **místní**.
4. V **kde chcete počítače replikovat**, vyberte **do Azure**.
5. V **jsou vaše počítače virtualizovaných**, vyberte **Ano, s technologií Hyper-V**.
6. V **používáte System Center VMM**, vyberte **ne**. Pak klikněte na **OK**.

    ![Cílem replikace](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení prostředí zdroje, přidání hostitelů Hyper-V na Hyper-V lokalitě, stáhněte a nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure a zaregistrujte web Hyper-V v trezoru. 

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**.
2. Klikněte na tlačítko **serveru technologie Hyper-V +**a zadejte název lokality jsme vytvořili v předchozí kurzu **ContosoHyperVSite**.
3. Klikněte na tlačítko **+ technologie Hyper-V Server**.
4. Stáhněte si instalační soubor zprostředkovatele.
5. Stáhnout registrační klíč trezoru Musíte to, když spustíte instalační program zprostředkovatele. Klíč je platný pět dní od jeho vygenerování.

    ![Stáhnout poskytovatele](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>Nainstalujte zprostředkovatele

Spusťte instalační soubor zprostředkovatele (AzureSiteRecoveryProvider.exe) na každém hostitel technologie Hyper-V můžete přidat do **ContosoHyperVSite** lokality. Instalační program nainstaluje zprostředkovatele Azure Site Recovery a agenta služeb zotavení, na každém hostiteli technologie Hyper-V.

1. V průvodci Instalace zprostředkovatele Azure Site Recovery > **Microsoft Update**, vyjádřit výslovný souhlas používat službu Microsoft Update ke kontrole aktualizací zprostředkovatele.
2. V **instalace**, přijměte výchozí umístění instalace pro zprostředkovatel a agent a klikněte na tlačítko **nainstalovat**.
3. Po instalaci, v Průvodce registrací Microsoft Azure Site Recovery > **nastavení trezoru**, klikněte na tlačítko **Procházet**a v **soubor klíče**, vyberte klíč trezoru soubor, který Stáhnout. 
4. Zadejte předplatné Azure Site Recovery, název trezoru (**ContosoVMVault**) a v lokalitě Hyper-V (**ContosoHyperVSite**), ke které patří server Hyper-V.
5. V **nastavení proxy serveru**, vyberte **připojit se přímo k Azure Site Recovery bez serveru proxy**.
6. V **registrace**po server je zaregistrovaný v trezoru, klikněte na tlačítko **Dokončit**.

Načte Azure Site Recovery metadata ze serveru technologie Hyper-V a server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**. To může trvat až 30 minut.


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky. 

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte předplatné a skupina prostředků **ContosoRG**, ve kterém virtuální počítače Azure bude vytvořen po převzetí služeb při selhání.
3. Vyberte **Resource Manager "** modelu nasazení.

Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.


## <a name="set-up-a-replication-policy"></a>Nastavení zásad replikace

1. Klikněte na tlačítko **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.
2. V **vytvořit a přidružit zásady**, zadejte název zásady, **ContosoReplicationPolicy**.
3. Ponechejte výchozí nastavení a klikněte na tlačítko **OK**.
    - **Frekvence kopírování** označuje, že rozdílová data (po počáteční replikaci) bude replikovat každých pět minut.
    - **Uchování bodu obnovení** označuje, že windows uchování pro každý bod obnovení bude dva dvě hodiny.
    - **Frekvence snímkování konzistentní aplikace vzhledem** označuje, že body obnovení obsahující snímky konzistentní aplikace se vytvoří každou hodinu.
    - **Čas spuštění počáteční replikace**, označuje, že počáteční replikace se spustí okamžitě.
4. Po vytvoření zásady, klikněte na tlačítko **OK**. Když vytvoříte novou zásadu je automaticky přiřazen k zadané lokalitě Hyper-V (**ContosoHyperVSite**)

    ![Zásady replikace](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Povolení replikace


1. V **replikujte aplikaci**, klikněte na tlačítko **zdroj**. 
2. V **zdroj**, vyberte **ContosoHyperVSite** lokality. Pak klikněte na **OK**.
3. V **cíl**, ověřte Azure jako cíl, předplatné trezoru a **Resource Manager** modelu nasazení.
4. Vyberte **contosovmsacct1910171607** účet úložiště, které jsme vytvořili v předchozí kurzu pro replikovaná data a **ContosoASRnet** síť, ve které virtuálních počítačích Azure budou umístěné po převzetí služeb při selhání.
5. V **virtuální počítače** > **vyberte**, vyberte virtuální počítač, které chcete replikovat. Pak klikněte na **OK**.

 Můžete sledovat průběh **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po **dokončení ochrany** úloha dokončena počáteční replikace je dokončena a virtuální počítač je připraven k převzetí služeb při selhání.

## <a name="next-steps"></a>Další postup
[Spuštění postupu zotavení po havárii](tutorial-dr-drill-azure.md)
