---
title: "Nastavení prostředí pro zdroj (VMware do Azure) | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit prostředí místní spuštění replikace virtuálních počítačů VMware do Azure."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2018
ms.author: anoopkv
ms.openlocfilehash: ff927a4846ba63d3f00d0e81b8cb818af1441449
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Nastavení prostředí pro zdroj (VMware do Azure)
> [!div class="op_single_selector"]
> * [Z VMware do Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fyzické do Azure](./site-recovery-set-up-physical-to-azure.md)

Tento článek popisuje, jak nastavit zdroj, v místním prostředí, replikovat virtuální počítače běžící ve VMware do Azure. Její součástí jsou kroky pro výběr váš scénář replikace, nastavení se místní počítač jako server konfigurace Site Recovery, a automaticky zjišťování místních virtuálních počítačů. 

## <a name="prerequisites"></a>Požadavky

Článek předpokládá, že již máte:
- [Nastavení prostředků](tutorial-prepare-azure.md) v [portál Azure](http://portal.azure.com).
- [Nastavit místní VMware](tutorial-prepare-on-premises-vmware.md), včetně vyhrazený účet pro automatické zjišťování.



## <a name="choose-your-protection-goals"></a>Volba cílů ochrany

1. V portálu Azure přejděte do **služeb zotavení** okno trezoru a vyberte svůj trezor.
2. V nabídce prostředků úložiště, přejděte na **Začínáme** > **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **do Azure**a zvolte **Ano, s hypervisoru VMware vSphere**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Nastavení konfigurace serveru

Nastavení konfigurace serveru jako místní virtuálních počítačů VMware, použijte šablonu otevřete virtualizace formát OVF (). [Další informace](concepts-vmware-to-azure-architecture.md) o součástech, které se nainstalují na virtuální počítač VMware. 

1. Další informace o [požadavky](how-to-deploy-configuration-server.md#prerequisites) pro nasazení konfiguračního serveru. [Zkontrolujte kapacitu čísla](how-to-deploy-configuration-server.md#capacity-planning) pro nasazení.
2. [Stáhněte si](how-to-deploy-configuration-server.md#download-the-template) a [importovat](how-to-deploy-configuration-server.md#import-the-template-in-vmware) OVF šablony (how-k-nasazení konfigurace server.md) nastavit VMware místní virtuální počítač, který běží na konfiguračním serveru.
3. Zapnout virtuální počítač VMware a [registraci](how-to-deploy-configuration-server.md#register-the-configuration-server) v služeb zotavení trezoru.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Přidejte účet VMware pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Připojení k serveru VMware

Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery.

Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Běžné problémy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Další postup
[Nastavení cílového prostředí](./site-recovery-prepare-target-vmware-to-azure.md) v Azure.
